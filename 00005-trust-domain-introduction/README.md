# 00005: Trust domain introduction
- Authors:[ Mike Bursell](mike@p2ptrust.org)
- Status:[ PROPOSED](/README.md#proposed)
- Since: 2020-02-13
- Status Note: rough consensus
- Start Date: 2020-02-11
- Tags: trust

## Summary

Trust domains are a key concept within Enarx. This RFC provides an introduction
to trust domains.


## Motivation

In order to explain the motivations behind Enarx, our design principles and
aims, we need to be able to explain what trust domains are. This RFC attempts to
provide an introduction.


## Tutorial

Before considering trust, we need to consider why it is relevant to the issue of
TEEs and Enarx. The fundamental concept behind Enarx is that it allows
applications - also referred to as "workloads" - to be run on machines - also
referred to as "hosts" - which are not under the control of the owner of the
workload, or which are vulnerable to compromise by malicious parties. The
workload owner does not trust the host or the host owner. But what does this
mean?

Trust, in the human sphere, can be thought of as the expectation one person -
the trustor - has that another person - the trustee - will behave in a
particular way. The mapping to the computing domain is not perfect, but if we
want to think about the relevant human actors, we can model the trustor as the
workload owner and the host owner as trustee. In order to map this more directly
to the realm of computer systems, we can say that the process deploying the
workload is the trustor, and that the host is the trustee.

How can the deploying process trust the host to execute the workload as
expected, and what do we mean by "expected"? The specific expectations in which
we are interested are the confidentiality of the workload and the integrity of
the workload. In the normal computing case, there can be no checkable or
cryptographically-attested assurance that the host will meet these expectations.
Trusted Execution Environments provide means to build such assurances, and for
trust to be built in such a way that the deploying process (which represents, or
has agency for, the workload owner) can have assurances that the host will not -
and cannot - break confidentiality or integrity of a workload which it (the
host) is executing.

A definition of trust, from [What is trust?](http://aliceevebob.com/2017/05/09/what-is-trust-with-apologies-to-pontius-pilate/).

"Trust is the assurance that one entity holds that another will perform
particular actions according to a specific expectation."  
First corollary: "Trust is always contextual."  
Second corollary: "One of the contexts for trust is always time."  
Third corollary: "Trust relationships are not symmetrical."


#### Applying this to Enarx

The key trust relationship for Enarx is that held by the workload owner
(typically, the "tenant") to the workload it owns. The key contexts for this
trust, in an Enarx-enabled system, are integrity and confidentiality, and Enarx
aims to provide assurances (and by extension, mechanisms) to the tenant that the
integrity and confidentiality of that workload are protected. Typically, the
continued protection of integrity and confidentiality through the actions of the
workload and Enarx increase the trust relationship: trust may decay or be built
up in the workload - this decay or increase represents the time context.

Note: In this case, the workload does not have an equivalent set of assurances
about the tenant's integrity and confidentiality, so the relationship (if there
were one) would not be symmetrical. For our purposes, it is the ability to
assert and have assurances in regards to properties such as these - integrity
and confidentiality, both security properties - that defines how we consider
trust relationships.


### Trust domains

A trust domain can be defined as a set of entities or components that share the
same security policies. These may be human or machine, but machine entities are
usually assumed, and are what are relevant to Enarx. We can refer to these
entities as members of a trust domain. Trust domains can be subsets or supersets
of other trust domains, but may not intersect: a true intersection between two
trust domains implies that they share the same policies, which means that they
are in union (and thus the same).

Members of a trust domain, by definition, trust each other to perform particular
actions - typically to provide particular services. Trust relationships within a
trust domain are irrelevant to any external view, as a trust domain provides a
single unit of trust from the point of view of external entities: it is a
system-type abstraction. A trust domain has a single identity, when considered
from outside the domain. Although multiple components within a trust domain may
expose interfaces which can be accessed externally, these exposed components are
considered to represent the trust domain. If such interfaces are exposed by a
component in a trust domain but do _not_ represent the trust domain to which it
belongs, that component is likely misplaced. All interactions between trust
domains - or from any external entity to a trust domain - should occur across
well-defined interfaces, preferably to well-defined specifications. Without
these assurances, it is not possible for external parties to form trust
relationships with the trust domain as defined above.


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

None.


## Future Possibilities

None.

