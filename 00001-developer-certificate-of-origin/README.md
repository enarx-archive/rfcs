# 00001: Developer Certificate of Origin
- Authors: [axel simon](axel@redhat.com)
- Status: [PROPOSED](/README.md#proposed)
- Since: 2020-07-27
- Status Note: N/A
- Supersedes: N/A
- Start Date: 2020-07-27
- Tags: legal, open source, contributors

## Summary

A proposition to use the Developer Certificate or Origin within the Enarx
project.

## Motivation

Enarx is by design, by conviction and by necessity free and open source
software.

As the Enarx contributor community grows, so will the diversity of people and
organisations who hold authors' right (copyright) over portions of the code. For
the health of the the project, Enarx needs to ensure that these contributions
are legitimate, that is to say that the contributor has the right to contribute
that code, and does not infringe on anyone's rights by doing so.

The Developer Certificate of Origin (henceforth "DCO") is a way of both:
- stating the importance of the legitimacy of contributions for the long-term
  health of the project and;
- providing guarantees regarding these contributions.

It is worth noting that the [GitHub Terms of
Service](https://docs.github.com/en/github/site-policy/github-terms-of-service#6-contributions-under-repository-license),
which apply to Enarx as a GitHub-hosted project, already explicit state that:
> Whenever you make a contribution to a repository containing notice of a
> license, you license your contribution under the same terms, and you agree
> that you have the right to license your contribution under those terms. 

Having a DCO would simply make this more explicit.

## Tutorial

An individual or organisationy wishes to make a code contribution to the
project.

Before they can submit a pull request, they MUST be informed that Enarx uses the
DCO to guarantee all contributions are of legitimate origin.

When they wish to push their code contribution, they MUST first sign their
contribution by signing-off their git commits, with the `git commit --sign-off`
command.

When they submit their pull request, automated tests MUST verify that the
commits are signed-off and prevent merging if that is not the case.

At the end of the day, the only change required in terms of workflow for
contributors should be to commit with `--sign-off` (or `-s`).

## Reference

N/A

## Drawbacks

Adding a DCO adds a small amount of administrative friction, that may put some
contributors off.

## Rationale and alternatives

- What is the impact of not doing this?  
Not doing this may have repercussions in the medium to long term, as some
organisations may not wish to use or contribute to Enarx without the legal
guarantees that a DCO or CLA (see below) provide to the project. Furthermore,
there is a possibility that using such a mechanism to guarantee legitimacy of
contributions will be a requirement of the Confidential Computing Consortium,
the organisation which is home to Enarx.
- What are the alternatives?  
  There are alternatives to the DCO, such as CLAs (Contributor Licence
Agreements) but these tend to be more complex legally as well as typically more
binding for the contributer. They also usually require to be drafted by legal
specialists as they are not standardised and also suffer from the same problem
regarding administrative friction described above, only worse.  
Comparatively, the DCO is a document simple enough to fit in a single screen, is
not overly filled with complex legal language, takes less than 2 minutes to read
and doesn't require potential contributors to disclose personal information if
they do not wish to do so.

## Prior art

The DCO is used by many other projects, among wich the Linux Kernel.

For completeness, here are a link to the DCO website and the full text of the
DCO, demonstrating its shortness:

https://developercertificate.org/

> Developer Certificate of Origin
> Version 1.1
> 
> Copyright (C) 2004, 2006 The Linux Foundation and its contributors.
> 1 Letterman Drive
> Suite D4700
> San Francisco, CA, 94129
> 
> Everyone is permitted to copy and distribute verbatim copies of this
> license document, but changing it is not allowed.
> 
> 
> Developer's Certificate of Origin 1.1
> 
> By making a contribution to this project, I certify that:
> 
> (a) The contribution was created in whole or in part by me and I
>     have the right to submit it under the open source license
>     indicated in the file; or
> 
> (b) The contribution is based upon previous work that, to the best
>     of my knowledge, is covered under an appropriate open source
>     license and I have the right under that license to submit that
>     work with modifications, whether created in whole or in part
>     by me, under the same open source license (unless I am
>     permitted to submit under a different license), as indicated
>     in the file; or
> 
> (c) The contribution was provided directly to me by some other
>     person who certified (a), (b) or (c) and I have not modified
>     it.
> 
> (d) I understand and agree that this project and the contribution
>     are public and that a record of the contribution (including all
>     personal information I submit with it, including my sign-off) is
>     maintained indefinitely and may be redistributed consistent with
>     this project or the open source license(s) involved.

## Unresolved questions

- A DCO is not an individual contributor agreement, it does not transfer
  "ownership" of copyright / authors' rights on the contributions. As such,
should the Enarx project wish to relicense to a different (open source) licence
in the future, a DCO would do little to make that easy. While this doesn't sound
like an issue currently, it may be worth bearing in mind.

## Implementations

N/A

## Future Possibilities

By design, this RFC should not extend into other similar domains: we want to
keep the administrative overhead of participating to the project as low as 
possible.

Especially, the Enarx project is attached to making it possible for contributors
to participate to the project in ways they feel safe and confortable doing so,
wich requires a strong consideration for the privacy of its contributors.
