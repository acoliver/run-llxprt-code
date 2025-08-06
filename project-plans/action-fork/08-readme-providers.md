# Task 3.1: README.md Provider Documentation

## Objective
Add comprehensive multi-provider configuration examples and migration guide to the README.md file.

## Context
The README currently only documents Gemini usage. It needs to be expanded to showcase LLxprt Code's multi-provider capabilities and help users migrate from the original gemini-cli action.

## File to Modify
`/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/README.md`

## Sections to Add/Update

### 1. Features Section Update
After the overview, update the features to highlight multi-provider support:

```markdown
## Features

- **Multi-Provider Support**: Use Anthropic Claude, OpenAI GPT, Google Gemini, and more
- **Flexible Authentication**: API keys, OAuth, and Workload Identity Federation
- **Dual-Provider Mode**: Use Gemini for web search while using another provider for chat
- **Backward Compatible**: Existing Gemini workflows continue to work
- **Automation**: Trigger workflows based on events or schedules
- **On-demand Collaboration**: Use `@llxprt` in comments to get AI assistance
- **Extensible with Tools**: Leverage tool-calling capabilities across providers
```

### 2. Add Provider Configuration Section
Add this new section after Quick Start:

```markdown
## Provider Configuration

LLxprt Code supports multiple AI providers. Configure your preferred provider using action inputs:

### Anthropic Claude
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'anthropic'
    model: 'claude-3-5-sonnet-20241022'  # Optional, uses provider default if not specified
    api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
```

### OpenAI GPT
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'openai'
    model: 'gpt-4-turbo-preview'  # Optional
    api_key: '${{ secrets.OPENAI_API_KEY }}'
```

### Google Gemini (Default)
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    gemini_api_key: '${{ secrets.GEMINI_API_KEY }}'
    # OR use OAuth (no key needed)
```

### Dual-Provider Mode (Advanced)
Use Gemini for web search/fetch while using another provider for chat:
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'anthropic'
    api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
    gemini_api_key: '${{ secrets.GEMINI_API_KEY }}'  # Enables web tools
```
```

### 3. Update Inputs Documentation
Replace the current inputs section with:

```markdown
### Inputs

- `provider`: _(Optional, default: `gemini`)_ The AI provider to use (anthropic, openai, gemini, etc.)
- `model`: _(Optional)_ The model to use for the specified provider. If not set, uses provider defaults.
- `api_key`: _(Optional)_ API key for the specified provider.
- `api_key_file`: _(Optional)_ Path to file containing the API key.
- `gemini_api_key`: _(Optional)_ Gemini API key for backward compatibility or ServerTools in dual-provider mode.
- `prompt`: _(Optional, default: `You are a helpful assistant.`)_ The prompt to pass to LLxprt Code.
- `settings`: _(Optional)_ JSON string for `.llxprt/settings.json` configuration.
- `llxprt_version`: _(Optional, default: `latest`)_ Version of LLxprt Code to install.
[... keep other existing inputs ...]
```

### 4. Add Migration Guide Section
Add before the Authentication section:

```markdown
## Migration from run-gemini-cli

If you're currently using `google-github-actions/run-gemini-cli`, migration is straightforward:

### Quick Migration
1. **Update your workflow file:**
   ```yaml
   # Old:
   - uses: 'google-github-actions/run-gemini-cli@v0'
   
   # New:
   - uses: 'acoliver/run-llxprt-code@v1'
   ```

2. **Update command references:**
   - Change `@gemini-cli` to `@llxprt` in your comments
   - Update any documentation or templates

3. **Your existing configuration continues to work:**
   ```yaml
   with:
     gemini_api_key: '${{ secrets.GEMINI_API_KEY }}'
   ```

### Taking Advantage of New Features
Once migrated, you can:
- Switch to other providers (Anthropic, OpenAI)
- Use dual-provider mode for enhanced capabilities
- Leverage provider-specific models and features

### Backward Compatibility
- All existing Gemini authentication methods still work
- Default provider is Gemini when not specified
- `GEMINI_API_KEY` environment variable is still supported
```

### 5. Update Examples Section
Add provider-specific examples:

```markdown
### Examples

#### PR Review with Claude
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'anthropic'
    model: 'claude-3-5-sonnet-20241022'
    api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
    prompt: 'Review this pull request for code quality and potential issues'
```

#### Issue Triage with GPT-4
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'openai'
    model: 'gpt-4-turbo-preview'
    api_key: '${{ secrets.OPENAI_API_KEY }}'
    prompt: 'Triage this issue and suggest appropriate labels'
```
```

## Implementation Instructions

1. Read the current README.md
2. Add the new sections in the appropriate locations
3. Update the existing inputs documentation
4. Ensure all examples use the new action reference
5. Keep existing Gemini examples for backward compatibility reference
6. Add clear migration instructions

## Validation

After implementation:
1. Features section highlights multi-provider support
2. Provider configuration section has examples for each provider
3. Migration guide is clear and complete
4. Inputs documentation includes new parameters
5. Examples show different provider usage
6. Backward compatibility is clearly explained

## Important Notes

- Keep existing Gemini documentation for backward compatibility
- Make it clear that Gemini is the default provider
- Emphasize that existing workflows will continue to work
- Highlight the dual-provider capability for ServerTools
- Use consistent action version references (v1)

## Expected Outcome

Users will be able to:
1. Understand multi-provider capabilities immediately
2. Configure their preferred provider easily
3. Migrate from run-gemini-cli smoothly
4. Use dual-provider mode when needed
5. Find examples for their use case