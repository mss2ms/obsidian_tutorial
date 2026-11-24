
둘 다 vault의 `.obsidian/` 폴더 안에 있는데, 하나는 **설정**이고 하나는 **화면 상태**입니다.

- **`app.json`**은 Settings에서 바꾼 옵션을 저장합니다. 앞에서 말씀드린 첨부파일 폴더(`attachmentFolderPath: "pic"`)나 Source mode 같은 값이 여기에 들어갑니다. 직접 바꾸지 않으면 그대로 유지됩니다.
- **`workspace.json`**은 지금 열려 있는 탭, 창 분할 배치, 사이드바 상태, 최근 연 파일 목록을 저장합니다. 창을 움직일 때마다 자동으로 계속 바뀝니다.

그래서 Git으로 vault를 관리할 때는 보통 `app.json`은 커밋하고 `workspace.json`은 `.gitignore`에 넣습니다.

## appearance.json

`appearance.json` stores the options from **Settings → Appearance**: the light/dark mode, the community theme, the font size, and which CSS snippets are turned on.

In this tutorial vault it is almost empty, because we changed only one option (showing the left ribbon):

```json
{
  "showRibbon": true
}
```

After a student picks a theme, changes the font size, and turns on a snippet, it looks more like this:

```json
{
  "theme": "obsidian",
  "cssTheme": "Minimal",
  "baseFontSize": 18,
  "showRibbon": true,
  "enabledCssSnippets": ["wide-tables"]
}
```

| Key | Meaning | Where to change it |
|---|---|---|
| `theme` | `"obsidian"` = dark, `"moonstone"` = light, `"system"` = follow the OS | Appearance → Base color scheme |
| `cssTheme` | Name of the community theme (empty = default theme) | Appearance → Themes |
| `baseFontSize` | Editor font size in pixels | Appearance → Font size |
| `showRibbon` | Show or hide the left icon bar | Appearance → Show ribbon |
| `enabledCssSnippets` | CSS files in `.obsidian/snippets/` that are turned on | Appearance → CSS snippets |

Only the options you changed are written. If a key is missing, Obsidian uses the default value.

To learn how to write and turn on your own snippets, see [[3 CSS snippets]].

## core-plugins.json

`core-plugins.json` stores which **core plugins** (the plugins built into Obsidian) are turned on or off. It matches the toggles in **Settings → Core plugins**.

Here is part of the file from this tutorial vault:

```json
{
  "file-explorer": true,
  "global-search": true,
  "graph": true,
  "backlink": true,
  "daily-notes": true,
  "templates": true,
  "command-palette": true,
  "slash-command": true,
  "outline": true,
  "word-count": true,
  "footnotes": false,
  "random-note": false,
  "slides": false,
  "publish": false
}
```

- `true` means the plugin is on, `false` means it is off.
- For example, `"slash-command": true` is why typing `/` in a note opens the command menu. Turn it off in Settings and the value becomes `false`.

> **Note:** Older versions of Obsidian used a simple list that contained only the enabled plugins:
>
> ```json
> ["file-explorer", "global-search", "graph", "slash-command"]
> ```
>
> Newer versions use the `"id": true/false` object shown above, so you can also see which plugins are turned off.

The settings *inside* each core plugin are saved in a separate file named after the plugin, for example `daily-notes.json` or `templates.json`. Community plugins are listed in a different file, `community-plugins.json`.

## Summary: which files to commit to Git

| File | What it stores | Commit to Git? |
|---|---|---|
| `app.json` | Settings options (e.g. attachment folder) | Yes |
| `appearance.json` | Theme, font size, CSS snippets | Yes |
| `core-plugins.json` | Which core plugins are on/off | Yes |
| `workspace.json` | Open tabs, layout, recent files | No, add to `.gitignore` |

The first three change only when you change a setting, so they are safe to share. `workspace.json` changes every time you move a window, so committing it causes constant changes and merge conflicts.
