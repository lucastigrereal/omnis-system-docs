# 🧠 OMNIS — Motor de Execução Agêntica Modular

> **Holding Digital Inteligente | App Factory + Akasha + Control Plane**
> Versão: 1.0 | Maio 2026 | Autor: Lucas (Tigrão)

---

## O que é o OMNIS?

O **OMNIS** é um **motor de execução agêntica modular** — um sistema de inteligência artificial que orquestra múltiplos agentes, skills, crews e workflows para executar missões de IA de ponta a ponta, de forma semi-autônoma.

> **"Qualquer ideia em linguagem natural → sistema funcional, campanha ativa, decisão informada ou automação operando — sem precisar saber programar, sem ter que lembrar de nada, sem retrabalho."**

---

## 📚 Documentação

| Arquivo | Descrição |
|---|---|
| [MASTER.md](./docs/MASTER.md) | 📋 Documento Mestre Completo |
| [ARCHITECTURE.md](./docs/ARCHITECTURE.md) | 🏗️ Arquitetura das 4 Camadas |
| [APP_FACTORY.md](./docs/APP_FACTORY.md) | 🏭 App Factory — Spec Completa |
| [AKASHA.md](./docs/AKASHA.md) | 🧬 Akasha — Memória Operacional |
| [DEPARTMENTS.md](./docs/DEPARTMENTS.md) | 🏢 16 Departamentos + Squads |
| [WORKFLOWS.md](./docs/WORKFLOWS.md) | 🔄 Workflows por Departamento |
| [STACK.md](./docs/STACK.md) | ⚙️ Stack Tecnológica |
| [GUARDRAILS.md](./docs/GUARDRAILS.md) | 🛡️ Guardrails e Segurança |
| [ROADMAP.md](./docs/ROADMAP.md) | 🗺️ Roadmap Fases 1–4 |
| [GLOSSARY.md](./docs/GLOSSARY.md) | 📖 Glossário |

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

## 🚀 Quick Start para Devs

```bash
git clone https://github.com/lucastigrereal/omnis-system-docs
# Leia docs/MASTER.md primeiro
# Depois docs/ARCHITECTURE.md
# Depois o módulo que vai implementar
```

---

## 🧩 Os 3 Módulos Core

| Módulo | Função | Stack |
|---|---|---|
| **OMNIS Core** | Control plane, event bus, políticas | LangGraph + Redis + PostgreSQL |
| **App Factory** | Ideia → aplicativo funcional | CrewAI + OpenHands + Git Worktrees |
| **Akasha** | Memória operacional e aprendizado | ChromaDB + LlamaIndex + PostgreSQL |

---

## 📊 Estado Atual

- ✅ 365 testes automatizados
- ✅ 81 arquivos Python
- ✅ 17 skills operacionais
- ✅ App Factory (4 papéis ativos)
- ✅ Akasha com RAG básico
- 🔄 Economic Brain (em progresso)
- 📋 Dashboard web (planejado Q3 2026)

---

*OMNIS v1.0 — Maio 2026 | [lucastigrereal](https://github.com/lucastigrereal)*
