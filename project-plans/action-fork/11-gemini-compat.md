# Task 4.1: Gemini API Key Compatibility

## Objective
Ensure the gemini_api_key input continues working with proper routing to maintain full backward compatibility.

## Context
Existing users have workflows using `gemini_api_key`. This must continue to work exactly as before while also enabling ServerTools in dual-provider mode.

## Requirements (from overview.md)
- Existing workflows using `gemini_api_key` must continue working
- Keep GEMINI_API_KEY environment variable functional
- Enable dual-provider mode: main provider for chat + Gemini for ServerTools

## File to Modify
`/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/action.yml`

## Current State
After previous tasks:
- `gemini_api_key` input exists
- `provider` input defaults to 'gemini'
- CLI execution builds dynamic command

## Specific Implementation Required

### 1. Environment Variable Export
Ensure GEMINI_API_KEY is always exported when gemini_api_key is provided:

```bash
# This should already exist, verify it's still there:
- name: 'Set up Gemini API key'
  if: '${{ inputs.gemini_api_key }}'
  shell: 'bash'
  run: |
    echo "GEMINI_API_KEY=${{ inputs.gemini_api_key }}" >> "${GITHUB_ENV}"
    # This enables ServerTools even when using other providers
```

### 2. Routing Logic for Backward Compatibility
When ONLY gemini_api_key is provided (backward compat mode):

```bash
# In the CLI execution section, add logic:

# Backward compatibility: if only gemini_api_key is set and no provider specified,
# use it as the main API key for Gemini
if [[ -z "${LLXPRT_API_KEY}" && -n "${{ inputs.gemini_api_key }}" && "${LLXPRT_PROVIDER:-gemini}" == "gemini" ]]; then
  # Use gemini_api_key as the main key for Gemini provider
  LLXPRT_CMD="${LLXPRT_CMD} --key ${{ inputs.gemini_api_key }}"
fi
```

### 3. Dual-Provider Mode Support
The setup should support these scenarios:

**Scenario A: Legacy Gemini-only**
```yaml
with:
  gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
```
- Sets GEMINI_API_KEY environment variable
- Provider defaults to 'gemini'
- Uses gemini_api_key as --key for llxprt

**Scenario B: Dual-provider mode**
```yaml
with:
  provider: 'anthropic'
  api_key: ${{ secrets.ANTHROPIC_API_KEY }}
  gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
```
- Sets GEMINI_API_KEY environment variable (for ServerTools)
- Uses anthropic as provider with its API key
- Gemini available for web-search/fetch

**Scenario C: New Gemini with api_key**
```yaml
with:
  provider: 'gemini'
  api_key: ${{ secrets.GEMINI_API_KEY }}
```
- Uses api_key input for Gemini
- Also sets GEMINI_API_KEY env var if not set

## Implementation Instructions

1. Read action.yml
2. Verify GEMINI_API_KEY environment export exists and works
3. Add backward compatibility routing in CLI execution
4. Ensure dual-provider scenarios work correctly
5. Test all three scenarios mentally

## Validation Checklist

### Environment Variable Setting
- [ ] GEMINI_API_KEY gets exported when gemini_api_key input provided
- [ ] Export happens before CLI execution
- [ ] Export is unconditional (enables ServerTools)

### Backward Compatibility
- [ ] Legacy workflows with only gemini_api_key work
- [ ] Provider defaults to 'gemini'
- [ ] gemini_api_key gets passed as --key when appropriate

### Dual-Provider Mode
- [ ] GEMINI_API_KEY available for ServerTools
- [ ] Other provider's api_key used for main chat
- [ ] No conflict between the two keys

### Edge Cases
- [ ] Both api_key and gemini_api_key for Gemini provider
- [ ] No keys provided (OAuth fallback)
- [ ] Only api_key for Gemini (should also set GEMINI_API_KEY)

## Common Issues to Check

1. **Key Routing**: gemini_api_key must route correctly based on provider
2. **Environment Export**: GEMINI_API_KEY must always be set for ServerTools
3. **Priority**: api_key input should take precedence over gemini_api_key when both exist
4. **Default Behavior**: No provider specified = use Gemini

## Expected Outcome

The action will:
1. Maintain 100% backward compatibility with existing workflows
2. Export GEMINI_API_KEY for ServerTools support
3. Route keys correctly based on provider selection
4. Support all three usage patterns seamlessly