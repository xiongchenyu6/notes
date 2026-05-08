# Emacs to Neovim Migration

Date: 2026-03-04

## Why

- Replace emacs with neovim as primary editor
- Replace org-mode with Obsidian for notes/knowledge management
- Simplify toolchain — one editor, not two

## What Changed

### Dotfiles (`~/dotfiles`)

#### Emacs removed from all hosts
All emacs config extracted to a standalone file `home-modules/emacs.nix` — not imported by any host. To re-enable emacs on any host, add `./emacs.nix` to that host's home-manager imports.

Files modified:
- `home-modules/gui/common.nix` — removed `programs.emacs` (100+ packages)
- `home-modules/gui/default.nix` — removed `services.emacs` daemon
- `home-modules/nixos-desktop.nix` — removed `services.emacs` daemon
- `home-modules/hyprland/default.nix` — removed `emacs30-pgtk` override
- `home-modules/xmonad/default.nix` — removed `emacs29` override + picom rule
- `home-modules/zsh.nix` — removed emacs antidote plugin
- `home-modules/stow-config.nix` — removed `~/.config/emacs` persistence
- `home-modules/cli.nix` — removed emacs topgrade command
- `nixos-modules/shell-enhance.nix` — removed emacs from ohMyZsh plugins
- `nixos-modules/gui.nix` — removed `emacs-all-the-icons-fonts`

#### Neovim set as default editor
- `home-modules/neovim.nix` — `defaultEditor = true`
- `home-configurations/freeman.xiong.nix` — cleaned up EDITOR override comment

### Neovim plugins added for emacs parity

#### Debugging (DAP)
| Plugin | Keybinding | Purpose |
|---|---|---|
| `nvim-dap` | `<leader>db` | Toggle breakpoint |
| | `<leader>dc` | Continue |
| | `<leader>di/do/dO` | Step into/over/out |
| | `<leader>dr` | Toggle REPL |
| | `<leader>dt` | Terminate |
| `nvim-dap-ui` | `<leader>du` | Toggle DAP UI |
| | `<leader>de` | Eval expression |
| `nvim-dap-virtual-text` | — | Inline debug values |

#### HTTP Client (replaces emacs restclient)
| Plugin | Keybinding | Purpose |
|---|---|---|
| `rest-nvim` | `<leader>hr` | Run HTTP request under cursor |
| | `<leader>hl` | Re-run last request |

Create `.http` files with standard HTTP syntax to use.

#### Language Support

| Language | Plugin               | LSP Server                | Notes                                      |
| -------- | -------------------- | ------------------------- | ------------------------------------------ |
| Rust     | `rust-tools-nvim`    | `rust-analyzer`           | Auto-configures LSP, runs `clippy` on save |
| Haskell  | `haskell-tools-nvim` | `haskell-language-server` | Auto-configures from PATH                  |
| Scala    | `nvim-metals`        | `metals`                  | Attaches on `scala`, `sbt`, `java` files   |
| CMake    | `vim-cmake`          | `cmake-language-server`   | Build system support                       |
| PlantUML | `plantuml-syntax`    | —                         | Syntax + `plantuml` binary in PATH         |
| Solidity | `vim-solidity`       | `solc`                    | Syntax highlighting                        |
| Protobuf | treesitter           | `buf`                     | LSP via buf tool                           |
| Graphviz | `graphviz-vim`       | `graphviz`                | Dot file support                           |

#### UI
- **lualine** re-enabled — statusline with mode, branch, diff, diagnostics, LSP indicator, file info

#### Other
- **leetcode.nvim** — solve leetcode problems in nvim (default: python3)

### LSP servers added to extraPackages
```
rust-analyzer
haskell-language-server
metals
cmake-language-server
buf
plantuml
graphviz
solc
```

## Emacs features intentionally dropped

| Feature | Reason |
|---|---|
| org-mode (org-roam, org-contrib, ox-hugo, etc.) | Replaced by Obsidian |
| lispy / sly (Lisp editing) | No longer writing Lisp |
| aidermacs (AI coding) | Using Claude Code instead |
| rime (Chinese input in emacs) | System-level fcitx5 handles this |
| gnu-apl-mode | Niche, unused |
| gcmh (GC tuning) | Emacs-specific, N/A |
| ace-link | flash.nvim covers navigation |
| pdf-tools | Use system PDF viewer |

## How to re-enable emacs

Add to any host's home-manager imports:
```nix
imports = [
  ./emacs.nix
  # ... other imports
];
```

The standalone file includes all packages, the daemon service, and comments for platform-specific package overrides (emacs30-pgtk for Wayland, emacs29 for X11).

## Neovim keybinding cheatsheet

### Navigation
| Key | Action |
|---|---|
| `<leader><space>` | Find files |
| `<leader>ff` | Find files |
| `<leader>fg` | Git files |
| `<leader>fG` | Live grep |
| `<leader>fr` | Recent files |
| `<leader>fb` | Buffers |
| `<leader>fs` | Document symbols |
| `<leader>fS` | Workspace symbols |

### Code
| Key | Action |
|---|---|
| `gd` | Go to definition |
| `gr` | References |
| `gi` | Implementation |
| `K` | Hover docs |
| `<leader>rn` | Rename |
| `<leader>ca` | Code action |
| `<leader>f` | Format |
| `<leader>cd` | Line diagnostics |
| `[d` / `]d` | Prev/next diagnostic |

### Git
| Key | Action |
|---|---|
| `<leader>gs` | Git status (fugitive) |
| `<leader>gb` | Blame line |
| `<leader>gd` | Diff view |
| `<leader>gc` | Git commit |
| `<leader>gp` | Git push |
| `]h` / `[h` | Next/prev hunk |

### Debug
| Key | Action |
|---|---|
| `<leader>db` | Toggle breakpoint |
| `<leader>dc` | Continue |
| `<leader>di` | Step into |
| `<leader>do` | Step over |
| `<leader>dO` | Step out |
| `<leader>du` | Toggle DAP UI |
| `<leader>de` | Eval |

### Other
| Key | Action |
|---|---|
| `<leader>e` | File explorer |
| `<leader>t` | Terminal |
| `<leader>xx` | Toggle Trouble |
| `<leader>sr` | Search & replace (Spectre) |
| `s` / `S` | Flash jump / treesitter |
| `<leader>u` | Undo tree |
| `<leader>hr` | Run HTTP request |
