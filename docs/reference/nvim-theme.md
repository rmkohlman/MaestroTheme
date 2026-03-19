# NvimTheme YAML Reference

**Kind:** `NvimTheme`
**APIVersion:** `devopsmaestro.io/v1`

An `NvimTheme` resource defines a Neovim colorscheme configuration. Themes are applied via `nvp theme use`, shared via `nvp apply`, and generate Lua files via `nvp generate`.

---

## Full Annotated Example

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: my-custom-theme           # Required. Unique theme identifier.
  description: My personal theme  # Optional. Human-readable description.
  author: Your Name               # Optional. Theme author.
  category: dark                  # Optional. dark | light | both
spec:
  plugin:
    repo: folke/tokyonight.nvim   # GitHub repository. Empty = standalone theme.
    branch: main                  # Optional. Git branch to pin.
    tag: v4.0.0                   # Optional. Git tag to pin.
  style: night                    # Optional. Plugin variant name.
  transparent: false              # Optional. Enable transparent background.
  colors:                         # Optional. Semantic color overrides.
    bg: "#1a1b26"
    fg: "#c0caf5"
    accent: "#7aa2f7"
    error: "#f7768e"
    warning: "#e0af68"
    info: "#7dcfff"
    hint: "#1abc9c"
    comment: "#565f89"
    selection: "#33467c"
    border: "#29a4bd"
  promptColors:                   # Optional. Starship prompt segment overrides.
    directory: "#7aa2f7"
    git_branch: "#bb9af7"
  options:                        # Optional. Plugin-specific setup options.
    dim_inactive: true
    styles:
      comments: italic
      keywords: bold
```

---

## Field Reference

### metadata

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `metadata.name` | string | Yes | Unique identifier for the theme. Used in `nvp theme use <name>`. |
| `metadata.description` | string | No | Human-readable description. |
| `metadata.author` | string | No | Theme author name. |
| `metadata.category` | string | No | Theme category: `dark`, `light`, or `both`. |

### spec

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `spec.plugin.repo` | string | No | GitHub repository path (e.g., `folke/tokyonight.nvim`). Empty for standalone themes. |
| `spec.plugin.branch` | string | No | Git branch to use when installing the plugin. |
| `spec.plugin.tag` | string | No | Git tag to pin the plugin to a specific version. |
| `spec.style` | string | No | Variant name passed to the plugin's setup function. |
| `spec.transparent` | boolean | No | Enable transparent background. Default: `false`. |
| `spec.colors` | map | No | Semantic color overrides as hex strings. |
| `spec.promptColors` | map | No | Starship prompt segment color overrides. |
| `spec.options` | map | No | Plugin-specific options passed to the setup call. |

---

## Field Details

### metadata.name

The unique identifier for the theme. Used in all `nvp theme` commands.

Naming conventions:

- Use kebab-case: `my-custom-theme`
- Include the collection or plugin prefix: `coolnight-ocean`, `tokyonight-night`
- User theme names override library themes with the same name

### metadata.category

Controls how the theme is categorized in listings.

| Value | Meaning |
|-------|---------|
| `dark` | Dark background theme |
| `light` | Light background theme |
| `both` | Theme supports both modes |

### spec.plugin.repo

The GitHub repository path for the colorscheme plugin. If empty, the theme is standalone - it applies highlight groups directly without an external plugin.

```yaml
# Plugin-backed theme
spec:
  plugin:
    repo: folke/tokyonight.nvim

# Standalone theme (no external plugin)
spec:
  plugin:
    repo: ""
  colors:
    bg: "#011628"
    fg: "#CBE0F0"
    # ... all colors required for standalone
```

All 21 CoolNight variants are standalone themes.

### spec.style

The variant name for plugins that support multiple styles. The field name in the generated Lua varies by plugin:

| Plugin | Style values | Lua mapping |
|--------|-------------|-------------|
| `folke/tokyonight.nvim` | `night`, `moon`, `storm`, `day` | `style = "..."` |
| `catppuccin/nvim` | `mocha`, `macchiato`, `frappe`, `latte` | `flavour = "..."` |
| `ellisonleao/gruvbox.nvim` | `hard`, `medium`, `soft` | `contrast = "..."` |
| `rebelot/kanagawa.nvim` | `wave`, `dragon`, `lotus` | `theme = "..."` |
| `rose-pine/neovim` | `main`, `moon`, `dawn` | `variant = "..."` |
| `navarasu/onedark.nvim` | `dark`, `darker`, `cool`, `deep`, `warm`, `warmer` | `style = "..."` |

### spec.colors

A map of semantic color names to hex color values. Colors defined here are passed as overrides to the plugin's setup call (e.g., `on_colors` for tokyonight, `color_overrides` for catppuccin).

For standalone themes, `spec.colors` defines the complete palette and all values must be present.

```yaml
spec:
  colors:
    # Background colors
    bg: "#1a1b26"              # Main background
    bg_dark: "#16161e"         # Darker panels
    bg_highlight: "#292e42"    # Selection/highlight background
    bg_search: "#3d59a1"       # Search match background
    bg_visual: "#283457"       # Visual selection background

    # Foreground colors
    fg: "#c0caf5"              # Main text
    fg_dark: "#a9b1d6"         # Dimmed text
    fg_gutter: "#3b4261"       # Line numbers, gutter

    # UI
    border: "#29a4bd"
    selection: "#33467c"
    comment: "#565f89"

    # Syntax
    accent: "#7aa2f7"
    keyword: "#bb9af7"
    string: "#9ece6a"
    function: "#7aa2f7"
    type: "#2ac3de"
    constant: "#ff9e64"

    # Status
    error: "#f7768e"
    warning: "#e0af68"
    info: "#7dcfff"
    hint: "#1abc9c"

    # Terminal ANSI
    ansi_black: "#15161e"
    ansi_red: "#f7768e"
    ansi_green: "#9ece6a"
    ansi_yellow: "#e0af68"
    ansi_blue: "#7aa2f7"
    ansi_magenta: "#bb9af7"
    ansi_cyan: "#7dcfff"
    ansi_white: "#a9b1d6"
    cursor: "#c0caf5"
