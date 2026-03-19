# Supported Colorscheme Plugins

MaestroTheme includes tailored Lua generation for 12 popular colorscheme plugins. When `nvp generate` runs, the generator produces plugin-specific setup code for each supported plugin.

---

## Supported Plugins

| Plugin Repository | Setup Name | Style Field |
|-------------------|------------|-------------|
| `folke/tokyonight.nvim` | `tokyonight` | `style`: `night`, `moon`, `storm`, `day` |
| `catppuccin/nvim` | `catppuccin` | `style`: `mocha`, `macchiato`, `frappe`, `latte` |
| `ellisonleao/gruvbox.nvim` | `gruvbox` | `style`: `hard`, `medium`, `soft` |
| `shaunsingh/nord.nvim` | `nord` | (no style variants) |
| `rebelot/kanagawa.nvim` | `kanagawa` | `style`: `wave`, `dragon`, `lotus` |
| `rose-pine/neovim` | `rose-pine` | `style`: `main`, `moon`, `dawn` |
| `EdenEast/nightfox.nvim` | `nightfox` | (uses colorscheme name directly) |
| `navarasu/onedark.nvim` | `onedark` | `style`: `dark`, `darker`, `cool`, `deep`, `warm`, `warmer` |
| `Mofiqul/dracula.nvim` | `dracula` | (no style variants) |
| `sainnhe/everforest` | `everforest` | `style`: `dark`, `light` |
| `sainnhe/sonokai` | `sonokai` | `style`: `default`, `atlantis`, `andromeda`, `shusia`, `maia` |
| `projekt0n/github-nvim-theme` | `github-theme` | (uses colorscheme name) |

---

## How Plugins Are Configured

The generator produces tailored setup code for each supported plugin:

### tokyonight

Calls `require("tokyonight").setup({...})` with an `on_colors` callback to apply color overrides from `spec.colors`.

```yaml
spec:
  plugin:
    repo: folke/tokyonight.nvim
  style: night
  colors:
    bg: "#1a1b26"
    fg: "#c0caf5"
```

### catppuccin

Calls `require("catppuccin").setup({...})` using `flavour` for the style and `color_overrides` for color customizations.

```yaml
spec:
  plugin:
    repo: catppuccin/nvim
  style: mocha
```

### gruvbox

Calls `require("gruvbox").setup({...})` using `contrast` for the style and `palette_overrides` for colors.

```yaml
spec:
  plugin:
    repo: ellisonleao/gruvbox.nvim
  style: hard
```

### nord

Sets `vim.g.nord_*` global variables and calls `require("nord").set()`.

```yaml
spec:
  plugin:
    repo: shaunsingh/nord.nvim
```

### kanagawa

Calls `require("kanagawa").setup({...})` using the `theme` key for the style variant.

```yaml
spec:
  plugin:
    repo: rebelot/kanagawa.nvim
  style: wave
```

### rose-pine

Calls `require("rose-pine").setup({...})` using `variant` for the style and `disable_background` for transparency.

```yaml
spec:
  plugin:
    repo: rose-pine/neovim
  style: main
```

### onedark

Calls `require("onedark").setup({...})` followed by `require("onedark").load()`.

```yaml
spec:
  plugin:
    repo: navarasu/onedark.nvim
  style: dark
```

### dracula

Sets `vim.g.dracula_*` global variables.

```yaml
spec:
  plugin:
    repo: Mofiqul/dracula.nvim
```

### Generic fallback

Plugins not in the supported list use a generic `pcall`-based fallback. The module name is derived by taking the repository path after the `/`, stripping `.nvim` or `-nvim` suffixes, and replacing `-` with `_`.

```lua
local ok, plugin = pcall(require, "module_name")
if ok and plugin.setup then
  plugin.setup({
    transparent = false,
  })
end
```

---

## Style Mapping Reference

Different plugins use different option names for style variants. The generator maps the `spec.style` field to the correct option:

| Plugin | YAML `style` value | Generated option |
|--------|--------------------|-----------------|
| `tokyonight` | `night` | `style = "night"` |
| `catppuccin` | `mocha` | `flavour = "mocha"` |
| `gruvbox` | `hard` | `contrast = "hard"` |
| `kanagawa` | `wave` | `theme = "wave"` |
| `rose-pine` | `main` | `variant = "main"` |
| `onedark` | `dark` | `style = "dark"` |
| Generic | any | `style = "..."` (passed through) |

---

## Standalone Themes

When `spec.plugin.repo` is empty, the theme is standalone. No external colorscheme plugin is required. The generator produces:

- A `theme/colorscheme.lua` file that applies all highlight groups via `vim.api.nvim_set_hl()`
- A lazy.nvim spec that references the local `theme/colorscheme.lua` instead of a remote plugin

All 21 CoolNight variants are standalone themes.

---

## Next Steps

- [Theme Library](../themes/library.md) - Which library themes use which plugins
- [NvimTheme Reference](../reference/nvim-theme.md) - Full YAML field reference including `spec.plugin`
- [Commands Reference](../commands.md) - `nvp generate` and `nvp apply` commands
