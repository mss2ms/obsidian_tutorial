# Many sites with Caddy

When you have several Obsidian vaults (a course wiki, a lab wiki, a personal notes site), you can serve all of them from **one server** with **one Caddy container**. Each site gets its own subdomain and its own HTTPS certificate, automatically.

```
obsidian.mss2ms.com ─┐
lab.mss2ms.com      ─┼─► server (76.13.25.16) ─► Caddy (Docker) ─► /srv/sites/<name>/
notes.mss2ms.com    ─┘
```

## Why Caddy instead of nginx

| | nginx | Caddy |
|---|---|---|
| HTTPS | install certbot, run it, renew it | automatic (issue + renew) |
| Config for one site | ~15 lines | ~5 lines |
| http → https redirect | write it yourself | automatic |

## The idea: Git moves the files

Quartz output is just HTML, so we keep **only the built HTML** in a GitHub repo per site. The vault itself (with `.obsidian`, drafts, private notes) never leaves your computer.

```
Mac:    vault ─(quartz build)─► <name>_site repo ─(git push)─► GitHub
Server: GitHub ─(git pull)─► /srv/sites/<name> ─► Caddy
```

There are two kinds of repos:

1. **Site repos** (`mss2ms/obsidian_tutorial_site`, ...): the built HTML of one vault.
2. **Server repo** (`mss2ms/site_server`): the list of sites and the Caddy setup.

```json
// site_server/sites.json
{
  "sites": [
    { "name": "obsidian_tutorial", "repo": "mss2ms/obsidian_tutorial_site",
      "domains": ["obsidian.mss2ms.com"] }
  ]
}
```

A small script (`sync.py`) on the server reads `sites.json`, pulls every repo, writes the Caddyfile, and reloads Caddy. For each site it writes a block like this:

```
obsidian.mss2ms.com {
	root * /srv/sites/obsidian_tutorial
	try_files {path} {path}.html {path}/index.html
	file_server
}
```

`try_files` matters for Quartz: links look like `/3-setup/setup`, but the file is `setup.html`.

## Step 1. DNS (once)

In your DNS provider (here Cloudflare), add:

| Type | Name | Value | Proxy |
|---|---|---|---|
| A | `*` | server IP | DNS only |

The wildcard `*` means every new subdomain already points to the server. "DNS only" lets Caddy get the certificate directly.

## Step 2. Server (once)

On a fresh Ubuntu server, `bootstrap.sh` installs Docker, creates a read-only **deploy key** for each repo (you paste it into GitHub → repo → Settings → Deploy keys), clones everything, and starts Caddy.

## Step 3. Publish (every time)

On the Mac:

```bash
./deploy.sh --sync    # build all vaults, push, then update the server over ssh
```

Or in two steps: `./deploy.sh` (build and push), then on the server `python3 /srv/site_server/sync.py`.

## Adding another vault

```bash
./deploy.sh add-site lab --vault ~/github/mss2ms/lab_wiki --domain lab.mss2ms.com
```

It prints the remaining steps: create the site repo with `gitnew`, push the server repo, add the new deploy key, and sync.

## Exercise

1. Why do we push the built HTML instead of the Markdown vault?
2. What breaks if the Cloudflare record is "Proxied" (orange cloud) instead of "DNS only"?
3. Remove `try_files` from the Caddyfile. Which pages stop working, and why?
