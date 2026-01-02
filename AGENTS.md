# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a kickstart.nvim configuration - a minimal, single-file Neovim configuration designed as a starting point for personal customization. It is NOT a Neovim distribution, but rather a teaching tool with comprehensive inline documentation.

**Key Philosophy**: Every line of code should be readable and understandable. The configuration is intentionally kept in a single file (init.lua) for educational purposes, though users may choose to modularize it later.

## Configuration Structure

### Core Files
- `init.lua` - Main configuration file containing all settings, keymaps, and plugin definitions
- `lua/kickstart/plugins/*.lua` - Optional plugin configurations that can be enabled by uncommenting requires
- `lua/custom/plugins/*.lua` - Directory for user's own plugin additions (empty by default)
- `lua/kickstart/health.lua` - Health check module for verifying system setup
- `.stylua.toml` - Formatting configuration for Lua code

### Plugin Management
Uses lazy.nvim as the plugin manager. Plugins are configured inline within the `require('lazy').setup()` call in init.lua.

## Essential Commands

### Plugin Management
- `:Lazy` - View plugin status (press `?` for help, `q` to close)
- `:Lazy update` - Update all plugins
- `:checkhealth` - Run health checks to verify configuration

### LSP and Tools
- `:Mason` - Open Mason UI to install/manage LSP servers, formatters, and linters (press `g?` for help)
- `:ConformInfo` - View conform.nvim formatter information

### Formatting
- `<leader>f` - Format current buffer (leader is `<space>`)
- Format on save is enabled by default (except for C/C++)

### Diagnostics
- `<leader>q` - Open diagnostic quickfix list
- `:checkhealth kickstart` - Run kickstart-specific health checks

## Important Keybindings

Leader key: `<space>`

### Search (Telescope)
- `<leader>sh` - Search help
- `<leader>sk` - Search keymaps
- `<leader>sf` - Search files
- `<leader>sg` - Search by grep (live grep)
- `<leader>sd` - Search diagnostics
- `<leader>sr` - Search resume
- `<leader>sn` - Search Neovim config files
- `<leader><leader>` - Find existing buffers
- `<leader>/` - Fuzzy search in current buffer

### LSP
- `grn` - Rename symbol
- `gra` - Code action
- `grr` - Go to references
- `gri` - Go to implementation
- `grd` - Go to definition
- `grD` - Go to declaration
- `grt` - Go to type definition
- `gO` - Document symbols
- `gW` - Workspace symbols
- `<leader>th` - Toggle inlay hints

### Window Navigation
- `<C-h/j/k/l>` - Move focus between windows
- `<Esc>` - Clear search highlights

## Architecture

### Single-File Design
The configuration intentionally uses a single init.lua file containing:
1. Leader key configuration (must be set before plugins load)
2. Basic Vim options and settings
3. Basic keymaps
4. Autocommands
5. lazy.nvim plugin manager bootstrap
6. Plugin specifications with inline configuration

### Plugin Loading Strategy
Plugins use various loading strategies:
- `event = 'VimEnter'` - Load on Vim start (which-key, telescope, blink.cmp)
- `event = 'BufWritePre'` - Load before buffer write (conform.nvim)
- `ft = 'lua'` - Load for specific filetype (lazydev)
- `opts = {}` - Automatically calls setup() and forces plugin load
- `config = function() ... end` - Custom setup logic

### LSP Configuration Flow
1. Mason installs LSP servers and tools to Neovim's stdpath
2. mason-lspconfig bridges Mason and lspconfig
3. mason-tool-installer ensures specified tools are installed
4. LSP servers are configured in the `servers` table with optional overrides
5. LspAttach autocmd sets up buffer-local keymaps when LSP attaches
6. Capabilities are enhanced by blink.cmp for autocompletion

### Completion Stack
- blink.cmp - Completion engine (default preset uses `<c-y>` to accept)
- LuaSnip - Snippet engine
- lazydev - Lua LSP for Neovim config development
- LSP servers provide completions

### Formatting
conform.nvim handles formatting with:
- Format on save enabled (except C/C++)
- `<leader>f` manual format
- Falls back to LSP formatting if no formatter configured
- stylua configured for Lua files

## Development Workflow

### Adding New Plugins
1. Add plugin spec to the `require('lazy').setup()` table in init.lua
2. Use `opts = {}` for simple setup or `config = function() ... end` for custom configuration
3. Run `:Lazy` to verify installation

### Adding Custom Plugins
Place plugin files in `lua/custom/plugins/*.lua` and uncomment the import line:
```lua
{ import = 'custom.plugins' }
```

### Enabling Optional Kickstart Plugins
Uncomment require statements around line 981-986 in init.lua:
- `require 'kickstart.plugins.debug'` - DAP debugging setup
- `require 'kickstart.plugins.indent_line'` - Indent guides
- `require 'kickstart.plugins.lint'` - Linting with nvim-lint
- `require 'kickstart.plugins.autopairs'` - Auto-close brackets
- `require 'kickstart.plugins.neo-tree'` - File explorer
- `require 'kickstart.plugins.gitsigns'` - Extended git signs keymaps

### Adding LSP Servers
1. Add server name to `servers` table in init.lua (around line 678)
2. Optionally provide configuration overrides (cmd, filetypes, capabilities, settings)
3. Mason will automatically install when configured via mason-tool-installer

### Formatting Code
Use stylua for Lua formatting:
```bash
stylua .
```

Configuration in .stylua.toml:
- 2-space indentation
- Unix line endings
- Single quotes preferred
- No call parentheses for function calls

## Important Configuration Details

### Leader Key
Must be set to `<space>` BEFORE plugins load (line 90-91). This is critical for proper keymap configuration.

### Nerd Font
Set `vim.g.have_nerd_font = true` in init.lua if using a Nerd Font. This affects:
- which-key icon display
- nvim-web-devicons
- statusline icons
- diagnostic signs

### Clipboard
Clipboard syncs with OS (`clipboard = 'unnamedplus'`) but is set in a scheduled function to avoid startup delay.

### Diagnostic Configuration
Customized to:
- Show rounded borders on float windows
- Only underline ERROR severity
- Sort by severity
- Custom virtual text formatting
- Conditional Nerd Font icons

### Version Compatibility
Includes compatibility handling for Neovim 0.10 vs 0.11 (see `client_supports_method` function around line 585).

## Troubleshooting

### Plugin Installation Issues
1. Run `:checkhealth` to identify problems
2. Check `:Lazy` for plugin status
3. Verify external dependencies: git, make, unzip, gcc, ripgrep, fd-find

### LSP Issues
1. Run `:checkhealth` to verify LSP setup
2. Check `:Mason` to ensure servers are installed
3. Verify server is running with `:LspInfo`

### Windows-Specific
- telescope-fzf-native may require CMake and Microsoft C++ Build Tools
- Alternative: Use chocolatey to install gcc/make (see README.md)
- LuaSnip jsregexp build step disabled on Windows by default
