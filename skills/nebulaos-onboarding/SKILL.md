---
description: Setup inicial do ambiente NebulaOS para novos engineers
---

# NebulaOS Onboarding

Use este guia para configurar o ambiente de desenvolvimento do NebulaOS pela primeira vez.

## Pré-requisitos

- Node.js 18+
- pnpm instalado
- Acesso ao NebulaOS Cloud (Personal Access Token)

## Fluxo de Setup

### 1. Autenticação

```bash
# Login com Personal Access Token (PAT)
nebulaos auth login --token neb_pat_... --api-url http://localhost:4100

# Para ambiente de produção
nebulaos auth login --token neb_pat_... --api-url https://api.nebulaos.dev --context-name production

# Verificar autenticação
nebulaos auth status
```

**Output esperado:**
```
Authentication Status
Context:         default
API URL:         http://localhost:4100
Authenticated:   Yes
Organization ID: <uuid>
```

### 2. Criar Client

```bash
# Criar novo client para seu projeto
nebulaos clients create --client-id meu-projeto

# Verificar criação
nebulaos clients list
```

### 3. Obter API Key

```bash
# Criar API key para o client
nebulaos clients api-keys create <client-id>
```

**IMPORTANTE**: A key é mostrada apenas UMA VEZ. Salve imediatamente!

### 4. Configurar Projeto

Crie o arquivo `.env` na raiz do projeto:

```bash
NEBULAOS_URL=http://localhost:4100
NEBULAOS_API_KEY=nb_...
```

### 5. Verificar Setup

```bash
# Rodar o client
pnpm dev

# Em outro terminal, verificar se está online
nebulaos clients list
```

O client deve aparecer com status "Online".

## Gerenciar Múltiplos Ambientes

```bash
# Login em dev
nebulaos auth login -t neb_pat_dev -u http://localhost:4100 -n dev

# Login em staging
nebulaos auth login -t neb_pat_stg -u https://staging.api.nebulaos.dev -n staging

# Login em prod
nebulaos auth login -t neb_pat_prd -u https://api.nebulaos.dev -n production

# Ver todos os contextos
nebulaos config list

# Usar contexto específico
nebulaos --context production resources list
```

## Troubleshooting

### Token inválido
```bash
nebulaos auth logout -y
nebulaos auth login
```

### Erro de conexão
```bash
nebulaos auth status
curl http://localhost:4100/health
```

### Client não aparece
```bash
nebulaos clients list
# Verificar logs do terminal onde roda pnpm dev
```

## Próximos Passos

Após o setup:
1. Use `/nebulaos:nebulaos-dev` para desenvolvimento iterativo
2. Use `/nebulaos:nebulaos-debug` para debugging
3. Use `/nebulaos:nebulaos-llm` para configurar LLM Gateway
