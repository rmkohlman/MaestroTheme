# Quick Start

Get up and running with theme management in a few commands.

---

## Browse Available Themes

All 34+ embedded themes are available immediately without any installation step:

```bash
nvp theme list
```

The output shows both library themes (embedded in the binary) and any user themes saved in `~/.nvp/themes/`.

---

## Use a Theme

Set any theme as active:

```bash
nvp theme use coolnight-ocean
```

This sets the active theme. Run `nvp generate` to write Lua files for Neovim.

---

## Preview a Theme

See a theme's color palette in the terminal before switching:

```bash
nvp theme preview coolnight-ocean
nvp theme preview catppuccin-mocha
```

---

## Create a Custom CoolNight Variant

The parametric generator creates new CoolNight themes from any hue angle (0-360), hex color, or preset name:

```bash
# From a hue angle
nvp theme create --from "280" --name my-purple --use

# From a hex color
nvp theme create --from "#2aa198" --name my-teal --use

# From a preset name
nvp theme create --from "synthwave" --name my-synth --use
```

The `--use` flag sets the new theme as active immediately after creation.

---

## Install from the Embedded Library

Browse and install themes from the library into your user theme store (`~/.nvp/themes/`). This creates a local copy you can edit:

```bash
# Browse library themes
nvp theme library list
nvp theme library list --category dark

# Show details before installing
nvp theme library show catppuccin-mocha

# Install and activate
nvp theme library install catppuccin-mocha --use
```

---

## Generate Lua Files for Neovim

After setting a theme, generate the Lua configuration files:

```bash
nvp generate
```

This writes theme files to `~/.config/nvim/lua/`:

```
~/.config/nvim/lua/
  theme/
    palette.lua       # Color palette module
    init.lua          # Theme setup and helpers
  plugins/nvp/
    colorscheme.lua   # lazy.nvim plugin spec
```

Restart Neovim to apply the new theme.

---

## Apply from YAML

Define a theme as a YAML resource and apply it directly:

```bash
nvp apply -f theme.yaml
```

Example `theme.yaml`:

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: my-custom-theme
  category: dark
spec:
  plugin:
    repo: folke/tokyonight.nvim
  style: night
  colors:
    bg: "#1a1b26"
    fg: "#c0caf5"
```

---

## Next Steps

- [Theme Overview](../themes/overview.md) - Understand the theme system
- [Theme Library](../themes/library.md) - Explore all 34+ embedded themes
- [CoolNight Collection](../themes/coolnight.md) - Learn about the 21 CoolNight variants
- [Parametric Generator](../themes/parametric.md) - Create custom variants
- [Commands Reference](../commands.md) - Full command reference
