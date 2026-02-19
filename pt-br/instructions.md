# KardOps — Instrucoes Operacionais

Este documento define as regras operacionais da metodologia KardOps para gestao de tarefas via arquivos Markdown dentro de `kanban/`.

## 1. Estrutura

- `0-backlog/backlog.md`: fonte de verdade do que ainda precisa ser feito no projeto.
- `1-todo/`: cards prontos para iniciar.
- `2-doing/`: cards em andamento.
- `3-blocked/`: cards temporariamente bloqueados por dependencia externa, falha nao relacionada ou impedimento tecnico.
- `4-done/`: cards concluidos e validados.
- `5-archive/`: cards concluidos arquivados (mais de 30 dias em `4-done/`).
- `9-templates/`: modelos oficiais de card.

## 2. Fluxo de Trabalho

1. Registrar demandas no `0-backlog/backlog.md`.
2. Quebrar backlog em cards menores e independentes.
3. Criar um arquivo `.md` por card em `1-todo/` usando template oficial.
4. **[IMPORTANTE]** Ao iniciar execucao, **MOVER IMEDIATAMENTE** o card para `2-doing/`.
   - Atualizar o status no card para `status: doing`.
   - Nunca trabalhar um card que esteja em `1-todo/`.
5. Implementar a tarefa e atualizar o card com progresso objetivo (data + resultado).
6. Se houver impedimento real, mover para `3-blocked/` e registrar causa/bloqueio.
7. Ao desbloquear, mover de `3-blocked/` para `2-doing/`.
8. Antes de finalizar, executar o script de validacao do projeto. Cada repositorio deve manter seu proprio script de validacao na raiz, cobrindo no minimo: analise estatica e testes automatizados. O formato do script e livre (shell script, Makefile, npm script, etc.).
9. Apenas se a validacao passar e os criterios estiverem marcados, atualizar status para `status: done` e mover para `4-done/`.

**FLUXO OBRIGATORIO: `1-todo/` -> `2-doing/` -> `4-done/` (ou `3-blocked/` durante)**

**ANTI-PADRAO: NAO mover direto de `1-todo/` para `4-done/` sem passar por `2-doing/`**

## 3. Regras Obrigatorias para Cards

Todo card deve conter:

- `id`: identificador unico (ex.: `CARD-001`).
- `titulo`: objetivo curto e claro.
- `prioridade`: `P0`, `P1`, `P2` ou `P3`.
- `origem`: referencia rastreavel (backlog, analise tecnica, issue, ou outro documento do projeto).
- `descricao`: contexto funcional/tecnico.
- `escopo`: o que entra e o que nao entra.
- `plano de implementacao`: passos tecnicos em ordem.
- `criterios de aceite`: checklist verificavel.
- `validacao`: comandos/testes que comprovam conclusao.
- `status`: `todo`, `doing`, `blocked` ou `done`.
- `progresso`: log temporal curto e objetivo.

Campos opcionais:

- `tamanho`: `P`, `M` ou `G` — estimativa de esforco para apoiar decisoes de WIP e planejamento.

## 4. Regras de Consistencia

- **O fluxo correto e sempre: `1-todo/` -> `2-doing/` -> `4-done/` (ou `3-blocked/` durante a execucao).**
- **NUNCA mover diretamente de `1-todo/` para `4-done/`.**
- O mesmo `id` nao pode existir em mais de uma coluna ao mesmo tempo.
- Movimentacao e por `mv` (nunca copiar card entre colunas).
- Card em `2-doing` deve ter:
  - `status: doing`
  - progresso atualizado regularmente com timestamps
- Card em `4-done` deve ter:
  - `status: done`
  - criterios de aceite marcados
  - validacao obrigatoria marcada
  - data de conclusao no progresso
- Card em `3-blocked` deve explicitar:
  - `status: blocked`
  - motivo do bloqueio
  - acao necessaria para desbloquear
  - timestamp de quando foi bloqueado

## 5. Convencao de Nome de Arquivo

Padrao: `kanban/<coluna>/CARD-XXX-slug-curto.md`

Exemplo: `kanban/1-todo/CARD-012-ajustar-sync-offline.md`

## 6. Controle de Sequencia de IDs

O campo `ultimo_id` no final de `0-backlog/backlog.md` registra o ultimo ID utilizado. Antes de criar um novo card, verificar este campo e usar o proximo sequencial. Apos criar, atualizar o campo.

## 7. Template Oficial

Template oficial: `kanban/9-templates/CARD-001-template.md`

Processo recomendado:

1. Duplicar o template para `1-todo/` com novo nome.
2. Atualizar `id`, titulo, prioridade e origem.
3. Preencher descricao, escopo, plano, criterios e validacao.

## 8. Qualidade Minima por Card

