# OMNIS — Documento Mestre v1.0
> Motor de Execução Agêntica Modular | Holding Digital Inteligente
> Versão: 1.0 | Data: Maio 2026 | Autor: Lucas (Tigrão)

---

## 1. O QUE É O OMNIS

### 1.1 Definição Canônica

O **OMNIS** é um **motor de execução agêntica modular** — um sistema de inteligência artificial que orquestra múltiplos agentes, skills, crews e workflows para executar missões de IA de ponta a ponta, de forma semi-autônoma, sem exigir decisões técnicas do operador humano durante a execução.

O OMNIS não é um produto. É a **infraestrutura viva** que sustenta todos os produtos, serviços e operações da holding digital OMNIS Verse.

### 1.2 Visão Suprema

> **"Qualquer ideia em linguagem natural → sistema funcional, campanha ativa, decisão informada ou automação operando — sem precisar saber programar, sem ter que lembrar de nada, sem retrabalho."**

O operador humano (Lucas) é o **estrategista e aprovador**. O OMNIS é o **executor e memória**.

### 1.3 O que o OMNIS NÃO é

| Não é | É |
|---|---|
| Um chatbot | Um sistema agêntico de execução |
| Um único modelo de IA | Um orquestrador de múltiplos modelos |
| Uma ferramenta isolada | Uma infraestrutura modular integrada |
| Um produto final | A plataforma que cria e opera produtos |
| Um substituto humano | Um amplificador de capacidade humana |
| Rígido e monolítico | Modular, extensível, auto-evoluível |

### 1.4 A Analogia Certa

Pense no OMNIS como o **sistema operacional de uma empresa digital**:
- O **OMNIS Core** = kernel (gerencia recursos, segurança, eventos)
- O **App Factory** = compilador (transforma ideias em software)
- O **Akasha** = memória RAM + HD (contexto imediato + conhecimento persistente)
- Os **Departamentos** = processos do SO (cada um faz sua função especializada)
- Os **Squads** = threads (times dinâmicos que executam missões específicas)
- Os **Agentes** = workers (executam tarefas dentro dos squads)

---

## 2. ARQUITETURA GERAL

### 2.1 As 4 Camadas Horizontais

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

### 2.2 Fluxo Supremo de Qualquer Missão

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

### 2.3 Regra do Mission Package

Toda missão OMNIS só é considerada **concluída** quando produz um **Mission Package** com:
1. Código/artefatos funcionais
2. Testes passando (cobertura mínima definida no PRD)
3. Documentação de uso
4. PatternRecord registrado no Akasha
5. Aprovação do operador humano no checkpoint final

---

## 3. OS 3 MÓDULOS CORE

### 3.1 OMNIS Core (Control Plane)

**Função:** O cérebro central do sistema. Não executa tarefas — orquestra quem executa.

**Responsabilidades:**
- **Event Bus (pub/sub):** todos os módulos se comunicam via eventos nomeados
- **Orquestração de agentes:** decide qual squad, quais agentes e quais modelos são ativados
- **Políticas globais:** guardrails de segurança, limites de custo, prioridades de execução
- **Roteamento de modelos:** usa modelo A para planejamento, modelo B para código, modelo C para review
- **Observabilidade:** loga cada evento com timestamp, custo em tokens, agente responsável, resultado

**Stack:**
- LangGraph (grafos stateful com checkpoints)
- Python 3.11+
- Redis (event bus)
- SQLite/PostgreSQL (estado persistente)
- Pydantic (validação de dados)

**Checkpoints de Human-in-the-Loop:**
1. Aprovação do PRD (antes de qualquer código)
2. Aprovação do Blueprint (antes de waves de execução)
3. Aprovação do Mission Package final (antes de deploy)

---

### 3.2 App Factory (Orquestrador de Produto)

**Função:** Transformar uma ideia em linguagem natural em um aplicativo funcional.

**Os 4 Papéis Sequenciais:**

#### PAPEL 1 — Planner
- Recebe: ideia em linguagem natural
- Produz: `AppIdeaIntake` (dataclass) + PRD completo
- Consulta Akasha antes de começar

#### PAPEL 2 — Architect
- Recebe: PRD aprovado
- Produz: Blueprint técnico (stack, árvore de arquivos, contratos de API)
- Consulta Akasha (qual stack venceu em projetos similares?)

#### PAPEL 3 — Program Manager
- Recebe: Blueprint aprovado
- Produz: Plano de waves com dependências, `allowed_files` e critérios de aceite

#### PAPEL 4 — Executor
- Recebe: Plano de waves aprovado
- Por wave: cria worktree Git isolada → implementa → testa → merge se passou

**Guardrails Globais do App Factory:**
- `dryrun=True` como padrão antes de qualquer ação destrutiva
- Merge apenas após testes passarem (zero exceções)
- Máximo de 3 retries por wave antes de escalar para humano
- Nenhum arquivo fora da `allowed_files` pode ser modificado

---

### 3.3 Akasha (Memória Operacional)

**Função:** A memória de longo prazo do OMNIS.

**Estrutura de Memória (4 camadas):**

```
AKASHA
├── MEMÓRIA DE TRABALHO (contexto atual da missão)
│   └── SQLite em memória | TTL = duração da missão
│
├── MEMÓRIA EPISÓDICA (histórico de missões)
│   └── PostgreSQL | PRDs, blueprints, resultados, custos
│
├── MEMÓRIA SEMÂNTICA (conhecimento e padrões)
│   └── Chroma/Pgvector | Docs técnicos, stacks, padrões
│
└── MEMÓRIA PROCEDURAL (prompts e workflows)
    └── Git + banco | Prompts mestres versionados
```

---

## 4. PRINCÍPIOS DE DESIGN (INVIOLÁVEIS)

1. **Planejamento antes de execução:** Planta completa antes do primeiro `print("hello")`
2. **Separação de papéis:** Planner não implementa. Executor não planeja.
3. **Isolamento de contexto:** Cada wave em sua própria worktree. Sem contaminação de estado.
4. **Prompts como código:** Prompts mestres são versionados, testados e têm changelog.
5. **Human-in-the-Loop mínimo mas preciso:** 3 checkpoints, não mais, não menos.
6. **Observabilidade total:** Se não está logado, não aconteceu.
7. **Memória acumulativa:** Akasha não é cache — é o aprendizado do sistema.
8. **Dryrun por padrão:** Nunca destruir sem confirmar.
9. **Missão = Mission Package:** Missão sem artefatos documentados não é missão concluída.
10. **O sistema aprende:** Cada projeto deve deixar o próximo mais fácil.

---

*Para documentação detalhada de cada módulo, veja os arquivos individuais em `/docs/`*
