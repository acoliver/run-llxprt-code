# LLxprt Code GitHub Action - Conversion Plan Overview

## Project Objective

Convert the `run-gemini-cli` GitHub Action to `run-llxprt-code`, enabling multi-provider AI capabilities in GitHub workflows while maintaining backward compatibility with existing Gemini functionality.

## Core Requirements

### 1. Branding and Identity

- Replace all references to "gemini-cli" with "llxprt-code" or "LLxprt Code"
- Update command references from `@gemini-cli` to `@llxprt`
- Change CLI command from `gemini` to `llxprt`
- Update package references from `@google/gemini-cli` to `@vybestack/llxprt-code`
- Use `.llxprt/` directory instead of `.gemini/`
- Update bot identity from `gemini-cli[bot]` to `llxprt[bot]`
- Remove Google LLC authorship, update with appropriate attribution

### 2. Multi-Provider Support

#### 2.1 Provider Configuration
- Support multiple AI providers: Anthropic, OpenAI, Google Gemini, and others
- Add `provider` input parameter (default: 'gemini' for backward compatibility)
- Add `model` input parameter for model selection
- Enable runtime provider switching via action inputs

#### 2.2 Authentication Flexibility
- Support API key authentication via `api_key` input
- Preserve `gemini_api_key` input for backward compatibility
- Enable dual-provider mode: main provider for chat + Gemini for ServerTools (web-search/fetch)
- Support authentication via CLI flags: `--provider`, `--key`, `--keyfile`
- Maintain Google OAuth flow support for free-tier Gemini access
- Continue supporting Workload Identity Federation for Google Cloud

### 3. Backward Compatibility

- Existing workflows using `gemini_api_key` must continue working
- Default to Gemini provider when no provider specified
- Preserve all Google Cloud authentication methods (OAuth, API key, Vertex AI)
- Keep GEMINI_API_KEY environment variable functional
- Maintain support for existing action inputs and outputs

### 4. Installation and Execution

- Update package installation to use `@vybestack/llxprt-code`
- Change version input from `gemini_cli_version` to `llxprt_version`
- Update CLI invocation from `gemini` to `llxprt`
- Ensure compatibility with llxprt's command-line interface
- Support both npm installation and git clone methods

### 5. Documentation Updates

#### 5.1 Primary Documentation
- Update README.md with multi-provider examples
- Add provider-specific configuration guides
- Create migration guide from run-gemini-cli
- Document dual-provider setup for ServerTools

#### 5.2 Authentication Documentation
- Expand beyond Google-only authentication
- Add sections for Anthropic, OpenAI, and other providers
- Document API key management best practices
- Explain ServerTools authentication scenario

#### 5.3 Workflow Examples
- Convert existing workflow examples to use llxprt
- Add provider-specific workflow examples
- Include dual-provider configuration examples
- Update issue triage and PR review workflows

### 6. Configuration Files

- Rename GEMINI.md to LLXPRT.md throughout examples
- Update settings path references to `.llxprt/settings.json`
- Support provider-specific configuration in settings
- Enable model parameter configuration

## Success Criteria

### Functional Requirements

1. **Multi-Provider Capability**
   - Users can specify any supported provider via action inputs
   - API keys are properly passed to the selected provider
   - Model selection works for each provider

2. **ServerTools Support**
   - Web-search and web-fetch work regardless of main provider
   - Gemini authentication (API key or OAuth) enables ServerTools
   - Dual-provider mode functions correctly

3. **Backward Compatibility**
   - Existing workflows continue functioning without modification
   - Gemini-only workflows work as before
   - All authentication methods remain supported

4. **Command Compatibility**
   - `@llxprt` triggers work in issues and PRs
   - All slash commands function correctly
   - Bot responses use correct identity

### Non-Functional Requirements

1. **User Experience**
   - Clear migration path from run-gemini-cli
   - Intuitive provider configuration
   - Helpful error messages for authentication issues

2. **Documentation Quality**
   - Complete examples for each provider
   - Clear authentication setup guides
   - Troubleshooting section for common issues

3. **Maintainability**
   - Clean separation of provider-specific logic
   - Reusable patterns for adding new providers
   - Clear configuration schema

## Out of Scope

- Creating new GitHub Action features beyond provider support
- Modifying core llxprt-code functionality
- Building provider-specific optimizations
- Implementing new authentication methods not in llxprt

## Dependencies

- `@vybestack/llxprt-code` package must be published and accessible
- llxprt CLI must support all required command-line flags
- Provider API keys must be obtainable by users
- GitHub Action environment supports required Node.js version

## Risks and Mitigations

### Risk: Breaking existing workflows
**Mitigation:** Extensive backward compatibility testing, default to Gemini provider

### Risk: Authentication complexity
**Mitigation:** Clear documentation, support multiple auth methods, helpful error messages

### Risk: Provider API differences
**Mitigation:** Rely on llxprt's provider abstraction, don't implement provider-specific logic in action

### Risk: User confusion during migration
**Mitigation:** Comprehensive migration guide, maintain parallel documentation during transition

## Definition of Done

- [ ] All gemini-cli references replaced with llxprt-code
- [ ] Multi-provider configuration fully functional
- [ ] Dual-provider mode (ServerTools) working
- [ ] Backward compatibility verified
- [ ] All workflow examples updated and tested
- [ ] Documentation complete and reviewed
- [ ] Migration guide published
- [ ] Action published and accessible
- [ ] Integration tests passing for all providers
- [ ] Community feedback incorporated from beta testing