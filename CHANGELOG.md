# Changelog

Releases appear on the [Releases page](https://github.com/miroqaan/tiller/releases),
each with its notes. This file collects them in one place.

The version numbers are Tiller's own. The engine versions a release carries are
named in its notes, because updating an engine means releasing the app.

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
