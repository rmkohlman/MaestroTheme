# theme package

Import path: `github.com/rmkohlman/MaestroTheme`

The root `theme` package provides all core types, YAML parsing, filesystem storage, Lua code generation, and terminal environment variable integration.

## Types

### Theme

`Theme` represents a Neovim colorscheme configuration.

```go
type Theme struct {
    Name         string            `yaml:"name" json:"name"`
    Description  string            `yaml:"description,omitempty" json:"description,omitempty"`
    Author       string            `yaml:"author,omitempty" json:"author,omitempty"`
    Category     string            `yaml:"category,omitempty" json:"category,omitempty"`
    Plugin       ThemePlugin       `yaml:"plugin" json:"plugin"`
    Style        string            `yaml:"style,omitempty" json:"style,omitempty"`
    Transparent  bool              `yaml:"transparent,omitempty" json:"transparent,omitempty"`
    Colors       map[string]string `yaml:"colors,omitempty" json:"colors,omitempty"`
    PromptColors map[string]string `yaml:"promptColors,omitempty" json:"promptColors,omitempty"`
    Options      map[string]any    `yaml:"options,omitempty" json:"options,omitempty"`
}
```

`PromptColors` holds Starship segment color overrides. `Options` holds plugin-specific configuration passed through to the generated Lua setup call.

### ThemePlugin

`ThemePlugin` defines the colorscheme plugin to use.

```go
type ThemePlugin struct {
    Repo   string `yaml:"repo" json:"repo"`
    Branch string `yaml:"branch,omitempty" json:"branch,omitempty"`
    Tag    string `yaml:"tag,omitempty" json:"tag,omitempty"`
}
```

When `Repo` is empty, the theme is treated as standalone (no external plugin). Standalone themes apply highlights directly via `vim.api.nvim_set_hl()`.

### ThemeYAML

`ThemeYAML` represents the full YAML document structure.

```go
type ThemeYAML struct {
    APIVersion string        `yaml:"apiVersion"`
    Kind       string        `yaml:"kind"`
    Metadata   ThemeMetadata `yaml:"metadata"`
    Spec       ThemeSpec     `yaml:"spec"`
}
```

`ParseYAML` validates that `Kind == "NvimTheme"` and `Metadata.Name` is non-empty before constructing a `Theme`.

### ThemeMetadata

```go
type ThemeMetadata struct {
    Name        string `yaml:"name"`
    Description string `yaml:"description,omitempty"`
    Author      string `yaml:"author,omitempty"`
    Category    string `yaml:"category,omitempty"`
}
```

### ThemeSpec

```go
type ThemeSpec struct {
    Plugin       ThemePlugin       `yaml:"plugin"`
    Style        string            `yaml:"style,omitempty"`
    Transparent  bool              `yaml:"transparent,omitempty"`
    Colors       map[string]string `yaml:"colors,omitempty"`
    PromptColors map[string]string `yaml:"promptColors,omitempty"`
    Options      map[string]any    `yaml:"options,omitempty"`
}
```

## Color Constants

Color key constants re-exported from `MaestroPalette` for backward compatibility. New code should import `MaestroPalette` directly.

### Background Colors

| Constant | Value |
|----------|-------|
| `ColorBg` | `palette.ColorBg` |
| `ColorBgDark` | `palette.ColorBgDark` |
| `ColorBgHighlight` | `palette.ColorBgHighlight` |
| `ColorBgSearch` | `palette.ColorBgSearch` |
| `ColorBgVisual` | `palette.ColorBgVisual` |
| `ColorBgFloat` | `palette.ColorBgFloat` |
| `ColorBgPopup` | `palette.ColorBgPopup` |
| `ColorBgSidebar` | `palette.ColorBgSidebar` |
| `ColorBgStatusline` | `palette.ColorBgStatusline` |

### Foreground Colors

| Constant | Value |
|----------|-------|
| `ColorFg` | `palette.ColorFg` |
| `ColorFgDark` | `palette.ColorFgDark` |
| `ColorFgGutter` | `palette.ColorFgGutter` |
| `ColorFgSidebar` | `palette.ColorFgSidebar` |

### UI and Diagnostic Colors

| Constant | Value |
|----------|-------|
| `ColorBorder` | `palette.ColorBorder` |
| `ColorComment` | `palette.ColorComment` |
| `ColorError` | `palette.ColorError` |
| `ColorWarning` | `palette.ColorWarning` |
| `ColorInfo` | `palette.ColorInfo` |
| `ColorHint` | `palette.ColorHint` |

### Category Constants

| Constant | Value |
|----------|-------|
| `CategoryDark` | `string(palette.CategoryDark)` |
| `CategoryLight` | `string(palette.CategoryLight)` |
| `CategoryBoth` | `string(palette.CategoryBoth)` |

## SupportedThemePlugins

`SupportedThemePlugins` maps plugin repository paths to their Lua setup function names. See the [Supported Plugins](supported-plugins.md) page for the full table.

```go
var SupportedThemePlugins = map[string]string{ ... }
```

