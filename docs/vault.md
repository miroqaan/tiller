# Your personal vault

Tiller keeps its own conversation history in ordinary folders you control. You can read and back up those files without a sync account. This page describes the current development preview, which may be ahead of the latest release. See [sync preview](sync.md) for optional encrypted uploads and their current limits.

## Manage local vaults

Click the **current vault name in the sidebar** to open the vault manager. Each vault keeps its own conversations, projects and working-folder mappings. Creating and managing local vaults requires no account or sync connection.

| Action | What happens |
|---|---|
| **Create vault** | Enter a name. Tiller creates `Documents/Tiller Vaults/<name>` automatically, adding `(2)`, `(3)`, etc. if a name is already occupied. No folder selection is needed. |
| **Import cloud vault** | Choose a remote vault, enter a local name and its encryption password. Tiller prepares a separate local vault at an automatic location and opens it in a new window, which downloads it. |
| **Open folder as vault** | Add an existing vault folder, or start a vault in an empty folder. Adding it to the list does not switch your current vault. |
| **Name on this device → Save** | Change the displayed name on this device. The folder and vault identity stay the same. |
| **Open in new window** | Open the vault in a window of its own, as in Obsidian. A vault that already has a window comes to the front. The current window keeps its conversations, running work and sync. |
| **Show in file manager** | Open the vault folder in your system file manager. |
| **Remove from list** | Remove this device's registration while keeping all files and conversation state. A connected vault also stops syncing on this device; its cloud copy stays. Open the same folder again to register the same vault identity. |

Each open vault has its own window. Closing a window stops only that vault's work and sync; if one of its conversations is still working, Tiller asks first. Closing the last window quits Tiller, and the next start opens the same windows again.

The default vault and vaults with an open window cannot be removed from the list. Working-folder mappings are retained for reopening a removed vault, but a mapping is not restored if another registered vault has since claimed that folder.

Select **Sync settings** for the current vault to open Tiller Sync. Sign in with GitHub, review the account's sync access and expiry, then connect the vault to a cloud vault you already use on another device, or create a new one. The test service is selected automatically. GitHub authorization opens in your browser; the connection steps stay in Tiller. See [One cloud vault for all devices](sync.md#one-cloud-vault-for-all-devices).

Test access is assigned to selected accounts and is not granted by signing in alone. Paid subscriptions are not on sale yet. To receive a remote vault, choose **Import cloud vault** from the vault manager or sync panel, sign in with GitHub if needed, select the remote vault, and enter its password and a name for this device. The imported vault then opens in a window of its own and downloads its contents; **Go to its window** brings that window forward again. Your current vault and connection are preserved. A remote vault already connected on this device is reused instead of creating another copy.

Newly created and imported vaults use the operating system's Documents folder under `Tiller Vaults`. Existing vault paths, including the original `Documents/Tiller` vault, stay unchanged. Renaming changes only the display name. Development and isolated test profiles use a separate managed directory inside their profile. **Open folder as vault** remains available for existing folders at any supported location. Server address overrides and the experimental encrypted-folder transport are available in advanced settings; the ordinary setup does not require them. See [sync preview](sync.md) for encryption and recovery details.

## Combining vaults

