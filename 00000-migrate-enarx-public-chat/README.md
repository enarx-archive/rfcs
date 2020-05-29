# 00000-migrate-enarx-public-chat
- Authors: Mark Bestavros, Axel Simon
- Status: [DEMONSTRATED](/README.md#demonstrated)
- Since: 2020-03-13
- Status Note: Rocket.Chat has donated an instance to the Enarx project;
  currently being trialed  
- Supersedes: None
- Start Date: 2020-03-06
- Tags: chat app, outreach

## Summary

We are currently using Gitter for Enarx's public chat. We are beginning to
notice its shortcomings, and believe it will not meet our needs as we scale.
This RFC proposes that we switch our public chat app to another more
feature-rich and better-organized open-source alternative. We have experimented
with Rocket.Chat and Zulip, and have evaluated several other alternatives; at
the present time, we believe that Rocket.Chat is the best option. We are
currently trialing a hosted Rocket.Chat instance; that can be found
[here](https://chat.enarx.io). 

## Motivation

We have noticed some intermittent reliability issues with Gitter recently.
Combined with a number of other feature shortcomings, we're growing a little
frustrated with it, and we're exploring alternatives.

## Tutorial

As mentioned in the summary, we've evaluated two alternatives in depth: Zulip
and Rocket.Chat. Here is a tutorial as if each one was selected as our chat app.

### As if Rocket.Chat is set up as our default chat app:

Enarx's public chat lives on Rocket.Chat. If you'd like to see what we're
talking about, check out `https://chat.enarx.io`. You'll be able to see what we're
talking about right away; all our conversations take place in the public, and
you don't need a login to browse around.

If you'd like to start getting involved in the discussion, you can create an
account -- or chat anonymously, if you prefer. We won't ask you to disclose your
identity if you'd rather not.

If you do decide to create an account, you'll need to provide an email and
password.  _(Or, if we choose to set it up, use OAuth authentication with a
[number](https://rocket.chat/docs/administrator-guides/authentication/oauth/) of
services.)_

Whether you have an account or not, feel free to come on in and have a look
around!

Rocket.Chat uses [**channels**](https://rocket.chat/docs/user-guides/channels/)
to organize categories of messages, much like Slack or Hangouts Chat. By
default, you'll be subscribed to the server's `#general` channel, which contains
general project discussion.

We've split different areas of discussion into other channels, which you are
free to
[join](https://rocket.chat/docs/user-guides/channels/#joining-new-channels-and-starting-direct-messages).
For example, if you have a question or point of discussion related to Enarx
development, you can move to the `#dev` channel. 

If you need to get in touch with us regarding a security issue or vulnerability
disclosure, you can do so through Rocket.Chat's [**end-to-end
encryption**](https://rocket.chat/docs/user-guides/end-to-end-encryption/).
_(Note: we intend to promote this as one of our primary methods to disclose
vulnerabilities.)_

Much like many other chat apps, you can `@` mention specific people, and they'll
be notified. You can also reply to existing messages by creating a
**discussion**. This is analogous to chat threads in Slack. Finally, if you'd
like to express your feelings in a more non-verbal way, feel free to make use of
emojis and emoji reactions -- including custom Enarx ones!

If you're talking about code, feel free to use Markdown to format your code as
part of your messages! Rocket.Chat supports Markdown, and syntax highlighting
for specific languages.

If you need to show us an image or share a file, you can upload one as part of
your message. There's a handy button next to the compose box.

Finally, if you just absolutely _need_ more Enarx discussion in your life, you
can get the Rocket.Chat mobile app for iOS or Android to chat on the go.

### As if Zulip is set up as our default chat app:

Enarx's public chat lives on Zulip. If you'd like to get involved in the
conversation, you can join us at `<public-chat-link>`. You'll need to create an
account -- you have options for Github and Google login, or you can use email. 

Once you've created an account, come on in and have a look around!

Zulip organizes categories of messages into
[**streams**](https://zulipchat.com/help/about-streams-and-topics). This is
roughly analogous to "channels" or "rooms" in other chat apps. By default,
you're subscribed to the `#general` stream, which contains general project
discussion.

We've split different areas of discussion into other streams, which you are free
to [join](https://zulipchat.com/help/browse-and-subscribe-to-streams). For
example, if you have a question or point of discussion related to Enarx
development, you can move to the `#dev` stream. Once there, you'll see messages
organized into
[**topics**](https://zulipchat.com/help/about-streams-and-topics). You can
browse previously-discussed topics and reply to them by clicking on a message in
that topic. Alternatively, you can [start a new
one](https://zulipchat.com/help/start-a-new-topic) if you haven't seen your
topic discussed yet. You can name it whatever you like (just be sure it's
descriptive).

When you're writing a message, you can `@` mention specific people. They'll
receive a notification. If you'd like to express your feelings in a more
non-verbal way, feel free to make use of emojis and emoji reactions -- including
custom Enarx ones!

If you're talking about code, feel free to use Markdown to format your code as
part of your messages! Zulip's Markdown also supports syntax highlighting for
specific languages.

If you need to show us an image, you can upload one as part of your message.
There's a handy button next to the compose box.

Finally, if you just absolutely _need_ more Enarx discussion in your life, you
can use the Zulip mobile apps for iOS and Android to chat on the go.

## Reference

Here is a handy chart with direct feature comparisons to our current solution,
Gitter. Particularly important considerations are bolded.

| **Comparison**                                  | **Rocket.Chat**                                                           | **Gitter**                                                                                                                                                    | **Zulip**                                                                                                                                                                             |
|-------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Fundamentals**                                |                                                                           |                                                                                                                                                               |                                                                                                                                                                                       |
| License                                            | MIT                                                                 | MIT                                                                                                                                                     | Apache 2.0                                                                                                                                                                      |
| Reliability                                     | Rocket.Chat's hosted option comes with an [SLA](https://rocket.chat/pricing#cloud)                                                                       | We've encountered some issues                                                                                                                                 | TBD                                                                                                                                                                                   |
| E2E Encryption                                  | **Yes** (optional)                                                        | No                                                                                                                                                            | No                                                                                                                                                                                    |
| Publicly-visible chat                           | **Yes**                                                                   | **Yes**                                                                                                                                                       | Not by default; `zulip-archive` is a workaround (see below for details)                                                                                                               |
| Fully anonymous chat (no login required at all) | **Yes**                                                                   | No                                                                                                                                                            | No                                                                                                                                                                                    |
| Maintained?                                     | Yes                                                                       | Yes, though they admit they don't have enough resources to develop key features (ex. mobile app)                                                              | Yes                                                                                                                                                                                   |
| **App support**                                 |                                                                           |                                                                                                                                                               |                                                                                                                                                                                       |
| Web app                                         | Yes                                                                       | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| Mobile apps                                     | Yes (iOS and Android)                                                     | Yes (iOS and Android), though they are not maintained, tend to break, and may be officially deprecated in the future. Android does not support notifications. | Yes (iOS and Android)                                                                                                                                                                 |
| Native Linux                                    | Yes                                                                       | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| Native Mac                                      | Yes                                                                       | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| Native Windows                                  | Yes                                                                       | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| **Organization**                                |                                                                           |                                                                                                                                                               |                                                                                                                                                                                       |
| Multiple rooms                                  | Yes (channels)                                                            | Yes                                                                                                                                                           | [Yes](https://zulipchat.com/help/about-streams-and-topics) (they're called "streams")                                                                                                 |
| Custom topics                                   | No                                                                        | No (chat is just a chronological stream of consciousness)                                                                                                     | [Yes](https://zulipchat.com/help/about-streams-and-topics) (and they support custom names)                                                                                            |
| Threading                                       | Yes                                                                       | Technically yes, but it's poorly implemented/missing features and we do not use it                                                                            | Yes, with custom topics                                                                                                                                                               |
| Global chat search                              | Yes                                                                       | No                                                                                                                                                            | [Yes](https://zulipchat.com/help/search-for-messages)                                                                                                                                 |
| Message pinning                                 | Yes                                                                       | No                                                                                                                                                            | Locally, but not globally                                                                                                                                                             |
| **Features**                                    |                                                                           |                                                                                                                                                               |                                                                                                                                                                                       |
| Github integration                              | [Yes](https://rocket.chat/docs/administrator-guides/integrations/github/) | No (there is an option for it, but it uses the deprecated Github Services and does not work)                                                                  | Yes                                                                                                                                                                                   |
| Chat bots                                       | Yes                                                                       | No                                                                                                                                                            | Yes                                                                                                                                                                                   |
| Sign-up ease                                    | Easy (email login, with other web logins optional)                        | Easy (Github login)                                                                                                                                           | Easy (Github, Google, email login) -- worth noting email confirmations usually take a long time to arrive                                                                             |
| Direct linking to specific messages in chat     | Yes                                                                       | No                                                                                                                                                            | Yes                                                                                                                                                                                   |
| File uploads/image sharing                      | Yes                                                                       | No                                                                                                                                                            | Yes                                                                                                                                                                                   |
| Bridging to other networks                      | Yes (how well it works remains to be seen)                                | Barebones                                                                                                                                                     | Officially for [Matrix](https://zulip.com/integrations/doc/matrix) and [IRC](https://zulip.com/integrations/doc/irc), but functionally broken (only syncs a stream and not a channel) |
| Markdown support                                | Yes                                                                       | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| Syntax highlighting                             | Yes                                                                       | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| **UX/UI/Other considerations**                  |                                                                           |                                                                                                                                                               |                                                                                                                                                                                       |
| UI complexity                                   | Fairly straightforward, not too busy                                      | Very straightforward                                                                                                                                          | Slightly higher learning curve; busier UI                                                                                                                                             |
| Dark mode                                       | Yes on mobile; not on desktop                                             | Yes                                                                                                                                                           | Yes                                                                                                                                                                                   |
| Emoji reactions to messages                     | Yes                                                                       | No                                                                                                                                                            | Yes                                                                                                                                                                                   |
| Custom emoji                                    | Yes                                                                       | No                                                                                                                                                            | Yes                                                                                                                                                                                   |
## Drawbacks

### Both

**Community**

The community we’ve begun building up on Gitter is easily the biggest reason we
have to stay, and the biggest con to moving anywhere. If we move chat clients,
we’d be asking them to juggle around yet another chat app, and their existing
lines of communication would end up unsupported by us. This is not ideal.

The counter-argument to this is that if a change is going to happen, it’s better
that it happens sooner rather than later. If we truly feel that Gitter won’t
meet our needs (especially as we scale), a change now will be far less painful
than when we’re a lot larger.

### Rocket.Chat

**It's self-hosted**

The single biggest blocker to adopting Rocket.Chat right now is the fact that
it's meant to be self-hosted. Unlike other options such as our current Gitter or
Zulip, there is no option to simply spin up an Enarx chat instance and start
using it for free. (There is a free trial, which we used to evaluate Rocket.Chat
for this RFC.) Rocket.Chat themselves do offer a [paid hosted
option](https://rocket.chat/pricing), which could be an option if there was a
funding source for it.

**A few minor annoyances**

- Rocket.Chat is not best-in-class in terms of chat organization; Zulip would
  probably receive that honor. That said, it's no worse than Gitter (rather,
  it's certainly better).
- Rocket.Chat does not officially support dark mode on its desktop clients.
  Fortunately, its mobile apps _do_ support it, and it is possible to work
  around it with, for example, a user-style extension in-browser and an
  [unofficial theme](https://github.com/0x0049/Rocket.Chat.Dark).

### Zulip

**Public chat visibility**

The biggest thing Rocket.Chat (even Gitter) does significantly _better_ than
Zulip is the ability to read messages when logged out. If someone new to the
project is idly curious what's going on in chat, they can just look at our chat
log in Gitter (or Rocket.Chat, if we use it) -- no login required.

Zulip, on the other hand, requires a login, which presents a barrier to people
being able to see the discussions going on about Enarx. An idle observer would
need to create an account, which they might not want to do.

There is a potential workaround present, however, with
[`zulip-archive`](https://github.com/zulip/zulip-archive). This is a tool,
runnable as a Github Action, that archives our chat logs and publishes them on a
publicly-accessible Github Pages site. For Enarx, this may manifest itself as a
sub-link of `enarx.io` -- for example, we may be able to point people to
`chat.enarx.io/archive` for a publicly-accessible chat record.

In terms of implementation, it seems fairly straightforward, and there are
examples of this being done on an organizational level (see
[here](https://leanprover-community.github.io/archive/)). An Enarx deployment of
this tool would look fairly similar. We would need to create a new repository
within the Enarx organization to hold the chat record, though it shouldn't need
to be maintained after `zulip-archive` is set up.

## Rationale and alternatives

We'll discuss the specifics around why we like both Rocket.Chat and Zulip, as
well as specific things each app does well that make it stand out.

### Both

- App support (especially mobile)
    - Gitter’s mobile app is, to put it kindly, not very good. It doesn’t
      support notifications at all, it is slow, and it doesn’t register read
      messages well. Additionally, Gitter themselves do not officially recommend
      their own apps, and [say](https://gitter.im/apps) they may be deprecated
      in the future. Both Zulip and Rocket.Chat have mobile apps that work well,
      support notifications, and generally support a much larger set of
      features.
    - Both Zulip and Rocket.Chat have desktop Linux apps available, though both
      are also available in browser.
    - Both have Windows and Mac desktop clients.
- Nice-to-have features
    - Both Zulip/Rocket.Chat support file uploads, where Gitter does not. This
      can be helpful if we need to share a screenshot, for example.
    - Both Zulip/Rocket.Chat allow users to link to a unique message in the
      chat.
    - Gitter's Github integration is out of date and does not work. Meanwhile,
      both [Zulip](https://zulipchat.com/api/running-bots) and
      [Rocket.Chat](https://rocket.chat/bots) offer powerful bot frameworks, and
      generally have more possibilities for integration.
    - Both Zulip and Rocket.Chat support searching chat logs to find a specific
      message, where Gitter offers nothing of the sort.

### Rocket.Chat
- Publicly visible by default
    - Rocket.Chat has the option for administrators to [allow anonymous read and
      write](https://rocket.chat/docs/administrator-guides/account-settings/).
      That means users can 1) read chat without logging in, and 2) _participate_
      in chat without needing to maintain an email/password login. The former is
      supported by Gitter, but the latter is new. As a project striving for
      complete transparency, this is a _huge_ advantage for us, and one of the
      main features that sold us on it.
- End-to-end encryption
    - Rocket.Chat allows users to have [end-to-end
      encrypted](https://rocket.chat/docs/user-guides/end-to-end-encryption/)
      conversations. This is really nice to have, and opens up Rocket.Chat as a
      nice non-email option for vulnerability disclosure.
- High automation potential
    - Rocket.Chat doesn't just have Github integration; it actually supports
      [free-form
      scripting](https://rocket.chat/docs/administrator-guides/integrations/github/)
      around Github webhook events. Combined with its powerful [bot
      framework](https://rocket.chat/bots), this could open up a number of
      really interesting Github integration/automation possibilities.
- Rocket.Chat's hosted option includes an SLA
    - Rocket.Chat offers [an SLA
      policy](https://rocket.chat/handbook/support/slas/) along with their
      hosting option. This is probably the strongest reliability guarantee we
      can hope for among all options.

### Zulip
- Organization
    - Zulip has topic-based organization, allowing better separation of thoughts
      and tasks than Gitter, which only allows for a chronological feed.
    - Like Gitter, it supports multiple rooms (known as “streams”). It also
      promotes stream discoverability better than Gitter.

Taken together, we believe that Rocket.Chat's unique features put it above and
beyond Zulip, making it the best option for a new chat app for the Enarx
project. 

### Alternatives 

In terms of alternatives: Following Enarx's design principles of open
development, we strongly value FOSS solutions, of which there are,
unfortunately, few. Beyond Zulip, there's:

- [Gitter](https://gitter.im), which we've already discussed and want to move
  away from
- [Matrix](https://matrix.org), which has some nice security/privacy features,
  but otherwise looks not as polished and not necessarily as feature-complete as
  Zulip/Rocket.Chat. It certainly lacks many of the nice "quality-of-life"
  features present elsewhere.
- [Mattermost](https://mattermost.org/) is another FOSS chat option that, like
  Rocket.Chat, is primarily designed for self-hosting. We did not consider it
  due to its lack of anonymous/publicly-readable chat.
- IRC, which is a non-starter.

## Prior art

We have used Gitter for a while, and have had annoyances with it for a while, as
previously discussed. Beyond that, Enarx has not used any other public chat apps
(apart from Rocket.Chat and Zulip, which we're discussing here).

## Unresolved questions

We still have no reference point for how reliable Zulip will be. This,
unfortunately, is something we can only learn with experience.

As discussed above, Rocket.Chat has an SLA, which makes this less of a concern
for it.

## Implementations

Thanks to the generosity of Rocket.Chat, the Enarx project now has its own
hosted Rocket.Chat server available at [`chat.enarx.io`](https://chat.enarx.io).
Given the research done in the process of making this document, the team's
consensus seems to be that Rocket is the preferred option, and we should move
towards making it our default.

### Migration process

With a server active, the first step of the migration process should be to
configure the app -- in particular:

- ensure it is available at `chat.enarx.io`
- ensure that end-to-end encryption channels are available for vulnerability
  disclosure
- enable Github OAuth login
- develop and trial the new-user process
- create a a starting set of channels

Once configured, the core team should trial the setup full-time for a full
working week. This will allow the administrators to deal with any issues before
the general public encounters them. However, during this trial period, it will
be important to simultaneously monitor the Gitter chat to ensure non-core
contributors (or anyone else asking questions there) get their questions
answered.

Finally, and only after the full-time trial is complete, we will officially
migrate Enarx's chat app to the Rocket.Chat instance. To do this, we will:

- Create announcement messages in all our Gitter channels, mentioning all users,
  that we have migrated to Rocket.Chat, available at our canonical chat URL.
- Update the links to our chat on our wiki, README, etc. to point to the new URL
- (Optional) Implement a bot that sends periodic reminders to the channel that
  we have moved, or automatically responds when a new message arrives to the
  channel with the new link

From there, the transition will be complete.

## Future Possibilities

One particularly exciting possibility exists around the alternatives' robust
integration potential. Both [Zulip](https://zulipchat.com/api/running-bots) and
[Rocket.Chat](https://rocket.chat/bots) have powerful bot frameworks, which
could potentially open up interesting automation opportunities with Github and
elsewhere. Some ideas could include a bot that pings PR reviewers when a
revision is pushed that resolves their review, or a bot that sends a message
when our wiki is edited (which would allow auditability of wiki changes even if
we open it up to the public).
