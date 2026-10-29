# Changelog

Releases appear on the [Releases page](https://github.com/miroqaan/tiller/releases),
each with its notes. This file collects them in one place.

The version numbers are Tiller's own. The engine versions a release carries are
named in its notes, because updating an engine means releasing the app.

## 0.3.0 — 2026-10-30

Third pre-release, **Windows x64 only**. The installer is not code-signed yet
and has no automatic updates.

| Engine | Version |
|---|---|
| Claude Code | 2.1.307 (Agent SDK 0.3.307) |
| Codex | 0.163.0 |

- **Split mode.** Arrange conversations in 4, 8, 16 or 32 cells across the monitors you pick, and fill them from recent activity or by search. Drag titles between cells and monitors, resize rows and columns or even them out from the header, and save layouts as named presets. One cell selection is shared by every monitor. The grid's list deletes conversations from its right-click menu, the selected cell's plan usage shows at the top right, cells go back and forward like tabs, and the grid comes back after a restart.
- **Integrated terminal.** Ctrl+backtick or the terminal icon opens a shell below the conversation, in its working folder, with several terminal tabs.
- **Command palette and title search.** Ctrl+Shift+P searches and runs the app's actions with their shortcuts. Ctrl+K finds a thread by its title from the sidebar.
- **Search.** Every conversation is searched from an index, matching words in any order regardless of spacing, case and width. A thread lists all its matches, and a filter searches the open thread only.
- **Reply to a message.** Choose **Reply** on an earlier message to ask about it; the reply keeps a link back to it. Conversations and messages can also be copied as links that open them.
- **Message times.** Each message and finished answer shows when it was written.
- **Sidebar sections.** Parallel tasks, code reviews and reference studies are listed in sections of their own, apart from your conversations.
- **References and reviews.** Reference studies, improvement loops and reference mode, which reads the vault's conversations and documents before answering, are available in every conversation. Any conversation can hand its change to the other engine for review.
- **Panes and windows.** Split panes by dragging a tab or conversation to a pane's edge, stacked or side by side. Browser tabs open in linked windows, and the built-in browser lists downloads.
- **Nothing lost on quit.** Input is held and the last save is confirmed before Tiller quits or restarts. Work cut off by quitting, a crash or closing a conversation is marked as interrupted.
- **Recovery.** A panel that fails to render shows an error alone with **Try again**. A failed turn says why, and an overloaded model can be retried on another. Saves that fail on a full disk are kept and retried. An engine idle for ten minutes is released and resumes on your next send.
- **Answers and previews.** Diagrams open enlarged with zoom, videos play at a chosen speed with a frame preview on the seek bar, and the wheel keeps scrolling while pictures and videos load. Previews wrap long lines, keep their copy buttons in view, page with Page Up/Down and show working-copy diffs. File links open at the lines they point to.
- **Vault instructions.** Create, import from another vault, rename and delete instructions, and choose the vault's test method; heavy test commands then run one at a time.
- **Composer.** Long messages fold into a preview. Prompt refinement can send at once or let you review first. Composer tools show as icons.
- **Requests before work** (off by default): two hours before a set work start, Tiller sends one short request to every signed-in Claude and Codex account.
- Linked Windows shortcuts can be run after confirming their program and arguments.

Not in this build: sync and Tiller sign-in (shown as coming soon), and with them the phone app.

Notes and download: [Tiller 0.3.0](https://github.com/miroqaan/tiller/releases/tag/v0.3.0).

## 0.2.0 — 2026-10-05

Second pre-release, **Windows x64 only**. The installer is not code-signed yet
and has no automatic updates.

| Engine | Version |
|---|---|
| Claude Code | 2.1.287 (Agent SDK 0.3.287) |
| Codex | 0.154.0-alpha.6.2 |

- **Orchestration.** A conversation can hand work to other conversations — a fresh one, a copy of itself or an existing thread — as many at once as the machine has room for. Tasks wait while the machine is busy, and each result comes back into the orchestrating conversation. Orchestrating threads have their own sidebar section, with their work listed under them.
- **Send in parallel.** Send a message while a turn is running, or from its row in the queue: a copy of the conversation works on it and reports back.
- **Built-in browser.** Claude and Codex browse in tabs beside the conversation through Tiller's own tools, upload files and answer a page's dialogs. Sites that refuse it, such as Google sign-in, open in a Chrome of Tiller's own.
- **Tabs and panes.** Open conversations side by side, in several panes at once, or pop them out into windows. Ctrl+W closes a tab, Ctrl+PageUp/PageDown move through tabs, and every shortcut is listed in App settings.
- **Composer.** PageUp/PageDown page through the conversation and Home/End go to its top and end. Pick the model and reasoning effort of a new thread before its first message.
- **Japanese UI.** The app is available in Japanese (日本語), beside English and Korean. Tiller picks it when the system language is Japanese, or choose it in App settings › Language. Dates and times follow the language.
- **Accounts.** When an account reaches its usage limit, work carries on with another signed-in account, or with the other engine. Accounts can be logged out of and deleted from the accounts menu.
- **Side chat** beside a thread, never saved.
- **Vaults.** Every vault opens in a window of its own; new vaults go under `Tiller Vaults` in the home folder and can be moved. Claude and Codex share Tiller's memory and use only the vault's instructions; global instructions from the terminal can be imported into them.
- **Answers.** Excel workbooks show as read-only tables, tables copy as Markdown, and pictures copy from their right-click menu.
- The system stays awake while background work runs.
- Tiller no longer reads conversations from the terminal or the Claude desktop app; engines run on Tiller's own accounts and folders.

Not in this build: sync and Tiller sign-in (shown as coming soon), and with them the phone app.

Notes and download: [Tiller 0.2.0](https://github.com/miroqaan/tiller/releases/tag/v0.2.0).

## 0.1.0 — 2026-09-29

First public build, **Windows x64 only**, as a pre-release. The installer is
not code-signed yet and has no automatic updates.

| Engine | Version |
|---|---|
| Claude Code | 2.1.280 (Agent SDK 0.3.280) |
| Codex | 0.154.0-alpha.6.2 |

Notes and download: [Tiller 0.1.0](https://github.com/miroqaan/tiller/releases/tag/v0.1.0).