Two vaults are combined only when a local vault that already has conversations connects to a cloud vault: choose **Sync with a cloud vault you already use** (disconnect a connected vault first) and confirm the warning, since the merge cannot be undone. See [One cloud vault for all devices](sync.md#one-cloud-vault-for-all-devices). There is no command to merge two local vaults on one device.

## Working folders on other devices

Conversations remember their working folder in a way each device can resolve:

- **Folders inside the vault**, such as its projects and the unfiled folder, continue in the same place of each device's vault. No setup is needed.
- **The home folder** of one device maps to each other device's home folder automatically.
- **Other folders**, such as a repository, are linked without asking: to a folder of the same name in this vault if there is one, otherwise to a new folder of the same name under the default location. The default location is `tiller-work` in your home folder, deliberately outside Documents, which OneDrive may sync. Change it with **Working folders on this device → Location for folders from other devices → Change…**; folders linked later go there, and **Relink…** moves one already linked. A folder whose conversation is open when it arrives stays read only until it can be linked, and then offers **Link to**, **Use this device's home folder** or **Choose folder…** in place of the message box.

A Codex conversation from another device has no native thread here. Opening it carries the conversation so far into a new Codex thread on this device, as switching engines does, and you continue in place with the model and effort last chosen for Codex on this device. If that fails, for example without a usable Codex engine, the history still opens with the reason and **Try again**. The other device then treats it the same way.

## Agent instructions

Open the current vault’s **Agent instructions** from the vault manager. **Common vault instructions** holds working rules used by both Claude and Codex conversations in that vault. Save them locally without an account, or sync them with the vault when connected.

Each device’s terminal global instructions (Codex `AGENTS.override.md` or `AGENTS.md` in its home, Claude `CLAUDE.md`) are added to the common vault instructions as marked blocks when a vault opens and before each sync, so conversations on every device use them. The original files are only read; this device’s home folder is written as `~`. **Imported instructions** lists each block with its device and state. **Keep as my instructions** turns a block into ordinary text, and **Stop importing** removes it and stops importing it on every device. A block you edited is not replaced when its original changes; Tiller tells you instead, and also when devices have differing versions.

**Additional instructions** automatically finds existing global and project files for the selected engine account and working folder, including Codex `AGENTS.md` / `AGENTS.override.md` and Claude `CLAUDE.md` / `CLAUDE.local.md`. Expand a source to see its original path and contents, then edit and save that original file. Global edits affect other vaults using that engine account. Here, “project” means the actual working folder’s instruction scope, rather than a conversation group name.

Tiller does not create a separate global/project settings hierarchy. Apart from the imported global instructions above, it does not copy existing files into the vault. Editing a source does not upload it directly; project instruction files can be selected separately in **Work file sync**. Native engines still decide which instructions to load, including precedence, size limits, settings, imports and rules; the panel identifies shadowed or conditional sources where possible.

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

Only files applicable to that conversation are present. `미분류` is the current on-disk name for the unfiled folder. Projects created in Tiller have identity markers, so their folders can be renamed without changing identity. Arbitrary folders placed beside them are not automatically made into projects.

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
| `conversation.json` | History as Tiller displays it, including threads that changed engines |
| `meta.json` | Conversation identity, title, engine and project information |
| `transcript.jsonl` | Claude's native history snapshot |
| `rollout.jsonl` | Local Codex history snapshot; not uploaded by the sync preview |
| `attachments/` | Attached images and durable generated image originals |
| `.tiller/file-sync/` | Portable file and folder selection rules |

Engine snapshots are retained as opaque history copies. Before resuming Claude, Tiller validates and materializes a needed native transcript. A different existing native transcript is preserved and the incoming history gets a fresh engine identity. Codex history from another device remains readable and searchable, without treating a copied file as a resumable Codex thread.

A vault owns its conversation records and can sync selected working files. Open **Work file sync** from the current vault's settings to include files or folders, including project key files such as `.env`, and exclude individual children. A link in a conversation does not automatically select its referenced working file. Videos offer an inclusion button in their card; received video/audio originals download on request or when kept offline. See [file selection and limits](sync.md#choose-working-files).

## Working folders

A working root identifies a project independently of its absolute path. On another device it is linked automatically as described in [Working folders on other devices](#working-folders-on-other-devices); **Working folders on this device → Relink…** maps it to another folder. Path mappings stay local. Until mapped, received conversations are read only.

Tiller shows the conversations you have in Tiller. It does not bring in Claude Code or Codex conversations from the terminal or other apps.

## Credentials and local-only state

Engine-managed credentials, app-managed API keys, sync account tokens and plaintext vault keys are excluded from portable conversation data. User-selected project key files are included in encrypted work-file sync. Device paths, queued messages, automatic execution state and permission settings are not restored from a remote vault. A received conversation does not inherit permission to run commands on the receiving device.

This boundary does not redact conversation content: a password or key pasted into a message, or included in a tool result or attachment, remains part of that history. Local vault files are ordinary plaintext files; end-to-end encryption protects the optional remote copy.

The local vault works without synchronization. [Sync preview](sync.md) explains connection, disconnection, deletion, transfer and recovery separately.
