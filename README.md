# OMNIS — Documento Mestre v1.0
> Motor de Execução Agêntica Modular | Holding Digital Inteligente
> Versão: 1.0 | Data: Maio 2026 | Autor: Lucas (Tigrão)

---

## 🧭 Navegação da Documentação

| Arquivo | Conteúdo |
|---|---|
| [README.md](./README.md) | Visão geral, o que é, arquitetura macro |
| [ARCHITECTURE.md](./ARCHITECTURE.md) | 4 camadas, fluxo supremo, módulos core |
| [APP_FACTORY.md](./APP_FACTORY.md) | Os 4 papéis, waves, dataclasses, guardrails |
| [AKASHA.md](./AKASHA.md) | Memória operacional, PatternRecords, RAG |
| [DEPARTMENTS.md](./DEPARTMENTS.md) | Os 16 departamentos e squads dinâmicos |
| [WORKFLOWS.md](./WORKFLOWS.md) | Workflows por departamento |
| [STACK.md](./STACK.md) | Stack tecnológica completa com justificativas |
| [ROADMAP.md](./ROADMAP.md) | Fases 1–4 com checklist |
| [GLOSSARY.md](./GLOSSARY.md) | Glossário completo |

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

## 2. PRINCÍPIOS DE DESIGN (INVIOLÁVEIS)

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

## 3. ESTADO ATUAL (Maio 2026)

| Componente | Status |
|---|---|
| OMNIS Core (base) | ✅ Ativo |
| App Factory (4 papéis) | ✅ Ativo |
| Akasha (RAG básico) | ✅ Ativo |
| Skills (17 ativas) | ✅ Ativo |
| Testes do sistema | ✅ 365 testes / 81 arquivos Python |
| Integração GitHub | ✅ Ativo |
| Dashboard de observabilidade | 📋 Planejado Q3 2026 |
| Multi-tenant | 📋 Planejado Q4 2026 |

---

## 4. COMO USAR ESTA DOCUMENTAÇÃO

### Para Dev IA (Claude Code, GPT-4, etc.)

```
Você está trabalhando no OMNIS — um motor de execução agêntica modular.
Leia os arquivos neste repositório antes de qualquer decisão técnica.
Siga a estrutura de arquivos em ARCHITECTURE.md.
Use as dataclasses em APP_FACTORY.md — não crie duplicatas.
Guardrails em APP_FACTORY.md são invioláveis.
Produza Mission Package ao final (código + testes + docs).
```

### Para Dev Humano

1. Leia este README (10 min)
2. Leia ARCHITECTURE.md (20 min)
3. Leia APP_FACTORY.md + AKASHA.md (20 min)
4. Rode o ambiente local:
```bash
git clone https://github.com/lucastigrereal/omnis-system-docs
cd omnis
cp config/.env.example config/.env
docker-compose up -d
python scripts/health_check.sh
```
5. Comece sempre pela menor wave possível

---

*OMNIS Documento Mestre v1.0 — Maio 2026*
*Repositório: https://github.com/lucastigrereal/omnis-system-docs*
