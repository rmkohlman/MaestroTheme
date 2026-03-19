# Theme Library

The embedded library contains 34+ themes compiled directly into the `nvp` binary. All library themes are available immediately without any installation step.

---

## Overview

| Category | Count | Description |
|----------|-------|-------------|
| CoolNight Collection | 21 | Parametrically generated standalone themes |
| Plugin-backed themes | 13+ | Popular colorscheme plugins (TokyoNight, Catppuccin, etc.) |

Library themes are read-only. To customize a library theme, install it to your user store first:

```bash
nvp theme library install catppuccin-mocha
# Now edit ~/.nvp/themes/catppuccin-mocha.yaml
```

---

## CoolNight Collection

21 parametrically generated variants built on the CoolNight Ocean base theme. All CoolNight themes are standalone (no external plugin required).

| Theme | Hue | Family | Description |
|-------|-----|--------|-------------|
| `coolnight-ocean` | 210° | Blue | Deep blue, the default and base theme |
| `coolnight-arctic` | 200° | Blue | Cool ice-like tones |
| `coolnight-midnight` | 230° | Blue | Deep midnight blue for intense focus |
| `coolnight-violet` | 280° | Purple | Soft violet tones |
| `coolnight-synthwave` | 270° | Purple | Neon purple inspired by 80s aesthetics |
| `coolnight-grape` | 290° | Purple | Rich grape purple |
| `coolnight-forest` | 140° | Green | Deep forest green |
| `coolnight-matrix` | 150° | Green | Digital green, high contrast |
| `coolnight-mint` | 165° | Green | Fresh mint green |
| `coolnight-ember` | 15° | Warm | Glowing ember red-orange |
| `coolnight-sunset` | 25° | Warm | Warm orange sunset tones |
| `coolnight-gold` | 45° | Warm | Rich golden yellow |
| `coolnight-crimson` | 0° | Red | Deep crimson red |
| `coolnight-sakura` | 330° | Pink | Soft cherry blossom pink |
| `coolnight-rose` | 350° | Pink | Elegant rose pink |
| `coolnight-mono-charcoal` | - | Mono | Pure grayscale (zero saturation) |
| `coolnight-mono-slate` | - | Mono | Cool gray with subtle blue tint |
| `coolnight-mono-warm` | - | Mono | Warm gray with subtle brown tint |
| `coolnight-nord` | 210° | Special | Nord-inspired blue-gray palette |
| `coolnight-dracula` | 260° | Special | Dracula-inspired purple tones |
| `coolnight-solarized` | 195° | Special | Solarized-inspired blue-cyan base |

See the [CoolNight Collection](coolnight.md) page for detailed descriptions, usage recommendations, and the complete parametric generator reference.

---

## Popular Themes

Plugin-backed themes that require a Neovim colorscheme plugin. The plugin is installed automatically by the generated lazy.nvim spec.

| Theme | Plugin | Style |
|-------|--------|-------|
| `tokyonight-night` | `folke/tokyonight.nvim` | night |
| `tokyonight-ocean` | `folke/tokyonight.nvim` | night |
| `tokyonight-custom` | `folke/tokyonight.nvim` | night |
| `catppuccin-mocha` | `catppuccin/nvim` | mocha |
| `catppuccin-latte` | `catppuccin/nvim` | latte |
| `gruvbox-dark` | `ellisonleao/gruvbox.nvim` | hard |
| `nord` | `shaunsingh/nord.nvim` | - |
| `dracula` | `Mofiqul/dracula.nvim` | - |
| `kanagawa` | `rebelot/kanagawa.nvim` | wave |
| `rose-pine` | `rose-pine/neovim` | - |
| `onedark` | `navarasu/onedark.nvim` | - |
| `everforest` | `sainnhe/everforest` | dark |
| `solarized-dark` | `ishan9299/nvim-solarized-lua` | - |

---

## Browsing the Library

```bash
# List all library themes
nvp theme library list

# Filter by category
nvp theme library list --category dark

# Show full details for a theme
nvp theme library show catppuccin-mocha

# List available categories
nvp theme library categories
```

---

## Installing from the Library

Installing copies a theme from the embedded library to your user theme store at `~/.nvp/themes/`. This lets you edit or customize the theme.

```bash
# Install a theme
nvp theme library install catppuccin-mocha

# Install and set as active immediately
nvp theme library install catppuccin-mocha --use
```

!!! note "Installation is optional"
    You do not need to install a library theme to use it. `nvp theme use catppuccin-mocha` works directly against the embedded library.

---

## Override Behavior

User themes (in `~/.nvp/themes/`) take precedence over library themes with the same name. This lets you customize any library theme:

1. Install the library theme: `nvp theme library install tokyonight-night`
2. Edit `~/.nvp/themes/tokyonight-night.yaml`
3. Your modified version is used instead of the library version

To revert to the library version, delete the user theme:

```bash
nvp theme delete tokyonight-night
```

---

## Next Steps

- [CoolNight Collection](coolnight.md) - All 21 CoolNight variants in detail
- [Supported Plugins](../configuration/supported-plugins.md) - Plugin setup details
- [Parametric Generator](parametric.md) - Create custom variants
