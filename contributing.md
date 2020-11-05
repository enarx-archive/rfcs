# Enarx RFCs

## Contributing

### Do you need an RFC?

Use an RFC to advocate substantial changes to the Enarx ecosystem, where
those changes need to be understood by developers who *use* Enarx. Minor
changes are not RFC-worthy.

### Preparation

Before writing an RFC, consider exploring the idea on the
[chat](https://chat.enarx.dev) or on a daily meeting call (see the [wiki page on
contributing](https://github.com/enarx/enarx/wiki/How-to-contribute)).
Encouraging feedback from maintainers is a good sign that you're on the right
track.

### How to propose an RFC

#### Process

1. Fork [the RFC repo](https://github.com/enarx/rfcs).
2. Choose a descriptive name for your RFC and note the next RFC number.
3. Create a directory with the descriptive name you chose and copy
   `template.md` to `<RFC#>-<your-descriptive-RFC-name>/README.md`.
4. Fill in the RFC's README.md.
5. Commit your changes.
6. Push your changes to your forked `rfcs` repo.
7. Submit a pull request from your forked repo to `enarx/rfcs`. Please make 
   sure your pull request uses the same title as your RFC, for consistency.

Should you need help with the Git forking and pull request mechanism, please
refer to our
[documentation](https://github.com/enarx/enarx/wiki/How-to-contribute:-Pull-Request-process)

#### Contents

Use MUST and SHOULD [per standard conventions](https://tools.ietf.org/html/rfc2119).

Please pay attention to details: RFCs that do not present convincing
motivation, demonstrate an understanding of the impact of the design, or are
disingenuous about the drawbacks or alternatives tend to be poorly received.
You can add supporting artifacts, such as diagrams and sample data, in the
RFC's folder.

Make sure that all of your commits conform to the license restrictions noted
[below](#intellectual-rights).

Enarx maintainers will check to see if the process has been followed, and
request any process changes before merging the PR.

#### Example
- I fork the repo and make a note of the number of latest RFC to have been
 added to it, say 0010.
- I chose as a descriptive RFC name, "My Great RFC".
- This gives me a directory name of `0011-my-great-rfc`.
- I copy 00000-template to `./0011-my-great-rfc/README.md` and start editing it.
- In README.md, the title I use is "0011: My Great RFC".
- When ready to publish, I commit, push and submit a pull request.


When the PR is merged, your RFC is now formally in the PROPOSED state.

### Changing an RFC Status

The lifecycle of an RFC is driven by the author or current champion of the 
RFC. To move an RFC along in the lifecycle, submit a PR with the following
characteristics:

- The PR should __ONLY__ change the RFC status.
- The title of the PR should include a deadline date for merging the PR and the referenced RFC.
  - Example: `Status to Accepted, deadline 2019-08-15, RFC 0095-basic-message`
- The PR comment should document why the status is being changed.
- The deadline date should be 2 weeks after announcing the proposed status
  change on an Enarx call. The PR should also be announced on the [chat](https://chat.enarx.dev).
- Barring negative feedback from the community, the repo's maintainers should
  merge the PR after the deadline.
- The deadline should be moved by two weeks after addressing each substantive
  change to the RFC made during the status change review period.


### How to get an RFC demonstrated

If your RFC is a feature, it's common (though not strictly required) for it to
go to a DEMONSTRATED state next. Write some code that embodies the concepts in
the RFC. Publish the code. Then [submit a PR](#changing-an-rfc-status) that adds
your early implementation to the [Implementations
section](/00000-template.md#implementations), and that changes the status to
DEMONSTRATED. These PRs should be accepted immediately, as long as all tests
pass.

### How to get an RFC accepted

After your RFC is merged and officially acquires the [PROPOSED status](
README.md#status--proposed), the RFC will receive feedback from the larger
community, and the author should be prepared to revise it. Updates may be made
via pull request, and those changes will be merged as long as the process is
followed.

When you believe that the RFC is mature enough (feedback is somewhat resolved,
consensus is emerging, and implementation against it makes sense), [submit a
PR](#changing-an-rfc-status) that changes the status to
[ACCEPTED](README.md#status--accepted). The status change PR will remain open
until the maintainers agree on the status change.

### How to get an RFC adopted

An accepted RFC is a standards-track document. It becomes an acknowledged
standard when there is evidence that the community is deriving meaningful
value from it. So:

- Implement the ideas, and find out who else is implementing.
- Socialize the ideas. Use them in other RFCs and documentation.
- Update the agent test suite to reflect the ideas.

When you believe an RFC is a _de facto_ standard, [raise a PR](#changing-an-rfc-status)
that changes the status to [ADOPTED](README.md#status--adopted).  If the
community is friendly to the idea, the doc will enter a two-week
"Final Comment Period" (FCP), after which there will be a vote on disposition.

### Intellectual Rights

This repository is licensed under an [Apache 2.0 License](LICENSE). This means
that any contributions you make must be licensed in an Apache-2-compatible way,
and must be free from patent encumbrances or additional terms and conditions. By
raising a PR, you certify that this is the case for your contribution.
