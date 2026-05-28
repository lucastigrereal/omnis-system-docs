# 🚀 Onboarding — Como Trabalhar com o OMNIS

## Para Dev IA (Claude Code, GPT-4, etc.)

Cole este prompt como contexto inicial antes de qualquer implementação:

```
Você está trabalhando no OMNIS — um motor de execução agêntica modular.

REPOSITÓRIO DE DOCUMENTAÇÃO: https://github.com/lucastigrereal/omnis-system-docs

ANTES DE QUALQUER COISA:
1. Leia docs/MASTER.md para entender o sistema completo
2. Leia docs/ARCHITECTURE.md para entender as camadas
3. Leia o doc do módulo que vai implementar

REGRAS INVIOLÁVEIS:
1. Toda nova funcionalidade precisa de testes (mínimo 70% cobertura)
2. Siga a estrutura de arquivos definida em ARCHITECTURE.md
3. Use as dataclasses existentes — não crie duplicatas
4. Todo evento deve ser logado no formato definido em GUARDRAILS.md
5. Guardrails são invioláveis — não remova verificações de segurança
6. Produza o Mission Package ao final (código + testes + docs)
7. Ao finalizar, sugira o PatternRecord para o Akasha

MISSÃO ATUAL: [descreva aqui o que quer implementar]
```

## Para Dev Humano

### Onboarding em 4 Passos

**Passo 1 — Leia a documentação (30-45 min)**
1. `docs/MASTER.md` — entenda o sistema completo
2. `docs/ARCHITECTURE.md` — entenda as camadas e decisões
3. `docs/APP_FACTORY.md` — entenda o módulo principal
4. `docs/GUARDRAILS.md` — entenda o que não pode ser violado

**Passo 2 — Configure o ambiente**
```bash
git clone https://github.com/lucastigrereal/omnis
cd omnis
cp config/.env.example config/.env
# Preencha as API keys no .env:
# ANTHROPIC_API_KEY=
# OPENAI_API_KEY=
# REDIS_URL=redis://localhost:6379
# DATABASE_URL=postgresql://...
docker-compose up -d
python scripts/health_check.sh
```

**Passo 3 — Rode uma missão de exemplo**
```bash
# Hello World do OMNIS
python core/orchestrator.py --mission examples/hello_world.yaml

# Observe o event log em tempo real
python core/observability.py --follow

# Explore os PatternRecords
python akasha/pattern_store.py --list
```

**Passo 4 — Comece pela menor wave possível**
- Não tente implementar tudo de uma vez
- Escolha uma wave (ex: W131 — Core Models)
- Implemente apenas os `allowed_files` daquela wave
- Rode os testes
- Só avance quando os testes passarem

### Convenções de Código

```python
# ✅ Correto — tipado, documentado, testável
@dataclass
class Wave:
    """Uma unidade atômica de trabalho do App Factory."""
    id: str
    name: str
    status: WaveStatus = WaveStatus.PENDING

# ❌ Errado — sem tipos, sem docs
class Wave:
    def __init__(self, id, name):
        self.id = id
        self.name = name
```

### Fluxo de Contribuição

```
1. Crie branch: git checkout -b feat/nome-da-feature
2. Implemente seguindo as dataclasses existentes
3. Escreva testes (mínimo 70% cobertura)
4. Rode: pytest tests/ --cov
5. Abra PR com tag [WAVE: W1XX] no título
6. Inclua no PR: o que mudou + testes passando + sugestão de PatternRecord
```

### Perguntas Frequentes

**Q: Posso criar novos agentes além dos definidos?**
A: Sim, via Capability Forge. Abra uma issue com tag [NEW SKILL] descrevendo a necessidade.

**Q: Como escolho qual modelo usar para uma tarefa?**
A: Consulte `config/models.yaml`. O roteamento de modelos é centralizado no OMNIS Core.

**Q: O que faço se uma wave falhar 3 vezes?**
A: O sistema escala automaticamente para o humano. Não tente bypass manual dos guardrails.

**Q: Posso modificar arquivos fora do allowed_files?**
A: Não. Isso é um guardrail inviolável. Se precisar modificar outros arquivos, crie uma nova wave.

**Q: Onde registro decisões arquiteturais importantes?**
A: Em `docs/ARCHITECTURE.md` na seção de ADRs, com PR e tag [ARCHITECTURE DECISION].
