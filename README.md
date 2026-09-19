<div align="center">
  <img src="docs/icon.png" width="96" alt="">
  <h1>tiller</h1>
  <p><strong>A desktop app for Claude Code and Codex, built around one idea: keep typing while the agent works.</strong></p>
  <p>
    <a href="https://github.com/miroqaan/tiller/releases">Releases</a> ·
    <a href="https://github.com/miroqaan/tiller/issues/new/choose">Report a bug</a> ·
    <a href="https://github.com/miroqaan/tiller/issues">Issues</a>
  </p>
</div>

---

Messages you send mid-turn land in a **visible queue** you can edit, reorder and merge before they go out. **Tab** sends one straight into the turn that is running, which keeps the work it has already done.

![Two follow-ups queued while a story streams, one edited and merged into the other, then Tab sends one at once and the queued message goes out after the reply](docs/queue-steer.gif)

## Why

The Claude Code CLI and the official desktop app do one of two things with a message sent while the agent is busy: drop it into an invisible queue, or interrupt on the spot. There is no way to see what is queued, fix it, reorder it, or pick one item to send first.

The same request keeps coming back in the Claude Code issue tracker:

- [anthropics/claude-code#92988](https://github.com/anthropics/claude-code/issues/92988) — the desktop Code tab has no "queue until the turn ends"
- [anthropics/claude-code#77537](https://github.com/anthropics/claude-code/issues/77537) — a first-class queue UI: view, delete and inject items individually
- [anthropics/claude-code#30492](https://github.com/anthropics/claude-code/issues/30492) — real-time steering between tool calls

tiller is a frontend that fills that gap. It does not replace the agent; it drives the one you already have.

## Queue and steer

| Key | Idle | Agent working |
|---|---|---|
| Enter | Send | **Add to queue** |
| Tab | Send | **Send into the running turn (steer)** |
| Shift+Enter | Newline | Newline |
| Esc (empty input) | — | Interrupt the current turn |
| Ctrl+F | Search conversations | Search conversations |

Queued messages sit right above the composer. Until the moment one is sent you can:

- **Edit** it, so you can rewrite a follow-up after seeing how the previous turn went
- **Reorder** by drag, or jump an item to the top
- **Merge** it into the item above; keep pressing to fold several messages into one
- **Send now**, delete one, or clear all

Close the app and the queue is still there when you come back.

## What else is in it

**Two engines, one window.** New conversations remember your last engine choice. With no saved choice, Codex is the default and appears first in the engine picker. Both get the same queue, steering, approvals, model and effort pickers, plan usage and notifications. A thread can be **carried to the other engine mid-conversation** and continue there.

**Threads that stay organised.** Make a project, name it, and put threads in it: a project is your own group, not a folder, so one folder's threads can go to different projects and one project can hold threads from several folders. Threads you have not filed stay grouped by the folder they run in. Or switch to priority and drop the grouping: every thread in the order it needs you, stopped on a question first, then running, errors, unread replies, and the quiet ones last. Or let a small model read the thread *titles* (never the conversations) and propose named groups. Pin, rename, fork from any message, move to a project.

**Permissions you can see.** Approval requests arrive as an inline card — allow once, always allow, deny — with a per-thread permission mode (default, accept edits, plan, bypass).

**Local videos.** Local video links such as `[title](clip.mp4)` or `![title](clip.mp4)` play inside the conversation and reader pane, with playback, seeking, volume and fullscreen controls. Absolute and relative paths work; wrap paths with spaces in `<...>`. MP4/M4V, WebM, MOV and OGV are recognized; codec support depends on the platform. Videos stream from disk without autoplay.

**Remove project groups without losing conversations.** Use a project’s … menu or right-click menu to delete the group after confirmation; its conversations return to the folder list and files are left untouched.

**Separate drafts for each conversation.** Switching threads restores that thread’s unsent text and image attachments during the current app session.

**Images and diagrams.** Paste or drop images into the composer; they travel with a queued or steered message. Answers render `mermaid` diagrams, `svg` blocks and local image files, and in Codex threads you can ask for a picture and get one.

**Search that reaches closed threads.** Ctrl+F finds threads by title and by what was said in them, including Codex threads that are not open.

**Your conversations, kept by tiller.** Every thread is copied into a local vault on your own disk, so a thread opens instantly and survives the engines tidying up their own files. A Claude transcript that Claude Code deleted is put back before the thread is resumed. See [docs/vault.md](docs/vault.md).

**Everything is local.** Authentication is your own API key or the Claude Code login already on your machine. tiller has no login screen, stores no tokens of ours, and sends your conversations to nobody but the engine you chose.

**English and Korean UI**, following the system language.

## Status

tiller is in development and **no installer has been published yet**. This repository is where releases will appear — [watch it](https://github.com/miroqaan/tiller/subscription) to hear about the first one.

| Platform | State |
|---|---|
| Windows | The development platform. Verified from the unpacked package; installer not yet published |
| macOS | Not built or notarized yet |
| Linux | AppImage planned |

Installers will carry both engines, so the app works straight after installation with nothing else to install. The Claude Code binary ships exactly as Anthropic publishes it: not modified, not re-signed, no sign-in method removed.

## Issues and requests

**This repository is the place for them.** Bug reports, feature requests and questions all go to [Issues](https://github.com/miroqaan/tiller/issues/new/choose) — the templates ask for the few things that make a report actionable (what you did, which engine, which version).

The application source is not here. tiller is proprietary and its source lives in a private repository, so this repository holds the release downloads, the issue tracker, and the published documentation. Pull requests are welcome for the documentation in `docs/`.

Before opening an issue, a quick look through [existing issues](https://github.com/miroqaan/tiller/issues?q=is%3Aissue) saves everyone a round trip. Something that looks like a security problem goes through [SECURITY.md](SECURITY.md) instead, not a public issue.

## Notes

- "Bypass permissions" mode runs everything without asking, including files outside the project. Turn it on only when you mean it.
- The optional computer-use tools move the real mouse and type on the real keyboard of the machine tiller runs on. Expect the cursor to be taken over while a turn runs.

## Legal

tiller is a personal project and is **not affiliated with, endorsed by or sponsored by Anthropic or OpenAI**. Claude and Claude Code are trademarks of Anthropic; Codex is a trademark of OpenAI. Using tiller with either engine is subject to that engine's own terms.

The application is proprietary. See [LICENSE](LICENSE) for what this repository covers.

[한국어 README](README.ko.md)
