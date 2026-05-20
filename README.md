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
toolkit() {
    local matrix=(
        $'\e[1;95m─── 🔍 Search & Navigation ─────────────────────────────────────\e[0m | SECTION-SEARCH | | '
        $'\e[36mSearch\e[0m      │ ripgrep (rg)         | Search & Nav | Faster grep replacement. Respects .gitignore. | rg \'pat\'; rg -i \'pat\'; rg -t py \'pat\''
        $'\e[36mSearch\e[0m      │ fd                   | Search & Nav | Simpler, multi-threaded alternative to find.    | fd pat; fd -e pdf; fd -x rm'
        $'\e[36mSearch\e[0m      │ fzf                  | Search & Nav | General-purpose interactive fuzzy finder.       | fzf; fzf --preview \'bat {}\''
        $'\e[36mSearch\e[0m      │ zoxide (z)           | Search & Nav | Smart cd replacement that remembers folders.    | z folder; z -; zoxide query -l'

        $'\e[1;92m─── 📂 File Management & Viewing ───────────────────────────────\e[0m | SECTION-FILE | | '
        $'\e[32mFile/View\e[0m   │ bat                  | File Viewer  | Upgraded cat with syntax highlighting.          | bat f.txt; bat -p f.txt'
        $'\e[32mFile/View\e[0m   │ dust                 | File Viewer  | Visual, color-coded recursive disk usage tree.  | dust; dust -d 1'
        $'\e[33mEditor\e[0m      │ micro                | Editors      | Modern and intuitive terminal text editor.      | micro f.txt; micro +10 f.txt'
        $'\e[33mEditor\e[0m      │ helix (hx)           | Editors      | A post-modern modal editor with built-in LSP.   | hx f.txt; hx --tutor'

        $'\e[1;96m─── 📊 Monitoring & Performance ───────────────────────────────\e[0m | SECTION-MONITOR | | '
        $'\e[35mMonitor\e[0m     │ htop / bottom (btm)  | Monitoring   | Graphical interactive system/process monitors.   | htop; btm; btm --basic'
        $'\e[35mMonitor\e[0m     │ procs                | Monitoring   | Human-readable ps with thread tracing.          | procs; procs --watch'
        $'\e[35mMonitor\e[0m     │ fastfetch            | Monitoring   | Fast, active system information dashboard.       | fastfetch'

        $'\e[1;94m─── 🐳 Docker & Container Ecosystem ────────────────────────────\e[0m | SECTION-DOCKER | | '
        $'\e[34mDocker\e[0m      │ lazydocker           | Docker/Infra | Interactive TUI dashboard for Docker clusters.  | lazydocker'
        $'\e[34mDocker\e[0m      │ dive                 | Docker/Infra | Layer-by-layer docker image analyzer.           | dive image'

        $'\e[1;91m─── 🌐 Network & API Utilities ─────────────────────────────────\e[0m | SECTION-NETWORK | | '
        $'\e[31mNetwork\e[0m     │ httpie (http)        | Network/API  | User-friendly curl alternative with JSON style. | http google.com; http POST :3000/api'
        $'\e[31mNetwork\e[0m     │ xh                   | Network/API  | Blazing fast Rust clone of httpie.             | xh google.com; xh GET :3000'
        $'\e[31mNetwork\e[0m     │ trippy (trip)        | Network/API  | Combined interactive ping and traceroute UI.    | trip google.com; trip -i eth0'
        $'\e[31mNetwork\e[0m     │ doggo                | Network/API  | Modern DNS client (dig alternate) with DoH/DoT. | doggo google.com; doggo @1.1.1.1 google.com'
        $'\e[31mNetwork\e[0m     │ bandwhich            | Network/API  | Real-time utility tracking bandwidth per PID.   | sudo bandwhich'

        $'\e[1;97m─── 📋 Code Helpers & Data Streams ─────────────────────────────\e[0m | SECTION-DATA | | '
        $'\e[37mData/Dev\e[0m    │ jq / yq              | Data Stream  | Powerful processors for JSON/YAML streams.      | cat f.json | jq \'.\'; yq \'.key\' f.yml'
        $'\e[37mData/Dev\e[0m    │ delta                | Data Stream  | Renders crisp side-by-side git diffs.           | git diff | delta; git show | delta'
        $'\e[37mData/Dev\e[0m    │ tldr                 | Data Stream  | Quick, real-world example cheatsheets.          | tldr tar; tldr find'

        $'\e[1;33m─── 💼 Workspace & Security ────────────────────────────────────\e[0m | SECTION-WORKSPACE | | '
        $'\e[96mWorkspace\e[0m   │ zellij               | Workspace    | Feature-rich terminal multiplexer (tmux alt).   | zellij'
        $'\e[96mWorkspace\e[0m   │ lazygit              | Workspace    | Visual terminal UI for git operations.          | lazygit'
        $'\e[91mSecurity\e[0m    │ age                  | Security     | Simple, modern file encryption tool.            | age -p f.txt > f.age; age -d f.age'

        $'\e[1;95m─── ⚡ Custom Workflow Combinations ────────────────────────────\e[0m | SECTION-WORKFLOW | | '
        $'\e[95mWorkflow\e[0m    │ fe                   | Custom Combo | Interactive File Opener (fd+fzf+bat+micro).     | fe'
        $'\e[95mWorkflow\e[0m    │ rgf                  | Custom Combo | Interactive Code Grepper (rg+fzf+bat+micro).    | rgf'
        $'\e[95mWorkflow\e[0m    │ glf                  | Custom Combo | Interactive Git Log Viewer (git+delta+fzf).     | glf'
        $'\e[95mWorkflow\e[0m    │ pkill-ui             | Custom Combo | Interactive Process Killer Menu (procs+fzf).    | pkill-ui'
        $'\e[95mWorkflow\e[0m    │ clean-files          | Custom Combo | Mass Deletion Safety Grid Panel (fd+fzf+rm).    | clean-files'
        $'\e[95mWorkflow\e[0m    │ dock-inspect         | Custom Combo | Instant Docker Container Filter (docker+fzf+jq). | dock-inspect'
        $'\e[95mWorkflow\e[0m    │ resolve-ips          | Custom Combo | Direct IP DNS Resolution Quick Filter.          | resolve-ips host'
    )
    printf "%s\n" "${matrix[@]}" | fzf \
        --header "🧰 DEV/SYSADMIN TOOLKIT ARSENAL (Type to filter, Esc to exit)
💡 Shortcuts: Ctrl-S: Search | Ctrl-F: File/View | Ctrl-M: Monitor | Ctrl-N: Network | Ctrl-W: Workflows | Ctrl-R: Reset" \
        --header-lines=0 \
        --ansi \
        --delimiter "\|" \
        --with-nth=1 \
        --bind "ctrl-s:change-query(Search),ctrl-f:change-query(File/View),ctrl-m:change-query(Monitor),ctrl-n:change-query(Network),ctrl-w:change-query(Workflow),ctrl-r:change-query()" \
        --preview "
            category=\$(echo \"{2}\" | xargs)
            if [[ \"\$category\" =~ ^SECTION- ]]; then
                echo -e \"\n  \x1b[1;95m📂 Category: \${category#SECTION-}\x1b[0m\n\"
                case \"\$category\" in
                    SECTION-SEARCH)
                        echo -e \"  \x1b[1;37m🔍 Search & Navigation\x1b[0m\"
                        echo -e \"  Speedy file searching (ripgrep, fd-find), interactive filtering\"
                        echo -e \"  (fzf), and intelligent directory jumps (zoxide).\"
                        ;;
                    SECTION-FILE)
                        echo -e \"  \x1b[1;37m📂 File Management & Viewing\x1b[0m\"
                        echo -e \"  Syntax-highlighted views (bat), space analysis (dust),\"
                        echo -e \"  and advanced text editors (micro, helix).\"
                        ;;
                    SECTION-MONITOR)
                        echo -e \"  \x1b[1;37m📊 Monitoring & Performance\x1b[0m\"
                        echo -e \"  Interactive resource dashboards (htop, bottom), process logs\"
                        echo -e \"  analyzer (procs), and active system specifications (fastfetch).\"
                        ;;
                    SECTION-DOCKER)
                        echo -e \"  \x1b[1;37m🐳 Docker & Container Ecosystem\x1b[0m\"
                        echo -e \"  Comprehensive container TUI dashboards (lazydocker) and\"
                        echo -e \"  detailed image layers analyzer (dive).\"
                        ;;
                    SECTION-NETWORK)
                        echo -e \"  \x1b[1;37m🌐 Network & API Utilities\x1b[0m\"
                        echo -e \"  Intuitive REST testing (httpie, xh), interactive network pathing\"
                        echo -e \"  (trippy), DNS records query (doggo), and bandwidth tracing (bandwhich).\"
                        ;;
                    SECTION-DATA)
                        echo -e \"  \x1b[1;37m📋 Code Helpers & Data Streams\x1b[0m\"
                        echo -e \"  Interactive JSON/YAML queries (jq/yq), syntax-colored diffs\"
                        echo -e \"  (delta), and concise terminal cheatsheets (tldr).\"
                        ;;
                    SECTION-WORKSPACE)
                        echo -e \"  \x1b[1;37m💼 Workspace & Security\x1b[0m\"
                        echo -e \"  Productive workspaces (zellij), terminal git manager (lazygit),\"
                        echo -e \"  and secure modern file encryption (age).\"
                        ;;
                    SECTION-WORKFLOW)
                        echo -e \"  \x1b[1;37m⚡ Custom Workflow Combinations\x1b[0m\"
                        echo -e \"  Synergistic commands combining search, filters, editors, and docker\"
                        echo -e \"  to automate common administration procedures.\"
                        ;;
                esac
                echo -e \"\n  \x1b[1;30m💡 Navigation: Use ↑/↓ to browse tools, or type to filter.\x1b[0m\"
            else
                echo -e \"\n  \x1b[1;36m🔧 Tool / Context:\x1b[0m {1}\n  \x1b[1;33m📁 Category:\x1b[0m {2}\n  \x1b[1;32m💡 When to use:\x1b[0m {3}\n  \x1b[1;34m⚡ Quick Hint:\x1b[0m \x1b[1;37m{4}\x1b[0m\n\"
            fi
        " \
        --preview-window=right:50%:wrap
}
alias arsenal='toolkit'
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

