---
description: Lê o PBI.md e quebra em tasks com checkpoints, progresso e arquivos de acompanhamento
---

Você é um agente de planejamento técnico. Leia o PBI.md em `.ignore/PBI.md` (ou raiz se não encontrar) e transforme em tasks executáveis.

**Regra fundamental:** Cada task será executada por um agente **sem acesso ao seu contexto**. Tudo que ele precisa DEVE estar no arquivo da task.

---

## PASSO 1 — Leitura do PBI

Leia o PBI.md e extraia:
- Objetivo principal
- Funcionalidades exigidas (todas)
- Regras de negócio
- Escopo atual vs fora de escopo
- Entidades/conceitos de domínio (nomes exatos)

Se houver ambiguidade, tome uma decisão explícita e registre.

---

## PASSO 2 — Análise do projeto (captura de padrões)

Investigue o projeto para capturar padrões. Para cada padrão encontrado, anote **apenas o caminho do arquivo e a linha/função de referência** — NÃO copie blocos inteiros de código nas tasks. O agente executor usará o caminho como referência para consultar o padrão.

### O que investigar (apenas o que for relevante ao PBI):

**Backend:** Modelos (herança, campos padrão, multi-tenancy), Serializers, ViewSets/Views, URLs (router/urlpatterns), Services/lógica, Tasks assíncronas, Testes (framework, factories)

**Frontend:** Hooks de API (React Query), Componentes (padrão de arquivos), Formulários (validação), Navegação (rotas, sidebar), Tipos TypeScript, Testes

**Infra:** Permissões, Multi-tenancy, Variáveis de ambiente

**Output:** Uma lista de `arquivo:função/classe` para cada padrão relevante. Isso irá para "Padrões de Referência" nas tasks.

---

## PASSO 3 — Quebra em tasks

### Granularidade:
- ~3-8 checkpoints por task (menos = agrupe, mais = quebre)
- Backend e frontend juntos se simples, separados se complexo

### Independência:
- Cada task auto-contida — executável lendo APENAS aquele arquivo
- Se Task B depende de Task A, descreva **exatamente** o que Task A produz (nomes, endpoints, tipos)

### Ordenação:
- Models/migrations primeiro → APIs → Frontend → Testes
- Numere na ordem de execução: `TASK-01-NOME.md`, `TASK-02-NOME.md`
- Local: `.ignore/`

---

## PASSO 4 — Estrutura de cada TASK-XX.md

```markdown
# Task XX: [Título]

## Objetivo
[Frase verificável — "foi feito? sim/não"]

## Contexto
[Nomes ESPECÍFICOS: arquivos, classes, funções relevantes do projeto existente]

## Padrões de Referência
[Para cada padrão relevante, apenas o ponteiro:]
- Model base: `backend/common/models.py` → classe `CustomBaseTenantModel`
- Serializer exemplo: `backend/app/serializers/example.py` → classe `ExampleSerializer`
- Hook exemplo: `frontend/src/hooks/useExample.ts` → função `useExample`
[O agente DEVE abrir esses arquivos e seguir o padrão encontrado]

## Escopo
### [Item — ex: Model Budget]
- **Arquivo:** `caminho/exato` (criar/modificar)
- **O que:** [Descrição precisa]
- **Campos/Props:** [Lista com tipos]
- **Validações:** [Se houver]

## Critérios de Aceite
- [ ] [Condição verificável e binária]
- [ ] [...]

## Validação
```bash
[Comandos exatos para verificar completude]
`` `

## Arquivos
| Arquivo | Ação | O quê |
|---------|------|-------|
| `caminho` | Criar | ... |

## NÃO Fazer
- NÃO [erro previsível que IA cometeria]

## Dependências
- **Depende de:** Task XX — [o que precisa existir, com nomes concretos]
- **Produz:** [o que outras tasks consomem, com nomes concretos]
```

---

## PASSO 5 — Criar progresso.md

Crie `.ignore/progresso.md`:

```markdown
# Progresso — [Nome do PBI]

## Antes de Iniciar Qualquer Task
1. Leia "O Que Já Foi Feito" — evita retrabalho
2. Leia "Problemas Conhecidos" — evita repetir erros
3. Verifique "Próxima Task" — respeite a ordem

## Glossário
| Termo PBI | Código | Tipo | Local |
|-----------|--------|------|-------|
| Ex: Orçamento | `Budget` | Model | `backend/budgets/models.py` |

## Decisões Técnicas
| Decisão | Justificativa |
|---------|---------------|
| ... | ... |

## Contratos Entre Tasks
| Task | Produz | Consumido por |
|------|--------|---------------|
| TASK-01 | Model `Budget` (campos X,Y,Z) | TASK-02, TASK-04 |

## Tasks
| Task | Título | Status | Aceite |
|------|--------|--------|--------|
| TASK-01 | ... | pendente | 0/N |

## O Que Já Foi Feito
[Atualizado a cada task concluída]

## Problemas Conhecidos
| Problema | Task | Severidade |
|----------|------|------------|

## Próxima Task
TASK-01 — [Título]
```

---

## PASSO 6 — Criar ONDE-ENCONTRAR.md

Crie `.ignore/ONDE-ENCONTRAR.md`:

```markdown
# Onde Encontrar — [Nome do PBI]

## Resumo
[2-3 frases: o que mudou na plataforma]

## Por Página

### [Nome da Página]
**Caminho:** [Menu] → [Item] → [Aba]
**URL:** `/caminho`

- **[Funcionalidade]:** [Onde na tela + como interagir] (TASK-XX)

## Backend/Processos (sem UI)
| O quê | Tipo | Como verificar | Task |
|-------|------|----------------|------|
| API exportação | Endpoint | `GET /api/v1/exports/` | TASK-XX |

## Referência Rápida
| O quê | Onde | Como chegar |
|-------|------|-------------|
| ... | ... | ... |
```

---

## PASSO 7 — Regras finais

- NÃO invente funcionalidades fora do PBI
- NÃO crie tasks para itens marcados "futuros"
- SEMPRE referencie arquivos reais (não caminhos genéricos)
- Padrões de referência = caminhos, NÃO blocos de código colados
- Todos os arquivos criados juntos (tasks + progresso + onde-encontrar)
- Use nomenclatura consistente (Glossário = fonte de verdade)

### Finalização:
Apresente resumo com: quantidade de tasks, ordem de execução, decisões importantes, ambiguidades resolvidas.

Comece agora lendo o PBI.md e analisando o projeto.
