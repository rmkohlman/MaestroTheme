# MaestroTheme - Neovim Theme System

MaestroTheme is the theme management engine for the [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) toolkit, powering all `nvp theme` commands. It provides 34+ embedded themes, the CoolNight parametric collection, and YAML-based theme configuration for Neovim.

## Key Features

- **34+ embedded themes** - Available immediately, no installation required
- **CoolNight Collection** - 21 parametrically generated variants covering every color family
- **Parametric generator** - Create custom CoolNight variants from a hue angle, hex color, or preset name
- **Theme hierarchy** - Themes cascade through dvm's Workspace -> App -> Domain -> Ecosystem hierarchy
- **YAML IaC** - Define, share, and apply themes as Infrastructure as Code
- **Lua generation** - Generates lazy.nvim-compatible Lua files for instant Neovim integration

## Quick Install

```bash
# Install DevOpsMaestro (includes nvp)
brew install rmkohlman/tap/devopsmaestro

# Or install NvimOps only (no container runtime needed)
brew install rmkohlman/tap/nvimops

# Verify
nvp version
```

## Quick Start

```bash
# List all available themes
nvp theme list

# Use a theme
nvp theme use coolnight-ocean

# Preview a theme in the terminal
nvp theme preview coolnight-synthwave

# Create a custom CoolNight variant (hue, hex, or preset)
nvp theme create --from "280" --name my-purple --use

# Install a theme from the embedded library
nvp theme library install catppuccin-mocha --use

# Generate Lua files for Neovim
nvp generate

# Apply a theme from a YAML file
nvp apply -f theme.yaml
```

## Documentation

Full documentation is available at [rmkohlman.github.io/MaestroTheme](https://rmkohlman.github.io/MaestroTheme/).

## Ecosystem

MaestroTheme is part of the [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) toolkit.

## License

GPL-3.0 with commercial dual-license. See [LICENSE](LICENSE) for details.
