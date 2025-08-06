# Configuring LLxprt Code Workflows

This guide covers how to customize and configure LLxprt Code workflows to meet your specific needs.

- [Configuring LLxprt Code Workflows](#configuring-llxprt-code-workflows)
  - [Provider Configuration](#provider-configuration)
    - [Anthropic Claude Opus 4.1](#anthropic-claude-opus-41)
    - [OpenRouter GPT-OSS 120B](#openrouter-gpt-oss-120b)
    - [Fireworks AI Qwen3-Coder 480B](#fireworks-ai-qwen3-coder-480b)
    - [Google Gemini (Default)](#google-gemini-default)
  - [How to Configure LLxprt Code](#how-to-configure-llxprt-code)
    - [Key Settings](#key-settings)
      - [Conversation Length (`maxSessionTurns`)](#conversation-length-maxsessionturns)
    - [Custom Context and Guidance (`LLXPRT.md`)](#custom-context-and-guidance-llxprtmd)
  - [GitHub Actions Workflow Settings](#github-actions-workflow-settings)
    - [Setting Timeouts](#setting-timeouts)
    - [Required Permissions](#required-permissions)

## Provider Configuration

LLxprt Code supports multiple AI providers. You can configure your preferred provider in your workflow YAML files:

### Anthropic Claude Opus 4.1
```yaml
with:
  provider: 'anthropic'
  model: 'claude-opus-4-1-20250805'
  api_key: '${{ secrets.ANTHROPIC_API_KEY }}'
```

### OpenRouter GPT-OSS 120B
```yaml
with:
  provider: 'openrouter'
  model: 'openai/gpt-oss-120b'
  api_key: '${{ secrets.OPENROUTER_API_KEY }}'
```

### Fireworks AI Qwen3-Coder 480B
```yaml
with:
  provider: 'fireworks'
  model: 'fireworks/qwen3-coder-480b-a35b-instruct'
  api_key: '${{ secrets.FIREWORKS_API_KEY }}'
```

### Google Gemini (Default)
```yaml
with:
  gemini_api_key: '${{ secrets.GEMINI_API_KEY }}'
  # OR use OAuth (no key needed)
```

## How to Configure LLxprt Code

LLxprt Code workflows are highly configurable. You can adjust their behavior by editing the corresponding `.yml` files in your repository.

LLxprt Code supports many settings that control how it operates. For a complete list, see the [LLxprt Code documentation](https://github.com/google-gemini/gemini-cli/blob/main/docs/cli/configuration.md#available-settings-in-settingsjson).

### Key Settings

#### Conversation Length (`maxSessionTurns`)

This setting controls the maximum number of conversational turns (messages exchanged) allowed during a workflow run.

**Default values by workflow:**

| Workflow                             | Default `maxSessionTurns` |
| ------------------------------------ | ------------------------- |
| [Issue Triage](./issue-triage)       | 25                        |
| [Pull Request Review](./pr-review)   | 20                        |
| [Gemini CLI Assistant](./gemini-cli) | 50                        |

**How to override:**

Add the following to your workflow YAML file to set a custom value:

```yaml
with:
  settings: |-
    {
      "maxSessionTurns": 10
    }
```

### Custom Context and Guidance (`LLXPRT.md`)

To provide LLxprt Code with custom instructions—such as coding conventions, architectural patterns, or other guidance—add a `LLXPRT.md` file to the root of your repository. LLxprt Code will use the content of this file to inform its responses.

## GitHub Actions Workflow Settings

### Setting Timeouts

You can control how long Gemini CLI runs by using either the `timeout-minutes` field in your workflow YAML, or by specifying a timeout in the `settings` input.

### Required Permissions

Only users with the following roles can trigger the workflow:

- Repository Owner (`OWNER`)
- Repository Member (`MEMBER`)
- Repository Collaborator (`COLLABORATOR`)
