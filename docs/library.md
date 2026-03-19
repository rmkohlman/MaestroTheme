# library package

Import path: `github.com/rmkohlman/MaestroTheme/library`

The `library` package provides an embedded collection of pre-defined themes. All YAML files are compiled into the binary using Go's `embed.FS`, so no external files are required at runtime.

## Overview

The library contains 34 themes in two categories:

- **13 plugin-backed themes**: Require an external Neovim colorscheme plugin.
- **1 standalone theme**: The CoolNight Ocean base theme, which applies highlights directly without a plugin.
- **20 parametric CoolNight variants**: Generated via HSL hue rotation from CoolNight Ocean. These are also standalone.

## Types

### ThemeInfo

`ThemeInfo` contains metadata about a library theme without loading the full `Theme` struct.

```go
type ThemeInfo struct {
    Name        string
    Description string
    Author      string
    Category    string
    Plugin      string // Plugin.Repo value, empty for standalone themes
}
```

## Functions

### List

```go
func List() ([]ThemeInfo, error)
```

Returns metadata for all themes in the library. Themes that fail to parse are silently skipped. The order follows the filesystem order of the embedded directory.

### Get

```go
func Get(name string) (*theme.Theme, error)
```

Retrieves a fully parsed `*theme.Theme` by name. Tries an exact filename match first (`themes/<name>.yaml`). If not found, tries the following variants in order:

1. `name` (exact)
2. `name` with `-dark` suffix removed
3. `name` with `-light` suffix removed
4. `name + "-dark"`
5. `name + "-mocha"`
6. `name + "-night"`

Returns an error if no variant is found.

### GetRaw

```go
func GetRaw(name string) ([]byte, error)
```

Returns the raw YAML bytes for a theme by exact name. Does not attempt variant matching.

### Has

```go
func Has(name string) bool
```

Returns `true` if `Get(name)` succeeds.

### Categories

```go
func Categories() ([]string, error)
```

Returns all unique non-empty category strings present in the library.

### ListByCategory

```go
func ListByCategory(category string) ([]ThemeInfo, error)
```

Returns themes filtered to a specific category string.

## Plugin-Backed Themes

These themes require the listed Neovim plugin to be installed.

| Theme Name | Plugin | Style |
|-----------|--------|-------|
| `catppuccin-latte` | `catppuccin/nvim` | latte |
| `catppuccin-mocha` | `catppuccin/nvim` | mocha |
| `dracula` | `Mofiqul/dracula.nvim` | (none) |
| `everforest` | `sainnhe/everforest` | dark |
| `gruvbox-dark` | `ellisonleao/gruvbox.nvim` | hard |
| `kanagawa` | `rebelot/kanagawa.nvim` | wave |
| `nord` | `shaunsingh/nord.nvim` | (none) |
| `onedark` | `navarasu/onedark.nvim` | (none) |
| `rose-pine` | `rose-pine/neovim` | (none) |
| `solarized-dark` | `ishan9299/nvim-solarized-lua` | (none) |
| `tokyonight-custom` | `folke/tokyonight.nvim` | night |
| `tokyonight-night` | `folke/tokyonight.nvim` | night |
| `tokyonight-ocean` | `folke/tokyonight.nvim` | night |

Note: `solarized-dark` uses `ishan9299/nvim-solarized-lua`, which is not in the `SupportedThemePlugins` map. The generator will use generic setup code for this theme.

## CoolNight Standalone Themes

These themes are standalone (no external plugin). The base theme and all 20 parametric variants are included.

| Theme Name | Description |
|-----------|-------------|
| `coolnight-ocean` | The original deep blue theme. Base for all parametric generation. |
| `coolnight-arctic` | Cooler blue with ice-like tones |
| `coolnight-midnight` | Deep midnight blue |
| `coolnight-synthwave` | Neon purple inspired by 80s aesthetics |
| `coolnight-violet` | Soft violet tones |
| `coolnight-grape` | Rich grape purple |
| `coolnight-matrix` | Digital green |
| `coolnight-forest` | Deep forest green |
| `coolnight-mint` | Fresh mint green |
| `coolnight-sunset` | Warm orange sunset tones |
| `coolnight-ember` | Glowing ember red-orange |
| `coolnight-gold` | Rich golden yellow |
| `coolnight-rose` | Elegant rose pink |
| `coolnight-crimson` | Deep crimson red |
| `coolnight-sakura` | Soft cherry blossom pink |
| `coolnight-mono-charcoal` | Pure grayscale (zero saturation) |
| `coolnight-mono-slate` | Cool gray with subtle blue tint (low saturation) |
| `coolnight-mono-warm` | Warm gray with subtle brown tint (low saturation) |
| `coolnight-nord` | Nord-inspired blue-gray palette |
| `coolnight-dracula` | Dracula-inspired purple tones |
| `coolnight-solarized` | Solarized-inspired blue-cyan base |

## Loading Themes

```go
import "github.com/rmkohlman/MaestroTheme/library"

// Get a single theme
t, err := library.Get("tokyonight-night")
if err != nil {
    log.Fatal(err)
}

// List all themes
all, err := library.List()
for _, info := range all {
    fmt.Printf("%s (%s) - %s\n", info.Name, info.Category, info.Plugin)
}

// Check existence
if library.Has("catppuccin-mocha") {
    t, _ := library.Get("catppuccin-mocha")
    // use t
}

// Filter by category
darkThemes, err := library.ListByCategory("dark")
```
