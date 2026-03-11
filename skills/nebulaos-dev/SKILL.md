---
description: Fluxo de desenvolvimento iterativo com NebulaOS CLI
---

# NebulaOS Development

Use este guia para o ciclo de desenvolvimento iterativo de agents, workflows e tools.

## Setup de Desenvolvimento

### Terminal 1: Rodar Client

```bash
pnpm dev
```

O client irá conectar ao NebulaOS Cloud e registrar seus recursos.

### Terminal 2: CLI para Testes

Use os comandos abaixo para testar seu código.

## Comandos de Recursos

### Listar Recursos

```bash
nebulaos resources list
nebulaos resources list --type agent
nebulaos resources list --type workflow
nebulaos resources list --type tool
nebulaos resources list -o json
```

### Ver Detalhes

```bash
nebulaos resources get <resource-id>
nebulaos resources get <resource-id> -o json
```

### Gerenciar Lifecycle

```bash
nebulaos resources promote <id>      # discovered -> official
nebulaos resources archive <id>      # ocultar
nebulaos resources unarchive <id>    # restaurar
```

## Executar Recursos

### Executar Agent/Workflow

```bash
nebulaos exec run <resource-id>
nebulaos exec run <resource-id> --input '{"prompt":"Olá"}'
nebulaos exec run <resource-id> --input '{"data":{"name":"João"}}'

# Conversa com contexto (multi-turn)
nebulaos exec run <resource-id> -i '"Oi"' -m session-123
nebulaos exec run <resource-id> -i '"Qual foi minha msg?"' -m session-123
```

### Ver Resultado

```bash
nebulaos exec get <execution-id>
nebulaos exec get <execution-id> -o json
```

### Listar Execuções

```bash
nebulaos exec list
nebulaos exec list --status completed
nebulaos exec list --status failed
nebulaos exec list --page 1 --page-size 20
```

### Ver Logs e Custos

```bash
nebulaos exec logs <execution-id>
nebulaos exec logs <execution-id> -o json
```

## Fluxo Típico

```bash
# 1. Fazer alteração no código
# 2. Hot-reload atualiza (pnpm dev)
# 3. Verificar registro
nebulaos resources list --type agent

# 4. Executar
nebulaos exec run meu_agent --input '{"prompt":"teste"}'

# 4b. Ou testar conversa multi-turn
nebulaos exec run meu_agent -i '"Oi"' -m test-123
nebulaos exec run meu_agent -i '"Continua..."' -m test-123

# 5. Ver resultado
nebulaos exec get <exec-id>

# 6. Se erro, ver logs
nebulaos exec logs <exec-id>
```

## Dicas

### Output JSON para Scripting

```bash
nebulaos resources list --type agent -o quiet
result=$(nebulaos exec run meu_agent --input '{"x":"y"}' -o json)
```

### Verificar Client Online

```bash
nebulaos clients list
nebulaos clients get <client-id>
```

## Troubleshooting

### Recurso não aparece
```bash
nebulaos clients list    # Client online?
```

### Execução falha
```bash
nebulaos exec get <exec-id> -o json
nebulaos exec logs <exec-id>
```

### Input JSON inválido
```bash
# CORRETO (aspas simples externas)
nebulaos exec run agent --input '{"x":"y"}'
```
