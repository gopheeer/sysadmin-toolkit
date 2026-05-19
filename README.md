# 🚀 Ultimate Modern DevOps & SysAdmin Toolkit

A curated collection of modern, fast, and user-friendly CLI tools to replace legacy UNIX utilities. Optimized for Ubuntu/Debian environments (**Bash/Zsh**), this setup dramatically enhances workflows across **Software Development, Systems Administration, and Network Engineering**.

---

## ⚡ Automated Quick-Install

Run the following block in your terminal to update your repositories, install all tools, map the interactive configurations, and load the new profile.

```bash
# 1. System Package Updates & Core Tool Installation
sudo apt update && sudo apt install -y ripgrep fd-find fzf bat htop httpie jq yq tldr git-delta cargo snapd

# 2. Advanced Diagnostic & Container Tools (Snaps)
sudo snap install dust procs trippy lazydocker

# 3. Network Architecture & DNS Utilities (Cargo)
cargo install doggo bandwhich
export PATH="$HOME/.cargo/bin:$PATH"

# 4. Inject Smart Tool Cross-Integrations and Aliases into Profiles
# This block appends the configuration to both .bashrc and .zshrc if they exist.
INSTALL_BLOCK=$(cat << 'EOF'

# ==============================================================================
# 🔥 MODERN CLI REPLACEMENTS & INTERACTIVE ALIASES
# ==============================================================================
export PATH="$HOME/.cargo/bin:$PATH"

# --- Basic Command Overrides ---
alias fd="fdfind"
alias bat="batcat"
alias du="dust"
alias ps="procs"
alias cat="batcat --paging=never"
alias grep="rg"

# --- 🔍 Code Search & File Traversal ---
# Find any file with a live syntax-highlighted preview. Press Enter to edit.
alias fe='file=$(fd --type f --hidden --exclude .git | fzf --preview "batcat --color=always --style=numbers {}") && [ -n "$file" ] && ${EDITOR:-nano} "$file"'

# Fuzzy search text strings across your project with a code window preview.
alias rgf='rg --line-number --no-heading --color=always --smart-case "" | fzf --ansi --delimiter : --preview "batcat --color=always --highlight-line {2} {1}" --preview-window=up:60% | awk -F: "{print \"+\" \$2 \" \" \$1}" | xargs -r ${EDITOR:-nano}'

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

# ==============================================================================
# 🧰 CLI ARSENAL INTERACTIVE CHEATSHEET
# ==============================================================================
arsenal() {
    local matrix=(
        "ripgrep (rg)        | Search & Nav  | Faster grep replacement. Respects .gitignore by default."
        "fd                  | Search & Nav  | Simpler, multi-threaded alternative to complex find commands."
        "fzf                 | Search & Nav  | General-purpose interactive fuzzy finder for streams and files."
        "zoxide (z)          | Search & Nav  | Smart cd replacement that remembers frequent folders."
        "bat                 | File Viewer   | Upgraded cat with language syntax highlighting and git gutters."
        "dust                | File Viewer   | Visual, color-coded recursive disk usage space tree."
        "htop / bottom (btm) | Monitoring    | Graphical, keyboard-interactive system and process monitors."
        "procs               | Monitoring    | Human-readable ps replacement with thread hierarchy tracing."
        "fastfetch           | Monitoring    | Fast, active system information and specs dashboard dashboard."
        "lazydocker          | Docker/Infra  | Interactive keyboard/mouse TUI dashboard for container clusters."
        "dive                | Docker/Infra  | Layer-by-layer docker image analyzer to find file bloat."
        "httpie (http)       | Network/API   | Unquoted user-friendly curl alternative with pretty JSON styling."
        "xh                  | Network/API   | Blazing fast Rust clone of httpie with zero startup latency."
        "trippy (trip)       | Network/API   | Combined interactive ping and traceroute diagnostic UI suite."
        "doggo               | Network/API   | Modern DNS client (dig alternate) with native DoH/DoT and JSON."
        "bandwhich           | Network/API   | Real-time terminal utility tracking bandwidth down to specific PIDs."
        "jq / yq             | Data Stream   | Ultra-powerful processors to slice, filter, and rewrite JSON/YAML."
        "delta               | Data Stream   | Renders crisp side-by-side git diff code modifications."
        "tldr                | Data Stream   | Quick, real-world example cheat sheets replacing long man pages."
        "zellij              | Workspace     | Feature-rich terminal multiplexer (tmux replacement) with no config."
        "lazygit             | Workspace     | Visual terminal UI for staging code, merging, and git branch trees."
        "age                 | Security      | Simple, modern file encryption tool replacing complex GPG keys."
        "--- WORKFLOW COMBINATIONS ---"
        "fe                  | Custom Combo  | Interactive File Opener (fd + fzf + bat + nano/vim)"
        "rgf                 | Custom Combo  | Interactive Code Grepper (ripgrep + fzf + bat + nano/vim)"
        "glf                 | Custom Combo  | Interactive Git Log Viewer (git + delta + fzf)"
        "pkill-ui            | Custom Combo  | Interactive Process Killer Menu (procs + fzf)"
        "clean-files         | Custom Combo  | Mass Deletion Safety Grid Panel (fd + fzf + rm)"
        "dock-inspect        | Custom Combo  | Instant Docker Container Inspection Filter (docker + fzf + jq + bat)"
        "resolve-ips         | Custom Combo  | Direct IP DNS Resolution Quick Filter (doggo + jq)"
    )
    print -l "${matrix[@]}" | fzf --header "🧰 DEV/SYSADMIN TOOLKIT ARSENAL (Type to filter, Esc to exit)" --header-lines=0 --ansi --delimiter "\|" --with-nth=1,2 --preview "echo -e \"\n\x1b[1;36m🔧 Tool / Context:\x1b[0m {1}\n\x1b[1;33m📁 Category:\x1b[0m {2}\n\x1b[1;32m💡 When to use:\x1b[0m {3}\n\"" --preview-window=right:50%:wrap
}
EOF

for rc in "$HOME/.bashrc" "$HOME/.zshrc"; do
    if [ -f "$rc" ]; then
        echo "$INSTALL_BLOCK" >> "$rc"
    fi
done

# 5. Refresh running shell session
[ -n "$BASH_VERSION" ] && [ -f ~/.bashrc ] && source ~/.bashrc
[ -n "$ZSH_VERSION" ] && [ -f ~/.zshrc ] && source ~/.zshrc
echo "🎉 Toolkit installation and alias compilation completed successfully!"
```

