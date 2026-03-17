# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

A personal Neovim configuration based on [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim). It is a single-file-primary config (`init.lua`) with optional modular plugin files, not a Neovim distribution.

## Architecture

- **`init.lua`** — The main config file containing ~1160 lines. It defines, in order: leader key, vim options, keymaps, autocommands, lazy.nvim bootstrap, and the full plugin specification list.
- **`lua/kickstart/plugins/`** — Optional kickstart-provided plugin configs (neo-tree, autopairs, lint, debug, gitsigns, indent_line). These are loaded by uncommenting `require 'kickstart.plugins.<name>'` in the lazy.setup() call in init.lua. Currently all are **commented out** (not active).
- **`lua/custom/plugins/`** — User custom plugins directory (currently empty `return {}`). Loaded via `{ import = 'custom.plugins' }` in lazy.setup() — also currently **commented out**.
- **`lua/kickstart/health.lua`** — Health check module (`:checkhealth kickstart`).

## Plugin Manager

Uses [lazy.nvim](https://github.com/folke/lazy.nvim). Key commands:
- `:Lazy` — view plugin status
- `:Lazy update` — update all plugins
- `lazy-lock.json` — lockfile tracked in git

## Key Plugins & Their Roles

| Plugin | Purpose |
|---|---|
| `telescope.nvim` + fzf-native + zoxide | Fuzzy finder (files, grep, LSP, buffers, zoxide dirs) |
| `nvim-lspconfig` + mason + mason-tool-installer | LSP setup. Servers defined in `servers` table (~line 709). Currently: `lua_ls`, `stylua` |
| `conform.nvim` | Autoformatting. Format-on-save enabled. Lua uses `stylua`. `<leader>f` to format manually |
| `blink.cmp` + LuaSnip | Autocompletion |
| `nvim-treesitter` | Syntax highlighting, indentation |
| `neo-tree.nvim` | File tree sidebar (`\` to toggle, `|` to reveal with cwd) |
| `mini.nvim` | ai textobjects, surround, files (`<leader>m`), statusline |
| `oil.nvim` | Buffer-based file explorer (not default explorer, delete-to-trash) |
| `which-key.nvim` | Shows pending keybinds after delay |
| `gitsigns.nvim` | Git gutter signs |
| `diffview.nvim` | Git diff viewer |
| `vim-visual-multi` | Multiple cursors |
| `vim-kitty-navigator` | Kitty terminal split navigation with `<C-hjkl>` |
| `telescope-zoxide` | Zoxide directory jumping (`<leader>z`) |

## Colorschemes

Multiple installed: tokyonight (active, `tokyonight-night`), kanagawa, catppuccin, monokai-pro. Preview with `<leader>sc`.

## Key Customizations (diverges from upstream kickstart)

- `vim.g.have_nerd_font = true`
- Leader is `<space>`, arrow keys disabled in normal mode
- Neo-tree: shows dotfiles, hides .git, `|` reveals with cwd
- MiniFiles: `<leader>m` toggle, custom keymaps for set cwd (`g~`), OS open (`gX`), yank path (`gy`)
- Oil: detail toggle with `gd`, sorts by type/name/mtime
- `<leader>e` opens init.lua for editing
- `<leader>b` opens Neo-tree buffer list on right
- `<leader>g` opens Neo-tree git status float
- Zoxide integration: `<leader>z` for directory picker, changes tab-local cwd
- `SelectAndOpenFolder()` global function for startup folder selection

## Formatting

Lua code is formatted with **stylua**. Format-on-save is enabled via conform.nvim.

## LSP Servers

Add new LSP servers to the `servers` table (~line 709 in init.lua). Mason auto-installs them. Use `vim.lsp.config()` + `vim.lsp.enable()` pattern (Neovim 0.11+).

## Adding Plugins

Two approaches:
1. Add directly to the `require('lazy').setup({...})` call in init.lua
2. Uncomment `{ import = 'custom.plugins' }` and add files to `lua/custom/plugins/`
