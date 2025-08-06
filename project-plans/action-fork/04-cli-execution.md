# Task 1.4: Implement CLI Execution with Provider Support

## Objective
Update CLI invocation to support provider flags and dual-provider mode, enabling multi-provider functionality.

## Context
The llxprt CLI is now installed and basic branding is updated. The final step is to modify how the CLI is executed to support provider selection and API key passing via command-line flags.

## Requirements (from overview.md)
- Support authentication via CLI flags: `--provider`, `--key`, `--keyfile`
- Enable dual-provider mode: main provider for chat + Gemini for ServerTools
- Maintain backward compatibility with existing workflows

## File to Modify
`/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/action.yml`

## Current Execution (line ~143)
```yaml
gemini --yolo --prompt "${PROMPT}"
```

## Specific Changes Required

### 1. Build Dynamic Command with Provider Flags
Replace the simple execution with a dynamic command that includes provider configuration:

```bash
# Build the llxprt command with optional flags
LLXPRT_CMD="llxprt --yolo"

# Add provider flag if specified
if [[ -n "${LLXPRT_PROVIDER}" ]]; then
  LLXPRT_CMD="${LLXPRT_CMD} --provider ${LLXPRT_PROVIDER}"
fi

# Add model flag if specified
if [[ -n "${LLXPRT_MODEL}" ]]; then
  LLXPRT_CMD="${LLXPRT_CMD} --model ${LLXPRT_MODEL}"
fi

# Add API key flag if specified (from api_key input)
if [[ -n "${LLXPRT_API_KEY}" ]]; then
  LLXPRT_CMD="${LLXPRT_CMD} --key ${LLXPRT_API_KEY}"
fi

# Add API key file flag if specified
if [[ -n "${LLXPRT_API_KEY_FILE}" ]]; then
  LLXPRT_CMD="${LLXPRT_CMD} --keyfile ${LLXPRT_API_KEY_FILE}"
fi

# Add the prompt (always required)
LLXPRT_CMD="${LLXPRT_CMD} --prompt \"${PROMPT}\""

# Execute the command
eval ${LLXPRT_CMD}
```

### 2. Ensure Dual-Provider Mode Support
The GEMINI_API_KEY should already be exported for ServerTools:
```bash
# This should already exist from original code:
export GEMINI_API_KEY="${{ inputs.gemini_api_key }}"

# This enables Gemini for ServerTools even when using another provider
```

### 3. Update the Execution Section
Locate where the CLI is invoked (around line 143) and replace with the dynamic command building logic.

## Implementation Instructions

1. Read the action.yml file
2. Find the CLI execution section (where `gemini --yolo --prompt` is called)
3. Replace the simple command with the dynamic command building logic
4. Ensure environment variables (LLXPRT_PROVIDER, LLXPRT_MODEL, etc.) are available
5. Verify GEMINI_API_KEY export remains for ServerTools support
6. Test that the command construction logic is correct

## Validation

After implementation, verify these scenarios would work:

### Scenario 1: Backward Compatible (Gemini only)
```yaml
with:
  gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
```
Should execute: `llxprt --yolo --prompt "..."`

### Scenario 2: Alternative Provider
```yaml
with:
  provider: anthropic
  api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```
Should execute: `llxprt --yolo --provider anthropic --key <key> --prompt "..."`

### Scenario 3: Dual-Provider Mode
```yaml
with:
  provider: openai
  api_key: ${{ secrets.OPENAI_API_KEY }}
  gemini_api_key: ${{ secrets.GEMINI_API_KEY }}
```
Should execute: `llxprt --yolo --provider openai --key <key> --prompt "..."`
With GEMINI_API_KEY exported for ServerTools

### Scenario 4: With Model Selection
```yaml
with:
  provider: anthropic
  model: claude-3-opus-20240229
  api_key: ${{ secrets.ANTHROPIC_API_KEY }}
```
Should execute: `llxprt --yolo --provider anthropic --model claude-3-opus-20240229 --key <key> --prompt "..."`

## Important Notes

- The command must be built dynamically based on which inputs are provided
- Empty/unset variables should not add flags to the command
- The --yolo flag should always be present (non-interactive mode)
- The --prompt flag is always required
- GEMINI_API_KEY environment variable enables ServerTools
- Use proper quote escaping for the prompt value

## Expected Outcome

The CLI execution will:
1. Build command dynamically based on inputs
2. Support provider and model selection
3. Pass API keys via command-line flags
4. Maintain backward compatibility
5. Enable dual-provider mode for ServerTools