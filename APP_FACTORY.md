# App Factory — Spec Completa

> Módulo de transformação de ideias em aplicativos funcionais
> Versão: 1.0 | Maio 2026

---

## Visão Geral

O **App Factory** é o módulo do OMNIS responsável por transformar uma ideia em linguagem natural em um aplicativo funcional, executando 4 papéis sequenciais com máxima autonomia e mínima intervenção humana.

**Fluxo macro:**
```
Ideia (texto) → PRD → Blueprint → Waves → Código → Testes → Mission Package
```

---

## Os 4 Papéis Sequenciais

### PAPEL 1 — Planner

**Recebe:** ideia em linguagem natural

**Processo:**
1. Converte para `AppIdeaIntake` (dataclass validada)
2. Consulta Akasha: há projetos similares? Qual stack venceu?
3. Gera PRD completo

**Produz: PRD completo com:**
- Descrição do produto e proposta de valor
- Usuários-alvo e personas
- Funcionalidades core (must-have) e nice-to-have
- Critérios de aceite mensuráveis (binários: passa/falha)
- Escopo claro + out-of-scope explícito
- Riscos técnicos e de negócio mapeados
- Estimativa de complexidade (waves necessárias)

**⏸️ CHECKPOINT 1: Aprovação humana do PRD**

---

### PAPEL 2 — Architect

**Recebe:** PRD aprovado

**Processo:**
1. Consulta Akasha: qual stack funcionou para este tipo de app?
2. Justifica escolhas com base em PatternRecords
3. Gera Blueprint técnico completo

**Produz: Blueprint técnico com:**
- Stack completa com justificativas
- Árvore de arquivos completa (todo arquivo que existirá)
- Contratos de API (OpenAPI spec)
- Schema de banco de dados
- Diagramas de fluxo de dados
- Decisões arquiteturais com alternativas consideradas
- Estimativa de custo em tokens

**⏸️ CHECKPOINT 2: Aprovação humana do Blueprint**

---

### PAPEL 3 — Program Manager (PM)

**Recebe:** Blueprint aprovado

**Produz: Plano de waves com:**
- Numeração sequencial (ex: W131, W132, W133...)
- Dependências explícitas entre waves
- Paralelismo possível identificado
- `allowed_files` por wave (lista EXATA de arquivos)
- `expected_output` por wave (descrição do resultado)
- Critérios de aceite binários por wave
- Ordem de merge respeitando dependências

---

### PAPEL 4 — Executor

**Recebe:** Plano de waves aprovado

**Por cada wave:**
1. Cria worktree Git isolada
2. Implementa APENAS os `allowed_files` da wave
3. Roda suite de testes
4. Se falhou: retry (máx 3x) → escala para humano
5. Se passou: merge → próxima wave

**⏸️ CHECKPOINT 3: Aprovação humana do Mission Package final**

---

## Estrutura de uma Wave (Dataclass)

```python
from dataclasses import dataclass, field
from datetime import datetime
from typing import Optional
from enum import Enum

class WaveStatus(Enum):
    PENDING = "PENDING"
    RUNNING = "RUNNING"
    PASSED = "PASSED"
    FAILED = "FAILED"
    SKIPPED = "SKIPPED"

@dataclass
class Wave:
    """Uma unidade atômica de trabalho do App Factory."""
    
    id: str                                  # Ex: "W131"
    name: str                                # Ex: "Core Models & Database Schema"
    description: str                         # O que esta wave entrega
    
    depends_on: list[str] = field(default_factory=list)     # IDs de waves anteriores
    parallel_with: list[str] = field(default_factory=list)  # Pode rodar em paralelo com
    
    allowed_files: list[str] = field(default_factory=list)  # Arquivos permitidos
    expected_output: str = ""                # Descrição do resultado esperado
    acceptance_criteria: list[str] = field(default_factory=list)  # Critérios binários
    
    # Estado de execução
    status: WaveStatus = WaveStatus.PENDING
    attempts: int = 0
    max_attempts: int = 3
    
    # Resultados
    test_results: Optional[dict] = None
    error_log: Optional[str] = None
    worktree_path: Optional[str] = None
    
    started_at: Optional[datetime] = None
    completed_at: Optional[datetime] = None
```

