# Task 1.1: Update action.yml Core Configuration

## Objective
Update the main action.yml file with llxprt-code configuration while preserving backward compatibility.

## Context
You are converting a GitHub Action from using `gemini-cli` to `llxprt-code`. The action.yml file at `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/action.yml` contains the core action configuration.

## Requirements (from overview.md)
- Replace references to "gemini-cli" with "llxprt-code" or "LLxprt Code"
- Change CLI command from `gemini` to `llxprt`
- Update package references from `@google/gemini-cli` to `@vybestack/llxprt-code`
- Use `.llxprt/` directory instead of `.gemini/`
- Update bot identity from `gemini-cli[bot]` to `llxprt[bot]`
- Remove Google LLC authorship, update with appropriate attribution

## Specific Changes Required

### 1. Action Metadata (lines 15-18)
- Change `name: 'Run Gemini CLI'` to `name: 'Run LLxprt Code'`
- Update `author: 'Google LLC'` to appropriate author
- Change description from "Invoke the Gemini CLI" to "Invoke LLxprt Code"

### 2. Input Renaming (lines 54-57)
- Rename `gemini_cli_version` to `llxprt_version`
- Keep description mentioning it controls which version to install
- Maintain default: 'latest'

### 3. Directory Updates (lines 67, 71-72, 98-99)
- Replace all `.gemini/` references with `.llxprt/`
- This includes settings directory creation and telemetry setup

### 4. Version Check (line 133)
- Change `gemini --version` to `llxprt --version`

### 5. CLI Execution (line 143)
- Change `gemini --yolo --prompt` to `llxprt --yolo --prompt`

## Implementation Instructions

1. Read the current action.yml file
2. Make the changes listed above using Edit or MultiEdit
3. Preserve all other functionality exactly as-is
4. Do NOT modify the installation logic yet (lines 109-137) - that's a separate task
5. Do NOT add new inputs yet - that's task 1.2
6. Ensure YAML remains valid

## Validation
After making changes:
1. Verify YAML syntax is valid
2. Check all gemini references in metadata are updated
3. Confirm .gemini directories changed to .llxprt
4. Verify version and execution commands use llxprt

## Output
Modified action.yml file with core branding and configuration updates, ready for next phase.

## Important Notes
- This task focuses ONLY on basic branding/naming updates
- Installation logic updates come in task 1.3
- New provider inputs come in task 1.2
- Maintain exact structure and formatting of the original file