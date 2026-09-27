# Your personal vault

tiller keeps its own conversation history in ordinary folders you control. You can read and back up those files without a sync account. This page describes the current development preview; no public installer has been released. See [sync preview](sync.md) for optional encrypted uploads and their current limits.

## Where your files live

The normal vault is the operating system's Documents folder plus `Tiller`. Documents may itself be redirected, for example by OneDrive. Each app project has a folder, and each conversation has a session folder:

```text
Documents/Tiller/
  .tiller/                       vault metadata and local backups
  <project>/
    .tiller/project.json         stable project identity
    sessions/<conversation-id>/
      meta.json
      conversation.json
      transcript.jsonl
      rollout.jsonl
      attachments/
  미분류/sessions/                unfiled conversations
```

Only files applicable to that conversation are present. `미분류` is the current on-disk name for the unfiled folder. Projects created in tiller have identity markers, so their folders can be renamed without changing identity. Arbitrary folders placed beside them are not automatically made into projects.

Open **Vaults and sync** in the sidebar to add a local vault or switch between registered vaults. Switching restarts the app after active work finishes. Additional vaults can live in folders you choose, with separate conversation state and working-folder mappings.

Device settings are separate from the vault:

| Platform | Normal device-state directory |
|---|---|
| Windows | `~/.tiller` (`%USERPROFILE%/.tiller`) |
| macOS | `~/Library/Application Support/tiller` |
| Linux | `~/.config/tiller` |

Windows development instances use `~/.tiller-dev`. Development and explicitly isolated test instances normally keep their default `Tiller` folder under their separate state directory. macOS and Linux paths describe the implementation, not completed platform acceptance.

| Override | Purpose |
|---|---|
| `TILLER_WORKSPACE_DIR` | Changes the default project-folder vault location |
| `TILLER_USER_DATA` | Changes device-state storage; also isolates the default vault under that directory unless a workspace override is provided |
| `TILLER_VAULT_DIR` | Legacy vault location to import; does not select the current project-folder vault |

The old `<userData>/vault/threads/` layout is imported while preserving its originals. Keep independent backups before moving or replacing a vault directory.

## What the vault contains

| Item | Purpose |
|---|---|
| `conversation.json` | History as tiller displays it, including threads that changed engines |
| `meta.json` | Conversation identity, title, engine and project information |
| `transcript.jsonl` | Claude's native history snapshot |
| `rollout.jsonl` | Local Codex history snapshot; not uploaded by the sync preview |
| `attachments/` | Images attached to that conversation |

Engine snapshots are retained as opaque history copies. Before resuming Claude, tiller validates and materializes a needed native transcript. A different existing native transcript is preserved and the incoming history gets a fresh engine identity. Codex history from another device remains readable and searchable, without treating a copied file as a resumable Codex thread.

A vault owns conversation records, not every file in its working folders. Source repositories, generated documents and other external project files need their own backup or transfer. A link in a conversation does not automatically include the referenced file.

## Working folders and terminal conversations

A working root identifies a project independently of its absolute path. On another device, **Working folders on this device → Link…** maps it to that device's folder. Path mappings stay local. Until mapped, received conversations are read only.

Claude CLI conversations in linked working folders are discovered while tiller runs, including terminal sessions created outside the app. **Find CLI conversations** requests a scan. Linking a folder can include more conversations than the ones you started in tiller; the panel asks for confirmation. There is no standalone background sync daemon in this preview.

## Credentials and local-only state

Engine credentials, API keys, sync account tokens and plaintext vault keys are excluded from portable conversation data. Device paths, queued messages, automatic execution state and permission settings are not restored from a remote vault. A received conversation does not inherit permission to run commands on the receiving device.

This boundary does not redact conversation content: a password or key pasted into a message, or included in a tool result or attachment, remains part of that history. Local vault files are ordinary plaintext files; end-to-end encryption protects the optional remote copy.

The local vault works without synchronization. [Sync preview](sync.md) explains connection, disconnection, deletion, transfer and recovery separately.
