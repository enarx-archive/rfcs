# 00004: Default Enarx trust process
- Authors: [Mike Bursell](mike@p2ptrust.org)
- Status: [PROPOSED](/README.md#proposed)
- Since: 2020-03-17
- Status Note: under discussion
- Start Date: 2020-03-17
- Tags: trust
- Addresses: [Issue #5](https://github.com/enarx/rfcs/issues/5)

## Summary

Setting up trust domains and being able to transfer certain entities between
them is central to Enarx.  This RFC describes the default trust process for
Enarx and lays out sets of requirements for implementations.

## Motivation

One of the key aims of the Enarx project is to allow clients to run workloads on
hosts which are "untrusted".  This RFC builds on previous RFCs to describe the
default model by which Enarx allows this to take place, using the capabilities
of TEEs, the underlying CPU(s) and firmware of the host and other parties.

It should be noted that within the context of the Enarx project, trust is always
considered first and foremost from the perspective of the workload owner - in
the case described in this document, the tenant.  We should remember that all
trust relationships are explicitly uni-directional (though this does not
preclude a corresponding trust relationship in the other direction).  In the
case of Enarx, it is trust relationships from the workload owner to other
entities (primarily the host owner in this document), and maintenance of the
tenant's trust domain, which are always the priority. Any other benefits which
may accrue to other parties are considered side benefits.

On a different note, this RFC deals mainly with the communications between the
various Enarx components.  Although the Orchestrator is involved and mentioned
in this document, it is not the focus of this document.

## Trust state transitions in Enarx

The states of the default Enarx trust model, defined in another RFC (WIP) are as
follows: 
 - State 0 - initial state
 - State 1 - pre-attestation
 - State 2 - attested
 - State 3 - provisioned


The changes between states are characterised as the transition of particular
components between different trust domains:
 1. the host owner trust domain;
 2. the tenant trust domain.

This RFC uses the same states, trust domains and components.  Although the level
of description may feel overly exhaustive, the members of the Enarx project
believe that it is important to be explicit about the trust models being
implemented, and how they reflect the "real world", in order to allow designers,
coders, documenters, testers, auditors and users of the project to have as full
an understanding of the process and architecture as possible.

### How the trust domain change happens

The key state change in terms of the Enarx default trust model is from state 1
to state 2.  It is at this point that at **TEE instance** on the host (and in
the **host owner's trust domain**), loaded with an **Enarx runtime image**
(which then executes, to become an **Enarx runtime instance**) becomes an
**Empty Keep**, within the **tenant trust domain** (having moved out of the
**host owner domain**. This is achieved by the use of a __trust pivot__: the
**host CPU + CPU firmware**.  The attestation process described in the State
1->2 change section explicitly notes that the cryptographic identity of the
**host CPU + CPU firmware** must be validated as part of the attestation
measurement check process.

Note: for more information about trust pivots, please refer to RFC #TBD.

The ability for the tenant to trust the host CPU + CPU firmware underpins the
entire trust model process, and it is vital that all implementations check it
very carefully.

### State 0->1 change

#### State 0 - initial state

 - host owner trust domain
    - host
    - host CPU + CPU firmware
    - Enarx host agent
    - Enarx runtime image
    - other workloads
 - tenant trust domain
    - Orchestrator
    - Enarx client agent
    - tenant workload image
    - tenant workload image repository

#### State 1 - pre-attestation

 - host owner trust domain
    - host
    - host CPU + CPU firmware
    - Enarx host agent
    - **TEE instance**
    - **Enarx runtime instance (within TEE instance)**
    - other workloads
 - tenant trust domain
    - Orchestrator
    - Enarx client agent
    - tenant workload image
    - tenant workload image repository

#### Changes

The changes between state 0 and state 1 are the creation of a TEE instance and
the loading of the Enarx runtime image into that instance.

Note that the **Enarx host agent**, which is considered as a single component in
this document for the purposes of trust domain discussions, can be further
decomposed into the following components:
 - Keep manager
   - one per host (future trust models might allow more than one per host)
   - keeps track of all Keeps on the host
   - launches Keep loader components (see below)
   - may persist state to allow recovery, or poll existing Keeps if required
 - Keep loader
   - one per Keep
   - lifespan tied to the Keep it manages
   - causes the creation of the **TEE instance**
   - loads **Enarx runtime instance** into **TEE instance**
   - creates a Main loop component
 - Main loop
   - one per Keep
   - lifespan tied to the Keep it services
   - provides the syscall processing from outside the Keep

The initialisation of the process is prompted by the **Orchestrator**, which
communicates with the **Enarx client agent**, which contacts the **Enarx host
agent** over a network connection.  This connection does not need to be
encrypted, as no confidential data is passed across it, because the **Enarx host
agent** is not a trusted entity since it is part of the **tenant host domain**.
A mechanism (such as a cryptographic nonce), allowing the Enarx client agent to
match sessions and requests, is included in the initial request.  The **Enarx
host agent** then requests the creation of the TEE instance, which is performed
by the **host CPU + CPU firmware**.

The **Enarx runtime image** is typically stored on host storage (local or remote
- the distinction is not relevant to the process).  The loading process varies
between architectures, but is requested by the **Enarx host agent**.  Although
the initial image may persist on host storage, there is no requirement for or
against this, and so the component is not listed separately in state 1.  We
follow this convention for later states.

Once the **TEE instance** has been created, the **host CPU + CPU firmware** load
the **Enarx runtime image** into the **TEE instance**.  The **Enarx runtime
image**, now loaded, is executed, and becomes the **Enarx runtime instance**,
though it is not yet attested.

As part of the initialisation process, the **Orchestrator** provides the **Enarx
client agent** with one of the following:
 1. access to the **tenant workload image repository** and a reference to the
    **tenant workload image** to be provisioned;
 2. direct access to the **tenant workload image**, either as a binary in memory
    or in storage;
 3. a reference allowing the **Enarx client agent** to recontact the
    **Orchestrator** in the event of successful attestation, in order then to
    obtain the **tenant workload image**.
 
Though the encryption of the channel between the Enarx client agent and the
Enarx host agent is not **required**, the following properties are considered to
be **desirable**:

 - authorisation of the Enarx client agent to the Enarx host agent
   - this allows the host to control which parties are permitted to create
   Enarx Keeps.  While this has no direct benefit to the tenant, an
   indirect benefit may be that the host can avoid DoS attacks, thereby
   maintaining availability for workload placement and execution.
 - integrity protection for the request
   - some DoS attacks or protocol attacks may be feasible, and mitigated
   by integrity protection.
 - encryption of the protocol handshake and of further communications
   - some attacks based around protocol versioning or replays may be
   feasible, and can be avoided by encryption.

For the reasons noted above, encrypting the channel will sometimes be considered
the simplest option.


### State 1->2 change

#### State 1 - pre-attestation

 - host owner trust domain
    - host
    - host CPU + CPU firmware
    - Enarx host agent
    - TEE instance
    - Enarx runtime instance (within TEE instance)
    - other workloads
 - tenant trust domain
    - Orchestrator
    - Enarx client agent
    - tenant workload image
    - tenant workload image repository


#### State 2 - attested

 - host owner trust domain
    - host
    - host CPU + CPU firmware
    - Enarx host agent
    - other workloads
 - tenant trust domain
    - Orchestrator
    - Enarx client agent
    - **Empty Keep**
    - tenant workload image
    - tenant workload image repository

#### Changes

The change between state 1 and state 2 is the transition of the TEE instance and
Enarx runtime image within it to an Enarx Keep.  This transition is considered
complete once the Keep has been attested.

The measurement of the TEE instance and Enarx runtime image within it is
requested by the **Enarx host agent** and performed by the **host CPU + CPU
firmware**.  The measurement is then transmitted by the **Enarx host agent** to
the **Enarx client agent**.

The **Enarx client agent** checks the attestation measurement database (see
[Issue #25](https://github.com/enarx/rfcs/issues/25)) to ensure that the
attestation measurement is correct.  This process includes checks for:

 - host CPU + CPU firmware validity
 - cryptographic correctness
 - TEE validity
 - valid Enarx runtime image.

If any of these checks fail, the state progression is stopped, and the **Enarx
client agent** logs an error and may send an error message to the **Enarx host
agent**.  The **Enarx client agent** also sends an error message to the
Orchestrator.

If the attestation measurement meets the checking criteria, the **Enarx client
agent** retrieves or derives the session key from the previous communication
originating in the **host CPU + CPU firmware** (the exact mechanism being CPU
architecture-dependent).  At this point, the state transition is considered
complete, and the TEE instance, loaded with the Enarx runtime image, is now an
**Empty Keep**, which includes a running **Enarx runtime instance**.

### State 2->3 change

#### State 2 - attested

 - host owner trust domain
    - host
    - host CPU + CPU firmware
    - Enarx host agent
    - Other workloads
 - tenant trust domain
    - Orchestrator
    - Enarx client agent
    - **Empty Keep**
    - tenant workload image
    - tenant workload image repository

#### State 3 - provisioned

 - host owner trust domain
    - host
    - host CPU + CPU firmware
    - Enarx host agent
    - other workloads
 - tenant trust domain
    - Orchestrator
    - Enarx client agent
    - **Provisioned Keep**
    - tenant workload image
    - tenant workload image repository


#### Changes

The change between state 2 and state 3 is the loading of the **tenant Workload
Image** into the **Empty Keep**, turning it into a **Provisioned Keep**.

During the state 0->1 change transition, the **Orchestrator** provided the
**Enarx client agent** with one of the following:
 1. access to the **tenant workload image repository** and a reference to
    **tenant workload image** to be provisioned;
 2. direct access to the **tenant workload image**, either as a binary in memory
    or in storage;
 3. a reference allowing the **Enarx client agent** to recontact the
    **Orchestrator** in the event of successful attestation.

The **Enarx client agent** now requires access to the **tenant workload image**.

 - In the case of 1. above, the **Enarx client agent** now exercises its access
   to the **tenant workload image repository** and retrieves the **tenant
   workload image**.
 - In the case of 2. above, the **Enarx client agent** already has access to the
   **tenant workload image**.
 - In the case of 3. above, the **Enarx client agent** contacts the
   **Orchestrator**, providing attestation measurement check status information,
   and receives the **tenant workload image** from the **Orchestrator**.  Note 
   the explicit approval may be included in the communication to allow the
   **Orchestrator** to check that the message is a "success" and not a "failure":
   this is a measure to reduce sensitive information leakage in the event of
   implementation errors.

The **Enarx client agent** gained access to a session key as part of the state
1->2 transition.  The **tenant workload image** must be encrypted under this
session key to be transmitted to the **Empty Keep**.  (Note that "the session
key's lifespan is expected to match that of the Keep in the default trust model:
**tenant workload images** are provided by the Orchestrator, and while the same
**tenant workload image** may be used for multiple Keeps, each instance will be
transmitted under a separate session key.)

At some point in the process (not defined in this document), the **Enarx client
agent** was provided with sufficient information to contact what is now the
**Empty Keep**, into which authorisation material was injected at
provision-time.  This material must then be tied to the attestation measurement,
and unique to each **Empty Keep** instance. The session key established in the
state 1->2 transition may be sufficient for this purpose.

The **Enarx client agent** now creates an encrypted communications channel to
the **Empty Keep**, using the session key established in the state 1->2
transition.  The **Empty Keep** will be listening for a network connection, and
once the encrypted communications channel is set up, the **Enarx client agent**
transmits the **tenant workload image** to the **Empty Keep**.  The **Empty
Keep** receives the **tenant workload image**, and instantiates it as a full
workload.  This completes the transition of the **Empty Keep** to a
**Provisioned Keep**.


## Requirements

### Priority of tenant

The integrity of the tenant's trust domain MUST always be given the highest
priority.

Protections for the tenant MUST always be prioritised above those of other
parties such as the host.

Confidentiality of the tenant's workload MUST be protected.

Integrity of the tenant's workload MUST be protected.

### Client agent to host agent channel considerations

The channel between the Enarx client agent and the Enarx host agent MAY be
encrypted.

The Enarx client agent SHOULD be authorised to the Enarx host agent.

Integrity protection of the request SHOULD be applied.

The protocol handshake and further steps SHOULD be encrypted.

If integrity protection is applied to the protocol handshake and further
communications between the Enarx client agent and the Enarx host agent, all
detected integrity failures SHOULD be reported to the Enarx client agent via the
Enarx host agent where possible.

### Mmeasurements and logs

All protocol errors SHOULD be logged by the Enarx client agent.

All attestation measurements SHOULD be logged by the Enarx client agent.

All Enarx client agent logs SHOULD be integrity protected.

All attestation measurement failures MUST be logged by the Enarx host agent.

### Cryptographic verifications

All attestation measurements MUST be cryptographically signed.

All attestation measurements MUST be checked.

All attestation measurement failures MUST result in a cessation of the standard
state progression.

The cryptographic validity of the host CPU + CPU firmware MUST be checked in
every attestation measurement check.

Any and all cryptographic validity check failures of the host CPU + CPU firmware
MUST result in an attestation failure.

### Session tracking

The Enarx client agent MUST generate and include session tracking information in
the initial communication with the Enarx host agent.

The session tracking information MUST be included in all communications between
the Enarx client agent and the Enarx host agent until the Keep is fully
provisioned.

### Tenant workload protection

The tenant workload MUST never be unencrypted on the network (in transit) within
the Enarx-managed process.

The tenant workload MUST never be unencrypted in storage once it has been
transmitted by the Enarx client agent.

The tenant workload MUST never be unencrypted in RAM once it has been
transmitted by the Enarx client agent.

The tenant workload MUST never be available in unencrypted form on the host.

The tenant workload image MUST be encrypted under a unique session key.

The cessation of the standard state progression before transmission of the
tenant workload MUST result in non-transmission of the workload from the Enarx
client agent to any component on the host.

The Enarx client agent MUST be the component which performs attestation
measurement checks.

### Orchestrator concerns

The Enarx client agent MAY pass attestation measurements to the Orchestrator.

All attestation measurement failures SHOULD result in an error message being
sent to the Orchestrator.

The Enarx client agent MUST NOT accept override instructions from the
Orchestrator to proceed with deployment of a tenant workload image in the case
of an attestation measurement failure.


## Reference

rfc#0000x - Wasm RFC  
[rfc#00005 - Trust domain introduction](https://github.com/enarx/rfcs/tree/master/00005-trust-domain-introduction)  
rfc#0000x - Trust domains and Enarx  
rfc#0000x - Trust anchors and pivots  
[Define attestation database - issue #25](https://github.com/enarx/rfcs/issues/25)


## Drawbacks

n/a

## Rationale and alternatives

n/a

## Prior art

n/a

## Unresolved questions

See Future Possibilities section

## Implementations

None.

## Future Possibilities

 - Alternative mechanisms for providing the **Enarx client agent** with
   sufficient information to contact the **Empty Keep** should be described and
   considered.  Options include:
   - via set-up information from the **Orchestrator**, as part of the
     communications with the **Enarx host agent** (the canonical case for the
     default process described in this document)
   - via "call-back" from the **Empty Keep**
 - In the latter case, the **Enarx client agent** must be certain (through
   cryptographic authentication) that the **Empty Keep** is the expected entity.
 - Options for managing key rotation for long-running Keep instances
 - Threat models should be documented and published as RFCs [Issue
   #240](https://github.com/enarx/enarx/issues/240)
 - Status of hardware/firmware in trust relationships in Enarx needs to be
   documented [Issue #193](https://github.com/enarx/enarx/issues/193)
