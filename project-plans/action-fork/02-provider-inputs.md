# Task 1.2: Implement Multi-Provider Inputs

## Objective
Add provider, model, and api_key inputs to action.yml with proper defaults to support multi-provider functionality.

## Context
The action.yml file has been updated with basic llxprt branding. Now you need to add new input parameters to support multiple AI providers while maintaining backward compatibility with existing Gemini workflows.

## Requirements (from overview.md)
- Add `provider` input parameter (default: 'gemini' for backward compatibility)
- Add `model` input parameter for model selection
- Support API key authentication via `api_key` input
- Preserve `gemini_api_key` input for backward compatibility
- Enable dual-provider mode: main provider for chat + Gemini for ServerTools

## File to Modify
`/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/action.yml`

## Specific Changes Required

### 1. Add New Input Parameters
Add the following inputs in the `inputs:` section (after existing inputs, before `llxprt_version`):

```yaml
  provider:
    description: 'AI provider to use (gemini, openai, anthropic, etc.)'
    required: false
    default: 'gemini'
    
  model:
    description: 'Model to use for the specified provider'
    required: false
    
  api_key:
    description: 'API key for the specified provider'
    required: false
    
  api_key_file:
    description: 'Path to file containing API key for the specified provider'
    required: false
```

### 2. Keep Existing gemini_api_key Input
Ensure the `gemini_api_key` input remains for backward compatibility. Update its description to clarify its dual use:

```yaml
  gemini_api_key:
    description: 'Gemini API key (for backward compatibility or ServerTools when using other providers)'
    required: false
```

### 3. Add Environment Variable Mapping
In the `runs:` section where environment variables are set, add handling for the new inputs:

```yaml
# After existing GEMINI_API_KEY setup, add:
- name: 'Set provider configuration'
  shell: 'bash'
  run: |
    # Export provider configuration
    if [[ -n "${{ inputs.provider }}" ]]; then
      echo "LLXPRT_PROVIDER=${{ inputs.provider }}" >> "${GITHUB_ENV}"
    fi
    
    if [[ -n "${{ inputs.model }}" ]]; then
      echo "LLXPRT_MODEL=${{ inputs.model }}" >> "${GITHUB_ENV}"
    fi
    
    # Handle api_key - could be for any provider
    if [[ -n "${{ inputs.api_key }}" ]]; then
      echo "LLXPRT_API_KEY=${{ inputs.api_key }}" >> "${GITHUB_ENV}"
    fi
    
    if [[ -n "${{ inputs.api_key_file }}" ]]; then
      echo "LLXPRT_API_KEY_FILE=${{ inputs.api_key_file }}" >> "${GITHUB_ENV}"
    fi
```

### 4. Update Input Descriptions in Comments
Add comments explaining the dual-provider mode capability:

```yaml
# Add near the top of inputs section:
# LLxprt Code supports dual-provider mode where Gemini handles web-search/web-fetch
# while another provider handles chat. Set gemini_api_key AND provider/api_key for this mode.
```

## Implementation Instructions

1. Read the current action.yml file
2. Add the new input parameters in the correct location
3. Update the gemini_api_key description
4. Add environment variable mapping in the runs section
5. Add explanatory comments
6. Ensure proper YAML formatting and indentation

## Validation

After implementation:
1. Verify all new inputs are properly defined with descriptions
2. Check default value for provider is 'gemini'
3. Confirm gemini_api_key input still exists
4. Verify environment variable mapping is correct
5. Ensure YAML syntax remains valid

## Important Notes

- The `provider` input MUST default to 'gemini' for backward compatibility
- The `model` input should NOT have a default (llxprt will use provider defaults)
- Keep gemini_api_key as a separate input for backward compatibility
- The environment variable setup should happen BEFORE the CLI is invoked
- These inputs will be used in task 1.4 for CLI execution

## Expected Outcome

The action.yml file will have:
1. Four new input parameters (provider, model, api_key, api_key_file)
2. Updated gemini_api_key description
3. Environment variable mapping for provider configuration
4. Comments explaining dual-provider mode
5. Full backward compatibility with existing workflows