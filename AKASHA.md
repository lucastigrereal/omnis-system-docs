# Akasha — Memória Operacional do OMNIS

> O sistema de memória de longo prazo que faz o OMNIS ficar melhor a cada projeto
> Versão: 1.0 | Maio 2026

---

## O que é o Akasha?

O **Akasha** é a memória operacional do OMNIS. Não é só um RAG (Retrieval-Augmented Generation) — é o banco de sabedoria acumulada do sistema. Inspirado no conceito de "Registro Akáshico" (repositório universal de todo conhecimento), o Akasha guarda não só documentos, mas **decisões, contextos, lições aprendidas e padrões validados**.

### A diferença do Akasha para um RAG comum

| RAG Comum | Akasha |
|---|---|
| Indexa documentos | Indexa documentos + decisões + outcomes |
| Responde perguntas | Orienta decisões arquiteturais |
| Estático | Cresce com cada projeto |
| Sem aprendizado | Registra o que funcionou e o que falhou |
| Contexto curto | Memória de longo prazo estruturada |

---

## As 4 Camadas de Memória

```
AKASHA
├── MEMÓRIA DE TRABALHO
│   └── Contexto ativo da missão em execução
│       Armazenamento: SQLite em memória
│       TTL: duração da missão
│       Conteúdo: PRD atual, waves em execução, decisões da sessão
│
├── MEMÓRIA EPISÓDICA
│   └── Histórico de todas as missões completas
│       Armazenamento: PostgreSQL
│       TTL: permanente
│       Conteúdo: PRDs, blueprints, resultados, custos, erros
│
├── MEMÓRIA SEMÂNTICA
│   └── Conhecimento geral e padrões arquiteturais
│       Armazenamento: ChromaDB / Pgvector
│       TTL: permanente
│       Conteúdo: docs técnicos, stacks vencedoras, padrões aprovados
│
└── MEMÓRIA PROCEDURAL
    └── Prompts e workflows versionados
        Armazenamento: Git + banco
        TTL: permanente (versionado)
        Conteúdo: prompts mestres, playbooks, SOPs
```

---

## PatternRecord — O DNA de Cada Projeto

Ao final de cada projeto bem-sucedido, o Akasha registra um `PatternRecord`:

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Optional

@dataclass
class PatternRecord:
    """Registro de aprendizado de um projeto concluído no Akasha."""
    
    id: str
    project_id: str
    project_name: str
    
    pattern_type: str           # "web_app" | "api" | "mobile" | "automation" | "content"
    industry: Optional[str]     # "hospitality" | "ecommerce" | "saas" | etc.
    
    stack: dict                 # Stack completa usada
    decision_rationale: str     # Por que essa stack foi escolhida
    alternatives_considered: list[dict]  # Alternativas e por que foram rejeitadas
    
    outcome: str                # "SUCCESS" | "PARTIAL" | "FAILED"
    metrics: dict               # tests_passed, waves_total, cost_tokens, time_hours
    
    lessons_learned: list[str]
    anti_patterns: list[str]    # O que NÃO fazer neste tipo de projeto
    
    # Embeddings para busca semântica
    embedding_vector: Optional[list[float]] = None
    
    created_at: datetime = field(default_factory=datetime.now)
    tags: list[str] = field(default_factory=list)
```

---

## Fluxo de Consulta ao Akasha

```
[PLANNER recebe nova ideia]
        │
        ▼
[AKASHA — busca semântica]
        │
        ├─► Encontrou padrões similares (similaridade ≥ 0.75)?
        │   ├─ SIM → retorna PatternRecords ordenados por outcome
        │   └─ NÃO → retorna "no prior pattern — generate from scratch"
        │
        ▼
[PLANNER usa contexto do Akasha]
        │
        ├─► Stacks que funcionaram para este tipo
        ├─► Stacks que falharam (evitar!)
        ├─► PRDs similares como referência
        └─► Lições aprendidas de projetos anteriores
        │
        ▼
[ARCHITECT consulta Akasha novamente]
        │
        ├─► "Qual stack venceu em projetos web_app + saas + python?"
        └─► Akasha retorna as 3 melhores com métricas
        │
        ▼
[AO FINAL DO PROJETO]
        │
        └─► Novo PatternRecord registrado com todos os dados
```

---

## Configuração do Akasha

```yaml
akasha:
  # Busca semântica
  similarity_threshold: 0.75      # Mínimo de similaridade para sugerir padrão
  max_patterns_returned: 5        # Máximo de padrões retornados por consulta
  
  # Contexto
  max_context_tokens: 200000      # Máximo de contexto por agente
  
  # Embeddings
  embedding_model: "text-embedding-3-large"  # Modelo de embedding
  vector_dimensions: 3072
  
  # Retenção
  keep_all_patterns: true         # Não deletar PatternRecords
  index_all_prd: true             # Indexar todos os PRDs
  index_all_blueprints: true      # Indexar todos os blueprints
  
  # Prompts
  prompt_versioning: true         # Versionamento de prompts no Git
  prompt_min_version: "v1.0"      # Versão mínima aceitável
```

---

## Métricas do Akasha

| Métrica | Descrição | Meta |
|---|---|---|
| `pattern_hit_rate` | % vezes que encontrou padrão útil | > 60% |
| `pattern_accuracy` | % padrões sugeridos que foram aceitos | > 70% |
| `avg_patterns_per_query` | Média de padrões retornados | 2-4 |
| `total_patterns` | Total de PatternRecords indexados | crescente |
| `embedding_freshness` | % embeddings com menos de 30 dias | > 80% |

---

*Akasha Spec v1.0 — Maio 2026*