### 📊 Monitoring & Performance

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| `top` | **`htop` / `bottom`** | Colorful, keyboard-interactive task and process graphs. |
| `ps` | **`procs`** | Multi-colored columns with automated thread hierarchy tracking. |
| *None* | **`fastfetch`** | Ultra-fast system info fetch tool with high customizability. |

### 🐳 Docker & Container Ecosystem

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
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

### 💼 Workspace & Security

| Legacy Tool | Modern Equivalent | Key Advantage |
| :--- | :--- | :--- |
| *None* | **`zellij`** | A modern, terminal workspace with layout and multiplexing options. |
| `git` | **`lazygit`** | Simple terminal UI for git commands, branches, and stashes. |
| `gpg` | **`age`** | Simple, modern, and secure file encryption tool. |

### ⚡ Custom Workflow Combinations

| Command | Primary Components | Key Advantage / Workflow |
| :--- | :--- | :--- |
| **`fe`** | `fd` + `fzf` + `bat` + `micro` | Interactive file opener with live syntax preview. |
| **`rgf`** | `rg` + `fzf` + `bat` + `micro` | Interactive code searcher opening editor at target line. |
| **`glf`** | `git` + `fzf` + `delta` | Interactive git log viewer with syntax-colored side-by-side diff. |
| **`pkill-ui`** | `procs` + `fzf` | Interactive process search and safe kill manager. |
| **`clean-files`** | `fd` + `fzf` + `rm` | Safe recursive file deletion grid with tab multi-select. |
| **`dock-inspect`** | `docker` + `fzf` + `jq` + `bat` | Fuzzy inspect Docker containers to colorized, pretty JSON. |
| **`resolve-ips`** | `doggo` + `jq` + `grep` | Direct DNS resolution query filtering to clean IP output. |


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

