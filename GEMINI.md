# Project Context: Ultimate Modern DevOps & SysAdmin Toolkit

This project is a curated collection of modern CLI tools, configurations, and scripts designed to replace legacy UNIX utilities and enhance DevOps/SysAdmin workflows on Ubuntu/Debian systems.

## Directory Overview

The project serves as a central repository for setup instructions, tool recommendations, and automated installation/uninstallation scripts. It is designed to be easy to navigate and highly robust for both Bash and Zsh users.

## Key Components (from README.md)

- **Automated Quick-Install:** A multi-step script for installing tools via `apt`, `snap`, and `cargo`. It includes idempotency checks to prevent duplicate configuration injections.
- **The Arsenal Command:** An interactive, `fzf`-powered cheatsheet (`arsenal`) for exploring the toolkit's capabilities.
- **Bash & Zsh Aliases & Integrations:** A comprehensive set of aliases (e.g., `rg` for `grep`, `fd` for `find`, `bat` for `cat`) and custom functions injected into `~/.bashrc` and `~/.zshrc`. This includes setting `micro` as the default `EDITOR` and `VISUAL` environment variables.
- **Tool Index:** A categorized mapping of legacy tools to their modern equivalents.
- **Maintenance & Rollback:** Scripts for SSH key generation and complete system environment rollback.

## Usage Guidelines

- **Setup:** Run the "Automated Quick-Install" script in `README.md`. It safely handles existing configurations.
- **Interactive Help:** Use the `arsenal` command in the terminal for real-time tool lookup and usage tips.
- **Modifications:** When adding tools, update the `INSTALL_BLOCK`, the `arsenal` matrix, and the Tool Index table.
- **Testing:** Verify script changes in both Bash and Zsh environments on Ubuntu/Debian.

## Technical Environment

- **OS Target:** Linux (specifically Ubuntu/Debian).
- **Package Managers:** `apt`, `snap`, `cargo`.
- **Shells:** Bash and Zsh (configurations target `~/.bashrc` and `~/.zshrc`).
- **Core Arsenal:** 
    - **Search:** `ripgrep`, `fd-find`, `fzf`, `zoxide`.
    - **File/System:** `bat`, `dust`, `htop`, `bottom`, `procs`, `fastfetch`, `micro`, `helix`.
    - **Infra/Docker:** `lazydocker`, `dive`, `lazygit`.
    - **Network:** `httpie`, `xh`, `trippy`, `doggo`, `bandwhich`.
    - **Data/Sec:** `jq`, `yq`, `tldr`, `git-delta`, `age`, `zellij`.
