# App Factory — Especificação Completa

## 1. O que é

O **App Factory** é o módulo do OMNIS responsável por transformar uma ideia em linguagem natural em um aplicativo funcional, executando 4 papéis sequenciais e um ciclo de waves de desenvolvimento.

## 2. Os 4 Papéis Sequenciais

### PAPEL 1 — Planner
- **Recebe:** ideia em linguagem natural
- **Produz:** `AppIdeaIntake` (dataclass) + PRD completo
- **PRD contém:** descrição do produto, usuários-alvo, funcionalidades, critérios de aceite, escopo, out-of-scope, riscos
- **Consulta Akasha** antes de começar (há PRD similar? qual resultado teve?)

### PAPEL 2 — Architect
- **Recebe:** PRD aprovado
- **Produz:** Blueprint técnico
- **Blueprint contém:** stack completa, árvore de arquivos, contratos de API (OpenAPI), diagrama de banco, decisões arquiteturais com justificativas
- **Consulta Akasha** (qual stack venceu em projetos similares?)

### PAPEL 3 — Program Manager (PM)
- **Recebe:** Blueprint aprovado
- **Produz:** Plano de waves
- **Plano de waves contém:** numeração (W131–W136+), dependências explícitas, paralelismo possível, `allowed_files` por wave, `expected_output` por wave, critérios de aceite binários

### PAPEL 4 — Executor
- **Recebe:** Plano de waves aprovado
- **Produz:** Código, testes, documentação
- **Por wave:** cria worktree Git isolada → implementa apenas `allowed_files` → roda testes → reporta → merge se passou → avança

## 3. Dataclasses Core

### AppIdeaIntake

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Optional

@dataclass
class AppIdeaIntake:
    raw_idea: str
    operator_id: str
    app_name: Optional[str] = None
    app_type: Optional[str] = None  # "web_app" | "api" | "mobile" | "automation"
    target_users: Optional[list[str]] = None
    core_problem: Optional[str] = None
    success_criteria: Optional[list[str]] = None
    similar_patterns: Optional[list[str]] = None
    recommended_stack: Optional[dict] = None
    id: str = field(default_factory=lambda: f"intake_{datetime.now().strftime('%Y%m%d_%H%M%S')}")
    created_at: datetime = field(default_factory=datetime.now)
    status: str = "PENDING"  # PENDING | PLANNING | ARCHITECTING | EXECUTING | DONE
```

### Wave

```python
@dataclass
class Wave:
    id: str                                  # Ex: "W131"
    name: str                                # Ex: "Core Models & Database Schema"
    description: str
    depends_on: list[str] = field(default_factory=list)
    parallel_with: list[str] = field(default_factory=list)
    allowed_files: list[str] = field(default_factory=list)
    expected_output: str = ""
    acceptance_criteria: list[str] = field(default_factory=list)
    status: str = "PENDING"                  # PENDING | RUNNING | PASSED | FAILED | SKIPPED
    attempts: int = 0
    max_attempts: int = 3
    test_results: Optional[dict] = None
    error_log: Optional[str] = None
    worktree_path: Optional[str] = None
    started_at: Optional[datetime] = None
    completed_at: Optional[datetime] = None
```

### Mission

```python
@dataclass
class Mission:
    id: str
    name: str
    type: str                   # "app_development" | "content" | "campaign" | "automation"
    initiator: str
    department: str
    squad: list[str]
    waves: list[Wave]
    status: str = "QUEUED"      # QUEUED | RUNNING | PAUSED | COMPLETED | FAILED
    current_wave: Optional[str] = None
    checkpoints: list[dict] = field(default_factory=list)
    awaiting_approval: bool = False
    approval_type: Optional[str] = None     # "PRD" | "BLUEPRINT" | "MISSION_PACKAGE"
    tokens_used: int = 0
    cost_usd: float = 0.0
    created_at: datetime = field(default_factory=datetime.now)
    started_at: Optional[datetime] = None
    completed_at: Optional[datetime] = None
    prd: Optional[dict] = None
    blueprint: Optional[dict] = None
    mission_package: Optional[dict] = None
```

## 4. Os 3 Checkpoints Human-in-the-Loop

```
CHECKPOINT 1: APROVAÇÃO DO PRD
├── Trigger: Planner conclui PRD
├── O que o humano revisa: objetivos, escopo, critérios de aceite, riscos
├── Opções: APROVAR | REVISAR (com comentários) | REJEITAR
└── Se aprovado: Architect inicia

CHECKPOINT 2: APROVAÇÃO DO BLUEPRINT
├── Trigger: Architect conclui Blueprint
├── O que o humano revisa: stack, árvore de arquivos, contratos de API
├── Opções: APROVAR | REVISAR | REJEITAR
└── Se aprovado: PM inicia waves

CHECKPOINT 3: APROVAÇÃO DO MISSION PACKAGE
├── Trigger: Todas as waves concluídas
├── O que o humano revisa: código funcional, resultados de testes, documentação
├── Opções: APROVAR E DEPLOYAR | REVISAR (nova wave de correção) | REJEITAR
└── Se aprovado: PatternRecord registrado + deploy
```

## 5. Guardrails Globais

```yaml
global:
  dryrun_default: true
  max_retries_per_wave: 3
  max_cost_per_mission_usd: 50.0
  max_tokens_per_wave: 100000

security:
  no_external_api_calls_without_approval: true
  no_financial_operations_without_human: true
  no_production_deploy_without_checklist: true
  allowed_file_extensions: [".py", ".ts", ".tsx", ".js", ".jsx", ".html", ".css", ".md", ".yaml", ".json", ".sql"]

code_quality:
  min_test_coverage: 0.70
  require_type_hints: true
  require_docstrings: true
  block_merge_on_failing_tests: true
```

## 6. Níveis de Risco

| Nível | Tipo de Ação | Política |
|---|---|---|
| 🟢 LOW | Leitura de dados, análise, pesquisa | Execução automática |
| 🟡 MEDIUM | Criação de arquivos, escrita em banco | dryrun=True → log → execute |
| 🔴 HIGH | Deploy, exclusão, envio de mensagens | Requer aprovação humana |
| 🚫 CRITICAL | Acesso financeiro, dados sensíveis, produção | Bloqueado até confirmação explícita |
