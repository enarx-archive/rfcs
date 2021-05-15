# 00006 Trust anchors and pivots
- Authors: [Mike Bursell](mike@p2ptrust.org)
- Status: [PROPOSED](/README.md#proposed)
- Since: 2020-03-09
- Status Note: discussion
- Supersedes: n/a
- Start Date: 2020-03-16 
- Tags: trust

## Summary

Trust anchors and trust pivots allow us to discuss how trust domains are
created and modified.

## Motivation

In order to describe some of the key features of Enarx, the concepts of
trust anchors and trust pivots need to be defined and explained.  This
RFC aims to provide that information.

## Tutorial

This RFC introduces three concepts:
- Endorsing authorities
- Trust anchors
- Trust pivots

These concepts are important to allow us to discuss how the various
components of Enarx work together.  These definitions are taken from
"Trust in computer systems and the cloud", Mike Bursell, not yet
published.

### Endorsing authorities
An endorsing authority is a human or organisational entity to
whom a trust relationship has been established.  An endorsing
authority can provide one or more trust anchors, endorsing them
as representing properties of another human or organisation
(such as their identity, physical location or ability to access
credit).  Endorsing authorities are sometimes referred to as
"trust anchors" - for our purposes, we consider the former as
endorsing the latter.

### Trust anchor
A trust anchor is a static component in a system whereby an
endorsing authority allows trustors to assume trust in a system
in which the anchor is contained.  Trust anchors are static in
terms of their interaction with a trust system, and the trust
relationship to a trust anchor is assumed - based on the endorsing
authority - rather than derived.  One example of a trust anchor
is a root certificate signed by a Certificate Authority (the
endorsing authority in this case).

### Trust pivot
A trust pivot is a component (or set of components) and associated
process (that is, algorithm, rather than necessarily executing
process) which allows a trust relationship from one entity (trustor
A) to another (trustee X) to be transferred, or added, to
another entity (trustor B), so that trustor B now has the trust
relationship to trustee X.  The validity of the pivot assumes
the existence of one or more trust anchors.  The concept of a
trust pivot is new, and two examples may help explain it.

#### A will as a trust pivot
A will (as in "last will and testament") provides a legal
mechanism whereby the ownership and management of property can
be passed from one person to another.  In this case, the trust
pivot is the will itself, the trust anchor is the legal recognition
by the state of the validity of properly created and registered wills,
and the endorsing authority is the state.  (Note: "Trusts" are
another mechanism, but the name is somewhat confusing in this
context!).

#### A CPU+firmware as a trust pivot of a TEE
Most relevant to Enarx is the use of TEE-enabled CPU+firmware and
an attestation process to allow a TEE instance to pivot from one
trust domain (the host's) to another (the workload owner's).  An
example of a trust anchor in this case is the cryptographic
key or certificate which the CPU+firmware component uses to sign
aspects of the attestation.


## Reference

n/a

## Drawbacks

n/a

## Rationale and alternatives

n/a

## Prior art

n/a

## Unresolved questions

n/a

## Implementations

n/a

## Future Possibilities

n/a
