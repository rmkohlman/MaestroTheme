# CoolNight Collection

The CoolNight Collection is a set of 21 parametrically generated themes designed for extended development sessions. All variants share consistent color relationships derived from the CoolNight Ocean base theme using HSL hue rotation.

---

## Overview

CoolNight themes are standalone - they require no external Neovim plugin. Highlight groups are applied directly via Lua without a colorscheme plugin dependency.

Design goals:

- **Reduced eye strain** - Calibrated contrast ratios for long sessions
- **Consistent syntax highlighting** - The same color semantics across all 21 variants
- **Parametric generation** - All variants are mathematically derived from the Ocean base
- **Semantic color stability** - Error (red), warning (yellow), and cursor colors are fixed across all variants

---

## Color Science

### HSL Color Space

CoolNight uses HSL (Hue, Saturation, Lightness) for color generation:

- **Hue** - A value from 0-360 degrees that determines the dominant color family
- **Saturation** - Fixed at calibrated levels (40-70%) to ensure vibrancy without oversaturation
- **Lightness** - Fixed per semantic role to maintain consistent contrast ratios

The parametric generator rotates all non-semantic colors to a new hue while preserving their saturation and lightness relationships from the Ocean base palette.

### Accessibility

CoolNight themes maintain WCAG AA contrast ratios (4.5:1 minimum) between foreground text and background colors. Semantic colors (errors, warnings) are never rotated - they retain the original red, green, and yellow values regardless of theme hue.

---

## Complete Collection

### Blue Family

| Theme | Hue | Character | Best For |
|-------|-----|-----------|----------|
| `coolnight-arctic` | 200° | Ice blue, crisp and clean | TypeScript, Go, documentation |
| `coolnight-ocean` | 210° | Deep blue, the default | General development, Python |
| `coolnight-midnight` | 230° | Dark blue, intense focus | Late-night coding, C++ |

```bash
nvp theme use coolnight-arctic
nvp theme use coolnight-ocean
nvp theme use coolnight-midnight
```

### Purple Family

| Theme | Hue | Character | Best For |
|-------|-----|-----------|----------|
| `coolnight-violet` | 280° | Soft violet, gentle on eyes | Web development, CSS |
| `coolnight-synthwave` | 270° | Neon purple, retro vibes | JavaScript, creative coding |
| `coolnight-grape` | 290° | Rich grape, sophisticated | Rust, systems programming |

```bash
nvp theme use coolnight-violet
nvp theme use coolnight-synthwave
nvp theme use coolnight-grape
```

### Green Family

| Theme | Hue | Character | Best For |
|-------|-----|-----------|----------|
| `coolnight-forest` | 140° | Deep forest green, earthy | Bash scripts, DevOps |
| `coolnight-matrix` | 150° | Digital green, high contrast | Terminal work, security |
| `coolnight-mint` | 165° | Fresh mint, modern feel | React, Vue.js |

```bash
nvp theme use coolnight-forest
nvp theme use coolnight-matrix
nvp theme use coolnight-mint
```

### Warm Family

| Theme | Hue | Character | Best For |
|-------|-----|-----------|----------|
| `coolnight-ember` | 15° | Glowing ember, energetic | Java, Spring Boot |
| `coolnight-sunset` | 25° | Warm orange, inviting | HTML, markup languages |
| `coolnight-gold` | 45° | Golden yellow, premium | Configuration files, YAML |

```bash
nvp theme use coolnight-ember
nvp theme use coolnight-sunset
nvp theme use coolnight-gold
```

### Red/Pink Family

| Theme | Hue | Character | Best For |
|-------|-----|-----------|----------|
| `coolnight-crimson` | 0° | Deep crimson, bold | Error handling, debugging |
| `coolnight-sakura` | 330° | Cherry blossom, elegant | Design systems |
| `coolnight-rose` | 350° | Rose pink, warm | Personal projects |

```bash
nvp theme use coolnight-crimson
nvp theme use coolnight-sakura
nvp theme use coolnight-rose
```

### Monochrome Family

