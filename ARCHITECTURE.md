# OMNIS — Arquitetura Completa

> Documento técnico de arquitetura do sistema OMNIS
> Versão: 1.0 | Maio 2026

---

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
│  RAG | PatternRecords | Prompts Versionados | Vector DB | SQL    │
├─────────────────────────────────────────────────────────────────┤
│  CAMADA 1 — OMNIS CORE (CONTROL PLANE)                          │
│  Event Bus | Orquestrador | Políticas | Roteamento de Modelos    │
└─────────────────────────────────────────────────────────────────┘
```

### Camada 1 — OMNIS Core (Control Plane)

O **cérebro central** do sistema. Não executa tarefas — orquestra quem executa.

**Responsabilidades:**
- **Event Bus (pub/sub):** todos os módulos se comunicam via eventos nomeados
  - Exemplos: `mission.started`, `wave.completed`, `pattern.saved`, `checkpoint.awaiting`
- **Orquestração de agentes:** decide qual squad, quais agentes e quais modelos são ativados
- **Políticas globais:** guardrails de segurança, limites de custo, prioridades
- **Roteamento de modelos:** modelo A para planejamento, modelo B para código, modelo C para review
- **Observabilidade:** loga cada evento com timestamp, custo em tokens, agente, resultado

**Stack:**
- LangGraph (grafos stateful com checkpoints)
- Python 3.11+
- Redis (event bus)
- PostgreSQL (estado persistente)
- Pydantic v2 (validação de dados)

### Camada 2 — Akasha (Memória Operacional)

A **memória de longo prazo** do OMNIS. Não é só RAG — é o banco de sabedoria acumulada.

Ver [AKASHA.md](./AKASHA.md) para spec completa.

### Camada 3 — Módulos Operacionais

Os **16 departamentos** que executam as missões específicas.

Ver [DEPARTMENTS.md](./DEPARTMENTS.md) para lista completa.

O módulo mais complexo é o **App Factory** — ver [APP_FACTORY.md](./APP_FACTORY.md).

### Camada 4 — Interface & Observabilidade

A **visão humana** do sistema em operação.

- Dashboard web com status de missões em tempo real
- Logs estruturados JSON com event timeline
- Métricas de custo, latência e qualidade
- Interface de aprovação para checkpoints Human-in-the-Loop

---

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

---

## 3. Regra do Mission Package

Toda missão OMNIS só é considerada **concluída** quando produz um **Mission Package** com:

1. ✅ Código/artefatos funcionais
2. ✅ Testes passando (cobertura mínima definida no PRD)
3. ✅ Documentação de uso
4. ✅ PatternRecord registrado no Akasha
5. ✅ Aprovação do operador humano no checkpoint final

---

## 4. Event Log — Estrutura

Todo evento no sistema é registrado neste formato:

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
    "files_created": ["src/models/user.py", "src/models/booking.py"],
    "tests_passed": 12,
    "tests_failed": 0
  }
}
```

---

## 5. Comparação com Padrões da Literatura

| Padrão | Onde aparece no OMNIS |
|---|---|
| Plan-then-Execute | Planner → Architect → PM → Executor |
| RTPM (Reflection, Tool-use, Planning, Multi-Agent) | Cada camada do OMNIS Core |
| Event-driven Multi-Agent | Event Bus Redis + módulos pub/sub |
| Orchestrator-Worker | OMNIS Core + Squads dinâmicos |
| RAG + Agents | Akasha consultado antes de cada decisão |
| Stateful Agent Graph | LangGraph com checkpoints por missão |
| Hierarchical Multi-Agent | Control Plane → Departamentos → Squads → Agentes |

---

## 6. Alinhamento com Benchmarks (SWE-bench)

A arquitetura do App Factory é comparável a:

| Sistema | SWE-bench Verified | Arquitetura similar |
|---|---|---|
| OpenHands SDK | ~72-77% | Event-sourced, modular — base do App Factory |
| MASAI | ~65-70% | Multi-papel separado por função (= Planner/Architect/PM/Executor) |
| SWE-agent | ~50-60% | ACI custom + worktrees |
| mini-SWE-agent | ~65-74% | Single-agent minimalista (base do modo Fast Track) |

O App Factory está arquiteturalmente no nível **MASAI + OpenHands**, que é o topo do mercado open-source.

---

*OMNIS Architecture v1.0 — Maio 2026*
