# API Reference

Complete API reference for all packages in `github.com/rmkohlman/MaestroTheme`.

## Package: theme

Import: `github.com/rmkohlman/MaestroTheme`

### Types

#### Theme

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

#### ThemePlugin

```go
type ThemePlugin struct {
    Repo   string `yaml:"repo" json:"repo"`
    Branch string `yaml:"branch,omitempty" json:"branch,omitempty"`
    Tag    string `yaml:"tag,omitempty" json:"tag,omitempty"`
}
```

#### ThemeYAML

```go
type ThemeYAML struct {
    APIVersion string        `yaml:"apiVersion"`
    Kind       string        `yaml:"kind"`
    Metadata   ThemeMetadata `yaml:"metadata"`
    Spec       ThemeSpec     `yaml:"spec"`
}
```

#### ThemeMetadata

```go
type ThemeMetadata struct {
    Name        string `yaml:"name"`
    Description string `yaml:"description,omitempty"`
    Author      string `yaml:"author,omitempty"`
    Category    string `yaml:"category,omitempty"`
}
```

#### ThemeSpec

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

#### Store (interface)

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

#### FileStore

```go
type FileStore struct { /* unexported */ }

func NewFileStore(basePath string) *FileStore

func (s *FileStore) Init() error
func (s *FileStore) Path() string
func (s *FileStore) Get(name string) (*Theme, error)
func (s *FileStore) List() ([]*Theme, error)
func (s *FileStore) Save(theme *Theme) error
func (s *FileStore) Delete(name string) error
func (s *FileStore) GetActive() (*Theme, error)
func (s *FileStore) SetActive(name string) error
```

#### MemoryStore

```go
type MemoryStore struct { /* unexported */ }

func NewMemoryStore() *MemoryStore

func (s *MemoryStore) Path() string
func (s *MemoryStore) Get(name string) (*Theme, error)
func (s *MemoryStore) List() ([]*Theme, error)
func (s *MemoryStore) Save(theme *Theme) error
func (s *MemoryStore) Delete(name string) error
func (s *MemoryStore) GetActive() (*Theme, error)
func (s *MemoryStore) SetActive(name string) error
```

#### Generator

```go
type Generator struct{}

func NewGenerator() *Generator

func (g *Generator) Generate(t *Theme) (*GeneratedTheme, error)
```

#### GeneratedTheme

```go
type GeneratedTheme struct {
    PaletteLua     string
    InitLua        string
    PluginLua      string
    ColorschemeLua string
}
```

### Functions

```go
func ParseYAML(data []byte) (*Theme, error)
func GetSetupName(repo string) string
func GetTerminalEnvVarsForTheme(store Store, themeName string) (map[string]string, error)
```

### Theme Methods

```go
func (t *Theme) ToYAML() ([]byte, error)
func (t *Theme) Validate() error
func (t *Theme) IsStandalone() bool
func (t *Theme) HasCustomColors() bool
func (t *Theme) HasOptions() bool
func (t *Theme) GetColorschemeCommand() string
func (t *Theme) ToPalette() *palette.Palette
func (t *Theme) ToTerminalColors() map[string]string
func (t *Theme) TerminalEnvVars() map[string]string
```

### Variables

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

### Constants

```go
// Background colors (re-exported from MaestroPalette)
const (
    ColorBg           = palette.ColorBg
    ColorBgDark       = palette.ColorBgDark
    ColorBgHighlight  = palette.ColorBgHighlight
    ColorBgSearch     = palette.ColorBgSearch
    ColorBgVisual     = palette.ColorBgVisual
    ColorBgFloat      = palette.ColorBgFloat
    ColorBgPopup      = palette.ColorBgPopup
    ColorBgSidebar    = palette.ColorBgSidebar
    ColorBgStatusline = palette.ColorBgStatusline
)

// Foreground colors (re-exported from MaestroPalette)
const (
    ColorFg        = palette.ColorFg
    ColorFgDark    = palette.ColorFgDark
    ColorFgGutter  = palette.ColorFgGutter
    ColorFgSidebar = palette.ColorFgSidebar
)

// UI and diagnostic colors (re-exported from MaestroPalette)
const (
    ColorBorder  = palette.ColorBorder
    ColorComment = palette.ColorComment
    ColorError   = palette.ColorError
    ColorWarning = palette.ColorWarning
    ColorInfo    = palette.ColorInfo
    ColorHint    = palette.ColorHint
)

// Theme category constants (re-exported from MaestroPalette)
const (
    CategoryDark  = string(palette.CategoryDark)
    CategoryLight = string(palette.CategoryLight)
    CategoryBoth  = string(palette.CategoryBoth)
)
```

---

## Package: library

Import: `github.com/rmkohlman/MaestroTheme/library`

### Types

#### ThemeInfo

```go
type ThemeInfo struct {
    Name        string
    Description string
    Author      string
    Category    string
    Plugin      string
}
```

### Functions

```go
func List() ([]ThemeInfo, error)
func Get(name string) (*theme.Theme, error)
func GetRaw(name string) ([]byte, error)
func Has(name string) bool
func Categories() ([]string, error)
func ListByCategory(category string) ([]ThemeInfo, error)
```

---

## Package: parametric

Import: `github.com/rmkohlman/MaestroTheme/parametric`

### Types

#### Generator

```go
type Generator struct { /* unexported */ }

func NewGenerator() *Generator

func (g *Generator) GenerateFromHue(hue float64, name, description string) *theme.Theme
func (g *Generator) GenerateFromHex(hex string, name, description string) (*theme.Theme, error)
func (g *Generator) AnalyzeColor(hex string) (palette.HSL, ColorRelationship, error)
func (g *Generator) GetBaseHue() float64
func (g *Generator) GetColorRelationships() map[string]ColorRelationship
```

#### ColorRelationship

```go
type ColorRelationship struct {
    HueDelta    float64
    Saturation  float64
    Lightness   float64
    IsRelative  bool
    Description string
}
```

#### ThemePreset

```go
type ThemePreset struct {
    Name        string  `json:"name"`
    Description string  `json:"description"`
    Family      string  `json:"family"`
    Hue         float64 `json:"hue"`
    Notes       string  `json:"notes,omitempty"`
}
```

### Functions

```go
func FamilyNames() []string
func PresetsByFamily() map[string][]ThemePreset
func GetPreset(name string) (ThemePreset, bool)
func ListPresets() []string
func GeneratePreset(presetName string) (*theme.Theme, error)
```

### Variables

```go
var AllPresets = map[string]ThemePreset{ ... } // 21 entries

var CoolNightOceanColors = map[string]string{ ... } // reference palette
```
