# 🚀 Ultimate Modern DevOps & SysAdmin Toolkit

A curated collection of modern, fast, and user-friendly CLI tools to replace legacy UNIX utilities. Optimized for Ubuntu/Debian environments (**Bash/Zsh**), this setup dramatically enhances workflows across **Software Development, Systems Administration, and Network Engineering**.

---

## 📖 Table of Contents
- [⚡ Automated Quick-Install](#-automated-quick-install)
- [🧰 The Arsenal Command](#-the-arsenal-command)
- [🛠️ Tool Index & Cheat Sheet](#️-tool-index--cheat-sheet)
- [🔑 New Laptop Quick-Start](#-new-laptop-quick-start-github-ssh-key-setup)
- [🧹 Automated Rollback](#-automated-rollback-uninstall-script)

---

## ⚡ Automated Quick-Install

Run the following block in your terminal to update your repositories, install all tools, map the interactive configurations, and load the new profile.

```bash
# 1. System Package Updates & Core Tool Installation
sudo apt update && sudo apt install -y \
    ripgrep fd-find fzf bat htop httpie jq yq tldr git-delta cargo snapd \
    zoxide age fastfetch micro

# 2. Advanced Diagnostic & Container Tools (Snaps)
sudo snap install dust procs trippy lazydocker bottom lazygit
sudo snap install helix --classic

# 3. Network Architecture & Workspace Utilities (Cargo)
cargo install doggo bandwhich zellij xh
export PATH="$HOME/.cargo/bin:$PATH"

# 4. Inject Smart Tool Cross-Integrations and Aliases into Profiles
# This block appends the configuration to both .bashrc and .zshrc if they exist, with idempotency check.
INSTALL_BLOCK=$(cat << 'EOF'

# ==============================================================================
# 🔥 MODERN CLI REPLACEMENTS & INTERACTIVE ALIASES (MANAGED BY TOOLKIT)
# ==============================================================================
export PATH="$HOME/.cargo/bin:$PATH"
export EDITOR="micro"
export VISUAL="micro"

# --- Basic Command Overrides ---
alias fd="fdfind"
alias bat="batcat"
alias du="dust"
alias ps="procs"
alias cat="batcat --paging=never"
alias grep="rg"

# --- 🔍 Code Search & File Traversal ---
# Find any file with a live syntax-highlighted preview. Press Enter to edit.
alias fe='file=$(fd --type f --hidden --exclude .git | fzf --preview "batcat --color=always --style=numbers {}") && [ -n "$file" ] && ${EDITOR:-micro} "$file"'

# Fuzzy search text strings across your project with a code window preview.
alias rgf='rg --line-number --no-heading --color=always --smart-case "" | fzf --ansi --delimiter : --preview "batcat --color=always --highlight-line {2} {1}" --preview-window=up:60% | awk -F: "{print \"+\" \$2 \" \" \$1}" | xargs -r ${EDITOR:-micro}'

# --- 📂 Git & Version Control ---
# Interactive git history viewer with side-by-side syntax-highlighted diffs.
alias glf='git log --oneline --color=always | fzf --ansi --no-sort --preview "git show --color=always {1} | delta" --preview-window=right:60% | awk "{print \$1}" | xargs -r git show'

# --- ⚙️ System Maintenance & Disk Management ---
# Interactive process search and kill facility.
alias pkill-ui='procs --color always | fzf --ansi --header-lines=1 | awk "{print \$1}" | xargs -r kill -9'

# Visual recursive file deletion safety grid. Multi-select files with [TAB].
alias clean-files='files=$(fd --type f --hidden | fzf --multi --preview "batcat --color=always --style=changes {}") && [ -n "$files" ] && echo "$files" | xargs -I {} rm -iv "{}"'

# --- 🐳 Container Ecosystem ---
# Fuzzy search container status configurations and output colorized, readable JSON.
alias dock-inspect='container=$(docker ps --format "{{.Names}} ({{.Image}})" | fzf | awk "{print \$1}") && [ -n "$container" ] && docker inspect "$container" | jq . | batcat --language=json'

# --- 🌐 Networking & Diagnostics ---
# Direct-to-IP DNS resolution filter.
alias resolve-ips='function _resolve() { doggo "$1" --json | jq -r ".responses[].answers[].address" | grep -v "null"; }; _resolve'

# --- 🚀 Zoxide (Smart CD) ---
eval "$(zoxide init ${SHELL##*/})"

# ==============================================================================
# 🧰 CLI ARSENAL INTERACTIVE CHEATSHEET
# ==============================================================================
arsenal() {
    local matrix=(
        "ripgrep (rg)        | Search & Nav  | Faster grep replacement. Respects .gitignore. | rg 'pat'; rg -i 'pat'; rg -t py 'pat'"
        "fd                  | Search & Nav  | Simpler, multi-threaded alternative to find.    | fd pat; fd -e pdf; fd -x rm"
        "fzf                 | Search & Nav  | General-purpose interactive fuzzy finder.       | fzf; fzf --preview 'bat {}'"
        "zoxide (z)          | Search & Nav  | Smart cd replacement that remembers folders.    | z folder; z -; zoxide query -l"
        "bat                 | File Viewer   | Upgraded cat with syntax highlighting.          | bat f.txt; bat -p f.txt"
        "dust                | File Viewer   | Visual, color-coded recursive disk usage tree.  | dust; dust -d 1"
        "htop / bottom (btm) | Monitoring    | Graphical interactive system/process monitors.   | htop; btm; btm --basic"
        "procs               | Monitoring    | Human-readable ps with thread tracing.          | procs; procs --watch"
        "fastfetch           | Monitoring    | Fast, active system information dashboard.       | fastfetch"
        "micro               | Editors       | Modern and intuitive terminal text editor.      | micro f.txt; micro +10 f.txt"
        "helix (hx)          | Editors       | A post-modern modal editor with built-in LSP.   | hx f.txt; hx --tutor"
        "lazydocker          | Docker/Infra  | Interactive TUI dashboard for Docker clusters.  | lazydocker"
        "dive                | Docker/Infra  | Layer-by-layer docker image analyzer.           | dive image"
        "httpie (http)       | Network/API   | User-friendly curl alternative with JSON style. | http google.com; http POST :3000/api"
        "xh                  | Network/API   | Blazing fast Rust clone of httpie.             | xh google.com; xh GET :3000"
        "trippy (trip)       | Network/API   | Combined interactive ping and traceroute UI.    | trip google.com; trip -i eth0"
        "doggo               | Network/API   | Modern DNS client (dig alternate) with DoH/DoT. | doggo google.com; doggo @1.1.1.1 google.com"
        "bandwhich           | Network/API   | Real-time utility tracking bandwidth per PID.   | sudo bandwhich"
        "jq / yq             | Data Stream   | Powerful processors for JSON/YAML streams.      | cat f.json | jq '.'; yq '.key' f.yml"
        "delta               | Data Stream   | Renders crisp side-by-side git diffs.           | git diff | delta; git show | delta"
        "tldr                | Data Stream   | Quick, real-world example cheatsheets.          | tldr tar; tldr find"
        "zellij              | Workspace     | Feature-rich terminal multiplexer (tmux alt).   | zellij"
        "lazygit             | Workspace     | Visual terminal UI for git operations.          | lazygit"
        "age                 | Security      | Simple, modern file encryption tool.            | age -p f.txt > f.age; age -d f.age"
        "--- WORKFLOW COMBINATIONS ---"
        "fe                  | Custom Combo  | Interactive File Opener (fd+fzf+bat+micro).     | fe"
        "rgf                 | Custom Combo  | Interactive Code Grepper (rg+fzf+bat+micro).    | rgf"
        "glf                 | Custom Combo  | Interactive Git Log Viewer (git+delta+fzf).     | glf"
        "pkill-ui            | Custom Combo  | Interactive Process Killer Menu (procs+fzf).    | pkill-ui"
        "clean-files         | Custom Combo  | Mass Deletion Safety Grid Panel (fd+fzf+rm).    | clean-files"
        "dock-inspect        | Custom Combo  | Instant Docker Container Filter (docker+fzf+jq). | dock-inspect"
        "resolve-ips         | Custom Combo  | Direct IP DNS Resolution Quick Filter.          | resolve-ips host"
    )
    printf "%s\n" "${matrix[@]}" | fzf --header "🧰 DEV/SYSADMIN TOOLKIT ARSENAL (Type to filter, Esc to exit)" --header-lines=0 --ansi --delimiter "\|" --with-nth=1,2 --preview "echo -e \"\n\x1b[1;36m🔧 Tool / Context:\x1b[0m {1}\n\x1b[1;33m📁 Category:\x1b[0m {2}\n\x1b[1;32m💡 When to use:\x1b[0m {3}\n\x1b[1;34m⚡ Quick Hint:\x1b[0m {4}\n\"" --preview-window=right:50%:wrap
}
EOF
)

for rc in "$HOME/.bashrc" "$HOME/.zshrc"; do
    if [ -f "$rc" ]; then
        if ! grep -q "MANAGED BY TOOLKIT" "$rc"; then
            echo "$INSTALL_BLOCK" >> "$rc"
        else
            echo "Skipping injection for $rc: Configuration block already exists."
        fi
    fi
done

# 5. Refresh running shell session
[ -n "$BASH_VERSION" ] && [ -f ~/.bashrc ] && source ~/.bashrc
[ -n "$ZSH_VERSION" ] && [ -f ~/.zshrc ] && source ~/.zshrc
echo "🎉 Toolkit installation and alias compilation completed successfully!"
```

---

## 🧰 The Arsenal Command

Once installed, simply type `arsenal` in your terminal to open the interactive toolkit guide. Use arrows or type to search, and view usage tips in the preview panel.

---

## 🛠️ Tool Index & Cheat Sheet

### 🔍 Search & Navigation

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `grep` | **`ripgrep` (`rg`)** | Orders of magnitude faster; honors `.gitignore` rules by default. |
| `find` | **`fd`** | Intuitive pattern syntax; multi-threaded parallel execution. |
| `cd` | **`zoxide` (`z`)** | Smart cd that remembers your most used directories. |
| *None* | **`fzf`** | General-purpose command-line fuzzy finder for streams, files, and histories. |

### 📂 File Management & Viewing

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `cat` | **`bat`** | On-the-fly programming language syntax highlighting and git-change gutters. |
| `du` | **`dust`** | Instant graphical, colorized visualization of directory sizes. |
| `nano` / `vim` | **`micro`** | Intuitive, mouse-supported editor with familiar keybindings (Ctrl-S, Ctrl-C). |
| `vim` / `neovim` | **`helix` (`hx`)** | Post-modern modal editor with batteries-included LSP and tree-sitter. |

### ⚙️ System Monitoring & Containers

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `top` | **`htop` / `bottom`** | Colorful, keyboard-interactive task and process graphs. |
| `ps` | **`procs`** | Multi-colored columns with automated thread hierarchy tracking. |
| *None* | **`fastfetch`** | Ultra-fast system info fetch tool with high customizability. |
| `docker` | **`lazydocker`** | Fully mouse-and-keyboard UI dashboard for container clusters and log tracking. |
| *None* | **`dive`** | Layer-by-layer file overhead analyzer for cleaning up docker images. |

### 🌐 Network & API Utilities

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `curl` | **`httpie` / `xh`** | Clean, unquoted HTTP syntax and pretty-printed JSON outputs. |
| `traceroute` | **`trippy`** | Interactive path mapping engine with automated node latency histograms. |
| `dig` | **`doggo`** | Modern DNS utility supporting DoH/DoT transport protocols and JSON formats. |
| `iftop` | **`bandwhich`** | Maps network consumption tables directly back to active Process PIDs. |

### 📋 Code Helpers & Data Streams

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `sed` / `awk` | **`jq` / `yq`** | Specialized processing engines built to slice, filter, and rewrite JSON/YAML streams. |
| `git diff` | **`delta`** | Renders crisp side-by-side modifications with granular character highlights. |
| `man` | **`tldr`** | Drops bloated instructional textbooks for immediate, practical command examples. |
| *None* | **`zellij`** | A terminal workspace with "batteries included" (layout, tabs, etc.) |

---

## 🔑 New Laptop Quick-Start: GitHub SSH Key Setup

When moving to a fresh laptop, run this snippet to generate secure Ed25519 SSH keys, start the authentication daemon, and output the key to link to your GitHub account so you can pull down this repository.

```bash
# 1. Generate a modern, secure SSH key (Replace with your email)
ssh-keygen -t ed25519 -C "your_email@example.com" -f ~/.ssh/id_ed25519 -N ""

# 2. Start the ssh-agent in the background and add the key
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

# 3. Print the public key to terminal for GitHub copy-paste
cat ~/.ssh/id_ed25519.pub
```
* **Next Step:** Copy the printed terminal output, navigate to **GitHub Settings ➔ SSH and GPG keys ➔ New SSH Key**, and paste it there.

---

## 🧹 Automated Rollback (Uninstall Script)

If you ever need to clean a machine and completely remove these packages, configurations, and custom aliases, paste and execute this script:

```bash
# 1. Remove Snap applications
sudo snap remove dust procs trippy lazydocker bottom lazygit helix

# 2. Remove Cargo utilities
cargo uninstall doggo bandwhich zellij xh

# 3. Purge system packages
sudo apt purge -y ripgrep fd-find fzf bat htop httpie jq yq tldr git-delta cargo zoxide age fastfetch micro
sudo apt autoremove -y

# 4. Strip the configuration injector block out of shell profiles
for rc in "$HOME/.bashrc" "$HOME/.zshrc"; do
    if [ -f "$rc" ]; then
        sed -i '/# ==============================================================================/,/# ==============================================================================/d' "$rc"
    fi
done

# 5. Reload shell profile
[ -n "$BASH_VERSION" ] && [ -f ~/.bashrc ] && source ~/.bashrc
[ -n "$ZSH_VERSION" ] && [ -f ~/.zshrc ] && source ~/.zshrc
echo "🧹 Environment has been rolled back to factory defaults."
```

