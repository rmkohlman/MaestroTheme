# MaestroTheme

MaestroTheme is the Neovim theme management engine powering the `nvp theme` commands in the [DevOpsMaestro](https://github.com/rmkohlman/devopsmaestro) toolkit. It provides a curated library of 34+ embedded themes, the CoolNight parametric collection, and YAML-based theme configuration.

---

## What MaestroTheme Provides

| Feature | Description |
|---------|-------------|
| **34+ embedded themes** | Available immediately in the binary - no installation needed |
| **CoolNight Collection** | 21 parametrically generated variants covering every color family |
| **Parametric generator** | Create custom CoolNight variants from hue angles, hex colors, or preset names |
| **Theme hierarchy** | Themes cascade through dvm's Workspace -> App -> Domain -> Ecosystem levels |
| **YAML IaC** | Define, share, and version themes as `NvimTheme` YAML resources |
| **Lua generation** | Generates lazy.nvim-compatible colorscheme files for Neovim |

---

## Quick Install

```bash
# Install DevOpsMaestro (includes both dvm and nvp)
brew install rmkohlman/tap/devopsmaestro

# Or install NvimOps only (no container runtime needed)
brew install rmkohlman/tap/nvimops

# Verify
nvp version
nvp theme list
```

---

## Quick Example

```bash
# Browse available themes
nvp theme list

# Use a theme immediately (no installation needed)
nvp theme use coolnight-ocean

# Create a custom CoolNight variant
nvp theme create --from "280" --name my-purple --use

# Generate Lua files for Neovim
nvp generate
```

---

## Next Steps

<div class="grid cards" markdown>

- **[Installation](getting-started/installation.md)**

    Install nvp via Homebrew or build from source.

- **[Quick Start](getting-started/quickstart.md)**

    Browse themes, create variants, and generate Neovim configs.

- **[Theme Library](themes/library.md)**

    Explore the 34+ embedded themes available out of the box.

- **[CoolNight Collection](themes/coolnight.md)**

    Learn about the 21 parametric CoolNight variants.

- **[Parametric Generator](themes/parametric.md)**

    Create custom themes from any hue angle or hex color.

- **[Commands Reference](commands.md)**

    Complete reference for all `nvp theme` commands.

</div>
