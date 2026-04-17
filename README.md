# OpenCode — Comandos e Skills

Referencia rapida de todos os comandos e skills disponiveis.

---

## Comandos (`/comando`)

### Fluxo de Trabalho

| Comando | O que faz |
|---------|-----------|
| `/commit` | Cria um commit git com mensagem automatica baseada nas mudancas |
| `/commit-push-pr` | Faz commit, push e abre uma PR no GitHub de uma vez |
| `/review-pr` | Faz review de PR com agentes especializados e publica um unico comentario consolidado na PR com problemas e sugestoes |
| `/code-review` | Review de codigo de uma PR usando comentarios inline no GitHub |
| `/feature-dev` | Guia desenvolvimento de feature nova — analisa o codebase, planeja arquitetura e implementa |
| `/clean-gone` | Limpa branches locais que ja foram deletadas no remote (marcadas como `[gone]`) |

### Gestao de Tarefas

| Comando | O que faz |
|---------|-----------|
| `/create-card` | Coleta info sobre uma tarefa, refina com analise do repo e cria card no ClickUp automaticamente na lista Prod e Tech |
| `/break-pbi` | Le o arquivo PBI.md e quebra em tasks executaveis com checkpoints e progresso |
| `/do-pbi` | Executa a proxima task pendente do PBI (uma por vez) |

### Plugins e Hooks

| Comando | O que faz |
|---------|-----------|
| `/create-plugin` | Cria um plugin completo do zero com wizard guiado — estrutura, componentes e validacao |
| `/hookify` | Cria hooks para prevenir comportamentos indesejados a partir de instrucoes ou analise de conversa |
| `/hookify-configure` | Ativa ou desativa regras de hookify interativamente |
| `/hookify-list` | Lista todas as regras de hookify configuradas |
| `/hookify-help` | Mostra ajuda sobre o plugin hookify |

### Outros

| Comando | O que faz |
|---------|-----------|
| `/new-sdk-app` | Cria e configura uma nova aplicacao usando o Claude Agent SDK |

---

## Skills

Skills sao conhecimentos especializados que os agentes carregam para executar tarefas especificas.

| Skill | O que faz |
|-------|-----------|
| **Agent Development** | Guia para criar agentes — estrutura, system prompts, triggers e boas praticas |
| **Command Development** | Guia para criar slash commands — frontmatter, argumentos, interatividade |
| **Skill Development** | Guia para criar skills — estrutura, progressive disclosure, boas praticas |
| **Plugin Structure** | Guia de arquitetura de plugins — diretorio, manifest, componentes |
| **Plugin Settings** | Como armazenar configuracoes de plugin com arquivos `.local.md` e YAML frontmatter |
| **Hook Development** | Guia para criar hooks — PreToolUse, PostToolUse, Stop, automacao |
| **MCP Integration** | Como integrar servidores MCP (Model Context Protocol) em plugins |
| **Writing Hookify Rules** | Sintaxe e padroes para escrever regras do hookify |
| **frontend-design** | Cria interfaces frontend com alto nivel de design — componentes, paginas, aplicacoes |
| **explanatory-output-style** | Adiciona explicacoes educativas sobre o codebase enquanto trabalha |
| **learning-output-style** | Modo aprendizado interativo — incentiva o usuario a escrever codigo com explicacoes |
| **claude-opus-4-5-migration** | Migra prompts e codigo para Claude Opus 4.5 — atualiza model strings e ajusta prompts |
