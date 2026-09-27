# Personal vault sync preview

Vault synchronization is implemented in the development preview. **No public download release or hosted cloud service is available yet.** Local integration tests exercise the client and service together, but live cloud sign-in and billing, real physical devices and Windows/macOS/Linux interoperability still need acceptance testing.

The default is **Local only**. You can use local vaults without a sync account. Connecting a vault explicitly enables an encrypted remote copy of its supported conversation data. This preview is for your own vaults and devices; it does not provide shared team vaults.

## Controls in the preview

The sidebar's **Vaults and sync** panel provides local vault selection, working-folder links, sync status, manual sync, recovery review and connection controls. The cloud path accepts a service address and GitHub sign-in, separately from your Claude or Codex account. It supports creating a remote vault or joining one with its password or recovery key. These controls require a separately configured service; an example address is not a running service.

New vault passwords require at least 12 characters. Creation shows a recovery key once; save it separately. The service cannot recover a lost password. A connected cloud vault also offers **Change vault password**, using the current password or recovery key. This replaces the password wrapper while preserving the vault key, recovery key, ciphertext and existing connected devices. It does not revoke devices; use **Connected devices → Revoke** for that.

The service implements monthly subscription checks: expiry makes remote content access read only and prevents new uploads. Local conversations remain available. No paid service or checkout has launched, and no public price is announced here.

## What is encrypted and uploaded

Supported uploads include conversation history, Claude transcript snapshots, conversation metadata, project and root descriptions, and supported conversation image attachments. Codex conversation history can be read and searched on another device, but its native rollout and engine database are not synchronized for resume. External working-folder files are not uploaded merely because a root is linked.

The client encrypts content and the file manifest before upload using AES-256-GCM. A random vault key is wrapped separately by a password-derived key and a recovery key. Password derivation uses scrypt. The server holds encrypted objects and encrypted key wrappers, not the plaintext vault key or password. It still sees operational metadata such as account/vault/device identifiers, object sizes, versions and request timing. This preview has not had an independent security audit.

Engine credentials, API keys, sync account tokens, local path mappings, queues, automatic execution state and permission grants are excluded. Secrets already inside conversation text or attachments remain inside that encrypted history. Your ordinary local vault remains readable plaintext on your device.

## Working across devices

On a receiving device, register an empty local vault, connect the remote vault and map its working roots to local folders. A missing root mapping keeps the conversation read only. Root mappings do not copy your source repository or working files.

Claude CLI conversations in linked roots are included while tiller runs. The conversation menu can copy a terminal-resume command after preparing the native Claude transcript. Run that command yourself with the official CLI and your own engine login. Open app conversations and recently modified terminal transcripts defer incoming changes to protect active work. This does not provide a headless sync agent when tiller is closed.

**Codex conversations received from another device are read only.** History viewing and search are supported; native cross-device Codex resume is not. Recovered history is also read only, regardless of its original engine.

## Disconnect, delete, move and recover

| Action | Result |
|---|---|
| Disconnect | Stops this vault's connection and preserves both local files and the remote copy |
| Revoke a device | Invalidates that registration; files already saved on that device remain, and it must enroll again to reconnect |
| Delete a synced conversation | Sends deletion to the connected vault and its other devices; offline devices catch up later |
| Move a working root to another vault | Creates new conversation identities in the destination and deletes the source identities, including on the source vault's other devices; external project files stay in place |
| Restore a recovery copy | Creates new read-only history without native engine resume IDs; the deleted original stays deleted and the backup remains |
| Discard a recovery copy | Permanently removes that selected local recovery backup |

Deletion wins over stale offline edits. Conflicting local content can be retained as a recovery candidate rather than silently recreating a deleted thread. Review those candidates in the panel. Valid conversation-history snapshots can be restored; other files can be inspected in the backup folder. Recovery copies are not automatically uploaded or restored. Restoring history does not restore arbitrary attachments or permission grants.

Deletion is not instant erasure of every copy: disconnected devices, independent backups and external engine history may remain. Remote unreferenced objects are collected separately. Disconnecting or signing out is not a remote-vault deletion request.

## Encrypted folder transport

The panel also has an experimental encrypted-folder option for local testing. Use a separate destination on **one filesystem**, outside the plaintext vault and device-state directories. Multiple isolated clients can exercise the same destination locally.

**Dropbox, OneDrive and other asynchronously replicated folders are not supported transports.** Their conflict-copy and replication behavior does not provide the serialization this transport requires. Passing local tests does not establish safety on a network filesystem or on two physical machines.

For the ordinary local folder layout and current storage paths, see [Your personal vault](vault.md).
