# Theme Overview

Themes in DevOpsMaestro are `NvimTheme` YAML resources that define a Neovim colorscheme configuration. They specify the colorscheme plugin to use, style variants, color overrides, and plugin-specific options.

---

## What Themes Are

A theme defines:

- **Plugin** - The Neovim colorscheme plugin to install (e.g., `folke/tokyonight.nvim`)
- **Style** - A variant name for plugins that support multiple styles (e.g., `night`, `mocha`)
- **Colors** - Semantic color overrides for backgrounds, foregrounds, UI elements, and syntax
- **Options** - Plugin-specific configuration passed to the setup call
- **Category** - Whether the theme is `dark`, `light`, or `both`

Standalone themes have no plugin - they apply highlight groups directly via Lua without a colorscheme plugin.

---

## Library vs User Themes

| Type | Location | Editable | Priority |
|------|----------|----------|----------|
| **Library** | Embedded in binary | No (read-only) | Lower |
| **User** | `~/.nvp/themes/` | Yes | Higher |

Library themes are available automatically. User themes override library themes with the same name.

---

## Installing Themes

### Library themes (automatic)

All library themes are available immediately with no installation step:

```bash
nvp theme use coolnight-ocean
nvp theme use catppuccin-mocha
nvp theme use tokyonight-night
```

### Install to user store

Copy a library theme to `~/.nvp/themes/` to create an editable local copy:

```bash
nvp theme library install catppuccin-mocha
```

### From a YAML file

Apply any `NvimTheme` YAML file:

```bash
nvp apply -f my-theme.yaml
nvp apply -f https://example.com/theme.yaml
nvp apply -f github:user/repo/my-theme.yaml
```

### From the parametric generator

Create a custom CoolNight variant:

```bash
nvp theme create --from "280" --name my-purple
```

---

## Using Themes

### Set the active theme

```bash
nvp theme use coolnight-ocean
```

### View the active theme

```bash
nvp theme get
```

### Generate Lua files

After setting a theme, generate the Neovim configuration files:

```bash
nvp generate
```

---

## Theme YAML Format

All themes use `apiVersion: devopsmaestro.io/v1` with `kind: NvimTheme`:

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: my-custom-theme
  description: My personal colorscheme
  author: Your Name
  category: dark
spec:
  plugin:
    repo: folke/tokyonight.nvim   # GitHub repository
    branch: main                  # optional
    tag: v4.0.0                   # optional
  style: night                    # Plugin variant name
  transparent: false
  colors:
    bg: "#1a1b26"
    fg: "#c0caf5"
    accent: "#7aa2f7"
    error: "#f7768e"
    warning: "#e0af68"
  options:
    dim_inactive: true
    styles:
      comments: italic
      keywords: bold
```

When `spec.plugin.repo` is empty, the theme is standalone and applies highlight groups directly without a colorscheme plugin.

---

## Generated Files

Running `nvp generate` creates Lua files in `~/.config/nvim/lua/`:

```
~/.config/nvim/lua/
  theme/
    palette.lua       # Color palette (all colors, semantic aliases)
    init.lua          # Theme helpers (lualine, bufferline, telescope)
  plugins/nvp/
    colorscheme.lua   # lazy.nvim plugin spec
```

**palette.lua** - Exports all theme colors and semantic aliases. Reference it from other Neovim config files:

```lua
local palette = require("theme").palette
vim.api.nvim_set_hl(0, "MyHighlight", {
  fg = palette.colors.accent,
  bg = palette.colors.bg,
})
```

**init.lua** - Provides helpers for lualine, bufferline, and telescope theming.

**colorscheme.lua** - A lazy.nvim plugin spec that installs and configures the colorscheme plugin. For standalone themes, it references a local colorscheme file instead.

---

## Switching Themes

```bash
# Switch to a different theme
nvp theme use gruvbox-dark

# Regenerate Lua files
nvp generate

# Restart Neovim to apply
```

---

## Next Steps

- [Theme Library](library.md) - Browse all 34+ embedded themes
- [CoolNight Collection](coolnight.md) - The 21 parametric CoolNight variants
- [Parametric Generator](parametric.md) - Create custom variants
- [NvimTheme Reference](../reference/nvim-theme.md) - Full YAML field reference
