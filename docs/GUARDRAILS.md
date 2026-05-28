# 🛡️ Guardrails e Segurança

## Níveis de Risco

| Nível | Tipo de Ação | Política |
|---|---|---|
| 🟢 LOW | Leitura de dados, análise, pesquisa | Execução automática |
| 🟡 MEDIUM | Criação de arquivos, escrita em banco | dryrun=True → log → execute |
| 🔴 HIGH | Deploy, exclusão, envio de mensagens | Requer aprovação humana |
| 🚫 CRITICAL | Acesso financeiro, dados sensíveis, produção | Bloqueado até confirmação explícita |

## Configuração Global

```yaml
# config/guardrails.yaml

global:
  dryrun_default: true
  max_retries_per_wave: 3
  max_cost_per_mission_usd: 50.0
  max_tokens_per_wave: 100000

security:
  no_external_api_calls_without_approval: true
  no_financial_operations_without_human: true
  no_production_deploy_without_checklist: true
  allowed_file_extensions:
    - ".py"
    - ".ts"
    - ".tsx"
    - ".js"
    - ".jsx"
    - ".html"
    - ".css"
    - ".md"
    - ".yaml"
    - ".json"
    - ".sql"

code_quality:
  min_test_coverage: 0.70
  require_type_hints: true
  require_docstrings: true
  block_merge_on_failing_tests: true

memory:
  pattern_similarity_threshold: 0.75
  max_context_tokens: 200000
```

## Os 3 Checkpoints de Human-in-the-Loop

### CHECKPOINT 1 — Aprovação do PRD
```
Trigger: Planner conclui PRD
O que revisar: objetivos, escopo, critérios de aceite, riscos
Opções: APROVAR | REVISAR (com comentários) | REJEITAR
Se aprovado: Architect inicia
```

### CHECKPOINT 2 — Aprovação do Blueprint
```
Trigger: Architect conclui Blueprint
O que revisar: stack, árvore de arquivos, contratos de API
Opções: APROVAR | REVISAR | REJEITAR
Se aprovado: PM inicia waves
```

### CHECKPOINT 3 — Aprovação do Mission Package
```
Trigger: Todas as waves concluídas
O que revisar: código funcional, testes, documentação
Opções: APROVAR E DEPLOYAR | REVISAR | REJEITAR
Se aprovado: PatternRecord registrado + deploy
```

## Taxonomia de Falhas (para detecção automática)

| Tipo de Falha | Sinal | Ação Automática |
|---|---|---|
| Loop improdutivo | Wave em retry > 2x com mesmo erro | Pausa + notifica humano |
| Custo explosivo | Cost > 80% do limite | Alerta + freeze de novas waves |
| Conflito de arquivo | Arquivo fora de allowed_files | Bloqueio imediato + rollback |
| Teste crítico falhando | Critério de aceite binário = false | Bloqueia merge |
| Agente sem resposta | Timeout > 120s | Retry com modelo fallback |
| Context overflow | Tokens > max_context | Comprime contexto + continua |

## Observabilidade — Estrutura do Event Log

```python
{
    "event_id": "evt_20260528_143022_7f3a",
    "event_type": "wave.completed",
    "mission_id": "mission_20260528_142000",
    "wave_id": "W132",
    "agent_id": "executor_agent_01",
    "model_used": "claude-sonnet-4-5",
    "timestamp": "2026-05-28T14:30:22Z",
    "duration_ms": 45320,
    "tokens_input": 8421,
    "tokens_output": 3108,
    "cost_usd": 0.0847,
    "status": "PASSED",
    "metadata": {
        "files_created": ["src/models/user.py"],
        "tests_passed": 12,
        "tests_failed": 0
    }
}
```

## Métricas de Saúde

| Métrica | Alerta |
|---|---|
| `mission.success_rate` | < 80% |
| `wave.avg_duration_ms` | > 120s |
| `mission.avg_cost_usd` | > $30 |
| `akasha.pattern_hit_rate` | < 40% |
| `agent.retry_rate` | > 20% |
| `test.coverage_avg` | < 70% |
