# Installation

`nvp` is the command-line tool that powers all theme management. It is distributed as part of the DevOpsMaestro toolkit and as a standalone `nvimops` package.

---

## Homebrew (Recommended)

=== "Full Toolkit"

    Installs both `dvm` (workspace management) and `nvp` (Neovim/theme management):

    ```bash
    brew install rmkohlman/tap/devopsmaestro
    ```

=== "NvimOps Only"

    Installs only `nvp`. No container runtime required:

    ```bash
    brew install rmkohlman/tap/nvimops
    ```

Verify the installation:

```bash
nvp version
nvp theme list
```

---

## From Source

Build from the DevOpsMaestro repository:

```bash
# Clone the repository
git clone https://github.com/rmkohlman/devopsmaestro.git
cd devopsmaestro

# Build nvp
go build -o nvp ./cmd/nvp/

# Install to your PATH
sudo mv nvp /usr/local/bin/

# Verify
nvp version
```

!!! note "Go version"
    Building from source requires Go 1.25 or later.

---

## Verify Installation

```bash
# Check version
nvp version

# List all available themes (confirms the embedded library is accessible)
nvp theme list
```

If `nvp theme list` shows 34+ themes, installation is complete and the embedded library is working.

---

## Shell Completion

Enable tab completion for `nvp` commands:

=== "Bash"

    ```bash
    # Add to ~/.bashrc
    eval "$(nvp completion bash)"
    ```

=== "Zsh"

    ```bash
    # Add to ~/.zshrc
    eval "$(nvp completion zsh)"
    ```

=== "Fish"

    ```bash
    # Add to ~/.config/fish/config.fish
    nvp completion fish | source
    ```

---

## Next Steps

- [Quick Start](quickstart.md) - Use your first theme in minutes
- [Theme Library](../themes/library.md) - Browse all 34+ embedded themes
- [Commands Reference](../commands.md) - Complete command reference
