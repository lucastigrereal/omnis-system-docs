# OMNIS — Arquitetura do Sistema

## 1. As 4 Camadas Horizontais

```
┌─────────────────────────────────────────────────────────────────┐
│  CAMADA 4 — INTERFACE & OBSERVABILIDADE                         │
│  Dashboard OMNIS | Logs | Métricas | Alertas | Human-in-Loop    │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 3 — MÓDULOS OPERACIONAIS                                │
│  App Factory | Conteúdo | Vendas | Mkt | Jurídico | Financeiro  │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 2 — AKASHA (MEMÓRIA OPERACIONAL)                        │
│  RAG | PatternRecords | Prompts Versionados | Vector DB | SQL   │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 1 — OMNIS CORE (CONTROL PLANE)                          │
│  Event Bus | Orquestrador | Políticas | Roteamento de Modelos   │
└─────────────────────────────────────────────────────────────────┘
```

## 2. Fluxo Supremo de Qualquer Missão

```
[IDEIA/TRIGGER]
      │
      ▼
[OMNIS CORE recebe o evento]
      │
      ├─► Consulta AKASHA (há padrão similar? qual stack vencedora?)
      │
      ▼
[SQUAD DINÂMICO é montado]
      │
      ├─► Planner → gera PRD
      ├─► Architect → define stack e blueprint
      ├─► Program Manager → quebra em waves
      ├─► Executor → implementa wave por wave
      │
      ▼
[TESTES AUTOMÁTICOS por wave]
      │
      ├─► Falhou? → Auto-correção → retry → escala para humano
      ├─► Passou? → Merge + avança para próxima wave
      │
      ▼
[MISSION PACKAGE gerado]
      │
      ├─► Artefatos de código
      ├─► Documentação
      ├─► Resultados de testes
      │
      ▼
[AKASHA registra PatternRecord]
      │
      └─► Sistema fica melhor para a próxima missão
```

## 3. Os 3 Módulos Core

### 3.1 OMNIS Core (Control Plane)

**Responsabilidades:**
- **Event Bus (pub/sub):** todos os módulos se comunicam via eventos nomeados
- **Orquestração de agentes:** decide qual squad, quais agentes e quais modelos são ativados
- **Políticas globais:** guardrails de segurança, limites de custo, prioridades
- **Roteamento de modelos:** modelo A para planejamento, modelo B para código, modelo C para review
- **Observabilidade:** loga cada evento com timestamp, custo em tokens, agente responsável

**Stack:**
- LangGraph (grafos stateful com checkpoints)
- Python 3.11+
- Redis (event bus)
- SQLite/PostgreSQL (estado persistente)
- Pydantic (validação de dados)

### 3.2 App Factory

Ver [APP_FACTORY.md](./APP_FACTORY.md)

### 3.3 Akasha

Ver [AKASHA.md](./AKASHA.md)

## 4. Árvore de Arquivos do Repositório Principal

```
omnis/
│
├── README.md
├── ARCHITECTURE.md
├── CHANGELOG.md
│
├── core/
│   ├── __init__.py
│   ├── orchestrator.py
│   ├── event_bus.py
│   ├── policy_engine.py
│   ├── model_router.py
│   ├── observability.py
│   └── models/
│       ├── mission.py
│       ├── wave.py
│       ├── event.py
│       └── agent.py
│
├── app_factory/
│   ├── __init__.py
│   ├── intake.py
│   ├── planner.py
│   ├── architect.py
│   ├── program_manager.py
│   ├── executor.py
│   ├── tester.py
│   ├── merger.py
│   ├── mission_package.py
│   └── models/
│
├── akasha/
│   ├── __init__.py
│   ├── memory_engine.py
│   ├── rag_engine.py
│   ├── pattern_store.py
│   ├── prompt_registry.py
│   ├── semantic_search.py
│   └── models/
│
├── departments/
│   ├── content/
│   ├── traffic/
│   ├── sales/
│   ├── automation/
│   ├── legal/
│   ├── finance/
│   ├── hr/
│   ├── strategy/
│   ├── support/
│   ├── product/
│   ├── data/
│   ├── devops/
│   ├── bizdev/
│   └── knowledge/
│
├── skills/
│   ├── web_search.py
│   ├── code_execution.py
│   ├── file_operations.py
│   ├── browser_automation.py
│   ├── image_generation.py
│   ├── data_analysis.py
│   ├── email_sender.py
│   ├── notion_integration.py
│   ├── github_integration.py
│   ├── n8n_integration.py
│   └── [+ 7 outras skills]
│
├── prompts/
│   ├── planner/
│   │   ├── v1.0.md
│   │   └── current.md
│   ├── architect/
│   ├── program_manager/
│   └── executor/
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── scripts/
│   ├── create_worktree.sh
│   ├── run_wave.sh
│   ├── merge_wave.sh
│   └── health_check.sh
│
└── config/
    ├── settings.py
    ├── models.yaml
    ├── guardrails.yaml
    └── .env.example
```

## 5. Observabilidade — Event Log

```json
{
    "event_id": "evt_20260527_143022_7f3a",
    "event_type": "wave.completed",
    "mission_id": "mission_20260527_142000",
    "wave_id": "W132",
    "agent_id": "executor_agent_01",
    "model_used": "claude-sonnet-4-5",
    "timestamp": "2026-05-27T14:30:22Z",
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

## 6. Métricas do Sistema

| Métrica | Descrição | Alerta |
|---|---|---|
| `mission.success_rate` | % missões concluídas com sucesso | < 80% → alerta |
| `wave.avg_duration_ms` | Tempo médio por wave | > 120s → revisar |
| `mission.avg_cost_usd` | Custo médio por missão | > $30 → revisar |
| `akasha.pattern_hit_rate` | % vezes que Akasha encontrou padrão útil | < 40% → enriquecer Akasha |
| `agent.retry_rate` | % waves que precisaram de retry | > 20% → revisar prompts |
| `test.coverage_avg` | Cobertura média de testes gerados | < 70% → bloquear merge |
