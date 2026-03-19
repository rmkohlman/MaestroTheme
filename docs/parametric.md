# parametric package

Import path: `github.com/rmkohlman/MaestroTheme/parametric`

The `parametric` package provides HSL-based parametric theme generation. It analyzes the color relationships in the CoolNight Ocean reference theme and generates new themes by applying those same relationships at any hue angle.

## How It Works

The parametric generator extracts the HSL (Hue, Saturation, Lightness) values from every color in the CoolNight Ocean reference palette. For each color it records:

- **HueDelta**: The difference between the color's hue and the base hue (210 degrees for ocean blue).
- **Saturation**: Absolute saturation (0.0 to 1.0).
- **Lightness**: Absolute lightness (0.0 to 1.0).

When generating a new theme at a target hue, each color's new hue is computed as:

```
newHue = targetHue + HueDelta
```

Semantic colors (red, green, yellow, cursor) are fixed and do not shift with the hue. They retain their original hex values regardless of the target hue:

| Key | Fixed Value |
|-----|-------------|
| `ansi_red` | `#E52E2E` |
| `ansi_green` | `#44FFB1` |
| `ansi_yellow` | `#FFE073` |
| `ansi_bright_red` | `#E52E2E` |
| `ansi_bright_green` | `#44FFB1` |
| `ansi_bright_yellow` | `#FFE073` |
| `cursor` | `#47FF9C` |

All generated themes are standalone (empty `Plugin.Repo`) with `Category: dark` and `Author: "DevOpsMaestro (parametric generator)"`.

## CoolNight Ocean Reference Colors

The exact reference colors used for analysis:

| Color Key | Hex Value | Description |
|-----------|-----------|-------------|
| `bg` | `#011628` | Main background |
| `bg_dark` | `#011423` | Darker background |
| `bg_highlight` | `#143652` | Selection highlight |
| `bg_search` | `#0A64AC` | Search highlight |
| `bg_visual` | `#275378` | Visual selection |
| `fg` | `#CBE0F0` | Main foreground |
| `fg_dark` | `#B4D0E9` | Darker foreground |
| `fg_gutter` | `#627E97` | Gutter text |
| `border` | `#547998` | Border color |
| `ansi_black` | `#214969` | ANSI black |
| `ansi_red` | `#E52E2E` | ANSI red (fixed semantic) |
| `ansi_green` | `#44FFB1` | ANSI green (fixed semantic) |
| `ansi_yellow` | `#FFE073` | ANSI yellow (fixed semantic) |
| `ansi_blue` | `#0FC5ED` | ANSI blue |
| `ansi_magenta` | `#a277ff` | ANSI magenta |
| `ansi_cyan` | `#24EAF7` | ANSI cyan |
| `ansi_white` | `#24EAF7` | ANSI white |
| `ansi_bright_black` | `#214969` | Bright black |
| `ansi_bright_red` | `#E52E2E` | Bright red (fixed semantic) |
| `ansi_bright_green` | `#44FFB1` | Bright green (fixed semantic) |
| `ansi_bright_yellow` | `#FFE073` | Bright yellow (fixed semantic) |
| `ansi_bright_blue` | `#A277FF` | Bright blue |
| `ansi_bright_magenta` | `#a277ff` | Bright magenta |
| `ansi_bright_cyan` | `#24EAF7` | Bright cyan |
| `ansi_bright_white` | `#24EAF7` | Bright white |
| `cursor` | `#47FF9C` | Cursor (fixed semantic) |
| `cursor_text` | `#011423` | Cursor text (same as bg_dark) |
| `selection` | `#033259` | Selection background |

## Types

### Generator

```go
type Generator struct { /* unexported fields */ }
```

Fields (unexported):

- `baseHue float64`: The base hue of the reference theme (210.0 degrees).
- `relationships map[string]ColorRelationship`: HSL relationships extracted from CoolNight Ocean.
- `semanticColors map[string]string`: Fixed semantic colors that do not shift.

### ColorRelationship

```go
type ColorRelationship struct {
    HueDelta    float64 // Difference from base hue (-180 to 180)
    Saturation  float64 // Absolute saturation (0-1)
    Lightness   float64 // Absolute lightness (0-1)
    IsRelative  bool    // Whether to shift with the target hue
    Description string  // Human-readable description
}
```

### ThemePreset

```go
type ThemePreset struct {
    Name        string  `json:"name"`
    Description string  `json:"description"`
    Family      string  `json:"family"`
    Hue         float64 `json:"hue"`
    Notes       string  `json:"notes,omitempty"`
}
```

## Generator Functions

### NewGenerator

```go
func NewGenerator() *Generator
```

Creates a new generator. Automatically calls `analyzeColorRelationships()` to extract HSL data from `CoolNightOceanColors`.

### GenerateFromHue

```go
func (g *Generator) GenerateFromHue(hue float64, name, description string) *theme.Theme
```

Generates a new `*theme.Theme` by rotating all hue-relative colors to the specified hue angle (0.0 to 360.0 degrees). Semantic colors are copied unchanged. The resulting theme is standalone.

### GenerateFromHex

```go
func (g *Generator) GenerateFromHex(hex string, name, description string) (*theme.Theme, error)
```

