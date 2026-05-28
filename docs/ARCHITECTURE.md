# 🏗️ OMNIS — Arquitetura Técnica

## As 4 Camadas

```
┌─────────────────────────────────────────────────────────────────┐
│  CAMADA 4 — INTERFACE & OBSERVABILIDADE                         │
│  Dashboard OMNIS | Logs | Métricas | Alertas | Human-in-Loop    │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 3 — MÓDULOS OPERACIONAIS                                │
│  App Factory | Conteúdo | Vendas | Mkt | Jurídico | Financeiro  │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 2 — AKASHA (MEMÓRIA OPERACIONAL)                        │
│  RAG | PatternRecords | Prompts Versionados | Vector DB | SQL    │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 1 — OMNIS CORE (CONTROL PLANE)                          │
│  Event Bus | Orquestrador | Políticas | Roteamento de Modelos    │
└─────────────────────────────────────────────────────────────────┘
```

## Comunicação Entre Módulos

Todos os módulos se comunicam **exclusivamente via eventos** pelo Event Bus do OMNIS Core.

```
App Factory ──[wave.completed]──► OMNIS Core ──[pattern.save]──► Akasha
Akasha ──[pattern.found]──► OMNIS Core ──[context.enriched]──► App Factory
```

### Tipos de Eventos

| Namespace | Evento | Emitido por | Consumido por |
|---|---|---|---|
| `mission.*` | `mission.started`, `mission.completed`, `mission.failed` | OMNIS Core | Observabilidade, Akasha |
| `wave.*` | `wave.started`, `wave.passed`, `wave.failed` | App Factory | OMNIS Core, Akasha |
| `pattern.*` | `pattern.queried`, `pattern.found`, `pattern.saved` | Akasha | App Factory, OMNIS Core |
| `agent.*` | `agent.spawned`, `agent.completed`, `agent.error` | Qualquer Squad | OMNIS Core |
| `human.*` | `human.approval_requested`, `human.approved`, `human.rejected` | OMNIS Core | Toda pipeline |
| `skill.*` | `skill.invoked`, `skill.completed`, `skill.error` | Qualquer Agente | OMNIS Core |

## Árvore de Arquivos do Repositório Principal

```
omnis/
│
├── core/                              # OMNIS Core (Control Plane)
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
├── app_factory/                       # Módulo App Factory
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
├── akasha/                            # Módulo Akasha
│   ├── memory_engine.py
│   ├── rag_engine.py
│   ├── pattern_store.py
│   ├── prompt_registry.py
│   ├── semantic_search.py
│   └── models/
│
├── departments/                       # 16 Departamentos
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
├── skills/                            # 17+ Skills reutilizáveis
│   ├── web_search.py
│   ├── code_execution.py
│   ├── file_operations.py
│   ├── browser_automation.py
│   ├── image_generation.py
│   ├── data_analysis.py
│   ├── email_sender.py
│   ├── calendar_manager.py
│   ├── crm_integration.py
│   ├── notion_integration.py
│   ├── github_integration.py
│   ├── n8n_integration.py
│   ├── whatsapp_integration.py
│   ├── payment_processor.py
│   ├── pdf_generator.py
│   └── video_processor.py
│
├── prompts/                           # Prompts mestres versionados
│   ├── planner/v1.0.md
│   ├── architect/v1.0.md
│   ├── program_manager/v1.0.md
│   └── executor/v1.0.md
│
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── scripts/
│   ├── create_worktree.sh
│   ├── run_wave.sh
│   └── health_check.sh
│
└── config/
    ├── settings.py
    ├── models.yaml
    └── guardrails.yaml
```

## Decisões Arquiteturais

### ADR-001: LangGraph como Control Plane
**Decisão:** Usar LangGraph para orquestração stateful.
**Rationale:** Suporte nativo a checkpoints, paralelismo por DAG, persistência de estado e Human-in-the-Loop nativo.
**Alternativas consideradas:** LangChain puro (sem estado nativo), Celery (sem suporte a agentes).

### ADR-002: Git Worktrees para Isolamento de Waves
**Decisão:** Cada wave executa em uma worktree Git separada.
**Rationale:** Isolamento completo de contexto, paralelismo seguro, rollback trivial em caso de falha.
**Alternativas consideradas:** Branches simples (risco de contaminação), containers Docker (overhead).

### ADR-003: Akasha como Memória Separada dos Agentes
**Decisão:** Memória operacional em módulo dedicado, não dentro dos agentes.
**Rationale:** Permite reuso de padrões entre projetos, persiste entre sessões, evolui independentemente.
**Alternativas consideradas:** Memória in-context (perdida entre sessões), Redis puro (sem semântica).

### ADR-004: OpenHands SDK como Executor de Código
**Decisão:** OpenHands como engine de execução autônoma de código.
**Rationale:** 72-77% em SWE-bench Verified, event-sourced, modular, open-source.
**Alternativas consideradas:** SWE-agent (menos modular), mini-SWE-agent (para missões simples).
