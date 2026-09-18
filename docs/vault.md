# The vault: where your conversations are kept

tiller keeps its own copy of every conversation, on your own disk. This page
says where it is, what is in it, and what is deliberately never in it.

## Why there is a copy at all

The engines own their history, and they tidy it up. Claude Code deletes old
transcripts. A Codex thread that is not open is not something tiller can read
from the engine at all. Without a copy of its own, tiller would be a window
onto two other programs' file formats, locations and clean-up rules — and a
conversation could simply vanish.

So every thread is mirrored as it goes. A thread opens from that copy
instantly, and the engine's version is compared in the background and used
instead when it is at least as complete. If the engine has lost the thread, the
copy is what you see; and a Claude transcript that Claude Code cleaned up is
written back into place before the thread is resumed, so the conversation can
continue.

## Where it is

```
<userData>/vault/threads/<thread id>/
```

`<userData>` is `%APPDATA%/tiller` on Windows, `~/Library/Application Support/tiller`
on macOS and `~/.config/tiller` on Linux. Two environment variables move things:

| Variable | Moves |
|---|---|
| `TILLER_VAULT_DIR` | the vault alone |
| `TILLER_USER_DATA` | everything tiller stores, the vault included |

The development instance stores its state separately (`tiller-dev`), so a
stable build and a development build never write to the same vault.

## What is in a thread folder

| File | What it is |
|---|---|
| `conversation.json` | the conversation as tiller shows it, in tiller's own shape — the same shape for both engines, and one file even for a thread that has been switched between them |
| `transcript.jsonl` | a byte-for-byte copy of the Claude Code transcript |
| `rollout.jsonl` | a byte-for-byte copy of the Codex rollout file |

`conversation.json` is the only one tiller interprets. The engine files are
copied whole and treated as opaque, because their job is to be handed back to
the engine that wrote them, unchanged, when a thread is resumed.

Two other folders sit next to the vault: `attachments/<thread>/` holds the
images you sent with a message, and `carried/<thread>.json` holds the part of a
switched thread that the receiving engine does not list among its own turns.
Deleting a thread removes its attachments with it.

## What is never in it

Credentials. Not by filtering them out — by an allowlist of the paths that may
be read and written, fixed in code and held in place by tests. Files such as
`~/.claude/.credentials.json`, `~/.claude.json`, `~/.claude/settings*.json` and
`~/.codex/auth.json` are outside it, and no input makes a credential file name
pass.

What tiller reads from Claude Code is a single assembled path —
`~/.claude/projects/<folder>/<session id>.jsonl`, built from a session id and a
working folder tiller already knows. It does not walk `~/.claude` and does not
glob for files.

One thing an allowlist cannot cover: a secret **you** paste into a
conversation is part of the conversation, and it is copied with it. That is
worth remembering before pasting a key into a chat, here or anywhere else.

## Where it goes next

Today the vault is local: nothing leaves your machine. The planned sync between
your own machines is end-to-end encrypted, with the server holding content it
cannot read, and it will be an optional paid subscription — the app and every
feature on this page stay free and work with no account. That format and its
encryption will be documented here before it ships.
