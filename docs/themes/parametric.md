# Parametric Generator

The parametric generator creates custom CoolNight theme variants from any hue angle, hex color, or preset name. Generated themes are saved to `~/.nvp/themes/` and are immediately available for use.

---

## What It Is

The generator analyzes the color relationships in the CoolNight Ocean reference palette using HSL color space. For each color in the palette, it records the hue offset from Ocean's base hue (210°), the saturation, and the lightness. When generating a new theme, it applies those same relationships at the new target hue.

The result is a theme that looks and feels like CoolNight but centered on your chosen color.

---

## Creating Variants

```bash
nvp theme create --from <value> --name <name> [--use] [--dry-run]
```

### Flags

| Flag | Required | Description |
|------|----------|-------------|
| `--from <value>` | Yes | Base color: hue angle (0-360), hex (#rrggbb), or preset name |
| `--name <name>` | Yes | Name for the new theme (saved to `~/.nvp/themes/<name>.yaml`) |
| `--use` | No | Set the new theme as active immediately after creation |
| `--dry-run` | No | Preview the generated theme without saving |

### Input Types

| Input | Format | Example |
|-------|--------|---------|
| Hue angle | Integer 0-360 | `"280"` |
| Hex color | `#rrggbb` | `"#8B00FF"` |
| Preset name | Named CoolNight preset | `"synthwave"` |

---

## Examples

```bash
# Create from a hue angle
nvp theme create --from "280" --name my-purple

# Create from a hex color (the hue is extracted from the color)
nvp theme create --from "#8B00FF" --name my-violet

# Create from a preset name
nvp theme create --from "synthwave" --name my-synth

# Create and set as active immediately
nvp theme create --from "120" --name my-green --use

# Preview without saving
nvp theme create --from "45" --name preview-gold --dry-run
```

---

## Hue Reference

The hue angle determines the primary color family of the generated theme.

| Hue Range | Color Family | Example Presets |
|-----------|--------------|-----------------|
| 0° | Red | `crimson` |
| 0° - 30° | Red to orange | `ember` (15°), `sunset` (25°) |
| 30° - 90° | Orange to yellow | `gold` (45°) |
| 90° - 165° | Yellow to green | `forest` (140°), `matrix` (150°), `mint` (165°) |
| 165° - 210° | Green to blue | `solarized` (195°), `arctic` (200°) |
| 210° | Blue (Ocean base) | `ocean`, `nord` |
| 210° - 270° | Blue to purple | `midnight` (230°) |
| 260° - 290° | Purple | `dracula` (260°), `synthwave` (270°), `violet` (280°), `grape` (290°) |
| 290° - 360° | Purple to red | `sakura` (330°), `rose` (350°) |

---

## Color Palette Structure

### Semantic Color Mapping

Every generated theme provides these color keys:

| Color Key | Role | Shifts with Hue |
|-----------|------|-----------------|
| `bg` | Main background | Yes |
| `bg_dark` | Darker background panel | Yes |
| `bg_highlight` | Selection/highlight | Yes |
| `bg_search` | Search highlight | Yes |
| `bg_visual` | Visual selection | Yes |
| `fg` | Main foreground text | Yes |
| `fg_dark` | Dimmed foreground | Yes |
| `fg_gutter` | Gutter/line number text | Yes |
| `border` | UI border | Yes |
| `ansi_black` | Terminal black | Yes |
| `ansi_blue` | Terminal blue | Yes |
| `ansi_magenta` | Terminal magenta | Yes |
| `ansi_cyan` | Terminal cyan | Yes |
| `ansi_white` | Terminal white | Yes |
| `ansi_red` | Terminal red | **Fixed** |
| `ansi_green` | Terminal green | **Fixed** |
| `ansi_yellow` | Terminal yellow | **Fixed** |
| `cursor` | Cursor color | **Fixed** |

Fixed (semantic) colors retain the original CoolNight Ocean values regardless of the target hue. This ensures that error indicators, success signals, and cursor visibility remain consistent across all variants.

### Fixed Semantic Values

| Key | Fixed Value | Reason |
|-----|-------------|--------|
| `ansi_red` | `#E52E2E` | Error indicator |
| `ansi_green` | `#44FFB1` | Success indicator |
| `ansi_yellow` | `#FFE073` | Warning indicator |
| `cursor` | `#47FF9C` | Cursor visibility |

### Color Relationships

The generator preserves these relationships from the Ocean base:

- Background colors use the primary hue at very low lightness (dark, saturated base)
- Foreground colors use the primary hue at high lightness (light, slightly saturated text)
- Syntax accent colors are hue variations (offsets of ±30° to ±60°)
- UI chrome colors use desaturated variants of the primary hue

---

## Named Presets

You can use any of the 21 built-in preset names as the `--from` value:

```bash
nvp theme create --from "ocean" --name my-ocean-variant
nvp theme create --from "synthwave" --name custom-synth
nvp theme create --from "forest" --name dark-forest
```

Using a preset name as `--from` is equivalent to using that preset's hue angle. The preset names are:

`ocean`, `arctic`, `midnight`, `synthwave`, `violet`, `grape`, `forest`, `matrix`, `mint`, `ember`, `sunset`, `gold`, `crimson`, `sakura`, `rose`, `mono-charcoal`, `mono-slate`, `mono-warm`, `nord`, `dracula`, `solarized`

---

## Next Steps

- [CoolNight Collection](coolnight.md) - All 21 built-in variants with usage recommendations
- [Commands Reference](../commands.md) - Full `nvp theme create` flag reference
- [NvimTheme Reference](../reference/nvim-theme.md) - YAML format for the generated themes
