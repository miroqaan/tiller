# Your personal vault

tiller keeps its own conversation history in ordinary folders you control. You can read and back up those files without a sync account. This page describes the current development preview; no public installer has been released. See [sync preview](sync.md) for optional encrypted uploads and their current limits.

## Manage local vaults

Click the **current vault name in the sidebar** to open the vault manager. Each vault keeps its own conversations, projects and working-folder mappings. Creating and managing local vaults requires no account or sync connection.

| Action | What happens |
|---|---|
| **Create vault** | Choose an existing parent folder and enter a name. Tiller creates a new folder with that name; it never replaces an existing folder. |
| **Open folder as vault** | Add an existing vault folder, or start a vault in an empty folder. Adding it to the list does not switch your current vault. |
| **Name on this device → Save** | Change the displayed name on this device. The folder and vault identity stay the same. |
| **Open this vault** | Save open conversations and restart Tiller in the selected vault after confirmation. Finish running work before switching. |
| **Show in file manager** | Open the vault folder in your system file manager. |
| **Remove from list** | Remove this device's registration while keeping all files and conversation state. Open the same folder again to register the same vault identity. |

The default vault and the currently open vault cannot be removed from the list. A vault connected to sync must be opened and disconnected before its registration can be removed. Working-folder mappings are retained for reopening a removed vault, but a mapping is not restored if another registered vault has since claimed that folder.

Select **Sync settings** for the current vault to open Tiller Sync. Sign in with GitHub, review the account's sync access and expiry, then create a new encrypted remote vault or connect an existing one. The test service is selected automatically. GitHub authorization opens in your browser; the connection steps stay in Tiller.

Test access is assigned to selected accounts and is not granted by signing in alone. Paid subscriptions are not on sale yet. To receive an existing remote vault on another device, first create and open an empty local vault, then connect it with the remote vault's password or recovery key. Server address overrides and the experimental encrypted-folder transport are available in advanced settings; the ordinary setup does not require them. See [sync preview](sync.md) for encryption and recovery details.

## Agent instructions

Open the current vault’s **Agent instructions** from the vault manager. **Common vault instructions** holds working rules used by both Claude and Codex conversations in that vault. Save them locally without an account, or sync them with the vault when connected.

**Additional instructions** automatically finds existing global and project files for the selected engine account and working folder, including Codex `AGENTS.md` / `AGENTS.override.md` and Claude `CLAUDE.md` / `CLAUDE.local.md`. Expand a source to see its original path and contents, then edit and save that original file. Global edits affect other vaults using that engine account. Here, “project” means the actual working folder’s instruction scope, rather than a conversation group name.

Tiller does not create a separate global/project settings hierarchy or copy those existing files into the vault. Editing a source does not automatically upload it; project instruction files can be selected separately in **Work file sync**. Native engines still decide which instructions to load, including precedence, size limits, settings, imports and rules; the panel identifies shadowed or conditional sources where possible.

Saved vault instructions take effect in new conversations, new side chats, or after closing and reopening a conversation. Running work keeps its current instructions. An unsaved editor draft stays available when you close the panel, collapse the sidebar or switch the interface language during this app session.

If a file changed outside the editor, Tiller preserves your draft and lets you inspect the latest original before saving. Instructions use UTF-8 text, up to 128 KiB; existing BOM and Windows line endings are preserved. Linked instruction files cannot be edited through this panel. Sync conflicts retain a local **Instruction recovery copy** that you can inspect and load into the vault editor as an unsaved draft. Saving that draft is a separate action and keeps the backup.

The vault-owned file is `.tiller/instructions.md`. Update Tiller on every connected device before syncing instructions; older preview builds do not recognize this new sync path.

## Where your files live

The normal vault is the operating system's Documents folder plus `Tiller`. Documents may itself be redirected, for example by OneDrive. Each app project has a folder, and each conversation has a session folder:

```text
Documents/Tiller/
  .tiller/                       vault metadata and local backups
    instructions.md              common agent instructions; included in vault sync
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

Additional vaults can live in folders you choose. Registered vault folders must be separate: one vault cannot be inside another.

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
| `.tiller/instructions.md` | Common agent instructions for this vault |
| `conversation.json` | History as tiller displays it, including threads that changed engines |
| `meta.json` | Conversation identity, title, engine and project information |
| `transcript.jsonl` | Claude's native history snapshot |
| `rollout.jsonl` | Local Codex history snapshot; not uploaded by the sync preview |
| `attachments/` | Attached images and durable generated image originals |
| `.tiller/file-sync/` | Portable file and folder selection rules |

Engine snapshots are retained as opaque history copies. Before resuming Claude, tiller validates and materializes a needed native transcript. A different existing native transcript is preserved and the incoming history gets a fresh engine identity. Codex history from another device remains readable and searchable, without treating a copied file as a resumable Codex thread.

A vault owns its conversation records and can sync selected working files. Open **Work file sync** from the current vault's settings to include files or folders, including project key files such as `.env`, and exclude individual children. A link in a conversation does not automatically select its referenced working file. Videos offer an inclusion button in their card; received video/audio originals download on request or when kept offline. See [file selection and limits](sync.md#choose-working-files).

## Working folders and terminal conversations

A working root identifies a project independently of its absolute path. On another device, **Working folders on this device → Link…** maps it to that device's folder. Path mappings stay local. Until mapped, received conversations are read only.

Claude CLI conversations in linked working folders are discovered while tiller runs, including terminal sessions created outside the app. **Find CLI conversations** requests a scan. Linking a folder can include more conversations than the ones you started in tiller; the panel asks for confirmation. There is no standalone background sync daemon in this preview.

## Credentials and local-only state

Engine-managed credentials, app-managed API keys, sync account tokens and plaintext vault keys are excluded from portable conversation data. User-selected project key files are included in encrypted work-file sync. Device paths, queued messages, automatic execution state and permission settings are not restored from a remote vault. A received conversation does not inherit permission to run commands on the receiving device.

This boundary does not redact conversation content: a password or key pasted into a message, or included in a tool result or attachment, remains part of that history. Local vault files are ordinary plaintext files; end-to-end encryption protects the optional remote copy.

The local vault works without synchronization. [Sync preview](sync.md) explains connection, disconnection, deletion, transfer and recovery separately.
