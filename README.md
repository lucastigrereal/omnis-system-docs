# 🧠 OMNIS — Motor de Execução Agêntica Modular

> **Holding Digital Inteligente | App Factory + Akasha + Control Plane**
> Versão: 1.0 | Data: Maio 2026 | Autor: Lucas (Tigrão)

---

## O que é o OMNIS?

O **OMNIS** é um **motor de execução agêntica modular** — um sistema de inteligência artificial que orquestra múltiplos agentes, skills, crews e workflows para executar missões de IA de ponta a ponta, de forma semi-autônoma, sem exigir decisões técnicas do operador humano durante a execução.

O OMNIS não é um produto. É a **infraestrutura viva** que sustenta todos os produtos, serviços e operações da holding digital OMNIS Verse.

### Visão Suprema

> **"Qualquer ideia em linguagem natural → sistema funcional, campanha ativa, decisão informada ou automação operando — sem precisar saber programar, sem ter que lembrar de nada, sem retrabalho."**

O operador humano (Lucas) é o **estrategista e aprovador**. O OMNIS é o **executor e memória**.

---

## 📚 Documentação

| Documento | Descrição |
|---|---|
| [ARCHITECTURE.md](./ARCHITECTURE.md) | Arquitetura completa — 4 camadas, fluxos, control plane |
| [APP_FACTORY.md](./APP_FACTORY.md) | Spec do App Factory — 4 papéis, waves, dataclasses |
| [AKASHA.md](./AKASHA.md) | Memória operacional — RAG, PatternRecords, aprendizado |
| [DEPARTMENTS.md](./DEPARTMENTS.md) | 16 departamentos — funções, squads, outputs |
| [WORKFLOWS.md](./WORKFLOWS.md) | Workflows por departamento — fluxos visuais |
| [STACK.md](./STACK.md) | Stack tecnológica — ferramentas, versões, benchmarks |
| [DATACLASSES.md](./DATACLASSES.md) | Modelos de dados — Python dataclasses core |
| [GUARDRAILS.md](./GUARDRAILS.md) | Segurança, políticas, Human-in-the-Loop |
| [ROADMAP.md](./ROADMAP.md) | Fases 1–4 com checklist e status |
| [GLOSSARY.md](./GLOSSARY.md) | Glossário completo de termos |
| [ONBOARDING.md](./ONBOARDING.md) | Como passar para um dev (humano ou IA) |

---

## 🏗️ Arquitetura em 4 Camadas

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

---

## ⚡ Os 3 Módulos Core

### 1. OMNIS Core (Control Plane)
O cérebro central. Não executa tarefas — orquestra quem executa.
- Event Bus pub/sub entre todos os módulos
- Roteamento de modelos por tipo de tarefa
- Políticas globais de segurança e custo
- Observabilidade total (logs estruturados JSON)

### 2. App Factory (Orquestrador de Produto)
Transforma ideias em aplicativos via 4 papéis sequenciais:
```
Planner → Architect → Program Manager → Executor
```
Com waves numeradas, allowed_files, testes automáticos e 3 checkpoints humanos.

### 3. Akasha (Memória Operacional)
A memória de longo prazo do OMNIS:
- Memória de Trabalho (contexto da missão atual)
- Memória Episódica (histórico de todas as missões)
- Memória Semântica (padrões arquiteturais, stacks)
- Memória Procedural (prompts mestres versionados)

---

## 🎯 Princípios de Design (Invioláveis)

1. **Planejamento antes de execução** — planta completa antes do primeiro `print("hello")`
2. **Separação de papéis** — Planner não implementa. Executor não planeja.
3. **Isolamento de contexto** — cada wave em sua própria worktree Git
4. **Prompts como código** — versionados, testados, com changelog
5. **Human-in-the-Loop mínimo mas preciso** — 3 checkpoints, não mais
6. **Observabilidade total** — se não está logado, não aconteceu
7. **Memória acumulativa** — Akasha não é cache, é aprendizado
8. **Dryrun por padrão** — nunca destruir sem confirmar
9. **Missão = Mission Package** — código + testes + docs
10. **O sistema aprende** — cada projeto deve deixar o próximo mais fácil

---

## 📊 Status Atual (Maio 2026)

| Componente | Status |
|---|---|
| OMNIS Core (base) | ✅ Ativo |
| App Factory (4 papéis) | ✅ Ativo |
| Akasha (RAG básico) | ✅ Ativo |
| 17 Skills operacionais | ✅ Ativo |
| 365 testes automatizados | ✅ Passando |
| Economic Brain | 🔄 Em desenvolvimento |
| Dashboard de Observabilidade | 📋 Planejado Q3 2026 |
| Multi-tenant | 📋 Planejado Q4 2026 |

---

## 🔗 Links Importantes

- **Repositório principal (código):** `lucastigrereal-dev/omnis`
- **Documentação (este repo):** `lucastigrereal/omnis-system-docs`
- **Benchmarks de referência:** [SWE-bench Verified](https://www.swebench.com)
- **Engine de execução:** [OpenHands SDK](https://github.com/All-Hands-AI/OpenHands)

---

*OMNIS System Docs v1.0 — Maio 2026*
*Para contribuir: abra PR com tag `[DOCS UPDATE]`*
