# KardOps — Instruções Operacionais

Este documento define as regras operacionais da metodologia KardOps para gestão de tarefas via arquivos Markdown dentro de `kanban/`.

## 1. Estrutura

- `0-backlog/backlog.md`: fonte de verdade do que ainda precisa ser feito no projeto.
- `1-todo/`: cards prontos para iniciar.
- `2-doing/`: cards em andamento.
- `3-blocked/`: cards temporariamente bloqueados por dependência externa, falha não relacionada ou impedimento técnico.
- `4-done/`: cards concluídos e validados.
- `5-archive/`: cards concluídos arquivados (mais de 30 dias em `4-done/`).
- `9-templates/`: modelos oficiais de card.

## 2. Fluxo de Trabalho

1. Registrar demandas no `0-backlog/backlog.md`.
2. Quebrar backlog em cards menores e independentes.
3. Criar um arquivo `.md` por card em `1-todo/` usando template oficial.
4. **[IMPORTANTE]** Ao iniciar execução, **MOVER IMEDIATAMENTE** o card para `2-doing/`.
   - Atualizar o status no card para `status: doing`.
   - Nunca trabalhar um card que esteja em `1-todo/`.
5. Implementar a tarefa e atualizar o card com progresso objetivo (data + resultado).
6. Se houver impedimento real, mover para `3-blocked/` e registrar causa/bloqueio.
7. Ao desbloquear, mover de `3-blocked/` para `2-doing/`.
8. Antes de finalizar, executar o script de validação do projeto. Cada repositório deve manter seu próprio script de validação na raiz, cobrindo no mínimo: análise estática e testes automatizados. O formato do script é livre (shell script, Makefile, npm script, etc.).
9. Apenas se a validação passar e os critérios estiverem marcados, atualizar status para `status: done` e mover para `4-done/`.

**FLUXO OBRIGATÓRIO: `1-todo/` -> `2-doing/` -> `4-done/` (ou `3-blocked/` durante)**

**ANTI-PADRÃO: NÃO mover direto de `1-todo/` para `4-done/` sem passar por `2-doing/`**

## 3. Regras Obrigatórias para Cards

Todo card deve conter:

- `id`: identificador único (ex.: `CARD-001`).
- `título`: objetivo curto e claro.
- `prioridade`: `P0`, `P1`, `P2` ou `P3`.
- `origem`: referência rastreável (backlog, análise técnica, issue, ou outro documento do projeto).
- `descrição`: contexto funcional/técnico.
- `escopo`: o que entra e o que não entra.
- `plano de implementacao`: passos técnicos em ordem.
- `critérios de aceite`: checklist verificável.
- `validação`: comandos/testes que comprovam conclusão.
- `status`: `todo`, `doing`, `blocked` ou `done`.
- `progresso`: log temporal curto e objetivo.

Campos opcionais:

- `tamanho`: `P`, `M` ou `G` — estimativa de esforço para apoiar decisões de WIP e planejamento.

## 4. Regras de Consistência

- **O fluxo correto é sempre: `1-todo/` -> `2-doing/` -> `4-done/` (ou `3-blocked/` durante a execução).**
- **NUNCA mover diretamente de `1-todo/` para `4-done/`.**
- O mesmo `id` não pode existir em mais de uma coluna ao mesmo tempo.
- Movimentação é por `mv` (nunca copiar card entre colunas).
- Card em `2-doing` deve ter:
  - `status: doing`
  - progresso atualizado regularmente com timestamps
- Card em `4-done` deve ter:
  - `status: done`
  - critérios de aceite marcados
  - validação obrigatória marcada
  - data de conclusão no progresso
- Card em `3-blocked` deve explicitar:
  - `status: blocked`
  - motivo do bloqueio
  - ação necessária para desbloquear
  - timestamp de quando foi bloqueado

## 5. Convenção de Nome de Arquivo

Padrão: `kanban/<coluna>/CARD-XXX-slug-curto.md`

Exemplo: `kanban/1-todo/CARD-012-ajustar-sync-offline.md`

## 6. Controle de Sequência de IDs

O campo `ultimo_id` no final de `0-backlog/backlog.md` registra o último ID utilizado. Antes de criar um novo card, verificar este campo e usar o próximo sequencial. Após criar, atualizar o campo.

## 7. Template Oficial

Template oficial: `kanban/9-templates/CARD-001-template.md`

Processo recomendado:

1. Duplicar o template para `1-todo/` com novo nome.
2. Atualizar `id`, título, prioridade e origem.
3. Preencher descrição, escopo, plano, critérios e validação.

## 8. Qualidade Mínima por Card

