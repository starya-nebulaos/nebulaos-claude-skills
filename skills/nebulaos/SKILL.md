---
description: Desenvolver agentes, tools, workflows e recursos com NebulaOS SDK
---

# NebulaOS SDK - Guia Completo

Use este guia para desenvolver AI agents com o NebulaOS SDK.

## Instalação

```bash
npm install @nebulaos/core @nebulaos/client @nebulaos/types
# ou
pnpm add @nebulaos/core @nebulaos/client @nebulaos/types
```

## Estrutura de Projeto

```
meu-projeto/
├── src/
│   ├── agents/           # Definições de agents
│   ├── tools/            # Tools customizadas
│   ├── workflows/        # Workflows
│   └── server.ts         # Entry point
├── .env                  # Configuração
└── package.json
```

## Criando um Agent

```typescript
import { Agent, InMemory, Tool, z } from "@nebulaos/core";
import { OpenAI } from "@nebulaos/openai";

const myAgent = new Agent({
  id: "meu_agent",                    // snake_case obrigatório
  name: "meu_agent",
  model: new OpenAI({
    apiKey: process.env.OPENAI_API_KEY,
    model: "gpt-4o"
  }),
  memory: new InMemory(),
  instructions: "Você é um assistente útil...",
  tools: [minhasTool],
  maxSteps: 10
});
```

## Criando Tools

```typescript
const minhaTool = new Tool({
  id: "buscar_cliente",               // snake_case, verbo + substantivo
  description: `
    Busca informações de um cliente no sistema.
    QUANDO USAR: Quando precisar de dados do cliente.
    RETORNA: Objeto com nome, email, plano.
  `,
  inputSchema: z.object({
    clienteId: z.string().describe("ID do cliente"),
    campos: z.array(z.string()).optional().describe("Campos específicos")
  }),
  handler: async (ctx, input) => {
    // Nunca lance exceções, retorne objetos de erro
    try {
      const cliente = await buscarNoBanco(input.clienteId);
      return { success: true, cliente };
    } catch (error) {
      return { success: false, error: error.message };
    }
  }
});
```

## NebulaClient

```typescript
import { NebulaClient } from "@nebulaos/client";

const client = new NebulaClient({
  agents: [meuAgent, outroAgent],
  workflows: [meuWorkflow],
  server: {
    url: process.env.NEBULAOS_URL,
    apiKey: process.env.NEBULAOS_API_KEY
  }
});

await client.start();
```

## Padrões Obrigatórios

- IDs de agentes: `snake_case` (ex: `atendimento_vendas`)
- IDs de tools: `snake_case`, verbo + substantivo (ex: `buscar_cliente`)
- IDs de workflows: `kebab-case` (ex: `processo-vendas`)
- Sempre use Zod para schemas com `.describe()` em cada campo
- Descrições de tools devem explicar QUANDO e COMO usar
- Nunca lance exceções em handlers, retorne objetos de erro

## CLI do NebulaOS

### Namespaces de Comandos

| Comando | Alias | Descrição |
|---------|-------|-----------|
| `nebulaos auth` | - | Autenticação (login, logout, status) |
| `nebulaos orgs` | - | Organizações (list, get, switch) |
| `nebulaos clients` | - | Clients e API keys |
| `nebulaos resources` | - | Agents, workflows, tools |
| `nebulaos execution` | `exec` | Executar e inspecionar |
| `nebulaos llm-gateway` | `llm` | LLM Gateway (routes, keys, catalog) |
| `nebulaos rag` | - | RAG connections e busca |
| `nebulaos features` | - | Feature flags |
| `nebulaos observability` | `obs` | Traces e métricas |
| `nebulaos config` | - | Configuração da CLI |

### Flags Globais

```
-o, --output <format>   table, json, yaml, quiet
    --context <name>    Usar contexto específico
    --api-url <url>     Override API URL
    --token <token>     Override token
    --verbose           Output detalhado
    --no-color          Desabilitar cores
```

### Variáveis de Ambiente

```bash
NEBULAOS_OUTPUT=json              # Formato padrão
NEBULAOS_TOKEN=neb_pat_...        # Token de autenticação
NEBULAOS_API_URL=http://...       # API URL
NEBULAOS_CONTEXT=production       # Contexto ativo
```

## Fluxo de Desenvolvimento

### 1. Setup Inicial

```bash
nebulaos auth login --token neb_pat_... --api-url http://localhost:4100
nebulaos auth status
nebulaos clients create --client-id meu-projeto
nebulaos clients api-keys create <client-id>
```

### 2. Desenvolvimento

```bash
# Terminal 1
pnpm dev

# Terminal 2
nebulaos resources list
nebulaos exec run <id> --input '{"x":"y"}'
nebulaos exec logs <exec-id>
```

## Comandos Essenciais

```bash
# Recursos
nebulaos resources list --type agent
nebulaos resources get <id>
nebulaos resources promote <id>

# Execuções
nebulaos exec run <id> --input '{"prompt":"x"}'
nebulaos exec list --page-size 10
nebulaos exec logs <exec-id>

# LLM Gateway
nebulaos llm catalog
nebulaos llm routes list
nebulaos llm usage

# Observabilidade
nebulaos obs traces search --agent <name>
nebulaos obs traces search --status error
```

## Skills Relacionadas

- `/nebulaos:nebulaos-onboarding` - Setup inicial
- `/nebulaos:nebulaos-dev` - Desenvolvimento iterativo
- `/nebulaos:nebulaos-debug` - Debugging e traces
- `/nebulaos:nebulaos-llm` - LLM Gateway
- `/nebulaos:nebulaos-rag` - RAG connections

## Documentação

- SDK: https://docs.nebulaos.dev/sdk
- CLI: https://docs.nebulaos.dev/cli
- Examples: https://github.com/nebulaos/examples
