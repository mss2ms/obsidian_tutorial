A **CSS snippet** is a small `.css` file that changes how Obsidian looks. You do not need a full theme: one snippet can change a single thing, such as the color of headings or hiding a sidebar tab.

Snippets live in the vault, in the folder `.obsidian/snippets/`. Which snippets are turned on is saved in `appearance.json` (see [[1 app.json and workspace.json]]).

## Step 1: Open the snippets folder

1. Open **Settings → Appearance**.
2. Scroll down to **CSS snippets**.
3. Click the **folder icon**. This opens `.obsidian/snippets/` in Finder (Mac) or File Explorer (Windows). If the folder does not exist yet, Obsidian creates it.

## Step 2: Create a .css file

Create a new text file in that folder and name it with the `.css` extension, for example `tutorial-headings.css`. Write this CSS in it:

```css
/* 1. Make level-1 headings orange (works in Reading view and Live Preview) */
body {
    --h1-color: #e67e22;
}

/* 2. Draw a gray line under level-2 headings (Reading view) */
.markdown-rendered h2 {
    border-bottom: 1px solid gray;
}
```

This file already exists in this tutorial vault, so you can use it right away.

## Step 3: Turn the snippet on

1. Go back to **Settings → Appearance → CSS snippets**.
2. Click the **refresh icon** (the circular arrow) so Obsidian finds the new file.
3. Turn on the toggle next to `tutorial-headings`.

Now open any note with `#` and `##` headings. The `#` heading is orange, and in Reading view the `##` heading has a line under it.

When you turn the toggle on, Obsidian writes the file name (without `.css`) to `appearance.json`:

```json
{
  "enabledCssSnippets": ["tutorial-headings"]
}
```

Turn the toggle off and the name is removed. The `.css` file itself stays in the folder.

## Step 4: Edit and see changes live

Keep the snippet turned on and change `#e67e22` to another color, such as `#2ecc71` (green), then save the file. Obsidian reloads the snippet automatically, so the heading color changes immediately.

## How to find what to style

Every part of the Obsidian window is HTML with class names. To find a class name:

1. Press **Ctrl+Shift+I** (Windows/Linux) or **Cmd+Option+I** (Mac) to open the Developer Tools.
2. Click the **element picker** (the arrow icon at the top left of the Developer Tools).
3. Click the part of the window you want to change.
4. Read its class name in the Elements panel, for example `workspace-tab-header` or `markdown-source-view`.
5. Use that class name as the selector in your snippet: `.workspace-tab-header { ... }`.

Useful selectors to start with:

| Selector | What it targets |
|---|---|
| `.markdown-rendered` | Note content in Reading view |
| `.markdown-source-view` | Note content in the editor (Live Preview or Source mode) |
| `.theme-dark` / `.theme-light` | Apply a rule only in dark or light mode |
| `body { --h1-color: ...; }` | Obsidian CSS variables, which work in every view |

> **Tip:** Obsidian uses CSS variables such as `--h1-color`, `--text-normal` and `--font-text-size`. Changing a variable is often easier than writing a selector, because it updates every view at once.

## More examples in this vault

This vault already has other snippets in `.obsidian/snippets/`. Open them to see more real examples:

- `hide-link-tabs.css` hides the Backlinks and Outgoing links tabs in the sidebar.
- `edit-mode-color.css` adds a blue bar on the left in editing mode, so you can tell editing mode and Reading view apart.
- `graph-focused-color.css` changes node colors in the Graph view.

## Git

Commit `.obsidian/snippets/` together with `appearance.json`. Then anyone who clones the vault gets the same snippets, already turned on.

![[CleanShot 2026-11-23 at 11.00.57 PM.png]]
![[CleanShot 2026-11-23 at 11.03.24 PM.png]]