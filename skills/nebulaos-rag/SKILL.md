---
description: Gerenciar RAG (Retrieval-Augmented Generation) no NebulaOS
---

# NebulaOS RAG

Use este guia para configurar e usar conexões RAG para busca semântica.

## Conceitos

- **Conexão RAG**: Configuração de um vector store (OpenAI, Azure)
- **Busca Semântica**: Encontrar documentos por similaridade
- **Embeddings**: Representação vetorial do texto

## Gerenciar Conexões

### Listar

```bash
nebulaos rag connections list
nebulaos rag connections list -o json
```

### Criar

```bash
# OpenAI
nebulaos rag connections create \
  --name minha-base \
  --type openai \
  --api-key sk-... \
  --model text-embedding-3-small

# Azure
nebulaos rag connections create \
  --name azure-embeddings \
  --type azure \
  --base-url https://my-resource.openai.azure.com \
  --api-key abc123 \
  --model text-embedding-ada-002 \
  --dimensions 1536
```

### Ver/Testar

```bash
nebulaos rag connections get <connection-id>
nebulaos rag connections test <connection-id>
```

### Atualizar/Deletar

```bash
nebulaos rag connections update <id> --model text-embedding-3-large
nebulaos rag connections delete <id> -y
```

## Busca Semântica

```bash
# Básica
nebulaos rag search \
  --connection <connection-id> \
  --query "como configurar autenticação?"

# Com limite
nebulaos rag search \
  --connection <connection-id> \
  --query "processo de onboarding" \
  --limit 5

# JSON
nebulaos rag search \
  --connection <connection-id> \
  --query "política de privacidade" \
  -o json
```

## Usar no SDK

```typescript
import { RagOpenAISkill } from "@nebulaos/rag-openai-skill";

const ragSkill = new RagOpenAISkill({
  connectionId: "<connection-id>",
  maxResults: 5
});

const agent = new Agent({
  skills: [ragSkill],
  // Tool de busca adicionada automaticamente
});
```

## Fluxo de Setup

```bash
# 1. Criar
nebulaos rag connections create --name docs --type openai --api-key sk-...

# 2. Testar
nebulaos rag connections test <connection-id>

# 3. Buscar
nebulaos rag search --connection <id> --query "teste"

# 4. Usar no código com RagOpenAISkill
```

## Troubleshooting

### Conexão falha
```bash
nebulaos rag connections get <id> -o json
# Verificar API key e base-url
```

### Sem resultados
```bash
nebulaos rag search --connection <id> --query "*" --limit 1
```

### Latência alta
```bash
# Usar modelo mais leve
# text-embedding-3-small é mais rápido que text-embedding-3-large
```

## Modelos de Embedding

- **text-embedding-3-small**: Rápido, baixo custo
- **text-embedding-3-large**: Melhor qualidade, maior custo
- **text-embedding-ada-002**: Legado