---

## 🛠️ Tool Index & Cheat Sheet

### 🔍 Search & Navigation


| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `grep` | **`ripgrep` (`rg`)** | Orders of magnitude faster; honors `.gitignore` rules by default. |
| `find` | **`fd`** | Intuitive pattern syntax; multi-threaded parallel execution. |
| *None* | **`fzf`** | General-purpose command-line fuzzy finder for streams, files, and histories. |

### 📂 File Management & Viewing


| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `cat` | **`bat`** | On-the-fly programming language syntax highlighting and git-change gutters. |
| `du` | **`dust`** | Instant graphical, colorized visualization of directory sizes. |

### ⚙️ System Monitoring & Containers


| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `top` | **`htop` / `bottom`** | Colorful, keyboard-interactive task and process graphs. |
| `ps` | **`procs`** | Multi-colored columns with automated thread hierarchy tracking. |
| `docker` | **`lazydocker`** | Fully mouse-and-keyboard UI dashboard for container clusters and log tracking. |
| *None* | **`dive`** | Layer-by-layer file overhead analyzer for cleaning up docker images. |

### 🌐 Network & API Utilities


| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `curl` | **`httpie`** | Clean, unquoted HTTP structural syntax and automatically indented JSON outputs. |
| `traceroute` | **`trippy`** | Interactive path mapping engine with automated node latency histograms. |
| `dig` | **`doggo`** | Modern DNS utility natively supporting DoH/DoT transport protocols and JSON formats. |
| `iftop` | **`bandwhich`** | Maps network consumption tables directly back to active Process PIDs. |

### 📋 Code Helpers & Data Streams


| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `sed` / `awk` | **`jq` / `yq`** | Specialized processing engines built to slice, filter, and rewrite JSON/YAML streams. |
| `git diff` | **`delta`** | Renders crisp side-by-side modifications with granular character highlights. |
| `man` | **`tldr`** | Drops bloated instructional textbooks for immediate, practical command examples. |

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
sudo snap remove dust procs trippy lazydocker

# 2. Remove Cargo utilities
cargo uninstall doggo bandwhich

# 3. Purge system packages
sudo apt purge -y ripgrep fd-find fzf bat htop httpie jq yq tldr git-delta cargo
sudo apt autoremove -y

# 4. Strip the configuration injector block out of shell profiles
for rc in "$HOME/.bashrc" "$HOME/.zshrc"; do
    if [ -f "$rc" ]; then
        sed -i '/# ==============================================================================/,/# ==============================================================================/d' "$rc"
        sed -i '/# 🔥 MODERN CLI REPLACEMENTS & INTERACTIVE ALIASES/,/alias resolve-ips.*/d' "$rc"
    fi
done

# 5. Reload shell profile
[ -n "$BASH_VERSION" ] && [ -f ~/.bashrc ] && source ~/.bashrc
[ -n "$ZSH_VERSION" ] && [ -f ~/.zshrc ] && source ~/.zshrc
echo "🧹 Environment has been rolled back to factory defaults."
```

