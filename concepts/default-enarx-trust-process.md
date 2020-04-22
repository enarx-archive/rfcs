# TBD: Default Enarx trust process
- Authors: [Mike Bursell](you@github-email)
- Status: [PROPOSED](/README.md#proposed)
- Since: 2020-03-17
- Status Note: under discussion
- Start Date: 2020-03-17 (date you started working on this idea)
- Tags: trust
- Addresses: (Issue #)[https://github.com/enarx/enarx/issues/]

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

## Tutorial

See rfc#00002 for an introduction to trust relationships.
See rfc#00003 for an introduction to trust domains and their application
to Enarx.
See rfc#00006 for an introduction to endorsing authorities, trust anchors
and trust pivots.

### Trust state transitions in Enarx
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
the level of description may feel exhaustive, the members of the Enarx
project believe that it is important to be explicit about the trust
models being implemented, and how they reflect the "real world", in order
to allow designers, coders, documenters, testers, auditors and users of
the project to have as full an understanding of the process and
architecture as possible.
 


See rfc#00002 for an introduction to trust relationships.

### Applying trust domains to Enarx - default model
There are various possible models for trust domains in an Enarx
deployment - this section describes the simplest, and default model.
In the default Enarx model, we start off by defining the following parties
- tenant
- host owner (the host operator may be separate, but is considered to
be the same for this description)
- entities running processes on the host (includes other workloads
and compromised processes)
- man-in-the-middle entities
Clearly, the first two are the key actors - the others are included for
reference (though a threat model is not defined here).

The following components are defined:
- host 
- host CPU + CPU firmware
- Orchestrator (e.g. Kubernetes, Openshift, Openstack)
- Enarx client agent (ECA)
- Enarx host agent (EHA)
- TEE instance
- Empty Keep
- Provisioned Keep
- Tenant workload
Not all of these exist at all points in the process.



### Applications and trust domains in Enarx


## Reference

rfc#00002

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