Monochrome variants apply desaturation after hue generation, producing gray-scale or near-gray themes with minimal color distraction.

| Theme | Saturation | Character | Best For |
|-------|------------|-----------|----------|
| `coolnight-mono-charcoal` | 0% | Pure grayscale | Distraction-free coding |
| `coolnight-mono-slate` | 15% | Subtle blue-gray | Enterprise development |
| `coolnight-mono-warm` | 15% | Subtle warm-gray | Long coding sessions |

```bash
nvp theme use coolnight-mono-charcoal
nvp theme use coolnight-mono-slate
nvp theme use coolnight-mono-warm
```

!!! note "Semantic colors in monochrome themes"
    Error, warning, and cursor colors retain their original saturated values even in monochrome variants. This preserves the visual distinction for diagnostics.

### Special Variants

Variants inspired by well-known themes, adapted to the CoolNight parametric system.

| Theme | Inspiration | Character | Best For |
|-------|-------------|-----------|----------|
| `coolnight-nord` | Nord theme | Arctic blue-gray, clean | Nordic aesthetic |
| `coolnight-dracula` | Dracula theme | Rich purple, dark gothic | High-contrast dark work |
| `coolnight-solarized` | Solarized theme | Blue-cyan, precise | Academic, research |

```bash
nvp theme use coolnight-nord
nvp theme use coolnight-dracula
nvp theme use coolnight-solarized
```

---

## Usage Recommendations

### By Development Environment

**Terminal-heavy workflows:**

- `coolnight-matrix` - High contrast green
- `coolnight-mono-charcoal` - Minimal, no distractions

**Web development:**

- `coolnight-mint` - Modern, fresh
- `coolnight-synthwave` - Creative, vibrant

**Systems programming:**

- `coolnight-midnight` - Deep focus
- `coolnight-grape` - Sophisticated

**Documentation writing:**

- `coolnight-arctic` - Clean and readable
- `coolnight-mono-warm` - Easy on eyes for long reading

**Presentations and screen sharing:**

- `coolnight-ocean` - Professional default
- `coolnight-sunset` - Warm, welcoming

### By Time of Day

**Morning:**

- `coolnight-arctic` - Fresh and energizing
- `coolnight-mint` - Bright start

**Daytime:**

- `coolnight-ocean` - Balanced and professional
- `coolnight-forest` - Natural and comfortable

**Evening:**

- `coolnight-sunset` - Warm transition
- `coolnight-ember` - Cozy

**Late night:**

- `coolnight-midnight` - Deep focus
- `coolnight-mono-slate` - Reduced stimulation

---

## Integration with Theme Hierarchy

CoolNight variants work well with dvm's hierarchical theme system:

```bash
# Set a professional default at ecosystem level
dvm set theme coolnight-ocean --ecosystem corporate

# Use high contrast for security work at domain level
dvm set theme coolnight-matrix --domain security

# Set a creative theme for a specific app
dvm set theme coolnight-synthwave --app creative-tool

# Override for your personal workspace
dvm set theme coolnight-midnight --workspace my-dev
```

See [Theme Hierarchy](../configuration/theme-hierarchy.md) for the full cascade model.

---

## Troubleshooting

### Theme Not Applying

```bash
# Check if the theme is available
nvp theme list | grep coolnight

# View theme details
nvp theme get coolnight-ocean -o yaml

# Regenerate Lua files
nvp generate
```

### Colors Look Wrong in Neovim

```bash
# Confirm true color support
echo $COLORTERM
# Expected: truecolor

# Check Neovim termguicolors
nvim -c 'set termguicolors?' -c 'q'
```

### Custom Variant Issues

```bash
# Review the generated theme
nvp theme get my-custom-theme -o yaml

# Regenerate after any changes
nvp generate
```

---

## Next Steps

- [Parametric Generator](parametric.md) - Create your own CoolNight variant
- [Theme Hierarchy](../configuration/theme-hierarchy.md) - Use themes across your dvm hierarchy
- [Commands Reference](../commands.md) - Full `nvp theme` command reference
