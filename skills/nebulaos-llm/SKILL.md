---
description: Gerenciar LLM Gateway do NebulaOS (rotas, keys, catálogo, custos)
---

# NebulaOS LLM Gateway

Use este guia para configurar e gerenciar o LLM Gateway do NebulaOS.

## Conceitos

- **Rota**: Configuração de um modelo LLM (provider, model, credentials)
- **Alias**: Nome curto para a rota (ex: `gpt4`, `claude`)
- **API Key**: Chave para acessar o gateway

## Catálogo de Modelos

```bash
nebulaos llm catalog
nebulaos llm catalog --search gpt-4
nebulaos llm catalog --provider openai
nebulaos llm catalog --provider anthropic
nebulaos llm catalog --page 2 --limit 50
```

## Gerenciar Rotas

### Listar

```bash
nebulaos llm routes list
nebulaos llm routes list -o json
```

### Criar

```bash
# Básico
nebulaos llm routes create \
  --alias gpt4 \
  --provider openai \
  --model gpt-4o

# Avançado
nebulaos llm routes create \
  --alias claude-fast \
  --provider anthropic \
  --model claude-3-5-sonnet \
  --max-tokens 4096 \
  --timeout 30000 \
  --retries 2

# Com credentials
nebulaos llm routes create \
  --alias azure-gpt4 \
  --provider azure \
  --model gpt-4o \
  --credential api_key=sk-... \
  --credential endpoint=https://my-resource.openai.azure.com
```

### Ver/Atualizar/Deletar

```bash
nebulaos llm routes get gpt4
nebulaos llm routes update gpt4 --max-tokens 8192
nebulaos llm routes delete old-route -y
```

## Gerenciar API Keys

```bash
# Listar
nebulaos llm keys list

# Criar
nebulaos llm keys create --name my-key --type personal
nebulaos llm keys create --name svc --type service --allowed-routes gpt4,claude

# Revogar
nebulaos llm keys revoke <key-id>
```

**IMPORTANTE**: A key é mostrada apenas UMA VEZ!

## Monitorar Uso e Custos

```bash
# Uso agregado
nebulaos llm usage

# Requests recentes
nebulaos llm requests
nebulaos llm requests --limit 20
nebulaos llm requests --route gpt4
nebulaos llm requests --status error
```

## Usar no SDK

```typescript
import { LLMGateway } from "@nebulaos/llm-gateway";

const model = new LLMGateway({
  gatewayUrl: process.env.NEBULAOS_URL,
  apiKey: process.env.NEBULAOS_LLM_KEY,
  model: "gpt4"  // alias da rota
});

const agent = new Agent({ model, ... });
```

## Troubleshooting

### Rota não funciona
```bash
nebulaos llm routes get <alias>
nebulaos llm requests --route <alias> --status error
```

### Custo alto
```bash
nebulaos llm usage -o json
nebulaos llm requests --limit 100 -o json | jq 'sort_by(.cost) | reverse | .[0:10]'
```

### Timeout
```bash
nebulaos llm routes update <alias> --timeout 60000
```

## Boas Práticas

```bash
# Rota produção
nebulaos llm routes create --alias gpt4-prod --provider openai --model gpt-4o

# Rota dev (mais barata)
nebulaos llm routes create --alias gpt4-dev --provider openai --model gpt-4o-mini
```
