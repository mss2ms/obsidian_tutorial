
[Quartz](https://quartz.jzhao.xyz/) is a free tool that turns an Obsidian-style folder of Markdown into a website. It understands `[[wikilinks]]`, backlinks, tags, and shows a graph view and a file explorer, so the site feels like Obsidian.

## Requirements

- Node.js **v22 or newer** (check with `node -v`)
- npm **v10.9.2 or newer** (check with `npm -v`)
- git

## Install and run locally

```bash
git clone https://github.com/jackyzha0/quartz.git
cd quartz
npm i
npx quartz create          # answers a few questions, sets up content/ and quartz.config.yaml
npx quartz build --serve   # build the site and start a local server
```

Open `http://localhost:8080` in your browser. You now have your own wiki running on your computer.

## What is in the folder

```
quartz/
├── content/              <- your notes go here (index.md is the home page)
├── public/               <- the built website (created by "build")
├── quartz.config.yaml    <- site title, baseUrl, plugins, layout
└── quartz/               <- Quartz source code (do not edit at first)
```

- Put your `.md` files in `content/`. The note `content/index.md` becomes the home page.
- `public/` is generated. Never edit it by hand; rebuild instead.

## Configure the site

Edit `quartz.config.yaml`:

- `pageTitle`: the name shown at the top of the site.
- `baseUrl`: the address where the site will live, without `https://`. Example: `student.github.io/my-wiki`. RSS and the sitemap need this to be correct.
- Layout: in Quartz v5 there is no separate layout file. Each plugin sets its own `layout`:
  - `position`: `left`, `right`, ... (which column)
  - `priority`: order inside that column. **A lower number is shown higher.**

### Example: Explorer and Graph on the left (Obsidian-like)

```yaml
plugins:
  - source: "@quartz-community/explorer"
    enabled: true
    layout:
      position: left
      priority: 50
  - source: "@quartz-community/graph"
    enabled: true
    layout:
      position: left   # default: right
      priority: 60     # default: 10
```

Resulting left column, top to bottom (priority in brackets):

```
page-title (10)
search (20)
darkmode (30)
reader-mode (35)
explorer (50)
graph (60)        <- moved here from the right
```

The right column keeps table-of-contents (30) and backlinks (50).

Try it: change the graph's `priority` to `15` and rebuild. The graph now appears right under the page title.

After any change, run `npx quartz build --serve` again (or keep it running; it rebuilds when files change).

## Writing in Obsidian, previewing in Quartz

The simplest setup: open the `content/` folder itself as your Obsidian vault. Then every note you write is already where Quartz looks for it.

If you want to keep your vault somewhere else, copy the notes into `content/` and let a small script watch for changes:

```bash
# sync-and-serve.sh : copy vault notes into Quartz, then serve
VAULT="$HOME/Documents/MyVault"
rsync -a --exclude ".obsidian" "$VAULT/" content/
npx quartz build --serve
```

Why copy instead of a symbolic link? `content/` must contain **real files** so that git can commit and push them to GitHub. A symlink would only push the link, not your notes.

## Next step

Choose where to host the `public/` folder: [[0 What is publishing]].
