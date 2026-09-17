# spicetify-tui

A [Spicetify](https://spicetify.app) theme that makes the Spotify desktop client look like [spotify-tui](https://github.com/Rigellute/spotify-tui): bordered panes with labels, monospace typography, and ASCII playback controls.

Forked from [AvinashReddy3108/spicetify-tui](https://github.com/AvinashReddy3108/spicetify-tui), with fixes for current Spotify/Spicetify releases (e.g. hiding the leading search icon).

## Features

- **TUI layout** — every pane gets a labeled border (`Pages`, `Library`, `Main`, `Playing`), plus a `Modal` label on dialogs
- **Monospace everything** — JetBrains Mono applied across the whole client
- **ASCII player controls** — shuffle, skip, play/pause, and repeat rendered as text glyphs instead of icons
- **Distraction-free toggles** — show or hide card, cover-art, header, library, and tracklist images via CSS variables
- **15 color schemes** — Spotify, Spicetify, Catppuccin (Mocha/Macchiato/Latte), Dracula, Gruvbox, Kanagawa, Nord, Rigel, Rose Pine (base/Moon), Solarized, Tokyo Night (base/Storm)
- **Two files only** — `tui/color.ini` (palettes) + `tui/user.css` (layout), no JS or assets to maintain

## Color schemes

Each `[Section]` in `tui/color.ini` is a selectable scheme (`main` = background, `accent` = highlight):

| Scheme | `main` | `accent` |
| --- | --- | --- |
| Spotify | `121212` | `1db954` |
| Spicetify | `2E2837` | `00e089` |
| CatppuccinMocha | `1e1e2e` | `a6e3a1` |
| CatppuccinMacchiato | `24273a` | `a6da95` |
| CatppuccinLatte | `303446` | `a6d189` |
| Dracula | `282a36` | `50fa7b` |
| Gruvbox | `282828` | `98971a` |
| Kanagawa | `1F1F28` | `76946A` |
| Nord | `2e3440` | `88c0d0` |
| Rigel | `002635` | `00cccc` |
| RosePine | `191724` | `ebbcba` |
| RosePineMoon | `232136` | `ea9a97` |
| Solarized | `002b36` | `859900` |
| TokyoNight | `1a1b26` | `9ece6a` |
| TokyoNightStorm | `24283b` | `9ece6a` |

## Prerequisites

- Spotify Desktop installed from the official installer (not the Microsoft Store / Snap version)
- [Spicetify CLI v2](https://spicetify.app/docs/getting-started) set up (`spicetify backup apply` has run at least once)

## Installation

1. Copy the `tui` folder into your Spicetify Themes directory:

   | Platform | Path |
   | --- | --- |
   | macOS / Linux | `~/.config/spicetify/Themes/` |
   | Windows | `%appdata%\spicetify\Themes\` |

   The result should be `<Themes>/tui/color.ini` and `<Themes>/tui/user.css`.

2. Apply the theme (example with Gruvbox):

   ```sh
   spicetify config current_theme tui color_scheme Gruvbox
   spicetify backup apply
   ```

3. Fully quit Spotify (Cmd/Ctrl+Q, not just closing the window) and reopen it.

> [!NOTE]
> After a Spotify client update, always run `spicetify restore backup apply` instead of plain `spicetify apply` so Spicetify re-patches the new client files before injecting the theme.

## Usage

Switch schemes without touching the theme:

```sh
spicetify config color_scheme Nord
spicetify apply
```

Available names are the exact `[Section]` headers from `tui/color.ini` — the match is case-sensitive (`Gruvbox`, not `gruvbox`).

Revert to stock Spotify:

```sh
spicetify config current_theme ""
spicetify apply
```

## Customization

All toggles live in the `:root` block at the top of `tui/user.css`. Edit, then run `spicetify apply` and restart Spotify.

| Variable | Default | Effect |
| --- | --- | --- |
| `--font-family` | `"JetBrains Mono", monospace` | Typeface for the whole client |
| `--display-card-image` | `block` | Card artwork on Home shelves |
| `--display-coverart-image` | `none` | Mini cover art in the now-playing bar |
| `--display-header-image` | `none` | Playlist/profile header images |
| `--display-library-image` | `block` | Sidebar library artwork |
| `--display-tracklist-image` | `none` | Track-row thumbnails |
| `--border-radius` | `0px` | Corner rounding (keep `0px` for the TUI look) |
| `--border-width` | `2px` | Pane border thickness |
| `--border-style` | `solid` | Any CSS border style (`dashed`, `double`, …) |

For the full TUI look, set every `--display-*-image` to `none`.

## Troubleshooting

### `spicetify apply` warns `Color scheme 'X' not found`

The scheme is looked up in the **active theme's** `color.ini`, not globally. This happens when `current_theme` points at the wrong folder (e.g. `marketplace`, whose `color.ini` only defines `[Marketplace]`). Fix:

```sh
spicetify config current_theme tui color_scheme Gruvbox
spicetify apply
```

### Theme applies with no visible change

1. Confirm the folder name matches: `spicetify config` must show `current_theme = tui`.
2. Confirm both `color.ini` and `user.css` exist in `Themes/tui/`.
3. Re-run `spicetify restore backup apply` and fully restart Spotify.

### `error: File name "xpui.js" is not found`

> [!WARNING]
> Do **not** add the legacy `[Patch]` snippet (`xpui.js_find_8008`) recommended by older guides. Recent Spotify clients split `xpui.js` into many chunk files, so that patch can never match and every apply ends with an error. This fork does not need it — a clean `config-xpui.ini` has an empty `[Patch]` section.

## Acknowledgements

- [AvinashReddy3108/spicetify-tui](https://github.com/AvinashReddy3108/spicetify-tui) — the original theme this fork is based on
- [@darkthemer's text theme](https://github.com/darkthemer) — initial inspiration for the upstream project
- [spotify-tui](https://github.com/Rigellute/spotify-tui) — the terminal UI this theme mimics
- [Spicetify](https://spicetify.app/docs/customization/themes) — theming docs (install, schemes, troubleshooting)
