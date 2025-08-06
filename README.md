# run-llxprt-code

## Overview

`run-llxprt-code` is a GitHub Action that integrates [LLxprt Code] into your development workflow. It acts both as an autonomous agent for critical routine coding tasks, and an on-demand collaborator you can quickly delegate work to.

Use it to perform GitHub pull request reviews, triage issues, perform code analysis and modification, and more using [AI models] conversationally (e.g., `@llxprt fix this issue`) directly inside your GitHub repositories.


- [run-llxprt-code](#run-llxprt-code)
  - [Overview](#overview)
  - [Features](#features)
  - [Quick Start](#quick-start)
    - [1. Get a Gemini API Key](#1-get-a-gemini-api-key)
    - [2. Add it as a GitHub Secret](#2-add-it-as-a-github-secret)
    - [3. Choose a Workflow](#3-choose-a-workflow)
    - [4. Try it out!](#4-try-it-out)
  - [Provider Configuration](#provider-configuration)
    - [Anthropic Claude](#anthropic-claude)
    - [OpenAI GPT](#openai-gpt)
    - [Google Gemini (Default)](#google-gemini-default)
    - [Dual-Provider Mode (Advanced)](#dual-provider-mode-advanced)
  - [Workflows](#workflows)
    - [Issue Triage](#issue-triage)
    - [Pull Request Review](#pull-request-review)
    - [Gemini CLI Assistant](#gemini-cli-assistant)
  - [Migration from run-gemini-cli](#migration-from-run-gemini-cli)
    - [Quick Migration](#quick-migration)
    - [Taking Advantage of New Features](#taking-advantage-of-new-features)
    - [Backward Compatibility](#backward-compatibility)
    - [Inputs](#inputs)
    - [Outputs](#outputs)
    - [Repository Variables](#repository-variables)
    - [Secrets](#secrets)
  - [Authentication](#authentication)
    - [Google Authentication](#google-authentication)
    - [GitHub Authentication](#github-authentication)
    - [Examples](#examples)
  - [Observability](#observability)
  - [Customization](#customization)
  - [Contributing](#contributing)

## Features

- **Multi-Provider Support**: Use Anthropic Claude, OpenAI GPT, Google Gemini, and more
- **Flexible Authentication**: API keys, OAuth, and Workload Identity Federation
- **Dual-Provider Mode**: Use Gemini for web search while using another provider for chat
- **Backward Compatible**: Existing Gemini workflows continue to work
- **Automation**: Trigger workflows based on events or schedules
- **On-demand Collaboration**: Use `@llxprt` in comments to get AI assistance
- **Extensible with Tools**: Leverage tool-calling capabilities across providers
- **Customizable**: Use a `LLXPRT.md` file in your repository to provide
  project-specific instructions and context to [LLxprt Code]

## Quick Start

Get started with LLxprt Code in your repository in just a few minutes:

### 1. Get a Gemini API Key
Obtain your API key from [Google AI Studio] with generous free-of-charge quotas

### 2. Add it as a GitHub Secret
Store your API key as a secret named `GEMINI_API_KEY` in your repository:
- Go to your repository's **Settings > Secrets and variables > Actions**
- Click **New repository secret**
- Name: `GEMINI_API_KEY`, Value: your API key

### 3. Choose a Workflow
You have two options to set up a workflow:

**Option A: Use setup command (Recommended)**
1. Start the Gemini CLI:

   ```shell
   gemini
   ```

2. In the chat interface, type:

   ```
   /setup-github
   ```

**Option B: Manually copy workflows**
1. Copy the pre-built workflows from the [`examples/workflows`](./examples/workflows) directory to your repository's `.github/workflows` directory.

### 4. Try it out!

## Provider Configuration

LLxprt Code supports multiple AI providers. Configure your preferred provider using action inputs:

### Anthropic Claude
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'anthropic'
    model: 'claude-opus-4-1-20250805'  # Optional, uses provider default if not specified
    api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
```

### OpenRouter
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'openrouter'
    model: 'openai/gpt-oss-120b'  # Optional
    api_key: '${{ secrets.OPENROUTER_API_KEY }}'
```

### Fireworks AI
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'fireworks'
    model: 'fireworks/qwen3-coder-480b-a35b-instruct'  # Optional
    api_key: '${{ secrets.FIREWORKS_API_KEY }}'
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

**Pull Request Review:**
- Open a pull request in your repository and wait for automatic review
- Comment `@llxprt /review` on an existing pull request to manually trigger a review

**Issue Triage:**
- Open an issue and wait for automatic triage
- Comment `@llxprt /triage` on existing issues to manually trigger triaging

**General AI Assistance:**
- In any issue or pull request, mention `@llxprt` followed by your request
- Examples:
  - `@llxprt explain this code change`
  - `@llxprt suggest improvements for this function`
  - `@llxprt help me debug this error`
  - `@llxprt write unit tests for this component`

## Workflows

This action provides several pre-built workflows for different use cases. Each workflow is designed to be copied into your repository's `.github/workflows` directory and customized as needed.

### Issue Triage

This action can be used to triage GitHub Issues automatically or on a schedule.
For a detailed guide on how to set up the issue triage system, go to the
[GitHub Issue Triage workflow documentation](./examples/workflows/issue-triage).

### Pull Request Review

This action can be used to automatically review pull requests when they are
opened. For a detailed guide on how to set up the pull request review system,
go to the [GitHub PR Review workflow documentation](./examples/workflows/pr-review).

### Gemini CLI Assistant

This type of action can be used to invoke a general-purpose, conversational Gemini
AI assistant within the pull requests and issues to perform a wide range of
tasks. For a detailed guide on how to set up the general-purpose Gemini CLI workflow,
go to the [Gemini CLI workflow documentation](./examples/workflows/gemini-cli).

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

### Inputs

<!-- BEGIN_AUTOGEN_INPUTS -->

-   <a name="provider"></a><a href="#user-content-provider"><code>provider</code></a>: _(Optional, default: `gemini`)_ The AI provider to use (anthropic, openai, gemini, etc.)

-   <a name="model"></a><a href="#user-content-model"><code>model</code></a>: _(Optional)_ The model to use for the specified provider. If not set, uses provider defaults.

-   <a name="api_key"></a><a href="#user-content-api_key"><code>api_key</code></a>: _(Optional)_ API key for the specified provider.

-   <a name="api_key_file"></a><a href="#user-content-api_key_file"><code>api_key_file</code></a>: _(Optional)_ Path to file containing the API key.

-   <a name="prompt"></a><a href="#user-content-prompt"><code>prompt</code></a>: _(Optional, default: `You are a helpful assistant.`)_ The prompt to pass to LLxprt Code.

-   <a name="settings"></a><a href="#user-content-settings"><code>settings</code></a>: _(Optional)_ A JSON string written to `.llxprt/settings.json` to configure the CLI's _project_ settings.

-   <a name="gemini_api_key"></a><a href="#user-content-gemini_api_key"><code>gemini_api_key</code></a>: _(Optional)_ Gemini API key for backward compatibility or ServerTools in dual-provider mode.

-   <a name="gcp_project_id"></a><a href="#user-content-gcp_project_id"><code>gcp_project_id</code></a>: _(Optional)_ The Google Cloud project ID.

-   <a name="gcp_location"></a><a href="#user-content-gcp_location"><code>gcp_location</code></a>: _(Optional)_ The Google Cloud location.

-   <a name="gcp_workload_identity_provider"></a><a href="#user-content-gcp_workload_identity_provider"><code>gcp_workload_identity_provider</code></a>: _(Optional)_ The Google Cloud Workload Identity Provider.

-   <a name="gcp_service_account"></a><a href="#user-content-gcp_service_account"><code>gcp_service_account</code></a>: _(Optional)_ The Google Cloud service account email.

-   <a name="use_vertex_ai"></a><a href="#user-content-use_vertex_ai"><code>use_vertex_ai</code></a>: _(Optional, default: `false`)_ A flag to indicate if Vertex AI should be used.

-   <a name="use_gemini_code_assist"></a><a href="#user-content-use_gemini_code_assist"><code>use_gemini_code_assist</code></a>: _(Optional, default: `false`)_ A flag to indicate if Gemini Code Assist should be used.

-   <a name="llxprt_version"></a><a href="#user-content-llxprt_version"><code>llxprt_version</code></a>: _(Optional, default: `latest`)_ Version of LLxprt Code to install.


<!-- END_AUTOGEN_INPUTS -->

### Outputs

<!-- BEGIN_AUTOGEN_OUTPUTS -->

-   `summary`: The summarized output from the Gemini CLI execution.


<!-- END_AUTOGEN_OUTPUTS -->

### Repository Variables

We recommend setting the following values as repository variables so they can be reused across all workflows. Alternatively, you can set them inline as action inputs in individual workflows or to override repository-level values.

| Name                        | Description                                            | Type     | Required | When Required             |
| --------------------------- | ------------------------------------------------------ | -------- | -------- | ------------------------- |
| `LLXPRT_VERSION`            | Controls which version of LLxprt Code is installed.   | Variable | No       | Pinning the CLI version   |
| `GCP_WIF_PROVIDER`          | Full resource name of the Workload Identity Provider.  | Variable | No       | Using Google Cloud        |
| `GOOGLE_CLOUD_PROJECT`      | Google Cloud project for inference and observability.  | Variable | No       | Using Google Cloud        |
| `SERVICE_ACCOUNT_EMAIL`     | Google Cloud service account email address.            | Variable | No       | Using Google Cloud        |
| `GOOGLE_CLOUD_LOCATION`     | Region of the Google Cloud project.                    | Variable | No       | Using Google Cloud        |
| `GOOGLE_GENAI_USE_VERTEXAI` | Set to `true` to use Vertex AI                         | Variable | No       | Using Vertex AI           |
| `GOOGLE_GENAI_USE_GCA`      | Set to `true` to use Gemini Code Assist                | Variable | No       | Using Gemini Code Assist  |
| `APP_ID`                    | GitHub App ID for custom authentication.               | Variable | No       | Using a custom GitHub App |


To add a repository variable:
1) Go to your repository's **Settings > Secrets and variables > Actions > New variable**.
2) Enter the variable name and value.
3) Save.

For details about repository variables, refer to the [GitHub documentation on variables][variables].

### Secrets

You can set the following secrets in your repository:

| Name              | Description                                   | Required | When Required                 |
| ----------------- | --------------------------------------------- | -------- | ----------------------------- |
| `GEMINI_API_KEY`  | Your Gemini API key from Google AI Studio.    | No       | You don't have a GCP project. |
| `APP_PRIVATE_KEY` | Private key for your GitHub App (PEM format). | No       | Using a custom GitHub App.    |

To add a secret:

1. Go to your repository's **Settings > Secrets and variables >Actions > New repository secret**.
2. Enter the secret name and value.
3. Save.

For more information, refer to the
[official GitHub documentation on creating and using encrypted secrets][secrets].

## Authentication

This action requires authentication to both Google services (for Gemini AI) and the GitHub API.

### Google Authentication

Choose the authentication method that best fits your use case:

1. **Gemini API Key:** The simplest method for projects that don't require Google Cloud integration
2. **Workload Identity Federation:** The most secure method for authenticating to Google Cloud services

### GitHub Authentication

You can authenticate with GitHub in two ways:

1.  **Default `GITHUB_TOKEN`:** For simpler use cases, the action can use the
    default `GITHUB_TOKEN` provided by the workflow.
2.  **Custom GitHub App (Recommended):** For the most secure and flexible
    authentication, we recommend creating a custom GitHub App.

For detailed setup instructions for both Google and GitHub authentication, go to the
[**Authentication documentation**](./docs/authentication.md).

### Examples

#### PR Review with Claude
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'anthropic'
    model: 'claude-opus-4-1-20250805'
    api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
    prompt: 'Review this pull request for code quality and potential issues'
```

#### Issue Triage with OpenRouter GPT-OSS
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'openrouter'
    model: 'openai/gpt-oss-120b'
    api_key: '${{ secrets.OPENROUTER_API_KEY }}'
    prompt: 'Triage this issue and suggest appropriate labels'
```

#### Dual-Provider: Claude + Gemini Web Tools
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'anthropic'
    api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
    gemini_api_key: '${{ secrets.GEMINI_API_KEY }}'  # Enables web search capabilities
    prompt: 'Research this issue and provide a comprehensive analysis'
```

#### Code Analysis with Fireworks AI
```yaml
- uses: 'acoliver/run-llxprt-code@v1'
  with:
    provider: 'fireworks'
    model: 'fireworks/qwen3-coder-480b-a35b-instruct'
    api_key: '${{ secrets.FIREWORKS_API_KEY }}'
    prompt: 'Analyze this codebase for potential optimizations and improvements'
```

## Observability

This action can be configured to send telemetry data (traces, metrics, and logs)
to your own Google Cloud project. This allows you to monitor the performance and
behavior of the [LLxprt Code] within your workflows, providing valuable insights
for debugging and optimization.

For detailed instructions on how to set up and configure observability, go to
the [Observability documentation](./docs/observability.md).

## Customization

Create a [LLXPRT.md] file in the root of your repository to provide
project-specific context and instructions to [LLxprt Code]. This is useful for defining
coding conventions, architectural patterns, or other guidelines the model should
follow for a given repository.

## Contributing

Contributions are welcome! Check out the LLxprt Code
[**Contributing Guide**](./CONTRIBUTING.md) for more details on how to get
started.

[secrets]: https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions
[settings_json]: https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/configuration.md#settings-files
[Gemini]: https://deepmind.google/models/gemini/
[Google AI Studio]: https://aistudio.google.com/apikey
[Gemini CLI]: https://github.com/google-gemini/gemini-cli/
[Google Cloud support]: https://cloud.google.com/support
[variables]: https://docs.github.com/en/actions/how-tos/write-workflows/choose-what-workflows-do/use-variables#creating-configuration-variables-for-a-repository
[GitHub CLI]: https://docs.github.com/en/github-cli/github-cli
[LLXPRT.md]: https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/configuration.md#context-files-hierarchical-instructional-context
