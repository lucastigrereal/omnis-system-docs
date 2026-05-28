# 🏭 App Factory — Spec Completa

## O que é

O **App Factory** é o módulo do OMNIS responsável por transformar uma ideia em linguagem natural em um aplicativo funcional, executando 4 papéis sequenciais antes de qualquer linha de código.

## Os 4 Papéis

```
[IDEIA] → [PLANNER] → [ARCHITECT] → [PROGRAM MANAGER] → [EXECUTOR] → [APP]
              │             │               │                  │
           Gera PRD    Gera Blueprint   Gera Waves        Implementa
```

## Dataclasses Core

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
    app_type: Optional[str] = None          # "web_app" | "api" | "mobile" | "automation"
    target_users: Optional[list[str]] = None
    core_problem: Optional[str] = None
    success_criteria: Optional[list[str]] = None
    similar_patterns: Optional[list[str]] = None
    recommended_stack: Optional[dict] = None
    id: str = field(default_factory=lambda: f"intake_{datetime.now().strftime('%Y%m%d_%H%M%S')}")
    created_at: datetime = field(default_factory=datetime.now)
    status: str = "PENDING"
```

### Wave

```python
@dataclass
class Wave:
    id: str                                 # Ex: "W131"
    name: str
    description: str
    depends_on: list[str] = field(default_factory=list)
    parallel_with: list[str] = field(default_factory=list)
    allowed_files: list[str] = field(default_factory=list)
    expected_output: str = ""
    acceptance_criteria: list[str] = field(default_factory=list)
    status: str = "PENDING"                 # PENDING | RUNNING | PASSED | FAILED | SKIPPED
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
    approval_type: Optional[str] = None    # "PRD" | "BLUEPRINT" | "MISSION_PACKAGE"
    tokens_used: int = 0
    cost_usd: float = 0.0
    created_at: datetime = field(default_factory=datetime.now)
    started_at: Optional[datetime] = None
    completed_at: Optional[datetime] = None
    prd: Optional[dict] = None
    blueprint: Optional[dict] = None
    mission_package: Optional[dict] = None
```

## Workflow Detalhado

```
TRIGGER: ideia em linguagem natural
         │
         ▼
[01] INTAKE
     └─ Converter para AppIdeaIntake
     └─ Consultar Akasha: projetos similares
         │
         ▼
[02] PLANNER ⏸️ CHECKPOINT 1
     └─ Gerar PRD completo
     └─ Definir critérios de aceite
     └─ Mapear riscos
     └─ AGUARDA APROVAÇÃO HUMANA
         │
         ▼
[03] ARCHITECT ⏸️ CHECKPOINT 2
     └─ Definir stack completa
     └─ Gerar árvore de arquivos
     └─ Definir contratos de API
     └─ AGUARDA APROVAÇÃO HUMANA
         │
         ▼
[04] PROGRAM MANAGER
     └─ Quebrar em waves numeradas
     └─ Definir dependências
     └─ Definir allowed_files por wave
     └─ Definir critérios de aceite binários
         │
         ▼
[05] EXECUTOR (por wave)
     ┌─ Para cada wave:
     │   └─ Criar worktree Git isolada
     │   └─ Implementar allowed_files
     │   └─ Rodar testes
     │   └─ Se falhou: retry (máx 3) → escalate
     │   └─ Se passou: merge → próxima wave
     └─ Continuar até todas concluídas
         │
         ▼
[06] MISSION PACKAGE ⏸️ CHECKPOINT 3
     └─ Compilar artefatos
     └─ Compilar resultados de testes
     └─ Gerar documentação
     └─ AGUARDA APROVAÇÃO HUMANA
         │
         ▼
[07] AKASHA REGISTRATION
     └─ Criar PatternRecord
     └─ Atualizar base de padrões
```

## Guardrails

```yaml
app_factory:
  dryrun_default: true
  max_retries_per_wave: 3
  max_cost_per_mission_usd: 50.0
  max_tokens_per_wave: 100000
  block_merge_on_failing_tests: true
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
```

## Exemplo de Plano de Waves

```yaml
mission: "Sistema de Reservas para Hotel"
waves:
  - id: W131
    name: "Core Models & Database Schema"
    depends_on: []
    parallel_with: []
    allowed_files:
      - "src/models/user.py"
      - "src/models/booking.py"
      - "src/models/room.py"
      - "migrations/001_initial.sql"
    acceptance_criteria:
      - "Todos os models importam sem erro"
      - "Migration roda sem falha"
      - "Tests de model passam (min 10 testes)"

  - id: W132
    name: "API Routes & Controllers"
    depends_on: ["W131"]
    parallel_with: []
    allowed_files:
      - "src/routes/bookings.py"
      - "src/routes/rooms.py"
      - "src/controllers/booking_controller.py"
    acceptance_criteria:
      - "Endpoints respondem corretamente"
      - "Validação de input funciona"
      - "Tests de integração passam"

  - id: W133
    name: "Frontend Dashboard"
    depends_on: ["W132"]
    parallel_with: []
    allowed_files:
      - "frontend/src/pages/Dashboard.tsx"
      - "frontend/src/components/BookingCard.tsx"
      - "frontend/src/hooks/useBookings.ts"
    acceptance_criteria:
      - "Componentes renderizam sem erro"
      - "Integração com API funciona"
      - "Tests de componente passam"
```
