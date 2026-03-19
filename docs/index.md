# MaestroTheme

Theme management for the DevOpsMaestro ecosystem.

MaestroTheme (`github.com/rmkohlman/MaestroTheme`) is a standalone Go module providing Neovim colorscheme types, YAML parsing, filesystem storage, Lua code generation, and a built-in library of pre-defined themes. It also includes a parametric theme generator that creates new themes via HSL hue rotation from a reference color.

## What MaestroTheme Provides

| Package | Purpose |
|---------|---------|
| `theme` (root) | Core types (`Theme`, `ThemePlugin`, `ThemeYAML`), YAML parsing, `FileStore`, Lua code generation, terminal env var integration |
| `library` | Embedded library of 34 pre-defined themes (13 plugin-backed + 1 standalone + 20 CoolNight parametric variants) |
| `parametric` | HSL-based parametric theme generator with 21 presets organized by family |

## Installation

```sh
go get github.com/rmkohlman/MaestroTheme
```

## Architecture

MaestroTheme is structured as a single Go module with three packages:

```
github.com/rmkohlman/MaestroTheme        # Root: theme package
github.com/rmkohlman/MaestroTheme/library     # Embedded theme library
github.com/rmkohlman/MaestroTheme/parametric  # Parametric generator
```

### Theme YAML Format

All themes use the `NvimTheme` kind with `apiVersion: devopsmaestro.io/v1`:

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: my-theme
  description: A description
  author: author-name
  category: dark
spec:
  plugin:
    repo: "folke/tokyonight.nvim"
    branch: ""   # optional
    tag: ""      # optional
  style: night   # optional variant name
  transparent: false
  colors:
    bg: "#1a1b26"
    fg: "#c0caf5"
    # ... additional color keys
  promptColors:   # optional Starship segment color overrides
    key: "#hexval"
  options:        # optional plugin-specific options
    styles:
      sidebars: dark
```

When `plugin.repo` is empty, the theme is treated as standalone and applies highlights directly via `vim.api.nvim_set_hl()`.

### Dependency on MaestroPalette

MaestroTheme depends on `github.com/rmkohlman/MaestroPalette` for:

- Color constants re-exported as `ColorBg`, `ColorFg`, etc.
- Category constants: `CategoryDark`, `CategoryLight`, `CategoryBoth`
- The `palette.Palette` and `palette.HSL` types used in conversion and generation
- `palette.HexToHSL()` for the parametric generator

## Quick Start

### Parse a theme

```go
import theme "github.com/rmkohlman/MaestroTheme"

data, _ := os.ReadFile("my-theme.yaml")
t, err := theme.ParseYAML(data)
if err != nil {
    log.Fatal(err)
}
fmt.Println(t.Name, t.Plugin.Repo)
```

### Use the file store

```go
store := theme.NewFileStore("/home/user/.config/nvp/themes")
_ = store.Init()

themes, _ := store.List()
active, _ := store.GetActive()
_ = store.SetActive("tokyonight-night")
```

### Load from the embedded library

```go
import "github.com/rmkohlman/MaestroTheme/library"

t, err := library.Get("catppuccin-mocha")
all, err := library.List()
```

### Generate parametric themes

```go
import "github.com/rmkohlman/MaestroTheme/parametric"

t, err := parametric.GeneratePreset("coolnight-synthwave")

gen := parametric.NewGenerator()
t := gen.GenerateFromHue(270.0, "my-purple", "Custom purple theme")
```

## Part of DevOpsMaestro

MaestroTheme is part of the [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) ecosystem.