```

Color values must be valid hex strings in `#rgb` or `#rrggbb` format. Alpha channel (`#rrggbbaa`) is also accepted.

### spec.promptColors

Optional map of Starship prompt segment names to hex colors. Used to generate terminal prompt color configuration that matches the theme.

```yaml
spec:
  promptColors:
    directory: "#7aa2f7"
    git_branch: "#bb9af7"
    time: "#e0af68"
```

### spec.options

Plugin-specific options passed through to the generated Lua setup call. The structure depends on the plugin.

```yaml
spec:
  options:
    # tokyonight-specific
    dim_inactive: true
    styles:
      comments: italic
      keywords: bold
      functions: {}
      variables: {}
    sidebars:
      - "qf"
      - "vista_kind"
      - "terminal"

    # catppuccin-specific
    integrations:
      telescope: true
      nvim_tree: true
      gitsigns: true
      lualine: true
```

---

## Theme Collections Examples

### CoolNight variant (standalone)

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: coolnight-ocean
  description: Deep blue - the original CoolNight base theme
  author: devopsmaestro
  category: dark
spec:
  plugin:
    repo: ""
  colors:
    bg: "#011628"
    bg_dark: "#011423"
    bg_highlight: "#143652"
    bg_search: "#0A64AC"
    bg_visual: "#275378"
    fg: "#CBE0F0"
    fg_dark: "#B4D0E9"
    fg_gutter: "#627E97"
    border: "#547998"
    ansi_black: "#214969"
    ansi_red: "#E52E2E"
    ansi_green: "#44FFB1"
    ansi_yellow: "#FFE073"
    ansi_blue: "#0FC5ED"
    ansi_magenta: "#a277ff"
    ansi_cyan: "#24EAF7"
    ansi_white: "#24EAF7"
    cursor: "#47FF9C"
```

### Plugin-backed theme

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: tokyonight-night
  description: Tokyo Night - dark blue colorscheme
  author: folke
  category: dark
spec:
  plugin:
    repo: folke/tokyonight.nvim
  style: night
```

### Theme with full color overrides

```yaml
apiVersion: devopsmaestro.io/v1
kind: NvimTheme
metadata:
  name: catppuccin-custom
  category: dark
spec:
  plugin:
    repo: catppuccin/nvim
  style: mocha
  transparent: true
  colors:
    bg: "#1e1e2e"
    fg: "#cdd6f4"
    accent: "#89b4fa"
  options:
    integrations:
      telescope: true
      gitsigns: true
```

---

## Color Guidelines

### Use semantic names

Color keys should describe purpose, not appearance:

```yaml
colors:
  accent: "#7aa2f7"     # Good: describes role
  error: "#f7768e"      # Good: describes role
  blue: "#7aa2f7"       # Avoid: describes color, not role
  red: "#f7768e"        # Avoid: describes color, not role
```

### Contrast

Ensure sufficient contrast between foreground and background:

- Main text (`fg`) against background (`bg`) should meet WCAG AA (4.5:1 ratio)
- Comment text may use a lower ratio but must remain legible
- Error and warning colors should stand out from both foreground and background

### Hex format

All color values must be valid hex strings:

```yaml
colors:
  bg: "#1a1b26"    # Valid: #rrggbb
  fg: "#c0caf5"    # Valid: #rrggbb
  bg: "#1AB"       # Valid: #rgb (expands to #11AABB)
  bg: "1a1b26"     # Invalid: missing # prefix
  bg: "rgb(26,27,38)"  # Invalid: not hex
```

---

## Validation Rules

- `metadata.name` is required and must be non-empty
- `metadata.name` should use kebab-case
- `metadata.category` must be `dark`, `light`, or `both` when provided
- `spec.plugin.repo` must be a valid GitHub repository path when provided (`owner/repo`)
- All values in `spec.colors` must be valid hex color strings
- Standalone themes (empty `spec.plugin.repo`) must define at least one color in `spec.colors`
- Theme names must be unique in the user theme store

---

## Related Resources

- [Theme Overview](../themes/overview.md) - How themes work
- [Supported Plugins](../configuration/supported-plugins.md) - Plugin-specific setup behavior
- [Commands Reference](../commands.md) - `nvp apply`, `nvp theme use`, `nvp generate`
- [Theme Hierarchy](../configuration/theme-hierarchy.md) - Applying themes via dvm
