# CodeCompanion.nvim Setup Guide

## Overview
CodeCompanion.nvim has been configured with OpenRouter as the provider, giving you access to multiple AI models for code assistance directly in Neovim.

## Configuration Status
✅ **Fully Configured** - All interaction types are set up and ready to use:
- **Chat** - Interactive AI chat in a buffer
- **Inline** - Direct code modifications with diff view
- **Cmd** - Command-line interactions
- **Agent** - Background AI assistance

## Available AI Models
The default model is **Claude 3.5 Sonnet**, but you can switch between:
- anthropic/claude-3.5-sonnet (default)
- anthropic/claude-3-opus
- openai/gpt-4-turbo
- openai/gpt-4
- openai/gpt-3.5-turbo
- google/gemini-pro
- google/gemini-pro-1.5
- meta-llama/llama-3-70b-instruct
- meta-llama/codellama-70b-instruct
- deepseek/deepseek-coder
- deepseek/deepseek-chat

## Environment Setup
Before using CodeCompanion, set your OpenRouter API key:

```bash
# Add to your shell profile (.bashrc, .zshrc, etc.)
export OPENROUTER_API_KEY="your-api-key-here"
```

Get your API key from: https://openrouter.ai/keys

## Key Bindings

### Chat Mode
- `<leader>cc` - Toggle CodeCompanion chat window
- `<leader>cp` - Add selected code to chat
- `<LocalLeader>a` - Alternative chat toggle
- In chat buffer:
  - `<CR>` (normal mode) or `<C-CR>` (insert mode) - Submit message
  - `q` (normal mode) or `<C-c>` (insert mode) - Close chat
  - `<C-c>` (normal mode) - Stop generation
  - `ga` - Accept diff suggestion
  - `gr` - Reject diff suggestion
  - `gd` - Toggle diff view

### Inline Mode (Code Modifications)
- `<leader>ci` - Open inline prompt for code modification
- **Visual mode quick actions:**
  - `<leader>cr` - Refactor selected code
  - `<leader>cf` - Fix bugs in selected code
  - `<leader>ce` - Explain selected code
  - `<leader>ct` - Generate tests for selected code
- In inline diff view:
  - `ga` - Accept changes
  - `gr` - Reject changes

### Command Mode
- `<leader>cm` - Open command prompt for AI interactions
- Use in normal or visual mode for context-aware commands

### Actions Menu
- `<leader>ca` or `<C-a>` - Open actions palette (Telescope menu)
- `ga` (visual mode) - Quick add to chat

## Usage Examples

### 1. Chat Interaction
```vim
" Open a file and press <leader>cc to start chatting
" Type your question and press <CR> to submit
" Example: "Explain this function and suggest improvements"
```

### 2. Inline Code Modification
```vim
" Select code in visual mode
" Press <leader>ci and type your modification request
" Example: "Add error handling to this function"
" Review the diff and press ga to accept or gr to reject
```

### 3. Quick Refactoring
```vim
" Select code block in visual mode
" Press <leader>cr for automatic refactoring
" The AI will suggest improvements inline
```

### 4. Generate Tests
```vim
" Select a function in visual mode
" Press <leader>ct to generate unit tests
" Tests will be suggested based on your code
```

### 5. Command Mode
```vim
" Press <leader>cm in normal mode
" Type a command like: "Create a README for this project"
" AI will generate content based on context
```

## Slash Commands in Chat
While in chat mode, you can use these commands:
- `/file` - Insert a file into the conversation
- `/buffer` - Insert content from open buffers
- `/symbols` - Insert symbols from current buffer
- `/help` - Insert help documentation

## Tips & Tricks

1. **Context Matters**: Select relevant code before invoking commands for better results
2. **Iterate**: Use the chat to refine suggestions iteratively
3. **Diff Review**: Always review inline changes before accepting
4. **Model Selection**: Change models in chat with the settings panel
5. **Token Count**: Monitor token usage shown in the chat window

## Troubleshooting

### API Key Not Working
```bash
# Verify the environment variable is set
echo $OPENROUTER_API_KEY

# Restart Neovim after setting the key
```

### Commands Not Working
```vim
" Check if plugin is loaded
:Lazy
" Look for codecompanion.nvim and ensure it's loaded

" Check health
:checkhealth codecompanion
```

### Keybindings Conflict
If keybindings conflict with other plugins, you can modify them in the configuration section of init.lua.

## Advanced Features

### Custom Prompts
The configuration includes pre-defined prompts for:
- Code explanation
- Refactoring
- Test generation
- Documentation
- Bug fixing
- Performance optimization
- Grammar improvement (for text)
- Text summarization

### Inline Prompts
Quick actions available with `:CodeCompanion <action>`:
- `refactor` - Improve code structure
- `fix` - Fix bugs
- `explain` - Add comments
- `tests` - Generate tests

## Model Costs
OpenRouter charges per token. Check current pricing at: https://openrouter.ai/models

## Next Steps
1. Set your OPENROUTER_API_KEY environment variable
2. Restart Neovim
3. Try `<leader>cc` to open chat
4. Select some code and press `<leader>ca` to see available actions

Enjoy your AI-powered coding experience! 🚀

