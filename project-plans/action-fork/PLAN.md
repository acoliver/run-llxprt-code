# LLxprt Code GitHub Action Conversion - Implementation Plan

This plan provides self-contained tasks for converting `run-gemini-cli` to `run-llxprt-code`. Each task is designed to be executed independently by subagents with clear verification steps.

Reference: [Overview and Requirements](./overview.md)

## Phase 1: Core Action Updates

### Task 1.1: Update action.yml Core Configuration
**File:** `01-action-yml-core.md`
**Description:** Update the main action.yml file with llxprt-code configuration while preserving backward compatibility

### Task 1.2: Implement Multi-Provider Inputs
**File:** `02-provider-inputs.md`
**Description:** Add provider, model, and api_key inputs to action.yml with proper defaults

### Task 1.3: Update CLI Installation Logic
**File:** `03-cli-installation.md`
**Description:** Replace gemini-cli installation with llxprt-code package installation

### Task 1.4: Implement CLI Execution with Provider Support
**File:** `04-cli-execution.md`
**Description:** Update CLI invocation to support provider flags and dual-provider mode

## Phase 2: Branding and Identity

### Task 2.1: Core File Branding Updates
**File:** `05-core-branding.md`
**Description:** Update package.json, README.md headers, and action metadata

### Task 2.2: Command Reference Updates
**File:** `06-command-references.md`
**Description:** Replace all @gemini-cli references with @llxprt throughout codebase

### Task 2.3: Directory and Configuration Updates
**File:** `07-directory-config.md`
**Description:** Update .gemini/ to .llxprt/ and GEMINI.md to LLXPRT.md references

## Phase 3: Documentation

### Task 3.1: README.md Provider Documentation
**File:** `08-readme-providers.md`
**Description:** Add multi-provider configuration examples and migration guide to README

### Task 3.2: Authentication Documentation Expansion
**File:** `09-auth-docs.md`
**Description:** Expand authentication.md to cover all providers and dual-provider setup

### Task 3.3: Update Workflow Examples
**File:** `10-workflow-examples.md`
**Description:** Convert example workflows to use llxprt with provider configurations

## Phase 4: Backward Compatibility

### Task 4.1: Gemini API Key Compatibility
**File:** `11-gemini-compat.md`
**Description:** Ensure gemini_api_key input continues working with proper routing

### Task 4.2: Default Provider Logic
**File:** `12-default-provider.md`
**Description:** Implement logic to default to Gemini when no provider specified

### Task 4.3: Google Cloud Authentication Preservation
**File:** `13-gcloud-auth.md`
**Description:** Verify and maintain Workload Identity and Vertex AI authentication paths

## Phase 5: Testing and Validation

### Task 5.1: Create Integration Test Workflows
**File:** `14-integration-tests.md`
**Description:** Build test workflows for each provider configuration

### Task 5.2: Migration Testing
**File:** `15-migration-test.md`
**Description:** Test migration path from existing gemini-cli workflows

## Phase 6: Final Verification

### Task 6.1: Full System Verification
**File:** `16-final-verification.md`
**Description:** Complete end-to-end testing of all requirements from overview.md

## Verification Agents

Each task will have a corresponding verification file (e.g., `01a-action-yml-core-verification.md`) that:
1. Checks implementation correctness
2. Validates requirements compliance
3. Tests for regressions
4. Identifies any "Claude fraud" (incomplete/incorrect implementations)

## Execution Notes

- Each task file will be self-contained with all context needed
- Tasks can be executed in parallel within phases
- Verification must pass before proceeding to next phase
- All paths will be relative to `/Volumes/XS1000/acoliver/projects/automerge/run-llxprt-code/`

## Success Metrics

- Zero breaking changes for existing workflows
- All providers functional with proper authentication
- Dual-provider mode operational for ServerTools
- Complete documentation coverage
- All verification steps passing