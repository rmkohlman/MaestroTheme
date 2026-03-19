# MaestroTheme

Theme management for the DevOpsMaestro ecosystem.

MaestroTheme is a standalone Go module providing Neovim colorscheme types, YAML parsing, filesystem storage, Lua code generation, and a built-in library of pre-defined themes. It also includes a parametric theme generator that creates new themes via HSL hue rotation from a base color.

## Installation

```sh
go get github.com/rmkohlman/MaestroTheme
```

## Quick Start

### Parse a theme from YAML

```go
import theme "github.com/rmkohlman/MaestroTheme"

data := []byte(`
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: my-theme
  category: dark
spec:
  plugin:
    repo: "folke/tokyonight.nvim"
  style: night
`)

t, err := theme.ParseYAML(data)
if err != nil {
    log.Fatal(err)
}
fmt.Println(t.Name) // my-theme
```

### Use the embedded library

```go
import "github.com/rmkohlman/MaestroTheme/library"

t, err := library.Get("tokyonight-night")
if err != nil {
    log.Fatal(err)
}
fmt.Println(t.Plugin.Repo) // folke/tokyonight.nvim

themes, err := library.List()
// returns all 34 embedded themes
```

### Generate a parametric theme

```go
import "github.com/rmkohlman/MaestroTheme/parametric"

// Generate from a preset
t, err := parametric.GeneratePreset("coolnight-synthwave")

// Or generate from any hue (0-360)
gen := parametric.NewGenerator()
t := gen.GenerateFromHue(270.0, "my-purple", "Custom purple theme")
```

## Packages

| Package | Import Path | Purpose |
|---------|-------------|---------|
| `theme` | `github.com/rmkohlman/MaestroTheme` | Core types, YAML parsing, store, Lua generation |
| `library` | `github.com/rmkohlman/MaestroTheme/library` | Embedded theme library (34 themes) |
| `parametric` | `github.com/rmkohlman/MaestroTheme/parametric` | HSL-based parametric theme generator |

## Documentation

For comprehensive documentation, see the [MaestroTheme Documentation](https://rmkohlman.github.io/MaestroTheme/).

## Ecosystem

MaestroTheme is part of the [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) toolkit.

## License

GPL-3.0 with commercial dual-license. See [LICENSE](LICENSE) for details.
