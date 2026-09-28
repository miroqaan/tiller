# Personal vault sync preview

Vault synchronization is implemented in the development preview. **No public download release or paid subscription service is available yet.** A staging cloud service is deployed, and GitHub sign-in and encrypted sync have been tested against it. Paid billing, real physical devices and Windows/macOS/Linux interoperability still need acceptance testing.

The default is **Local only**. You can use local vaults without a sync account. Connecting a vault explicitly enables an encrypted remote copy of its supported conversation data. This preview is for your own vaults and devices; it does not provide shared team vaults.

## Controls in the preview

Open the sync panel from the sidebar or the current vault's **Sync settings**. **Tiller Sync** selects the test service automatically; you do not need to enter a service address. Sign in with GitHub in your browser, then return to Tiller to review the account's sync access and expiry, create a remote vault, or connect an existing one using its password or recovery key. This sync account is separate from your Claude or Codex account. Local vault management, working-folder links, sync status, manual sync and recovery review remain in the app.

Test access is assigned to selected accounts; signing in alone does not grant access. Use the account refresh control to check for changes. A failed access check is shown as an error rather than as a missing or expired subscription. Custom service addresses and the encrypted-folder test transport are available in advanced settings; a previously configured custom service is preserved.

New vault passwords require at least 12 characters. Creation shows a recovery key once; save it separately. The service cannot recover a lost password. A connected cloud vault also offers **Change vault password**, using the current password or recovery key. This replaces the password wrapper while preserving the vault key, recovery key, ciphertext and existing connected devices. It does not revoke devices; use **Connected devices → Revoke** for that.

The service implements monthly subscription checks: expiry makes remote content access read only and prevents new uploads. Local conversations remain available. No paid service or checkout has launched, and no public price is announced here.

## Progress and background work

Close the sync panel to continue working in other conversations. Synchronization continues, and a compact status button lets you reopen its details. The panel shows the current stage, completed files or transferred bytes, elapsed time, and a recent **Sync log** with timestamps and errors. Progress percentages describe the current stage; preparation can remain indeterminate until the amount of work is known. The log is kept for the current app session and can be cleared.

Incoming updates keep the affected conversation closed until its complete snapshot has been applied. Other conversations can still be opened or created. Project-folder and root changes may briefly require a wider pause to keep file locations consistent. Settings that would disconnect or replace the active sync connection wait until the current operation finishes.

## What is encrypted and uploaded

Supported uploads include conversation history, Claude transcript snapshots, conversation metadata, project and root descriptions, supported conversation image attachments and generated image originals, and the vault’s common agent instructions (`.tiller/instructions.md`). Codex conversation history can be read and searched on another device, but its native rollout and engine database are not synchronized for resume. Working-folder files require selection in **Work file sync**; linking a root or editing a project instruction file does not select it automatically.

Generated originals use the existing local image formats and limit: PNG, JPEG, GIF or WebP, up to 20 MiB per image.

For [vault instructions](vault.md#agent-instructions), update every connected device to a version that supports the instructions editor. Older preview builds reject the new sync path. Concurrent instruction edits retain the local version as a recovery copy before applying the received version. Review it in **Agent instructions**, load it into the editor and save explicitly; the backup remains. Clearing the editor and saving syncs an empty instruction file.

The client encrypts content and the file manifest before upload using AES-256-GCM. A random vault key is wrapped separately by a password-derived key and a recovery key. Password derivation uses scrypt. The server holds encrypted objects and encrypted key wrappers, not the plaintext vault key or password. It still sees operational metadata such as account/vault/device identifiers, object sizes, versions and request timing. This preview has not had an independent security audit.

Engine-managed credentials, app-managed API keys, sync account tokens, local path mappings, queues, automatic execution state and permission grants are excluded. Project key files such as `.env` can be selected like any other working file. Secrets already inside conversation text or attachments remain inside that encrypted history. Local vault and working files remain readable plaintext on your device.

## Choose working files

Open the current vault's **Work file sync** to browse linked working folders or add another folder. Select individual files or a whole folder. A folder selection includes future files and project key files; more specific child selections take precedence. Use **Exclude** for individual exceptions, or **Use folder setting** to remove an override. Cache, temporary, dependency-cache and Tiller-managed history/state files are excluded. Selection changes sync across devices; absolute folder mappings stay local.

On another device, link the received root to its local working folder. Selected ordinary files download there automatically. Video and audio originals download on request; choose **Download** or **Keep offline** in the file list. A video card shows whether its original is local or included, offers **Include in sync**, and can download the original before playback. File transfers use bounded chunks in the background, with a 1 GiB limit per working file. Oversized files are shown as excluded rather than blocking other sync.

Excluding a file removes its cloud reference and stops syncing it while preserving existing local originals on every device. Concurrent edits to different selection rules are combined; simultaneous include/exclude changes to the same rule keep the exclusion. Conflicting local file edits are preserved in the recovery folder before a received version is applied. Removing a local file alone does not delete its cloud copy; sync can download it again. A missing local mapping never authorizes writing into the previous device's path. Linking a different folder preserves conflicting originals there before applying cloud files. Update all connected devices before using this feature; older builds reject its new sync paths.

## Working across devices

On a receiving device, register an empty local vault, connect the remote vault and map its working roots to local folders. A missing root mapping keeps the conversation read only. Only files selected in **Work file sync** are transferred into those folders.

Claude CLI conversations in linked roots are included while tiller runs. The conversation menu can copy a terminal-resume command after preparing the native Claude transcript. Run that command yourself with the official CLI and your own engine login. Open app conversations and recently modified terminal transcripts defer incoming changes to protect active work. This does not provide a headless sync agent when tiller is closed.

**Codex conversations received from another device are read only.** History viewing and search are supported; native cross-device Codex resume is not. Recovered history is also read only, regardless of its original engine.

## Disconnect, delete, move and recover

| Action | Result |
|---|---|
| Disconnect | Stops this vault's connection and preserves both local files and the remote copy |
| Revoke a device | Invalidates that registration; files already saved on that device remain, and it must enroll again to reconnect |
| Delete a synced conversation | Sends deletion to the connected vault and its other devices; offline devices catch up later |
| Move a working root to another vault | Moves its file selection policy, creates new conversation identities in the destination and deletes the source identities, including on the source vault's other devices; source-vault file sharing stops and external project files stay in place |
| Restore a recovery copy | Creates new read-only history without native engine resume IDs; the deleted original stays deleted and the backup remains |
| Discard a recovery copy | Permanently removes that selected local recovery backup |

Deletion wins over stale offline edits. Conflicting local content can be retained as a recovery candidate rather than silently recreating a deleted thread. Review those candidates in the panel. Valid conversation-history snapshots can be restored; other files can be inspected in the backup folder. Recovery copies are not automatically uploaded or restored. Restoring history does not restore arbitrary attachments or permission grants.

Deletion is not instant erasure of every copy: disconnected devices, independent backups and external engine history may remain. Remote unreferenced objects are collected separately. Disconnecting or signing out is not a remote-vault deletion request.

## Encrypted folder transport

The panel also has an experimental encrypted-folder option for local testing. Use a separate destination on **one filesystem**, outside the plaintext vault and device-state directories. Multiple isolated clients can exercise the same destination locally.

**Dropbox, OneDrive and other asynchronously replicated folders are not supported transports.** Their conflict-copy and replication behavior does not provide the serialization this transport requires. Passing local tests does not establish safety on a network filesystem or on two physical machines.

For the ordinary local folder layout and current storage paths, see [Your personal vault](vault.md).