- Plano de implementacao com passos executáveis (não apenas decisão vaga).
- Critérios de aceite passíveis de verificação objetiva.
- Validação com comandos reais do projeto.
- Escopo com limites claros para evitar crescimento indefinido.

## 9. Política de WIP e Bloqueios

- WIP recomendado em `2-doing`: máximo 3 cards simultâneos.
- Cards bloqueados não devem ficar em `2-doing`.
- Bloqueio por falha não relacionada deve ser registrado em `progresso` e no item de validação.

## 10. Transições de Estado Permitidas

**Fluxo Normal (Caminho Feliz):**

```text
1-todo/ (status: todo)
  | [ao iniciar execução]
2-doing/ (status: doing)
  | [após completar, testar e validar]
4-done/ (status: done)
```

**Fluxo com Bloqueio:**

```text
1-todo/ (status: todo)
  | [ao iniciar execução]
2-doing/ (status: doing)
  | [ao encontrar impedimento real]
3-blocked/ (status: blocked)
  | [ao desbloquear]
2-doing/ (status: doing)
  | [após completar, testar e validar]
4-done/ (status: done)
```

**Transições NÃO Permitidas:**

- `1-todo/` -> `4-done/` (pular `2-doing/`)
- `1-todo/` -> `3-blocked/` (bloquear sem iniciar)
- `4-done/` -> `2-doing/` (reabrir sem motivo explícito)
- Deixar card em `1-todo/` enquanto está sendo trabalhado

## 11. Checklists de Transição

### todo -> doing

Antes de mover:

- [ ] Li completamente o card, incluindo plano de implementacao
- [ ] Entendo todos os passos do plano
- [ ] Identifiquei os pré-requisitos e dependências
- [ ] Tenho claro quais são os critérios de aceite
- [ ] Identifiquei os comandos de validação

Ao mover:

- [ ] Movi o arquivo de `1-todo/` para `2-doing/`
- [ ] Atualizei `status: doing` no card
- [ ] Adicionei timestamp de início na seção de progresso

### doing -> blocked

Ao mover:

- [ ] Movi o arquivo de `2-doing/` para `3-blocked/`
- [ ] Atualizei `status: blocked` no card
- [ ] Preenchi a seção `## Bloqueio` com motivo, impacto e ação para desbloquear
- [ ] Adicionei timestamp do bloqueio na seção de progresso

### blocked -> doing

Ao mover:

- [ ] Movi o arquivo de `3-blocked/` para `2-doing/`
- [ ] Atualizei `status: doing` no card
- [ ] Registrei a resolução do bloqueio na seção de progresso com data

### doing -> done

Antes de mover:

- [ ] Implementei todos os passos do plano
- [ ] Executei validações locais com sucesso
- [ ] Marquei todos os critérios de aceite como [x]
- [ ] Executei todos os testes (lint, build, unit tests, etc)
- [ ] Adicionei timestamps de conclusão na seção de progresso
- [ ] Atualizei `status: done` no card

Ao mover:

- [ ] Movi o arquivo de `2-doing/` para `4-done/`
- [ ] Verifiquei que `status: done`
- [ ] Verifiquei que todos os critérios estão marcados [x]
- [ ] Verifiquei que todas as validações foram executadas [x]

## 12. Atualização do Backlog

- Ao criar cards a partir do backlog, manter rastreabilidade (`origem`).
- Quando todos os cards de um item estiverem em `4-done`, marcar item como concluído no `0-backlog/backlog.md` com data.
- Não apagar histórico de backlog; preferir marcação de conclusão.

## 13. Script de Validação

Cada repositório que adota esta metodologia deve manter um script de validação na raiz. O nome e formato do script são livres (shell script, Makefile, npm script, etc.). O script deve cobrir, no mínimo:

1. **Análise estática / linting** — detectar erros de código e violações de estilo.
2. **Testes automatizados** — executar a suite de testes do projeto.

O script pode incluir etapas adicionais conforme a necessidade do projeto (cobertura, build, verificação de tipos, etc.), desde que as duas etapas mínimas estejam presentes.

Se o projeto ainda não possui o script, criá-lo é pré-requisito antes de mover qualquer card para `4-done/`.

## 14. Política de Arquivamento

Cards em `4-done/` há mais de 30 dias podem ser movidos para `5-archive/`. O arquivamento:

- preserva o card íntegro (sem alterações)
- deve ser registrado no backlog quando todos os cards do item estiverem arquivados
- pode ser executado via prompt de auditoria (ver `prompts.md` — seção 3.3)

## 15. Definição de Pronto (DoD)

Um card só está pronto quando:

1. Implementação concluída.
2. Critérios de aceite marcados.
3. Script de validação do projeto executado com sucesso.
4. Card atualizado para `status: done`.
5. Card movido para `4-done/`.
