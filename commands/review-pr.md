---
description: "Comprehensive PR review using specialized agents"
argument-hint: "[review-aspects]"
allowed-tools: ["Bash", "Glob", "Grep", "Read", "Write", "Task", "AskUserQuestion"]
---

# PR Review — Consolidada e Pragmática

Review de pull request com agentes especializados. Todos os findings são consolidados por um agente final que publica **UM ÚNICO comentário** na PR, em português (pt-BR).

**Review Aspects (optional):** "$ARGUMENTS"

---

## PRINCÍPIOS FUNDAMENTAIS

1. **Analisar APENAS código novo/modificado** — nunca criticar código existente que não foi tocado nesta PR.
2. **Pragmatismo** — avaliar contra o nível atual do código do projeto, não contra perfeição teórica. Se o projeto já faz X de um jeito, não critique o dev por seguir o mesmo padrão.
3. **Só reportar o que importa** — bugs reais, falhas de segurança, erros de lógica, quebras de contrato. NÃO reportar: preferências de estilo, refatorações "nice to have", melhorias teóricas.
4. **Português correto** — revisar ortografia e gramática antes de publicar. Zero erros de escrita.
5. **Um único comentário final** — agentes NÃO publicam individualmente.

---

## Step 0: Determinar PR

1. Verificar se existe `pr.md` na raiz do projeto.
2. **Se existe**: extrair número/URL da PR.
3. **Se não existe**: perguntar ao usuário com AskUserQuestion e salvar em `pr.md`.
4. Validar com: `gh pr view <PR_NUMBER> --json number,title,url`

---

## Step 1: Coletar diff

```bash
# Arquivos alterados
gh pr diff <PR_NUMBER> --name-only

# Diff completo (apenas código novo)
gh pr diff <PR_NUMBER>
```

Salvar o diff em variável para passar aos agentes.

---

## Step 2: Lançar agentes de análise (em paralelo)

Lançar os agentes aplicáveis **em background**. Cada agente recebe o diff e retorna findings em formato estruturado.

### PROMPT OBRIGATÓRIO PARA TODOS OS AGENTES:

Cada agente DEVE receber estas instruções no prompt:

```
REGRAS OBRIGATÓRIAS:
1. Analise APENAS as linhas adicionadas/modificadas no diff abaixo. IGNORE código existente.
2. Seja pragmático: avalie contra o nível do código atual do projeto, não contra perfeição.
3. Só reporte problemas REAIS: bugs, falhas de segurança, erros de lógica, quebras de contrato.
4. NÃO reporte: preferências de estilo, refatorações opcionais, melhorias teóricas.
5. Para cada finding, retorne EXATAMENTE neste formato (sem nada extra):

FINDING:
- arquivo: <caminho/do/arquivo>
- linha: <número exato da linha no diff>
- severidade: critico | importante | sugestao
- problema: <1 frase curta descrevendo o problema>
- sugestao: <bloco de código corrigido>

Se não encontrar problemas reais, retorne: NENHUM_PROBLEMA

DIFF PARA ANALISAR:
<diff aqui>
```

### Agentes disponíveis:

- **code-reviewer** — Bugs, lógica incorreta, quebras de contrato. SEMPRE lançar.
- **silent-failure-hunter** — Erros engolidos, catch vazio, falhas silenciosas. Lançar se há try/catch no diff.
- **pr-test-analyzer** — Cobertura de testes. Lançar se há arquivos de teste no diff.
- **comment-analyzer** — Comentários incorretos/desatualizados. Lançar se há comentários novos no diff.
- **type-design-analyzer** — Types/interfaces incorretos. Lançar se há tipos novos no diff.

**NÃO lançar code-simplifier.** Simplificação não é escopo de review.

---

## Step 3: Consolidar findings (AGENTE FINAL)

Após TODOS os agentes completarem, coletar os resultados e passar para um agente consolidador.

O agente consolidador DEVE:

1. **Deduplicar** — remover findings repetidos entre agentes
2. **Validar linhas** — confirmar que cada finding aponta para uma linha real do diff
3. **Filtrar ruído** — remover findings que são preferência de estilo ou refatoração teórica
4. **Corrigir português** — garantir zero erros de ortografia e gramática
5. **Ordenar por severidade** — críticos primeiro, depois importantes, depois sugestões
6. **Formatar** o comentário final

---

## Step 4: Publicar UM ÚNICO comentário na PR

```bash
gh pr comment <PR_NUMBER> --body "$(cat <<'EOF'
## 🔍 Review de Código

### Resumo

| Severidade | Quantidade |
|------------|-----------|
| 🔴 Crítico | X |
| 🟡 Importante | X |
| 💡 Sugestão | X |

---

### 🔴 Problemas Críticos

📍 `arquivo.ts:42`
Descrição curta do problema (1 frase).
```suggestion
código corrigido aqui
```

---

### 🟡 Problemas Importantes

📍 `arquivo.ts:88`
Descrição curta (1 frase).
```suggestion
código corrigido aqui
```

---

### 💡 Sugestões

📍 `arquivo.ts:120`
Sugestão curta.
```suggestion
código corrigido aqui
```

---

✅ **Pontos positivos:** <1-2 coisas bem feitas nesta PR>

---
*Review automatizada — analisando apenas código novo desta PR*
EOF
)"
```

### REGRAS DO COMENTÁRIO FINAL:

- **Máximo 1 frase por finding.** O código sugerido é a entrega principal.
- **Sempre `arquivo:linha`** com 📍.
- **Sempre bloco ```suggestion```** com código corrigido. Sem sugestão de código = não é finding válido.
- **Se nenhum problema encontrado**, publicar:

```markdown
## ✅ Review de Código

Nenhum problema encontrado nesta PR. Código aprovado.

---
*Review automatizada — analisando apenas código novo desta PR*
```

- **Se há apenas sugestões** (nada crítico/importante), deixar claro que a PR está OK:

```markdown
## ✅ Review de Código — Aprovada com sugestões

Nenhum problema bloqueante. Algumas sugestões opcionais abaixo.

### 💡 Sugestões
...
```

---

## REGRAS GERAIS

1. **Todos os comentários em português (pt-BR) com ortografia correta.**
2. **Apenas UM comentário na PR** — o consolidado final. Agentes individuais NÃO publicam.
3. **Apenas código NOVO do diff** — nunca criticar código existente.
4. **Pragmatismo** — se o projeto já faz algo de um jeito, não critique o dev por seguir o padrão.
5. **Cada finding deve ter linha exata + código sugerido.** Sem isso, não publique.
6. **Máximo 15 findings** no comentário final. Se houver mais, priorizar por severidade e descartar os menores.
7. Usar `gh pr comment` com heredoc para evitar problemas de escaping.

---

## Uso

```
/review-pr              # Review completa (default)
/review-pr tests        # Apenas testes
/review-pr errors       # Apenas tratamento de erros
/review-pr code         # Apenas revisão geral
```
