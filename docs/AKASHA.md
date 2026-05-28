# 🧬 Akasha — Memória Operacional do OMNIS

## O que é

O **Akasha** é a memória de longo prazo do OMNIS. Não é apenas um RAG — é o banco de sabedoria acumulada do sistema, que fica mais inteligente a cada projeto concluído.

## Estrutura de Memória (4 camadas)

```
AKASHA
├── MEMÓRIA DE TRABALHO
│   ├── Propósito: contexto atual da missão em execução
│   ├── Storage: SQLite em memória
│   └── TTL: duração da missão
│
├── MEMÓRIA EPISÓDICA
│   ├── Propósito: histórico de todas as missões completas
│   ├── Storage: PostgreSQL
│   └── Dados: PRDs, blueprints, resultados, custos, lições
│
├── MEMÓRIA SEMÂNTICA
│   ├── Propósito: conhecimento geral e padrões arquiteturais
│   ├── Storage: ChromaDB / Pgvector
│   └── Dados: docs técnicos, stacks aprovadas, padrões de design
│
└── MEMÓRIA PROCEDURAL
    ├── Propósito: prompts e workflows versionados
    ├── Storage: Git + banco relacional
    └── Dados: prompts mestres, playbooks, SOPs
```

## PatternRecord — O DNA de Cada Projeto

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Optional

@dataclass
class PatternRecord:
    id: str
    project_id: str
    project_name: str
    pattern_type: str           # "web_app" | "api" | "mobile" | "automation" | "content"
    industry: Optional[str]     # "hospitality" | "ecommerce" | "saas" | etc.
    stack: dict                 # Stack completa usada
    decision_rationale: str     # Por que essa stack foi escolhida
    alternatives_considered: list[dict]
    outcome: str                # "SUCCESS" | "PARTIAL" | "FAILED"
    metrics: dict               # tests_passed, waves_total, cost_tokens, time_hours
    lessons_learned: list[str]
    anti_patterns: list[str]    # O que NÃO fazer neste tipo de projeto
    embedding_vector: Optional[list[float]] = None
    created_at: datetime = field(default_factory=datetime.now)
    tags: list[str] = field(default_factory=list)
```

## Fluxo de Consulta

```
[PLANNER recebe nova ideia]
        │
        ▼
[AKASHA: busca PatternRecords similares]
[similaridade semântica ≥ 0.75]
        │
        ├─ Encontrou? → Retorna stacks vencedoras, PRDs similares, lições
        └─ Não encontrou? → Retorna templates genéricos por tipo
        │
        ▼
[PLANNER/ARCHITECT usam contexto]
[Para decidir stack, estrutura, abordagem]
        │
        ▼
[Ao final do projeto]
        │
        ▼
[AKASHA registra novo PatternRecord]
[Com stack, outcome, métricas, lições]
```

## Configuração

```yaml
akasha:
  pattern_similarity_threshold: 0.75
  max_context_tokens: 200000
  vector_db: chromadb                    # ou pgvector
  embedding_model: text-embedding-3-large
  
  retention:
    working_memory_ttl_hours: 24
    episodic_memory_retention: permanent
    semantic_memory_retention: permanent
    procedural_memory_retention: permanent

  search:
    top_k_patterns: 5
    min_similarity: 0.75
    include_failed_patterns: true        # Aprender com falhas também
```

## Prompts Versionados

Todos os prompts críticos são tratados como código:

```
prompts/
├── planner/
│   ├── v1.0.md          # Versão inicial
│   ├── v1.1.md          # Melhorias baseadas em PatternRecords
│   └── current.md       # Symlink para versão ativa
├── architect/
├── program_manager/
└── executor/
```

Cada versão de prompt inclui:
- Data de criação
- Motivo da mudança
- Métricas de performance antes/depois
- Rollback: qual versão substituiu
