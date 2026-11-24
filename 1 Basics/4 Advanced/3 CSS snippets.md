A **CSS snippet** is a small `.css` file that changes how Obsidian looks. You do not need a full theme: one snippet can change a single thing, such as the color of headings.

Snippets live in the vault, in the folder `.obsidian/snippets/`. Which snippets are turned on is saved in `appearance.json` (see [[2 app.json and workspace.json]]).

## The easy way: change CSS variables

Obsidian already has named settings for its colors and sizes, called **CSS variables**. For example, `--h1-color` is the color of `#` headings. You don't need to find anything in the HTML. You just pick a variable from the list and give it a new value inside `body { ... }`:

```css
body {
    --h1-color: #e67e22;    /* # Heading 1  -> orange */
    --h2-color: #2980b9;    /* ## Heading 2 -> blue   */
    --link-color: #27ae60;  /* [[links]]    -> green  */
    --bold-color: #c0392b;  /* **bold**     -> red    */
}
```

Variables work everywhere at once: in Reading view, Live Preview, and Source mode.

Some useful variables to start with:

| Variable | What it changes |
|---|---|
| `--h1-color` ... `--h6-color` | Color of each heading level |
| `--h1-size` ... `--h6-size` | Size of each heading level |
| `--link-color` | Color of internal links |
| `--bold-color` | Color of **bold** text |
| `--italic-color` | Color of *italic* text |

The full list is in the official docs:

- [CSS variables](https://docs.obsidian.md/Reference/CSS+variables/CSS+variables) (all variables, grouped by topic)
- [Headings](https://docs.obsidian.md/Reference/CSS+variables/Editor/Headings) (all heading variables)
- [CSS snippets](https://help.obsidian.md/snippets) (Obsidian Help page about snippets)

## Step 1: Open the snippets folder

1. Open **Settings → Appearance**.
2. Scroll down to **CSS snippets**.
3. Click the **folder icon**. This opens `.obsidian/snippets/` in Finder (Mac) or File Explorer (Windows). If the folder does not exist yet, Obsidian creates it.

## Step 2: Create a .css file

Create a new text file in that folder and name it with the `.css` extension, for example `tutorial-headings.css`. Paste the `body { ... }` example from above into it and save.

This file already exists in this tutorial vault, so you can use it right away.

## Step 3: Turn the snippet on

1. Go back to **Settings → Appearance → CSS snippets**.
2. Click the **refresh icon** (the circular arrow) so Obsidian finds the new file.
3. Turn on the toggle next to `tutorial-headings`.

Now open any note with headings, links, and bold text. They show the new colors.

When you turn the toggle on, Obsidian writes the file name (without `.css`) to `appearance.json`:

```json
{
  "enabledCssSnippets": ["tutorial-headings"]
}
```

Turn the toggle off and the name is removed. The `.css` file itself stays in the folder.

## Step 4: Edit and see changes live

Keep the snippet turned on and change `#e67e22` to another color, such as `#8e44ad` (purple), then save the file. Obsidian reloads the snippet automatically, so the heading color changes immediately.

## No CSS at all: the Style Settings plugin

If you don't want to write CSS, install the community plugin **Style Settings** (see the Community Plugin chapter for how to install plugins). It adds a settings page with color pickers and sliders.

Keep in mind that Style Settings only shows options that a **theme or snippet provides**. With the default theme and no special snippets, the page is almost empty. It is most useful with themes that support it, such as Minimal.

## More examples in this vault

This vault already has other snippets in `.obsidian/snippets/`. Open them to see more real examples:

- `hide-link-tabs.css` hides the Backlinks and Outgoing links tabs in the sidebar.
- `edit-mode-color.css` adds a blue bar on the left in editing mode, so you can tell editing mode and Reading view apart.
- `graph-focused-color.css` changes node colors in the Graph view.

These snippets use **selectors** (class names) instead of variables. The next section shows how to find them.

## Git

Commit `.obsidian/snippets/` together with `appearance.json`. Then anyone who clones the vault gets the same snippets, already turned on.

## Advanced (optional): find class names with the Developer Tools

Variables cover the common cases. If you want to change something that has no variable, such as hiding a sidebar tab, you need its **class name**. Every part of the Obsidian window is HTML, and the Developer Tools show it.

### Open the Developer Tools

| OS | Shortcut |
|---|---|
| Mac | **Cmd+Option+I** |
| Windows / Linux | **Ctrl+Shift+I** |

> **Common mistake on Mac:** **Cmd+Shift+I** does *not* work. On Mac you must press **Option**, not Shift.

If the shortcut does nothing, use the Mac menu bar: **View → Toggle Developer Tools**.

### Find the class name of an element

1. Click the **element picker** (the arrow-in-a-square icon at the top left of the Developer Tools).
2. Click the part of the window you want to change, for example a sidebar tab.
3. The Elements panel shows its HTML, for example `<div class="workspace-tab-header" data-type="backlink">`.
4. Use it as a selector in your snippet:

```css
.workspace-tab-header[data-type="backlink"] {
    display: none !important;
}
```

This is exactly how `hide-link-tabs.css` was written.