Converts the hex color to HSL using `palette.HexToHSL()`, extracts its hue, then calls `GenerateFromHue()`. Returns an error if the hex string is invalid.

### AnalyzeColor

```go
func (g *Generator) AnalyzeColor(hex string) (palette.HSL, ColorRelationship, error)
```

Returns the HSL values and relationship of an arbitrary hex color relative to the base hue. Useful for debugging and understanding how a color would be positioned in a new theme.

### GetBaseHue

```go
func (g *Generator) GetBaseHue() float64
```

Returns the base hue (210.0).

### GetColorRelationships

```go
func (g *Generator) GetColorRelationships() map[string]ColorRelationship
```

Returns all color relationships extracted from the reference palette.

## Preset Functions

### AllPresets

```go
var AllPresets = map[string]ThemePreset{ ... }
```

Contains all 21 preset definitions keyed by name.

### FamilyNames

```go
func FamilyNames() []string
```

Returns the ordered list of family names: `blue`, `purple`, `green`, `warm`, `red`, `pink`, `monochrome`, `special`.

### PresetsByFamily

```go
func PresetsByFamily() map[string][]ThemePreset
```

Returns all presets grouped by family name.

### GetPreset

```go
func GetPreset(name string) (ThemePreset, bool)
```

Returns a preset by name and whether it was found.

### ListPresets

```go
func ListPresets() []string
```

Returns all preset names ordered by family (using `FamilyNames()` order). Within each family, order is not guaranteed.

### GeneratePreset

```go
func GeneratePreset(presetName string) (*theme.Theme, error)
```

Generates a theme from a preset by name. For `monochrome` family presets, applies desaturation after hue generation. Returns an error if the preset name is not found.

Monochrome desaturation rules:

| Preset | Target Saturation |
|--------|------------------|
| `coolnight-mono-charcoal` | 0.0 (pure grayscale) |
| `coolnight-mono-slate` | 0.15 (subtle tint) |
| `coolnight-mono-warm` | 0.15 (subtle tint) |

Semantic colors (red, green, yellow, cursor) are never desaturated in monochrome themes.

## All Presets

### Blue Family

| Name | Hue | Description |
|------|-----|-------------|
| `coolnight-ocean` | 210.0 | The original deep blue theme. Base for all parametric generation. |
| `coolnight-arctic` | 200.0 | Cooler blue with ice-like tones |
| `coolnight-midnight` | 230.0 | Deep midnight blue |

### Purple Family

| Name | Hue | Description |
|------|-----|-------------|
| `coolnight-synthwave` | 270.0 | Neon purple inspired by 80s aesthetics |
| `coolnight-violet` | 280.0 | Soft violet tones |
| `coolnight-grape` | 290.0 | Rich grape purple |

### Green Family

| Name | Hue | Description |
|------|-----|-------------|
| `coolnight-matrix` | 150.0 | Digital green |
| `coolnight-forest` | 140.0 | Deep forest green |
| `coolnight-mint` | 165.0 | Fresh mint green |

### Warm Family

| Name | Hue | Description |
|------|-----|-------------|
| `coolnight-sunset` | 25.0 | Warm orange sunset tones |
| `coolnight-ember` | 15.0 | Glowing ember red-orange |
| `coolnight-gold` | 45.0 | Rich golden yellow |

### Red/Pink Family

| Name | Hue | Family | Description |
|------|-----|--------|-------------|
| `coolnight-rose` | 350.0 | pink | Elegant rose pink |
| `coolnight-crimson` | 0.0 | red | Deep crimson red |
| `coolnight-sakura` | 330.0 | pink | Soft cherry blossom pink |

### Monochrome Family

| Name | Hue | Target Saturation | Description |
|------|-----|------------------|-------------|
| `coolnight-mono-charcoal` | 0.0 | 0.0 | Pure grayscale |
| `coolnight-mono-slate` | 210.0 | 0.15 | Cool gray with subtle blue tint |
| `coolnight-mono-warm` | 30.0 | 0.15 | Warm gray with subtle brown tint |

### Special Family

| Name | Hue | Description |
|------|-----|-------------|
| `coolnight-nord` | 210.0 | Nord-inspired blue-gray palette |
| `coolnight-dracula` | 260.0 | Dracula-inspired purple tones |
| `coolnight-solarized` | 195.0 | Solarized-inspired blue-cyan base |

## Usage Examples

```go
import "github.com/rmkohlman/MaestroTheme/parametric"

// Generate from a named preset
t, err := parametric.GeneratePreset("coolnight-grape")
if err != nil {
    log.Fatal(err)
}
fmt.Println(t.Name) // coolnight-grape

// Generate from an arbitrary hue
gen := parametric.NewGenerator()
t := gen.GenerateFromHue(120.0, "my-green", "A custom green theme")

// Generate from a hex color
t, err := gen.GenerateFromHex("#22aa44", "from-hex", "Generated from a green")

// List all presets by family
for _, family := range parametric.FamilyNames() {
    presets := parametric.PresetsByFamily()[family]
    for _, p := range presets {
        fmt.Printf("[%s] %s (hue: %.1f)\n", family, p.Name, p.Hue)
    }
}
```
