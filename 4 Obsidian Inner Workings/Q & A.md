## Q1. Must a vault id (like `a1b2c3d4e5f60718`) be created by Obsidian, or can I make one myself?

**Short answer:** You can make one yourself. Obsidian does not check how the id was made.

A vault id is just a **unique name tag** for a vault inside `obsidian.json`. Obsidian creates a random 16-character hex string, but there is no special meaning or validation. The only rule is that it must be **unique** among the vaults in `obsidian.json`.

Example: registering a vault by hand with a made-up id.

```json
{
  "vaults": {
    "a1b2c3d4e5f60718": {
      "path": "/Users/student/github/obsidian_tutorial",
      "ts": 1764002922000,
      "open": true
    },
    "0000000000000001": {
      "path": "/Users/student/Documents/My Test Vault",
      "ts": 1764003000000
    }
  }
}
```

After restarting Obsidian, `My Test Vault` appears in the vault switcher.

**Cautions**

1. **Quit Obsidian completely first.** Obsidian rewrites `obsidian.json` when it exits, so edits made while it is running can be overwritten.
2. **Changing an existing id has side effects.** The per-vault window file `<id>.json` no longer matches, so window size and position reset, and `obsidian://` links that use the id stop working. The vault's `.obsidian/` settings are **not** affected, because they live inside the vault folder, not under the id.
3. **One JSON syntax error can hide every vault.** A missing comma or quote can make the vault list appear empty. Make a backup copy of `obsidian.json` before editing.

**Safest way:** use **Open folder as vault** and let Obsidian generate the id for you. Edit by hand only to learn how it works.

**Key idea:** the id is a label that connects *id → folder path*. The settings belong to the folder; the id only tells Obsidian where the folder is.