## Functions

### ParseYAML

```go
func ParseYAML(data []byte) (*Theme, error)
```

Parses a YAML byte slice into a `Theme`. Returns an error if the document `kind` is not `NvimTheme` or if `metadata.name` is empty. An empty `plugin.repo` is valid and results in a standalone theme.

### GetSetupName

```go
func GetSetupName(repo string) string
```

Returns the Lua setup function name for a plugin repository path. Returns an empty string if the repository is not in `SupportedThemePlugins`.

## Theme Methods

### ToPalette

```go
func (t *Theme) ToPalette() *palette.Palette
```

Converts the theme to a `palette.Palette`. Maps `Theme.Category` to the corresponding `palette.Category` constant and copies `Colors` and `PromptColors`. Returns `nil` if called on a nil receiver.

### ToTerminalColors

```go
func (t *Theme) ToTerminalColors() map[string]string
```

Convenience method. Calls `ToPalette().ToTerminalColors()` and returns the result.

### ToYAML

```go
func (t *Theme) ToYAML() ([]byte, error)
```

Serializes the theme to YAML bytes using the `NvimTheme` document format with `apiVersion: devopsmaestro.io/v1`.

### Validate

```go
func (t *Theme) Validate() error
```

Validates the theme configuration:

- `Name` must be non-empty.
- Standalone themes (empty `Plugin.Repo`) must define at least one color (`HasCustomColors()` must return true).
- All values in `Colors` must be valid hex color strings (`#RGB` or `#RRGGBB` format).

### IsStandalone

```go
func (t *Theme) IsStandalone() bool
```

Returns `true` if `Plugin.Repo` is empty. Standalone themes do not require an external colorscheme plugin.

### HasCustomColors

```go
func (t *Theme) HasCustomColors() bool
```

Returns `true` if `Colors` has at least one entry.

### HasOptions

```go
func (t *Theme) HasOptions() bool
```

Returns `true` if `Options` has at least one entry.

### GetColorschemeCommand

```go
func (t *Theme) GetColorschemeCommand() string
```

Returns the colorscheme name string used in `vim.cmd.colorscheme(...)`.

- For standalone themes, returns `t.Name`.
- For plugin themes, looks up the setup name from `SupportedThemePlugins`. If not found, derives a name from the repo path by stripping the `.nvim` or `-nvim` suffix.
- If `Style` is set, appends it as `setupName-style`.

### TerminalEnvVars

```go
func (t *Theme) TerminalEnvVars() map[string]string
```

Returns a map of environment variables for terminal color configuration. Keys use the `DVM_COLOR_` prefix. Always includes `DVM_THEME` (set to the theme name) and `DVM_THEME_CATEGORY`.

Example keys generated:

| Env Var | Source Key |
|---------|-----------|
| `DVM_COLOR_BG` | `bg` |
| `DVM_COLOR_FG` | `fg` |
| `DVM_COLOR_BLACK` | `ansi_black` |
| `DVM_COLOR_RED` | `ansi_red` |
| `DVM_COLOR_GREEN` | `ansi_green` |
| `DVM_COLOR_YELLOW` | `ansi_yellow` |
| `DVM_COLOR_BLUE` | `ansi_blue` |
| `DVM_COLOR_MAGENTA` | `ansi_magenta` |
| `DVM_COLOR_CYAN` | `ansi_cyan` |
| `DVM_COLOR_WHITE` | `ansi_white` |
| `DVM_COLOR_BRIGHT_BLACK` | `ansi_bright_black` |
| `DVM_COLOR_BRIGHT_RED` | `ansi_bright_red` |
| `DVM_COLOR_BRIGHT_GREEN` | `ansi_bright_green` |
| `DVM_COLOR_BRIGHT_YELLOW` | `ansi_bright_yellow` |
| `DVM_COLOR_BRIGHT_BLUE` | `ansi_bright_blue` |
| `DVM_COLOR_BRIGHT_MAGENTA` | `ansi_bright_magenta` |
| `DVM_COLOR_BRIGHT_CYAN` | `ansi_bright_cyan` |
| `DVM_COLOR_BRIGHT_WHITE` | `ansi_bright_white` |
| `DVM_COLOR_CURSOR` | `cursor` |
| `DVM_COLOR_CURSOR_TEXT` | `cursor_text` |
| `DVM_COLOR_SELECTION` | `selection` |
| `DVM_COLOR_SELECTION_TEXT` | `selection_text` |
| `DVM_THEME` | theme name metadata |
| `DVM_THEME_CATEGORY` | theme category metadata |

## GetTerminalEnvVarsForTheme

```go
func GetTerminalEnvVarsForTheme(store Store, themeName string) (map[string]string, error)
```

Loads a theme by name from the provided `Store` and returns its terminal environment variables. Returns an error if `store` is nil or if the theme is not found.

## Store Interface

```go
type Store interface {
    Get(name string) (*Theme, error)
    List() ([]*Theme, error)
    Save(theme *Theme) error
    Delete(name string) error
    GetActive() (*Theme, error)
    SetActive(name string) error
    Path() string
}
```