- Plano de implementacao com passos executaveis (nao apenas decisao vaga).
- Criterios de aceite passiveis de verificacao objetiva.
- Validacao com comandos reais do projeto.
- Escopo com limites claros para evitar crescimento indefinido.

## 9. Politica de WIP e Bloqueios

- WIP recomendado em `2-doing`: maximo 3 cards simultaneos.
- Cards bloqueados nao devem ficar em `2-doing`.
- Bloqueio por falha nao relacionada deve ser registrado em `progresso` e no item de validacao.

## 10. Transicoes de Estado Permitidas

**Fluxo Normal (Caminho Feliz):**

```text
1-todo/ (status: todo)
  | [ao iniciar execucao]
2-doing/ (status: doing)
  | [apos completar, testar e validar]
4-done/ (status: done)
```

**Fluxo com Bloqueio:**

```text
1-todo/ (status: todo)
  | [ao iniciar execucao]
2-doing/ (status: doing)
  | [ao encontrar impedimento real]
3-blocked/ (status: blocked)
  | [ao desbloquear]
2-doing/ (status: doing)
  | [apos completar, testar e validar]
4-done/ (status: done)
```

**Transicoes NAO Permitidas:**

- `1-todo/` -> `4-done/` (pular `2-doing/`)
- `1-todo/` -> `3-blocked/` (bloquear sem iniciar)
- `4-done/` -> `2-doing/` (reabrir sem motivo explicito)
- Deixar card em `1-todo/` enquanto esta sendo trabalhado

## 11. Checklists de Transicao

### todo -> doing

Antes de mover:

- [ ] Li completamente o card, incluindo plano de implementacao
- [ ] Entendo todos os passos do plano
- [ ] Identifiquei os pre-requisitos e dependencias
- [ ] Tenho claro quais sao os criterios de aceite
- [ ] Identifiquei os comandos de validacao

Ao mover:

- [ ] Movi o arquivo de `1-todo/` para `2-doing/`
- [ ] Atualizei `status: doing` no card
- [ ] Adicionei timestamp de inicio na secao de progresso

### doing -> blocked

Ao mover:

- [ ] Movi o arquivo de `2-doing/` para `3-blocked/`
- [ ] Atualizei `status: blocked` no card
- [ ] Preenchi a secao `## Bloqueio` com motivo, impacto e acao para desbloquear
- [ ] Adicionei timestamp do bloqueio na secao de progresso

### blocked -> doing

Ao mover:

- [ ] Movi o arquivo de `3-blocked/` para `2-doing/`
- [ ] Atualizei `status: doing` no card
- [ ] Registrei a resolucao do bloqueio na secao de progresso com data

### doing -> done

Antes de mover:

- [ ] Implementei todos os passos do plano
- [ ] Executei validacoes locais com sucesso
- [ ] Marquei todos os criterios de aceite como [x]
- [ ] Executei todos os testes (lint, build, unit tests, etc)
- [ ] Adicionei timestamps de conclusao na secao de progresso
- [ ] Atualizei `status: done` no card

Ao mover:

- [ ] Movi o arquivo de `2-doing/` para `4-done/`
- [ ] Verifiquei que `status: done`
- [ ] Verifiquei que todos os criterios estao marcados [x]
- [ ] Verifiquei que todas as validacoes foram executadas [x]

## 12. Atualizacao do Backlog

- Ao criar cards a partir do backlog, manter rastreabilidade (`origem`).
- Quando todos os cards de um item estiverem em `4-done`, marcar item como concluido no `0-backlog/backlog.md` com data.
- Nao apagar historico de backlog; preferir marcacao de conclusao.

## 13. Script de Validacao

Cada repositorio que adota esta metodologia deve manter um script de validacao na raiz. O nome e formato do script sao livres (shell script, Makefile, npm script, etc.). O script deve cobrir, no minimo:

1. **Analise estatica / linting** — detectar erros de codigo e violacoes de estilo.
2. **Testes automatizados** — executar a suite de testes do projeto.

O script pode incluir etapas adicionais conforme a necessidade do projeto (cobertura, build, verificacao de tipos, etc.), desde que as duas etapas minimas estejam presentes.

Se o projeto ainda nao possui o script, cria-lo e pre-requisito antes de mover qualquer card para `4-done/`.

## 14. Politica de Arquivamento

Cards em `4-done/` ha mais de 30 dias podem ser movidos para `5-archive/`. O arquivamento:

- preserva o card integro (sem alteracoes)
- deve ser registrado no backlog quando todos os cards do item estiverem arquivados
- pode ser executado via prompt de auditoria (ver `prompts.md` — secao 3.3)

## 15. Definicao de Pronto (DoD)

Um card so esta pronto quando:

1. Implementacao concluida.
2. Criterios de aceite marcados.
3. Script de validacao do projeto executado com sucesso.
4. Card atualizado para `status: done`.
5. Card movido para `4-done/`.
