# Commands Reference

Complete reference for `nvp theme` commands and related commands used in theme management.

---

## `nvp theme list`

List all available themes, including both library themes and user themes.

```bash
nvp theme list [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--library` | Show library themes only |
| `--user` | Show user themes only |
| `-o, --output <format>` | Output format: `table` (default), `json`, `yaml` |

**Examples:**

```bash
nvp theme list
nvp theme list --library
nvp theme list --user
nvp theme list -o json
```

---

## `nvp theme get`

Show details of a theme. If no name is provided, shows the active theme.

```bash
nvp theme get [name] [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `-o, --output <format>` | Output format: `table` (default), `json`, `yaml` |

**Examples:**

```bash
# Show the active theme
nvp theme get

# Show a specific theme
nvp theme get coolnight-ocean

# Show as YAML
nvp theme get coolnight-ocean -o yaml

# Show as JSON
nvp theme get catppuccin-mocha -o json
```

---

## `nvp theme use`

Set a theme as the active theme.

```bash
nvp theme use <name>
```

**Examples:**

```bash
nvp theme use coolnight-ocean
nvp theme use catppuccin-mocha
nvp theme use tokyonight-night
```

After setting a theme, run `nvp generate` to write Lua files for Neovim.

---

## `nvp theme apply`

Apply a theme from a file, URL, or GitHub shorthand.

```bash
nvp apply -f <source>
```

**Source types:**

| Type | Example |
|------|---------|
| Local file | `my-theme.yaml` |
| URL | `https://example.com/theme.yaml` |
| GitHub shorthand | `github:user/repo/theme.yaml` |
| Stdin | `-` |

**Examples:**

```bash
nvp apply -f my-theme.yaml
nvp apply -f https://example.com/theme.yaml
nvp apply -f github:rmkohlman/my-themes/coolnight-custom.yaml
cat theme.yaml | nvp apply -f -
```

---

## `nvp theme delete`

Delete a user theme. Library themes cannot be deleted.

```bash
nvp theme delete <name> [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--force` | Skip confirmation prompt |

**Examples:**

```bash
nvp theme delete my-custom-theme
nvp theme delete my-custom-theme --force
```

!!! note
    Deleting a user theme with the same name as a library theme restores the library version.

---

## `nvp theme create`

Create a custom CoolNight theme variant using the parametric generator.

```bash
nvp theme create --from <value> --name <name> [flags]
```

The `--from` value accepts:

- A hue angle in degrees: `"210"`
- A hex color: `"#8B00FF"`
- A named CoolNight preset: `"synthwave"`, `"ocean"`, `"forest"`, etc.

**Flags:**

| Flag | Required | Description |
|------|----------|-------------|
| `--from <value>` | Yes | Base color: hue (0-360), hex (#rrggbb), or preset name |
| `--name <name>` | Yes | Name for the new theme |
| `--use` | No | Set the new theme as active after creation |
| `--dry-run` | No | Preview the generated theme without saving |

**Examples:**

```bash
# From a hue angle
nvp theme create --from "280" --name my-purple

# From a hex color
nvp theme create --from "#8B00FF" --name my-violet

# From a preset name
nvp theme create --from "synthwave" --name my-synth

# Create and activate immediately
nvp theme create --from "120" --name my-green --use

# Preview without saving
nvp theme create --from "45" --name preview-gold --dry-run
```

---

## `nvp theme preview`

Preview a theme's color palette in the terminal.

```bash
nvp theme preview <name> [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--ansi` | Show ANSI terminal color blocks |
| `--colors` | Show named color table |

**Examples:**

```bash
nvp theme preview coolnight-ocean
nvp theme preview catppuccin-mocha --colors
nvp theme preview coolnight-synthwave --ansi
```

---

## `nvp theme generate`

Generate Lua files for the active theme only.

```bash
nvp theme generate [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `-o, --output <dir>` | Output directory (default: `~/.config/nvim/lua`) |

**Examples:**

```bash
nvp theme generate
nvp theme generate --output ~/my-nvim/lua
```

!!! tip
    `nvp generate` generates Lua files for both plugins and the active theme. Use `nvp theme generate` to regenerate theme files only.

---

## `nvp theme library list`

List themes available in the embedded library.

```bash
nvp theme library list [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--category <cat>` | Filter by category (`dark`, `light`, `both`) |
| `-o, --output <format>` | Output format: `table` (default), `json`, `yaml` |

**Examples:**

```bash
nvp theme library list
nvp theme library list --category dark
nvp theme library list -o json
```

---

## `nvp theme library show`

Show full details for a library theme.

```bash
nvp theme library show <name>
```

**Examples:**

```bash
nvp theme library show catppuccin-mocha
nvp theme library show coolnight-synthwave
```

---

## `nvp theme library install`

Install one or more library themes to the user theme store (`~/.nvp/themes/`).

```bash
nvp theme library install <name> [name...] [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `--use` | Set the installed theme as active (applies to last named theme) |

**Examples:**

```bash
# Install a single theme
nvp theme library install catppuccin-mocha

# Install and activate
nvp theme library install catppuccin-mocha --use

# Install multiple themes
nvp theme library install catppuccin-mocha tokyonight-night gruvbox-dark
```

---

## `nvp theme library categories`

List available theme categories in the embedded library.

```bash
nvp theme library categories
```

---

## `nvp generate`

Generate Lua configuration files for installed plugins and the active theme.

```bash
nvp generate [flags]
```

**Flags:**

| Flag | Description |
|------|-------------|
| `-o, --output <dir>` | Output directory (default: `~/.config/nvim/lua`) |

**Examples:**

```bash
nvp generate
nvp generate --output ~/dotfiles/nvim/lua
```

Generated output structure:

```
<output-dir>/
  theme/
    palette.lua
    init.lua
  plugins/nvp/
    colorscheme.lua
    ...other plugin specs...
```

---

## Common Workflows

### Browse and use a library theme

```bash
nvp theme library list
nvp theme library show catppuccin-mocha
nvp theme use catppuccin-mocha
nvp generate
```

### Create a custom CoolNight variant

```bash
nvp theme create --from "280" --name my-purple --use
nvp generate
```

### Switch themes

```bash
nvp theme use gruvbox-dark
nvp generate
# Restart Neovim
```

### Install a theme from a file and activate

```bash
nvp apply -f my-theme.yaml
nvp theme use my-custom-theme
nvp generate
```

### Inspect the active theme

```bash
nvp theme get
nvp theme get -o yaml
```

### Preview before switching

```bash
nvp theme preview coolnight-synthwave
nvp theme preview coolnight-ocean
nvp theme use coolnight-ocean
nvp generate
```
