# TBD: Default Enarx trust process
- Authors: [Mike Bursell](mike@p2ptrust.org)
- Status: [PROPOSED](/README.md#proposed)
- Since: 2020-03-17
- Status Note: under discussion
- Start Date: 2020-03-17 (date you started working on this idea)
- Tags: trust
- Addresses: (Issue #)[https://github.com/enarx/enarx/issues/]

TODO: this is curently listed as a "concepts" document, but also
includes design information and implementation requirements.  Need
to decide where it best sits.

## Summary

Setting up trust domains and being able to transfer certain entities
between them in central to Enarx.  This RFC describes the default
trust process for Enarx.

## Motivation

One of the key aims of the Enarx project is to allow clients to run
workloads on hosts which are "untrusted".  This RFC builds on previous
RFCs to describe the default model by which Enarx allows this take
place, using the capabilities of TEEs, the underlying CPU(s) and
firmware of the host and other parties.

It should be noted that the key party for whom trust is considered
important within the Enarx project is the workload owner - in this case,
the tenant.  It is trust relationships from this entity to others, and
maintenance of the tenant's trust domain, which are always the
priority - any other benefits which may accrue to other parties, are
considered side benefits.

On a different note, this RFC deals mainly with the communications
between the various Enarx components.  Although the Orchestrator is
involved and mentioned in this document, it is not the document's focus.

## Trust state transitions in Enarx

See rfc#00002 for an introduction to trust relationships.
See rfc#00003 for an introduction to trust domains and their application
to Enarx.
See rfc#00006 for an introduction to endorsing authorities, trust anchors
and trust pivots.

rfc#00003 described the following Enarx use case states for the default
Enarx trust model (TODO: check correctness in rfc#00003 and normalise):
 * State 0 - initial state
 * State 1 - pre-attestation
 * State 2 - attested
 * State 3 - provisioned

Non-Enarx use cases were also discussed, but these are irrelevant to this
discussion, and therefore ignored in this RFC.

The changes between states were characterised as the transition of
particular components between to host domains:
 1. host owner trust domain;
 2. tenant trust domain.

This RFC uses the same states, trust domains and components.  Although
the level of description may feel overly exhaustive, the members of the
Enarx project believe that it is important to be explicit about the trust
models being implemented, and how they reflect the "real world", in order
to allow designers, coders, documenters, testers, auditors and users of
the project to have as full an understanding of the process and
architecture as possible.

### State 0->1 change

#### State 0 - initial state

 * host owner trust domain
    * host
    * host CPU + CPU firmware
    * Enarx host agent
    * Enarx runtime image
    * Other workloads
 * tenant trust domain
    * Orchestrator
    * Enarx client agent
    * Tenant workload image
    * Tenant workload image repository

#### State 1

 * host owner trust domain
    * host
    * host CPU + CPU firmware
    * Enarx host agent
    * **TEE instance**
    * **Enarx runtime image (within TEE instance)**
    * Other workloads
 * tenant trust domain
    * Orchestrator
    * Enarx client agent
    * Tenant workload image
    * Tenant workload image repository

#### Changes

The changes between state 0 and state 1 are the creation of a TEE instance
and the loading of the Enarx runtime image into that instance.

The initialisation of the process is prompted by **Orchestrator**, which
communicates with the **Enarx client agent**, which contacts the **Enarx
host agent** over a network connection (which does not need to be
encrypted).  Mechanism (such as a cryptographic nonce), allowing the
Enarx client agent to match sessions and requests, is included in the
initial request.  The **Enarx host agent** then requests the creation,
which is performed by the **host CPU + CPU firmware**.

The Enarx runtime image is typically stored on host storage (local or
remote - the distinction is not relevant to the process).  The loading
process varies between architectures (TODO: is this true?), but is
requested by the **Enarx host agent**.  Although the initial image may
persist on host storage, there is no requirement for or against this,
and so the component is not listed separately in state 1.  We follow
this convention for later states.

Though the encryption of the channel between the Enarx client agent
and the Enarx host agent is not **required**, the following properties are
considered to be *desirable*:

 * authentication of the Enarx client agent to the Enarx host agent
   * this allows the host to control which parties are permitted to create
   Enarx Keeps.  While this has no direct benefit to the tenant, an
   indirect benefit may be that the host can avoid DoS attacks, thereby
   maintaining availability for workload placement and execution.
   * note that the Enarx host agent is *not* trusted by the tenant, and
   it is therefore the responsibility of the host to decide whether to
   trust any such authentication.
 * integrity of the request
   * some DoS attacks or protocol attacks may be feasible, and mitigated
   by integrity protection.
 * encryption of the protocol handshake and further steps
   * some attacks based around protocol versioning or replays may be
   feasible, and can be avoided by encryption.

For the reasons noted above, encrypting the channel will sometimes be considered
the simplest option.



### State 1->2 change

#### State 1 - pre-attestation

 * host owner trust domain
    * host
    * host CPU + CPU firmware
    * Enarx host agent
    * TEE instance
    * Enarx runtime image (within TEE instance)
    * Other workloads
 * tenant trust domain
    * Orchestrator
    * Enarx client agent
    * Tenant workload image
    * Tenant workload image repository


#### State 2 - attested

 * host owner trust domain
    * host
    * host CPU + CPU firmware
    * Enarx host agent
    * Other workloads
 * tenant trust domain
    * Orchestrator
    * Enarx client agent
    * **Empty Keep**
    * Tenant workload image
    * Tenant workload image repository

#### Changes

The change between state 1 and state 2 is the transition of the TEE
instance and Enarx runtime image within it to an Enarx Keep.  This
transition is considered complete once the Keep has been attested.

The measurement of the TEE instance and Enarx runtime image within it
is requested by the **Enarx host agent** and performed by the **host
CPU + CPU firmware**.  The measurement is then transmitted from the
**Enarx host agent** to the **Enarx client agent**.

The **Enarx host agent** checks the attestation measurement database
(defined in a separate RFC) to ensure that the attestation measurement is
correct.  This process includes checks for:

 * cryptographic correctness
 * TEE correctness
 * valid Enarx runtime image.

If any of these checks fail, the state progression is stopped, and the
**Enarx client agent** logs an error and may send an error message to
the **Enarx host agent**.  The **Enarx client agent** also sends an
error message to the Orchestrator.

If the attestation measurement meets the checking criteria, the **Enarx
client agent** retrieves or derives the session key from the previous
communication (the exact mechanism being CPU architecture-dependent).  At
this point, the state transition is considered complete, and the TEE
instance, loaded with the Enarx runtime image, is now an **Empty Keep**.

### State 2->3 change

#### State 2 - pre-attestation

 * host owner trust domain
    * host
    * host CPU + CPU firmware
    * Enarx host agent
    * Other workloads
 * tenant trust domain
    * Orchestrator
    * Enarx client agent
    * **Empty Keep**
    * Tenant workload image
    * Tenant workload image repository

#### State 3 - attested

 * host owner trust domain
    * host
    * host CPU + CPU firmware
    * Enarx host agent
    * Other workloads
 * tenant trust domain
    * Orchestrator
    * Enarx client agent
    * **Provisioned Keep**
    * Tenant workload image
    * Tenant workload image repository


#### Changes



TODO NOW: Encrypt, send.  Note comms channel properties this time.
TODO NOW: When does the workload get to the ECA?


## Requirements
The channel between the Enarx client agent and the Enarx host agent MAY be
encrypted.

The Enarx client agent SHOULD authenticate to the Enarx host agent.

Integrity protection of the request SHOULD be applied.

The protocol handshake and further steps SHOULD be encrypted.

If integrity protection is applied, all detected integrity failures SHOULD
be reported to the Enarx client agent via the Enarx host agent where possible.

The integrity of the tenant's trust domain MUST always be given the
highest priority.

Protections for the tenant MUST always be prioritised above those of
other parties such as the host.

Confidentiality of the tenant's workload MUST be protected.

Integrity of the tenant's workload MUST be protected.

All protocol errors SHOULD be logged by the Enarx client agent.

All attestation measurements MUST be cryptographically signed.

All attestation measurements MUST be checked.

All attestation measurements SHOULD be logged by the Enarx client agent.

All Enarx client agent logs SHOULD be integrity protected.

All attestation measurement failures MUST result in a cessation of
the standard state progression.

The cessation of the standard state progression before transmission of
the tenant workload MUST result in non-transmission of the workload from
the Enarx client agent to any component on the host.

All attestation measurement failures MUST be logged by the Enarx host
agent.

All attestation measurement failures SHOULD result in an error message
being sent to the Orchestrator.

The Enarx client agent MUST generate and include session tracking
information in the initial communication with the Enarx host agent.

The session tracking information MUST be included in all communications
between the Enarx client agent and the Enarx host agent until the Keep
is fully provisioned.

## Reference

rfc#00002
rfc#00003
rfc#00006
[Define attestation database - issue #25](https://github.com/enarx/rfcs/issues/25)


## Drawbacks

n/a

## Rationale and alternatives

n/a

## Prior art

n/a

## Unresolved questions

- What parts of the design do you expect to resolve through the
RFC process before this gets merged?
- What parts of the design do you expect to resolve through the
implementation of this feature before stabilization?
- What related issues do you consider out of scope for this 
proposal that could be addressed in the future independently of the
solution that comes out of this doc?

## Implementations

None.

## Future Possibilities

- Threat models should be documented and published as RFCs (Issue #240)[https://github.com/enarx/enarx/issues/240]
- Status of hardware/firmware in trust relationships in Enarx
needs to be documented (Issue #193)[https://github.com/enarx/enarx/issues/193]