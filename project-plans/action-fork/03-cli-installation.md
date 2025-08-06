# Task 1.3: Update CLI Installation Logic

## Objective
Replace gemini-cli installation with llxprt-code package installation in the action.yml file.

## Context
The action currently installs `@google/gemini-cli` via npm or git clone. This needs to be updated to install `@vybestack/llxprt-code` instead.

## Requirements (from overview.md)
- Update package installation to use `@vybestack/llxprt-code`
- Support both npm installation and git clone methods
- Ensure compatibility with llxprt's command-line interface
- Change version input from `gemini_cli_version` to `llxprt_version` (already done in task 1.1)

## File to Modify
`/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/action.yml`

## Current Installation Logic (lines ~109-137)
The current code has two installation paths:
1. NPM installation of `@google/gemini-cli`
2. Git clone from `https://github.com/google-gemini/gemini-cli.git`

## Specific Changes Required

### 1. Update NPM Installation Path
Find the section that installs via npm (around line 120-121):
```yaml
# Current:
npm install -g "@google/gemini-cli@${VERSION_INPUT}"

# Change to:
npm install -g "@vybestack/llxprt-code@${VERSION_INPUT}"
```

### 2. Update Git Clone Path
Find the section that clones from git (around line 123-130):
```yaml
# Current:
git clone https://github.com/google-gemini/gemini-cli.git

# Change to:
git clone https://github.com/acoliver/llxprt-code.git
```

### 3. Update Directory References
After git clone, the code changes into the cloned directory:
```yaml
# Current:
cd gemini-cli

# Change to:
cd llxprt-code
```

### 4. Update Package.json Check
The code checks for package.json existence:
```yaml
# The path reference should now be llxprt-code/package.json
```

### 5. Update Version Input Variable
The code uses `VERSION_INPUT` which comes from:
```yaml
# Should already be using inputs.llxprt_version from task 1.1
VERSION_INPUT="${{ inputs.llxprt_version }}"
```

## Implementation Instructions

1. Read the action.yml file
2. Locate the installation section (starts with "Install the CLI")
3. Update the npm install command to use `@vybestack/llxprt-code`
4. Update the git clone URL to `https://github.com/acoliver/llxprt-code.git`
5. Update any directory references from `gemini-cli` to `llxprt-code`
6. Verify VERSION_INPUT uses `inputs.llxprt_version`
7. Preserve all other logic (version checking, error handling, etc.)

## Validation

After implementation:
1. NPM installation path uses `@vybestack/llxprt-code`
2. Git clone uses correct repository URL
3. Directory navigation uses `llxprt-code`
4. Version variable uses `llxprt_version` input
5. Installation logic structure remains intact
6. Error handling preserved

## Important Notes

- Keep BOTH installation methods (npm and git clone)
- The conditional logic choosing between them should remain
- Don't change the overall structure, just the package/repo references
- The npm package must be `@vybestack/llxprt-code` (with the @vybestack scope)
- The git repository is at `https://github.com/acoliver/llxprt-code.git`

## Expected Outcome

The installation section will:
1. Check for version input (now llxprt_version)
2. Try npm installation of `@vybestack/llxprt-code`
3. Fall back to git clone from correct repository
4. Successfully install llxprt CLI
5. Continue to version check with `llxprt --version`