---

## AppIdeaIntake (Dataclass)

```python
@dataclass
class AppIdeaIntake:
    """Input de uma ideia de aplicativo em linguagem natural."""
    
    raw_idea: str                            # Ideia original do operador
    operator_id: str                         # Quem submeteu
    
    # Campos derivados pelo Planner
    app_name: Optional[str] = None
    app_type: Optional[str] = None           # "web_app" | "api" | "mobile" | "automation"
    target_users: Optional[list[str]] = None
    core_problem: Optional[str] = None
    success_criteria: Optional[list[str]] = None
    
    # Contexto do Akasha
    similar_patterns: Optional[list[str]] = None  # IDs de PatternRecords similares
    recommended_stack: Optional[dict] = None
    
    # Metadados
    id: str = field(default_factory=lambda: f"intake_{datetime.now().strftime('%Y%m%d_%H%M%S')}")
    created_at: datetime = field(default_factory=datetime.now)
    status: str = "PENDING"
```

---

## Guardrails do App Factory

```yaml
app_factory:
  dryrun_default: true              # Toda ação destrutiva é dryrun por padrão
  max_retries_per_wave: 3           # Máximo de tentativas antes de escalar
  max_cost_per_mission_usd: 50.0    # Limite de custo por missão
  max_tokens_per_wave: 100000       # Limite de tokens por wave
  block_merge_on_failing_tests: true
  min_test_coverage: 0.70           # Mínimo 70% de cobertura
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

---

## Exemplos de Waves

### Exemplo: App de Reservas Hoteleiras

```yaml
waves:
  - id: W131
    name: "Core Models & Database Schema"
    depends_on: []
    parallel_with: []
    allowed_files:
      - "src/models/user.py"
      - "src/models/hotel.py"
      - "src/models/booking.py"
      - "src/models/room.py"
      - "migrations/001_initial.sql"
    expected_output: "Modelos core definidos com relacionamentos corretos"
    acceptance_criteria:
      - "Todos os modelos importam sem erro"
      - "Migration roda sem erro"
      - "Testes de unidade dos modelos passam"

  - id: W132
    name: "Authentication & Users API"
    depends_on: ["W131"]
    parallel_with: []
    allowed_files:
      - "src/api/auth.py"
      - "src/api/users.py"
      - "src/services/auth_service.py"
      - "tests/test_auth.py"
    expected_output: "Endpoints de auth e users funcionando com JWT"
    acceptance_criteria:
      - "POST /auth/login retorna JWT válido"
      - "GET /users/me retorna dados do usuário autenticado"
      - "Testes de integração de auth passam"

  - id: W133
    name: "Hotels & Rooms API"
    depends_on: ["W131"]
    parallel_with: ["W132"]  # Pode rodar em paralelo com W132
    allowed_files:
      - "src/api/hotels.py"
      - "src/api/rooms.py"
      - "src/services/hotel_service.py"
      - "tests/test_hotels.py"
    expected_output: "CRUD completo de hotéis e quartos"
    acceptance_criteria:
      - "GET /hotels retorna lista paginada"
      - "POST /hotels cria hotel com validações"
      - "Testes de CRUD passam"
```

---

## Modo Fast Track (Para Missões Simples)

Para apps simples (bug fix, refactor, feature pequena), o App Factory tem um **modo Fast Track** que pula as waves intermediárias:

```
Ideia → Planner (PRD simplificado) → Executor (1 wave) → Testes → Mission Package
```

**Trigger automático quando:**
- Escopo cabe em menos de 5 arquivos
- Estimativa de waves é 1
- Tipo: `bug_fix` | `refactor` | `small_feature`

---

*App Factory Spec v1.0 — Maio 2026*
