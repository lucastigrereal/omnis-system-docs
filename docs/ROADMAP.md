# 🗺️ OMNIS Roadmap — Fases 1–4

## FASE 1 — Fundação Sólida
**Status: 80% concluída**

- [x] OMNIS Core com event bus
- [x] App Factory — 4 papéis (Planner, Architect, PM, Executor)
- [x] Akasha com RAG básico e PatternRecords
- [x] 17 skills operacionais
- [x] 365 testes automatizados (81 arquivos Python)
- [x] Integração GitHub via MCP
- [ ] Economic Brain (monitoramento de custo em tempo real)
- [ ] Wave de Reflection entre Blueprint e Execução
- [ ] Modo Fast Track para missões simples

## FASE 2 — Operação Completa
**Target: Q3 2026**

- [ ] Dashboard de observabilidade web
- [ ] Todos os 16 departamentos com squads ativos
- [ ] Akasha com busca semântica avançada
- [ ] Integração completa n8n (todos os workflows)
- [ ] Sistema de alertas e notificações
- [ ] Capability Forge básico
- [ ] Métricas SWE-like para medir qualidade do App Factory
- [ ] Modo Fast Track single-agent para missões pequenas

## FASE 3 — Escala
**Target: Q4 2026**

- [ ] Multi-tenant (OMNIS como produto para clientes)
- [ ] Capability Forge automatizado
- [ ] Self-evolution pipeline (melhoria automática de prompts)
- [ ] Marketplace de skills e departamentos
- [ ] Economic Brain completo com orçamentos por projeto
- [ ] Dashboard mobile

## FASE 4 — OMNIS Verse
**Target: 2027**

- [ ] OMNIS como plataforma white-label
- [ ] API pública para terceiros
- [ ] Ecossistema de parceiros
- [ ] Versão SaaS com planos
- [ ] Certificação de agentes (marketplace curado)

---

## Backlog Priorizado (Próximas 3 Implementações)

### 🔥 PRIORIDADE 1 — Economic Brain
**O que é:** Componente que monitora custo em tokens e USD por missão em tempo real.
**Por que agora:** Risco de custo explosivo sem controle em multi-agente.
**Complexidade:** 2/5
**Impacto:** Alto

```python
@dataclass
class EconomicBrain:
    mission_budget_usd: float = 50.0
    wave_token_limit: int = 100_000
    alert_at_percent: float = 0.80
    
    def check_budget(self, mission: Mission) -> BudgetStatus:
        ...
    
    def should_pause_mission(self, mission: Mission) -> bool:
        ...
```

### 🔥 PRIORIDADE 2 — Wave de Reflection
**O que é:** Agente crítico que revisa o Blueprint antes de liberar as waves de código.
**Por que agora:** Evita seguir planos ruins que vão falhar na execução.
**Complexidade:** 2/5
**Impacto:** Alto

### 🔥 PRIORIDADE 3 — Modo Fast Track
**O que é:** Pipeline reduzida (Planner → Executor direto) para missões simples.
**Por que agora:** Overhead de 4 papéis para bugs simples é desnecessário.
**Complexidade:** 2/5
**Impacto:** Médio
