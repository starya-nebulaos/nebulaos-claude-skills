---
description: Debugging e observabilidade no NebulaOS
---

# NebulaOS Debugging

Use este guia para debugar execuções, analisar traces e investigar erros.

## Investigar Execução com Erro

### 1. Identificar a Execução

```bash
nebulaos exec list --status failed
nebulaos exec list --page-size 20
```

### 2. Ver Detalhes

```bash
nebulaos exec get <execution-id>
nebulaos exec get <execution-id> -o json
```

### 3. Ver Logs e Custos

```bash
nebulaos exec logs <execution-id>
nebulaos exec logs <execution-id> --verbose
```

### 4. Analisar Trace

```bash
nebulaos obs traces get --execution-id <execution-id>
nebulaos obs traces get --execution-id <execution-id> -o json
```

## Buscar Traces

### Por Agent

```bash
nebulaos obs traces search --agent meu_agent
nebulaos obs traces search --agent meu_agent --status error
```

### Por Status

```bash
nebulaos obs traces search --status error
nebulaos obs traces search --status success
```

### Por Data

```bash
nebulaos obs traces search --from 2026-03-01T00:00:00Z --to 2026-03-31T23:59:59Z
nebulaos obs traces search --limit 50
```

### Combinando Filtros

```bash
nebulaos obs traces search \
  --agent meu_agent \
  --status error \
  --from 2026-03-01 \
  --limit 20
```

## Ver Detalhes de Trace

```bash
nebulaos obs traces get <trace-id>
nebulaos obs traces get <trace-id> -o json
```

## Métricas

```bash
nebulaos obs metrics
nebulaos obs metrics --type latency
nebulaos obs metrics --from 2026-03-01 --to 2026-03-31
```

## Custos de LLM

### Por Execução

```bash
nebulaos exec logs <execution-id>
```

### Agregado

```bash
nebulaos llm usage
nebulaos llm requests --limit 50
nebulaos llm requests --route gpt4
```

## Fluxo de Debug

```bash
# 1. Identificar
nebulaos exec list --status failed

# 2. Ver erro básico
nebulaos exec get <exec-id>

# 3. Ver logs
nebulaos exec logs <exec-id>

# 4. Trace completo
nebulaos obs traces get --execution-id <exec-id> -o json

# 5. Buscar padrões
nebulaos obs traces search --agent <agent-name> --status error
```

## Scripts Úteis

```bash
# IDs de execuções com erro
nebulaos exec list --status failed -o json | jq -r '.[].id'

# Último erro de um agent
exec_id=$(nebulaos exec list --status failed -o quiet | head -1)
nebulaos exec get $exec_id -o json | jq '.error'
```
