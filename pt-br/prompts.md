# KardOps — Prompts

Catalogo de prompts reutilizaveis para operar o board KardOps via agentes de IA.
Substitua os placeholders `<...>` pelos valores reais antes de usar.

---

## 1. Implementacao de Card

### 1.1 Implementar card do todo

```text
Considere kanban/instructions.md
Implemente o card kanban/1-todo/<CARD-XXX-slug>.md

Siga rigorosamente o plano de implementacao descrito no card.
Ao concluir cada passo, atualize a secao "Progresso" do card com data e resultado.
Ao final, execute a validacao descrita no card e marque os criterios de aceite atendidos.
Se todos os criterios passarem, atualize o status para `done` e mova o card para kanban/4-done/.
```

### 1.2 Continuar card em andamento

```text
Considere kanban/instructions.md
Continue a implementacao do card kanban/2-doing/<CARD-XXX-slug>.md

Retome do ultimo passo registrado na secao "Progresso".
Siga o plano de implementacao e atualize o progresso a cada passo concluido.
Se encontrar impedimento, mova o card para kanban/3-blocked/ e registre o motivo.
Se finalizar, execute a validacao, marque os criterios e mova para kanban/4-done/.
```

### 1.3 Desbloquear e retomar card bloqueado

```text
Considere kanban/instructions.md
O card kanban/3-blocked/<CARD-XXX-slug>.md foi desbloqueado.

Mova-o para kanban/2-doing/, registre a resolucao do bloqueio em "Progresso" com data,
e retome a implementacao a partir do ultimo passo concluido.
```

---

## 2. Criacao de Cards

### 2.1 Criar card a partir de item do backlog

```text
Considere kanban/instructions.md e kanban/9-templates/CARD-001-template.md

Crie um novo card em kanban/1-todo/ a partir do seguinte item do backlog:
- Origem: 0-backlog/backlog.md#<item-referencia>
- Titulo: <titulo-curto>
- Prioridade: <P0|P1|P2|P3>

Preencha todas as secoes obrigatorias: descricao, escopo (inclui/nao inclui),
plano de implementacao com passos executaveis, criterios de aceite verificaveis
e comandos de validacao. Use o proximo ID sequencial disponivel.
```

### 2.2 Quebrar item do backlog em multiplos cards

```text
Considere kanban/instructions.md e kanban/9-templates/CARD-001-template.md

Analise o item "<descricao-do-item>" do backlog e quebre-o em cards menores
e independentes. Cada card deve:
- Ter escopo fechado e implementavel isoladamente
- Seguir o template oficial completo
- Ser criado em kanban/1-todo/ com ID sequencial
- Referenciar a origem no backlog

Liste os cards criados ao final.
```

---

## 3. Limpeza e Reorganizacao do Board

### 3.1 Auditoria completa do board

```text
Considere kanban/instructions.md

Faca uma auditoria completa do board Kanban em kanban/:
1. Liste todos os cards em cada coluna (1-todo, 2-doing, 3-blocked, 4-done).
2. Verifique se cada card possui todos os campos obrigatorios (id, titulo, prioridade, origem, descricao, escopo, plano, criterios, validacao, status, progresso).
3. Verifique se o status do card e consistente com a coluna em que se encontra.
4. Verifique se nao ha IDs duplicados entre colunas.
5. Verifique se o WIP em 2-doing respeita o limite de 3 cards.
6. Verifique se cards em 3-blocked tem motivo e acao de desbloqueio registrados.
7. Verifique se cards em 4-done tem criterios marcados e validacao executada.

Apresente um relatorio com problemas encontrados e sugestoes de correcao.
```

### 3.2 Corrigir inconsistencias do board

```text
Considere kanban/instructions.md

A partir da auditoria do board Kanban em kanban/:
1. Corrija cards cujo campo `status` nao corresponde a coluna onde estao.
2. Mova cards que estao na coluna errada para a coluna correta conforme seu status.
3. Preencha campos obrigatorios ausentes com valores placeholder marcados como [PENDENTE].
4. Remova duplicatas de ID, mantendo a versao mais recente (pelo progresso).
5. Registre todas as correcoes feitas na secao "Progresso" de cada card afetado.
```

### 3.3 Limpar cards concluidos antigos

```text
Considere kanban/instructions.md

Analise os cards em kanban/4-done/:
1. Liste todos os cards concluidos com suas datas de conclusao.
2. Identifique cards concluidos ha mais de <N> dias.
3. Verifique se os itens correspondentes no backlog estao marcados como concluidos.
4. Sugira quais cards podem ser arquivados (movidos para uma pasta kanban/5-archive/).

Nao execute o arquivamento automaticamente — apenas liste e aguarde confirmacao.
```

