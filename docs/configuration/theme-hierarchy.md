# Theme Hierarchy

Themes in DevOpsMaestro cascade through the resource hierarchy, providing a default at each level that is inherited by more-specific levels unless overridden.

---

## Cascade Model

Themes resolve from the most specific to the most general:

```
Workspace  -->  App  -->  Domain  -->  Ecosystem  -->  Global Default
(most specific)                               (least specific)
```

The first theme found in this order is used. If no theme is set at any level, no theme is applied.

---

## Setting Themes via dvm

### Workspace level

```bash
# Set theme for the current workspace context
dvm set theme coolnight-ocean

# Set theme for a specific workspace
dvm set theme coolnight-ocean --workspace my-dev
```

### App level

```bash
# Set theme for the current app context
dvm set theme tokyonight-night --app

# Set theme for a specific app
dvm set theme tokyonight-night --app my-api
```

### Domain level

```bash
# Set theme for the current domain context
dvm set theme catppuccin-mocha --domain

# Set theme for a specific domain
dvm set theme catppuccin-mocha --domain backend
```

### Ecosystem level

```bash
# Set theme for the current ecosystem context
dvm set theme coolnight-matrix --ecosystem

# Set theme for a specific ecosystem
dvm set theme coolnight-matrix --ecosystem my-platform
```

### Global default

```bash
# Set a global fallback theme
dvm config set theme coolnight-ocean
```

---

## Examples

### Per-environment themes

Use different themes to signal which environment you are in:

```bash
dvm set theme coolnight-ocean   --workspace dev
dvm set theme coolnight-ember   --workspace staging
dvm set theme coolnight-crimson --workspace prod
```

### Per-domain themes

Apply a team or language-specific theme at the domain level:

```bash
dvm set theme coolnight-ocean    --domain backend
dvm set theme catppuccin-latte   --domain frontend
dvm set theme coolnight-matrix   --domain security
```

### Personal workspace override

The team has a domain-level theme, but you prefer a different one for your own workspace:

```bash
# Team uses gruvbox-dark at domain level
dvm set theme coolnight-midnight --workspace my-dev
```

### Organizational default

Set a company-wide theme at the ecosystem level, overridden per domain:

```bash
dvm set theme coolnight-ocean    --ecosystem corporate
dvm set theme coolnight-synthwave --domain platform-team
dvm set theme gruvbox-dark       --domain data-team
```

---

## Viewing the Current Theme

```bash
# Show active theme and its hierarchy source
dvm get theme

# Show the full cascade table
dvm get theme --show-cascade
```

Example output from `--show-cascade`:

```
Theme Cascade for workspace 'dev' in app 'my-api':
Level       Theme              Status
Workspace   -                  Not set
App         coolnight-ocean    Active (resolved here)
Domain      gruvbox-dark       Overridden by app
Ecosystem   -                  Not set
Global      tokyonight-night   Default fallback

Resolved theme: coolnight-ocean (from App level)
```

---

## Clearing Theme Settings

```bash
# Clear the workspace-level theme (inherits from parent)
dvm unset theme --workspace

# Clear the app-level theme
dvm unset theme --app my-api

# Clear the domain-level theme
dvm unset theme --domain backend

# Clear the ecosystem-level theme
dvm unset theme --ecosystem my-platform
```

---

## YAML Configuration

Themes can also be set via YAML resource files:

```yaml
apiVersion: devopsmaestro.io/v1
kind: Ecosystem
metadata:
  name: my-platform
spec:
  theme: coolnight-ocean
---
apiVersion: devopsmaestro.io/v1
kind: Domain
metadata:
  name: backend
  ecosystem: my-platform
spec:
  theme: gruvbox-dark
---
apiVersion: devopsmaestro.io/v1
kind: App
metadata:
  name: user-service
  domain: backend
  ecosystem: my-platform
spec:
  theme: coolnight-synthwave  # Overrides domain theme
```

Apply the configuration:

```bash
dvm apply -f hierarchy.yaml
```

---

## Export and Import for Teams

Export a theme for sharing with teammates:

```bash
# Export as YAML
nvp theme get coolnight-synthwave -o yaml > team-theme.yaml

# Team members apply it
nvp apply -f team-theme.yaml
```

---

## Integration with nvp

The hierarchy system integrates with `nvp` for Neovim configuration generation:

```bash
# Set theme via dvm (resolves through hierarchy)
dvm set theme coolnight-ocean --app my-api

# Resolve and apply the theme in nvp
nvp theme use $(dvm get theme --name-only)

# Generate Neovim configuration
nvp generate
```

---

## Reference: Theme Commands

### Setting themes

| Command | Scope |
|---------|-------|
| `dvm set theme <name>` | Current workspace |
| `dvm set theme <name> --workspace <ws>` | Specific workspace |
| `dvm set theme <name> --app [name]` | App level |
| `dvm set theme <name> --domain [name]` | Domain level |
| `dvm set theme <name> --ecosystem [name]` | Ecosystem level |

### Getting themes

| Command | Purpose |
|---------|---------|
| `dvm get theme` | Active theme and source level |
| `dvm get theme --show-cascade` | Full cascade table |
| `nvp theme get` | Active theme in nvp store |

### Clearing themes

| Command | Purpose |
|---------|---------|
| `dvm unset theme` | Clear workspace theme |
| `dvm unset theme --app <name>` | Clear app theme |
| `dvm unset theme --domain <name>` | Clear domain theme |
| `dvm unset theme --ecosystem <name>` | Clear ecosystem theme |

---

## Next Steps

- [Commands Reference](../commands.md) - Full `nvp theme` command reference
- [CoolNight Collection](../themes/coolnight.md) - Theme variants suited for hierarchy use
- [NvimTheme Reference](../reference/nvim-theme.md) - YAML format for theme resources
