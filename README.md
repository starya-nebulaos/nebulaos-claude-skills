# NebulaOS Claude Code Skills

Official Claude Code skills for developing AI agents with NebulaOS SDK.

## Installation

```bash
# Add the marketplace
/plugin marketplace add nebulaos/claude-code-skills

# Install the plugin
/plugin install nebulaos@nebulaos-claude-code-skills
```

Or use the interactive UI:
```bash
/plugin
```

## Available Skills

| Skill | Description |
|-------|-------------|
| `/nebulaos:nebulaos` | Complete SDK reference and CLI commands |
| `/nebulaos:nebulaos-onboarding` | Initial setup for new engineers |
| `/nebulaos:nebulaos-dev` | Iterative development workflow |
| `/nebulaos:nebulaos-debug` | Debugging, traces, and observability |
| `/nebulaos:nebulaos-llm` | LLM Gateway management |
| `/nebulaos:nebulaos-rag` | RAG connections and semantic search |

## Usage

After installation, invoke skills in Claude Code:

```
/nebulaos:nebulaos-onboarding
```

Or use the main skill for general guidance:

```
/nebulaos:nebulaos
```

## What's Included

### nebulaos
Complete reference for the NebulaOS SDK:
- Agent, Tool, Workflow creation
- NebulaClient configuration
- CLI command reference
- Code patterns and conventions

### nebulaos-onboarding
First-time setup:
- Authentication with Personal Access Tokens
- Client and API key creation
- Environment configuration
- Multi-environment management

### nebulaos-dev
Development workflow:
- Resource listing and inspection
- Execution and testing
- Log viewing
- Iterative development cycle

### nebulaos-debug
Troubleshooting:
- Error investigation
- Trace analysis
- Metrics inspection
- LLM cost tracking

### nebulaos-llm
LLM Gateway:
- Model catalog browsing
- Route creation and management
- API key management
- Usage and cost monitoring

### nebulaos-rag
RAG (Retrieval-Augmented Generation):
- Connection management
- Semantic search
- RagOpenAISkill integration

## Requirements

- Claude Code CLI
- NebulaOS account with Personal Access Token
- Node.js 18+

## Documentation

- [NebulaOS SDK Docs](https://docs.nebulaos.dev/sdk)
- [NebulaOS CLI Docs](https://docs.nebulaos.dev/cli)
- [Examples](https://github.com/nebulaos/examples)

## License

MIT
