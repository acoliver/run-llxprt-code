# Task 2.1: Core File Branding Updates

## Objective
Update package.json, README.md headers, and action metadata with LLxprt Code branding.

## Context
Core configuration files need branding updates to reflect the change from gemini-cli to llxprt-code. This task focuses on the main identifying information in key files.

## Files to Modify
1. `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/package.json`
2. `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/README.md` (headers and title only)

## Specific Changes Required

### 1. package.json Updates

Update these fields:
```json
{
  "name": "run-llxprt-code",  // was: "run-gemini-cli"
  "description": "GitHub Action for running LLxprt Code",
  "keywords": [
    "llxprt",     // was: "gemini"
    "llxprt-code", // was: "gemini-cli"
    "ai",
    "actions"
  ],
  "author": "Vybestack",  // or appropriate author, was: "Google"
  "homepage": "https://github.com/acoliver/run-llxprt-code",
  "repository": {
    "type": "git",
    "url": "git+https://github.com/acoliver/run-llxprt-code.git"
  },
  "bugs": {
    "url": "https://github.com/acoliver/run-llxprt-code/issues"
  }
}
```

### 2. README.md Header Updates

Update ONLY these sections for now:

**Title and badges:**
```markdown
# run-llxprt-code

[Update any badges to point to correct repository]
```

**Overview paragraph:**
```markdown
`run-llxprt-code` is a GitHub Action that integrates [LLxprt Code] into your development workflow...
```

**First mentions:**
- Change `run-gemini-cli` to `run-llxprt-code`
- Change `[Gemini CLI]` to `[LLxprt Code]`
- Change `[Gemini]` to `[AI models]` or `[Multiple AI providers]`

**DO NOT change the command examples yet** (@gemini-cli → @llxprt is task 2.2)

### 3. Copyright/Attribution Updates

Remove or update copyright notices:
- Remove "Google Cloud support" disclaimers
- Remove "not an officially supported Google product" warnings
- Add appropriate attribution for LLxprt Code

## Implementation Instructions

1. Read package.json and update the specified fields
2. Preserve all other package.json content (dependencies, scripts, etc.)
3. Read README.md and update ONLY the header/title sections
4. Do not change command examples or detailed documentation yet
5. Ensure all URLs point to the correct repository

## Validation

After implementation:
1. package.json has correct name and metadata
2. README.md title says "run-llxprt-code"
3. Overview mentions LLxprt Code, not Gemini CLI
4. Repository URLs are correct
5. Author attribution is updated
6. No Google-specific disclaimers remain in headers

## Important Notes

- This task is ONLY for core branding in main files
- Don't update command examples (@gemini-cli) yet - that's task 2.2
- Don't update the detailed documentation - that's Phase 3
- Focus on package identity and main headers only
- Preserve all functional content

## Expected Outcome

The package will be properly identified as:
1. Named "run-llxprt-code" in package.json
2. Titled "run-llxprt-code" in README
3. Described as LLxprt Code action
4. Attributed to correct author
5. Pointing to correct repository