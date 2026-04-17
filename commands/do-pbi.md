---
description: Executa a próxima task pendente do PBI (uma por vez, manual)
---

Você é um agente executor de tasks de PBI. Seu trabalho é executar **UMA única task por vez** e parar.

---

## PASSO 1 — Ler progresso

Leia `.ignore/progresso.md` e identifique:
1. A seção "Tasks" — encontre a primeira task com status **pendente**
2. A seção "O Que Já Foi Feito" — entenda o contexto atual
3. A seção "Problemas Conhecidos" — evite repetir erros

Se **todas as tasks estão concluídas**, informe o usuário que o PBI está completo e pare.

---

## PASSO 2 — Ler a task

Leia o arquivo da task identificada em `.ignore/TASK-XX-*.md`.

Leia **TODOS** os arquivos listados em "Padrões de Referência" da task — você DEVE seguir os padrões encontrados.

Se a task tem dependências, verifique se as tasks anteriores estão marcadas como concluídas no progresso.md. Se não estiverem, informe o usuário e pare.

---

## PASSO 3 — Executar a task

Implemente **exatamente** o que a task descreve:
- Siga os padrões de referência (abra os arquivos indicados e replique o estilo)
- Crie/modifique apenas os arquivos listados na task
- Respeite a seção "NÃO Fazer"
- Valide cada critério de aceite

---

## PASSO 4 — Validar

Execute os comandos da seção "Validação" da task (se houver).
Verifique cada item dos "Critérios de Aceite" — marque como feito apenas se realmente passou.

Se algo falhar, corrija antes de prosseguir. Se não conseguir corrigir após 3 tentativas, registre em "Problemas Conhecidos" e informe o usuário.

---

## PASSO 5 — Atualizar progresso.md

Após concluir com sucesso:

1. Na tabela "Tasks": mude o status da task para **concluída** e atualize aceite (ex: `3/3`)
2. Em "O Que Já Foi Feito": adicione resumo do que foi implementado (nomes de arquivos, classes, endpoints criados)
3. Em "Próxima Task": atualize para a próxima task pendente (ou "Todas concluídas" se for a última)
4. Se houve problemas, registre em "Problemas Conhecidos"

---

## PASSO 6 — Parar e reportar

Apresente um resumo curto:
- ✅ Task executada: TASK-XX — [Título]
- Arquivos criados/modificados
- Critérios de aceite: X/Y passaram
- Próxima task: TASK-YY — [Título] (rode `/do-pbi` novamente quando quiser)

**PARE AQUI. NÃO execute a próxima task. Espere o usuário rodar `/do-pbi` novamente.**
