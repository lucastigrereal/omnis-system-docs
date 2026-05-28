# 🔄 Workflows por Departamento

## 1. App Factory — Desenvolvimento de Aplicativo

```
TRIGGER: ideia em linguagem natural
         │
[01] INTAKE → AppIdeaIntake + consulta Akasha
[02] PLANNER ⏸️ → PRD + APROVAÇÃO HUMANA
[03] ARCHITECT ⏸️ → Blueprint + APROVAÇÃO HUMANA
[04] PROGRAM MANAGER → Waves com allowed_files
[05] EXECUTOR → Wave por wave (worktree isolada)
[06] MISSION PACKAGE ⏸️ → APROVAÇÃO HUMANA
[07] AKASHA → PatternRecord registrado
```

## 2. Produção de Conteúdo

```
TRIGGER: briefing (tema, objetivo, plataforma, tom)
         │
[01] STRATEGY → ângulo, gancho, formato
               └─ Consulta Akasha: performance de conteúdos similares
         │
[02] CREATION (paralelo)
     ├─ ScriptAgent: roteiro/copy
     ├─ DesignAgent: assets visuais
     └─ SEOAgent: keywords e otimizações
         │
[03] REVIEW
     ├─ EditorAgent: qualidade
     └─ ComplianceAgent: política
         │
[04] PUBLISH → PublisherAgent via n8n
         │
[05] ANALYTICS (24h depois)
     └─ Métricas → Akasha para calibrar futuros
```

## 3. Campanha de Tráfego

```
TRIGGER: objetivo (produto, orçamento, prazo, público)
         │
[01] STRATEGY → KPIs alvo (CPL, ROAS, CPA)
[02] CREATIVE (paralelo)
     ├─ CopyAgent: headlines, body, CTAs
     └─ DesignAgent: criativos
[03] SETUP → configurar campanhas + tracking
[04] OPTIMIZATION (contínuo, a cada 6h)
     ├─ Pausar criativos abaixo do target
     └─ Escalar criativos acima do target
[05] REPORT → relatório semanal automático
```

## 4. Automação com n8n

```
TRIGGER: necessidade descrita em linguagem natural
         │
[01] DESIGN → mapear gatilhos e ações
[02] BUILD → n8nBuilderAgent constrói
[03] TEST → dados simulados + cenários de falha
[04] DEPLOY → ativar + MonitorAgent
```

## 5. Estratégia & BI

```
TRIGGER: questão estratégica ou decisão a tomar
         │
[01] RESEARCH → MarketAnalyst + Perplexity
[02] ANALYSIS → BIAgent + Python
[03] SYNTHESIS → StrategyAgent → recomendações
[04] REPORT → executivo + acionável
[05] AKASHA → registrar decisão + outcome esperado
```

## 6. Atendimento & Suporte

```
TRIGGER: ticket de cliente
         │
[01] TRIAGE → SupportAgent classifica complexidade
              ├─ Simples → resposta automática (Claude Haiku)
              ├─ Médio → EscalationAgent + base de conhecimento
              └─ Complexo → ResolverAgent (Claude Opus)
[02] RESOLUTION → resposta + ação corretiva
[03] DOCUMENTATION → DocAgent atualiza FAQ/base
[04] AKASHA → registrar padrão de problema se recorrente
```

## Event Bus — Todos os Eventos do Sistema

```json
{
  "event_id": "evt_20260528_143022_7f3a",
  "event_type": "wave.completed",
  "mission_id": "mission_20260528_142000",
  "wave_id": "W132",
  "agent_id": "executor_agent_01",
  "model_used": "claude-sonnet-4-5",
  "timestamp": "2026-05-28T14:30:22Z",
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
