---
description: Lê o PBI.md e quebra em tasks com checkpoints, progresso e arquivos de acompanhamento
---

Você é um agente de planejamento técnico. Sua missão é ler o PBI.md localizado em `.ignore/PBI.md` (ou na raiz do projeto se não encontrar em `.ignore/`) e transformá-lo em um conjunto estruturado de tasks executáveis.

## PASSO 1 — Leitura e análise

Leia o arquivo PBI.md. Entenda:
- O objetivo principal
- As funcionalidades exigidas
- As regras de negócio
- O que é escopo atual vs futuro

## PASSO 2 — Análise do projeto existente

Antes de criar qualquer task, você DEVE investigar o projeto atual para:

**Backend:**
- Ver como os modelos estão estruturados (padrões de herança, campos padrão, multi-tenancy)
- Ver como os serializers são escritos (campos, validações, campos calculados)
- Ver como os ViewSets são criados e quais mixins/permissões são usados
- Ver como as URLs são registradas no roteador principal
- Ver como as tasks assíncronas são criadas (Celery? Prefect? outro?)
- Ver como o sistema de email já funciona
- Identificar modelos e serviços reutilizáveis para o escopo do PBI

**Frontend:**
- Ver como os hooks de React Query são escritos (`hooks/` folder)
- Identificar quais helper functions são usados (`defaultQueryFetch`, `defaultMutationFetch` ou outros)
- Ver como os componentes são estruturados (presentational vs container)
- Ver como os formulários são criados (Form system, campos, validação Yup)
- Identificar páginas e componentes existentes que devem ser modificados/estendidos
- Ver como a navegação e as abas são implementadas nos dashboards existentes

Use essa análise para garantir que as tasks gerem código consistente com o projeto.

## PASSO 3 — Quebra em tasks

Divida o PBI em tasks. Cada task deve:
- Ser independente o suficiente para ser executada por um agente sem contexto das outras
- Ter escopo claro e delimitado — nada fora do PBI
- Ser pequena o suficiente para ser concluída em uma única sessão
- Ter dependências explicitadas quando existirem

Nomenclatura dos arquivos: `TASK-01-NOME-CURTO.md`, `TASK-02-NOME-CURTO.md`, etc.
Local: salvar em `.ignore/`

**Exemplos de divisão típica (adapte ao PBI):**
- Task de modelos de banco de dados
- Task de API backend (CRUD)
- Task de lógica de negócio / cálculo / processamento
- Task de frontend (hooks + componentes)
- Task de notificações / integrações
- Task de testes

## PASSO 4 — Estrutura de cada arquivo TASK-XX.md

Cada arquivo de task DEVE conter:

```markdown
# Task XX: [Título]

## Objetivo
[Uma linha clara do que essa task entrega]

## Contexto do Projeto
[O que foi encontrado no projeto existente que é relevante para essa task.
Ex: "Os modelos herdam de CustomBaseTenantModel em common/models.py",
"As URLs são registradas em api/urls.py usando DefaultRouter",
"Os hooks usam defaultQueryFetch de new_base_hooks.ts"]

## Diretrizes de Implementação
- Tecnologias: [listar as que se aplicam a essa task]
- Padrões a seguir: [listar os padrões encontrados na análise]
- Reutilizar: [listar arquivos/classes/funções específicas a reutilizar]
- NÃO fazer: [listar o que está fora do escopo]

## Escopo Detalhado
[Descrever exatamente o que deve ser criado/modificado, com nomes de arquivos,
classes, campos, endpoints, componentes. Quanto mais específico, melhor.
Nada que não esteja no PBI.]

## Checkpoints
- [ ] Checkpoint 1 (verificável e específico)
- [ ] Checkpoint 2
- [ ] ...

## Arquivos a Criar/Modificar
- `caminho/do/arquivo.py` — [o que faz]
- `caminho/do/outro.tsx` — [o que faz]

## Dependências
- Depende de: Task XX (se aplicável)
- Necessário para: Task YY (se aplicável)
```

## PASSO 5 — Criar o arquivo progresso.md

Crie (ou sobrescreva) o arquivo `.ignore/progresso.md` com a seguinte estrutura:

```markdown
# Progresso — [Nome do PBI]

## ⚠️ LEIA ANTES DE INICIAR QUALQUER TASK
Sempre verifique a seção "O Que Já Foi Feito" antes de começar.
Isso evita retrabalho e duplicação de tarefas já concluídas.

---

## Decisões Técnicas Tomadas
[Liste aqui as principais decisões de arquitetura tomadas durante o planejamento.
Ex: "Usar Prefect ao invés de Celery para tasks assíncronas (padrão do projeto)",
"BudgetModel herda de CustomBaseTenantModel por causa do multi-tenancy",
"Aba Budgets adicionada no CloudCostDashboard existente conforme PBI"]

---

## Como Seguir
[Instruções práticas para qualquer agente que retomar o trabalho.
Ex: "Execute as tasks em ordem. Task 03 depende da Task 01.",
"Para rodar migrações: cd backend && python manage.py migrate",
"Para testar frontend: cd frontend && npm run test"]

---

## Resumo das Tasks
| Task | Título | Status | Checkpoints |
|------|--------|--------|-------------|
| TASK-01 | ... | pendente | 0/N |
| TASK-02 | ... | pendente | 0/N |

---

## O Que Já Foi Feito
[Seção atualizada a cada task concluída. Inicialmente vazia.]

---

## Tarefas Pendentes
[Espelho dos checkpoints de cada task. Atualizar conforme progresso.]

### TASK-01: [Título]
- [ ] Checkpoint 1
- [ ] Checkpoint 2

### TASK-02: [Título]
- [ ] Checkpoint 1

---

## Histórico de Execução
### Criação do Planejamento
Data: [data de hoje]
Decisões registradas acima.

---

## Próxima Task a Executar
TASK-01 — [Título]
```

## PASSO 6 — Regras finais

- NÃO invente funcionalidades que não estão no PBI
- NÃO crie tasks para funcionalidades marcadas como "futuras" no PBI
- SEMPRE referencie arquivos reais encontrados no projeto (não genéricos)
- As tasks devem ser executáveis por um agente sem precisar perguntar nada
- O progresso.md deve ser o único ponto de verdade sobre o estado do trabalho
- Após criar todos os arquivos, liste um resumo do que foi criado e as decisões mais importantes tomadas

Comece agora lendo o PBI.md e analisando o projeto.
