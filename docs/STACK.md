# ⚙️ Stack Tecnológica do OMNIS

## Tabela Completa

| Camada | Tecnologia | Versão | Propósito |
|---|---|---|---|
| **Orquestração** | LangGraph | 0.2+ | Grafos stateful, checkpoints, paralelismo |
| **Times de Agentes** | CrewAI | 0.80+ | Squads com papéis e memória |
| **Execução de Código** | OpenHands SDK | Latest | Executor autônomo (72-77% SWE-bench Verified) |
| **LLM Principal** | Claude Sonnet 4.5 / Opus | Latest | Planejamento, arquitetura, review |
| **LLM de Execução** | Claude Code | Latest | Geração e edição de código |
| **LLM Rápido** | Claude Haiku | Latest | Tarefas simples, bookkeeping |
| **Vector DB** | ChromaDB / Pgvector | Latest | Memória semântica no Akasha |
| **Banco Relacional** | PostgreSQL | 16+ | Estado persistente, PatternRecords |
| **Cache / Event Bus** | Redis | 7+ | Pub/sub, cache de sessão |
| **Automações** | n8n | Self-hosted | Workflows e integrações externas |
| **RAG Framework** | LlamaIndex | 0.10+ | Pipeline de RAG no Akasha |
| **Linguagem** | Python | 3.11+ | Todo o backend |
| **Validação de Dados** | Pydantic | 2.x | Dataclasses tipadas |
| **Isolamento de Execução** | Git Worktrees | - | Paralelismo seguro por wave |
| **CI/CD** | GitHub Actions | - | Deploy automatizado |
| **Containerização** | Docker + Compose | - | Ambiente reproduzível |
| **Monitoramento** | Prometheus + Grafana | - | Métricas e dashboards |
| **Workflows RAG** | Dify | Latest | RAG reaproveitável por departamento |

## Por que essa Stack?

### LangGraph (vs LangChain puro)
- Suporte nativo a checkpoints e persistência de estado
- Human-in-the-Loop nativo (pause/resume)
- Paralelismo via DAG
- Streaming de eventos

### OpenHands SDK (vs SWE-agent, Devin)
- Open-source com 72-77% em SWE-bench Verified
- Arquitetura event-sourced (cada ação é um evento auditável)
- Modular: SDK + Tools + Workspace + Server
- Integração com múltiplos LLMs

### Git Worktrees (vs branches simples)
- Execução paralela de waves sem conflito
- Rollback imediato em caso de falha
- Isolamento completo de contexto por wave
- Merge controlado via script

### ChromaDB + PostgreSQL (vs Redis puro)
- ChromaDB: busca semântica para PatternRecords
- PostgreSQL: estado persistente estruturado
- Combinação permite busca híbrida (semântica + SQL)

## Benchmarks de Referência

| Sistema | SWE-bench Verified | Arquitetura |
|---|---|---|
| OpenHands + Claude Sonnet 4.5 | ~72-77% | Event-sourced, modular |
| SWE-agent | 18-23% (base) | Single-agent + ACI |
| mini-SWE-agent | ~65-74% | Minimalista (~100 linhas) |
| MASAI | ~28-32% | Multi-sub-agente |

*Fonte: SWE-bench.com leaderboard, papers 2024-2026*

## Roteamento de Modelos por Tarefa

```yaml
# config/models.yaml
model_routing:
  planning:
    primary: claude-opus-4
    fallback: claude-sonnet-4-5
    max_tokens: 8000
  
  architecture:
    primary: claude-opus-4
    fallback: gpt-4o
    max_tokens: 8000
  
  code_generation:
    primary: claude-sonnet-4-5     # Melhor custo-benefício para código
    fallback: claude-opus-4
    max_tokens: 16000
  
  code_review:
    primary: claude-opus-4
    fallback: claude-sonnet-4-5
    max_tokens: 4000
  
  bookkeeping:                     # Tarefas simples de organização
    primary: claude-haiku-3-5
    fallback: claude-sonnet-4-5
    max_tokens: 2000
  
  embedding:
    primary: text-embedding-3-large
    dimensions: 3072
```
