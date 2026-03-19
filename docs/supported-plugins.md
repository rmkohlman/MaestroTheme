# Supported Plugins

The `SupportedThemePlugins` variable in the root `theme` package maps Neovim plugin repository paths to their Lua setup function names.

```go
var SupportedThemePlugins = map[string]string{
    "folke/tokyonight.nvim":       "tokyonight",
    "catppuccin/nvim":             "catppuccin",
    "ellisonleao/gruvbox.nvim":    "gruvbox",
    "shaunsingh/nord.nvim":        "nord",
    "rebelot/kanagawa.nvim":       "kanagawa",
    "rose-pine/neovim":            "rose-pine",
    "EdenEast/nightfox.nvim":      "nightfox",
    "navarasu/onedark.nvim":       "onedark",
    "Mofiqul/dracula.nvim":        "dracula",
    "sainnhe/everforest":          "everforest",
    "sainnhe/sonokai":             "sonokai",
    "projekt0n/github-nvim-theme": "github-theme",
}
```

## Plugin Table

| Repository | Setup Name | Generator Behavior |
|-----------|-----------|-------------------|
| `folke/tokyonight.nvim` | `tokyonight` | Calls `require("tokyonight").setup({...})` with `on_colors` callback |
| `catppuccin/nvim` | `catppuccin` | Calls `require("catppuccin").setup({...})` with `flavour` and `color_overrides` |
| `ellisonleao/gruvbox.nvim` | `gruvbox` | Calls `require("gruvbox").setup({...})` with `contrast` and `palette_overrides` |
| `shaunsingh/nord.nvim` | `nord` | Sets `vim.g.nord_*` globals, calls `require("nord").set()` |
| `rebelot/kanagawa.nvim` | `kanagawa` | Calls `require("kanagawa").setup({...})` with `theme` key for style |
| `rose-pine/neovim` | `rose-pine` | Calls `require("rose-pine").setup({...})` with `variant` and `disable_background` |
| `EdenEast/nightfox.nvim` | `nightfox` | Calls `require("nightfox").setup({...})` with options table |
| `navarasu/onedark.nvim` | `onedark` | Calls `require("onedark").setup({...})` then `require("onedark").load()` |
| `Mofiqul/dracula.nvim` | `dracula` | Sets `vim.g.dracula_*` globals |
| `sainnhe/everforest` | `everforest` | Generic setup via `pcall` |
| `sainnhe/sonokai` | `sonokai` | Generic setup via `pcall` |
| `projekt0n/github-nvim-theme` | `github-theme` | Generic setup via `pcall` |

## GetSetupName

```go
func GetSetupName(repo string) string
```

Returns the setup name for a plugin repository. Returns an empty string if the repository is not in `SupportedThemePlugins`.

```go
name := theme.GetSetupName("folke/tokyonight.nvim") // "tokyonight"
name := theme.GetSetupName("unknown/plugin.nvim")   // ""
```

## Generic Fallback

When a plugin repository is not in `SupportedThemePlugins`, the generator uses a generic fallback. It derives the module name from the repo path by:

1. Taking the part after the `/`.
2. Stripping `.nvim` or `-nvim` suffixes.
3. Replacing `-` with `_`.

The generated code uses `pcall` to safely attempt setup:

```lua
local ok, theme = pcall(require, "module_name")
if ok and theme.setup then
  theme.setup({
    transparent = false,
    -- style and colors if present
  })
end
```

## Notes on Style Mapping

Different plugins use different option names for theme variants. The generator maps the `style` field accordingly:

| Plugin | Option Name | Example Value |
|--------|------------|---------------|
| tokyonight | `style` | `night`, `moon`, `storm`, `day` |
| catppuccin | `flavour` | `mocha`, `macchiato`, `frappe`, `latte` |
| gruvbox | `contrast` | `hard`, `medium`, `soft` |
| kanagawa | `theme` | `wave`, `dragon`, `lotus` |
| rose-pine | `variant` | `main`, `moon`, `dawn` |
| onedark | `style` | `dark`, `darker`, `cool`, `deep`, `warm`, `warmer` |
| Generic | `style` | passed through as-is |