### 3.4 Reorganizar backlog e sincronizar com cards

```text
Considere kanban/instructions.md

Sincronize o backlog (kanban/0-backlog/backlog.md) com o estado atual do board:
1. Identifique itens do backlog que ja possuem cards criados (em qualquer coluna).
2. Marque no backlog os itens cujos cards ja estao em 4-done com data de conclusao.
3. Identifique itens do backlog sem cards associados e liste-os.
4. Identifique cards orfaos (sem referencia valida de origem no backlog) e liste-os.
5. Sugira reordenacao do backlog por prioridade.
```

---

## 4. Gestao do Board

### 4.1 Status geral do board (daily/standup)

```text
Considere kanban/instructions.md

Gere um resumo de status do board Kanban para standup:
- Quantos cards em cada coluna (todo, doing, blocked, done).
- Cards em andamento (2-doing): titulo, ultimo progresso registrado.
- Cards bloqueados (3-blocked): titulo, motivo do bloqueio.
- Cards prontos para iniciar (1-todo): titulo e prioridade (ordenados por P0->P3).
- Alertas: WIP excedido, cards parados sem progresso recente.
```

### 4.2 Priorizar proximo card para execucao

```text
Considere kanban/instructions.md

Analise os cards em kanban/1-todo/ e recomende o proximo card a ser iniciado,
considerando:
- Prioridade (P0 > P1 > P2 > P3)
- Limite de WIP em 2-doing (maximo 3)
- Dependencias ou bloqueios de outros cards
- Complexidade estimada pelo plano de implementacao

Justifique a recomendacao.
```

### 4.3 Mover card entre colunas

```text
Considere kanban/instructions.md

Mova o card <CARD-XXX-slug>.md de kanban/<coluna-origem>/ para kanban/<coluna-destino>/.
Atualize o campo `status` do card para refletir a nova coluna.
Registre a movimentacao na secao "Progresso" com data.
Valide as regras de consistencia (ex.: card indo para done precisa ter criterios marcados).
```

---

## 5. Manutencao e Qualidade

### 5.1 Validar qualidade de um card especifico

```text
Considere kanban/instructions.md

Avalie a qualidade do card kanban/<coluna>/<CARD-XXX-slug>.md:
- Todos os campos obrigatorios estao preenchidos?
- O plano de implementacao tem passos executaveis e ordenados?
- Os criterios de aceite sao verificaveis objetivamente?
- A validacao inclui comandos reais do projeto?
- O escopo tem limites claros (inclui/nao inclui)?

Sugira melhorias concretas se necessario.
```

### 5.2 Padronizar cards existentes conforme template

```text
Considere kanban/instructions.md e kanban/9-templates/CARD-001-template.md

Revise todos os cards existentes no board (todas as colunas) e:
1. Identifique cards que nao seguem a estrutura do template oficial.
2. Para cada card fora do padrao, ajuste a estrutura para ficar conforme o template.
3. Preserve o conteudo original — apenas reorganize e adicione secoes ausentes.
4. Registre o ajuste na secao "Progresso" de cada card modificado.
```

### 5.3 Atualizar template oficial

```text
Considere kanban/instructions.md

Revise o template kanban/9-templates/CARD-001-template.md para garantir que:
- Reflete todas as regras e campos obrigatorios de kanban/instructions.md.
- Contem exemplos uteis em cada secao para facilitar o preenchimento.
- Esta alinhado com as convencoes atuais do projeto.

Faca os ajustes necessarios no template.
```

---

## 6. Relatorios e Metricas

### 6.1 Gerar relatorio de throughput

```text
Considere kanban/instructions.md

Analise os cards em kanban/4-done/ e gere um relatorio de throughput:
- Total de cards concluidos.
- Cards concluidos por periodo (semanal, se possivel inferir pelas datas de progresso).
- Tempo medio de ciclo (da criacao/todo ate done, baseado nas datas de progresso).
- Distribuicao por prioridade (quantos P0, P1, P2, P3 concluidos).
```

### 6.2 Gerar relatorio de bloqueios

```text
Considere kanban/instructions.md

Analise o historico de cards em kanban/3-blocked/ e kanban/4-done/ (que passaram por bloqueio):
- Quantos cards foram bloqueados.
- Motivos de bloqueio mais frequentes.
- Tempo medio de permanencia em blocked (se inferivel pelo progresso).
- Sugestoes para reduzir bloqueios futuros.
```
