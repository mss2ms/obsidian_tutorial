# What is publishing?

Your Obsidian vault is a folder of Markdown (`.md`) files. A web browser cannot show `.md` files as a website. **Publishing** means turning those notes into HTML pages and putting them on a web server, so anyone with the link can read your wiki.

```
Obsidian vault (.md)  ──build──►  public/ (.html, .css, .js)  ──copy──►  web server  ──►  https://...
   you write here               a static website               GitHub Pages,       readers
                                                                Docker + nginx, ...
```

Two separate jobs:

1. **Build**: a tool converts Markdown to a static website. In this course we use **Quartz**. See [[1 Quartz setup]].
2. **Host**: put the built `public/` folder somewhere a browser can reach it.

## Hosting options

| Option | Cost | Who can see it | Note |
| --- | --- | --- | --- |
| Obsidian Publish | Paid subscription | Public (or password) | One click from Obsidian, no Quartz needed |
| GitHub Pages | Free | Public | `https://<user>.github.io/<repo>/`, see [[2 GitHub Pages]] |
| Docker + nginx | Free (your own machine) | You decide (e.g. campus network only) | See [[3 Docker and nginx]] |
| Any web server | Varies | You decide | Copy `public/` with `rsync`, see [[4 Deploy to servers]] |

**Key idea:** Quartz always produces the same `public/` folder. Only the place you copy it to changes.

## Recommended student workflow

1. Open the professor's template repository on GitHub and click **Use this template** to make your own copy.
2. Clone your copy to your computer.
3. In Obsidian, **Open folder as vault** and choose the repo's `content/` folder.
4. Install the community plugin **Obsidian Git** and turn on auto commit and push.
5. Write notes. Each push triggers GitHub Actions, which rebuilds the site. Your wiki updates in a minute or two.

```
write note ─► Obsidian Git push ─► GitHub Actions builds ─► https://<user>.github.io/<repo>/
```