## FileStore

`FileStore` implements `Store` using the filesystem.

```go
type FileStore struct { /* unexported fields */ }
```

Directory structure created by `FileStore`:

```
basePath/
  themes/          # One YAML file per theme: <name>.yaml
  active-theme     # Plain text file containing the active theme name
```

### NewFileStore

```go
func NewFileStore(basePath string) *FileStore
```

Creates a new `FileStore`. Does not create directories; call `Init()` before use.

### FileStore Methods

| Method | Signature | Description |
|--------|-----------|-------------|
| `Init` | `() error` | Creates `themes/` directory with permissions `0755`. |
| `Path` | `() string` | Returns `basePath`. |
| `Get` | `(name string) (*Theme, error)` | Reads and parses `themes/<name>.yaml`. |
| `List` | `() ([]*Theme, error)` | Returns all valid `.yaml` files in `themes/`. Invalid files are silently skipped. |
| `Save` | `(theme *Theme) error` | Validates, serializes, and writes `themes/<name>.yaml`. Calls `Init()` automatically. |
| `Delete` | `(name string) error` | Removes `themes/<name>.yaml`. Clears `active-theme` if the deleted theme was active. |
| `GetActive` | `() (*Theme, error)` | Reads `active-theme` and returns the named theme. Returns `nil, nil` if no active theme is set. |
| `SetActive` | `(name string) error` | Verifies the theme exists, then writes its name to `active-theme`. |

## MemoryStore

`MemoryStore` implements `Store` using an in-memory map. Intended for testing.

```go
type MemoryStore struct { /* unexported fields */ }
```

### NewMemoryStore

```go
func NewMemoryStore() *MemoryStore
```

Creates a new empty `MemoryStore`.

`MemoryStore.Path()` always returns an empty string.

## Generator

`Generator` generates Lua code for themes.

```go
type Generator struct{}

func NewGenerator() *Generator
```

### GeneratedTheme

```go
type GeneratedTheme struct {
    PaletteLua     string // theme/palette.lua
    InitLua        string // theme/init.lua
    PluginLua      string // plugins/colorscheme.lua
    ColorschemeLua string // theme/colorscheme.lua (standalone themes only)
}
```

### Generate

```go
func (g *Generator) Generate(t *Theme) (*GeneratedTheme, error)
```

Generates all Lua files for a theme. Calls `Validate()` first. For standalone themes, also populates `ColorschemeLua`.

### Generated File Descriptions

**PaletteLua** (`theme/palette.lua`): Exports `M.name`, `M.style`, `M.transparent`, `M.colors` (all theme colors sorted alphabetically), `M.semantic` (derived semantic aliases), `M.get(name, fallback)`, and `M.is_transparent()`.

**InitLua** (`theme/init.lua`): Re-exports the palette as `M.palette`. Provides `M.setup()`, `M.highlight(group, opts)`, `M.lualine_theme()`, `M.bufferline_highlights()`, and `M.telescope_border()`.

**PluginLua** (`plugins/colorscheme.lua`): A lazy.nvim plugin spec. For plugin-backed themes, calls the plugin-specific setup function. For standalone themes, references the local `theme/colorscheme.lua` via `dir = vim.fn.stdpath("config") .. "/lua/theme"`.

**ColorschemeLua** (`theme/colorscheme.lua`, standalone only): A self-contained colorscheme that calls `hi clear`, `syntax reset`, sets `vim.g.colors_name`, then applies all highlight groups via `vim.api.nvim_set_hl()`. Highlight groups covered include UI groups (Normal, CursorLine, Visual, Search, StatusLine, TabLine, Pmenu, LineNr, SignColumn, VertSplit/WinSeparator), syntax groups (Comment, String, Number, Boolean, Constant, Identifier, Function, Statement, Keyword, Type, PreProc, Special, etc.), diagnostics (DiagnosticError/Warn/Info/Hint with undercurl variants), git signs (GitSignsAdd/Change/Delete), and Treesitter capture groups.

### Plugin-Specific Setup Generation

The generator produces tailored setup code for known plugins:

| Plugin | Setup Style |
|--------|------------|
| `tokyonight` | `require("tokyonight").setup({...})` with `on_colors` callback for color overrides |
| `catppuccin` | `require("catppuccin").setup({...})` using `flavour` for style and `color_overrides` |
| `gruvbox` | `require("gruvbox").setup({...})` using `contrast` for style and `palette_overrides` |
| `nord` | Sets `vim.g.nord_*` globals, calls `require("nord").set()` |
| `kanagawa` | `require("kanagawa").setup({...})` using `theme` for style |
| `rose-pine` | `require("rose-pine").setup({...})` using `variant` for style |
| `nightfox` | `require("nightfox").setup({...})` with options table |
| `onedark` | `require("onedark").setup({...})`, then `require("onedark").load()` |
| `dracula` | Sets `vim.g.dracula_*` globals |
| Generic | `pcall(require, moduleName)` with safe setup call |
