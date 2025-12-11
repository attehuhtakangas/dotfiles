# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a macOS dotfiles repository that uses GNU Stow to symlink configuration files to the home directory. It manages configurations for zsh, git, tmux, starship prompt, and ghostty terminal.

## Commands

### Installation

```bash
./setup.sh          # Full setup: installs Homebrew, packages, oh-my-zsh, plugins, and symlinks dotfiles
make install        # Symlink dotfiles only (uses stow)
make uninstall      # Remove symlinks
make update         # Git pull + install
```

### Brewfile Management

```bash
brew bundle         # Install packages from Brewfile
brew bundle dump --describe  # Export current Homebrew packages to Brewfile
```

## Architecture

### Stow-Based Structure

Each top-level directory corresponds to a stow package that gets symlinked to `~`:

- `git/` → `.gitconfig`
- `zsh/` → `.zshrc`, `.zsh/functions/`
- `starship/` → `.config/starship.toml`
- `ghostty/` → `.config/ghostty/config`
- `tmux/` → Uses Oh My Tmux (cloned to `~/.tmux-config`)

The Makefile defines which packages to stow: `PACKAGES=git zsh tmux starship ghostty`

### User-Specific Files (Not in Repo)

- `~/.zsh-secrets` - API keys and tokens (chmod 600)
- `~/.zsh-paths` - System-specific PATH additions

### Custom Zsh Functions

Located in `zsh/.zsh/functions/`, auto-loaded via `autoload -Uz`:

- `git_prune_squash_merged` / `git_list_squash_merged` - Clean up squash-merged branches
- `pr` - PR helper
- `push_stack` / `gt_stack_rebase` - Graphite/git-town stacking helpers

### Key Configuration Choices

- Shell: zsh with oh-my-zsh (agnoster theme, but overridden by starship)
- Prompt: Starship
- Git pager: delta with difftastic for diffs
- Terminal: Ghostty (with iTerm2 as alternative)
- Version manager: mise (with shim-based lazy loading)
- Plugin caching: evalcache for faster shell startup
