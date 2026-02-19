# KardOps — Prompts

Catálogo de prompts reutilizáveis para operar o board KardOps via agentes de IA.
Substitua os placeholders `<...>` pelos valores reais antes de usar.

---

## 1. Implementação de Card

### 1.1 Implementar card do todo

```text
Considere kanban/instructions.md
Implemente o card kanban/1-todo/<CARD-XXX-slug>.md

Siga rigorosamente o plano de implementação descrito no card.
Ao concluir cada passo, atualize a seção "Progresso" do card com data e resultado.
Ao final, execute a validação descrita no card e marque os critérios de aceite atendidos.
Se todos os critérios passarem, atualize o status para `done` e mova o card para kanban/4-done/.
```

### 1.2 Continuar card em andamento

```text
Considere kanban/instructions.md
Continue a implementação do card kanban/2-doing/<CARD-XXX-slug>.md

Retome do último passo registrado na seção "Progresso".
Siga o plano de implementação e atualize o progresso a cada passo concluído.
Se encontrar impedimento, mova o card para kanban/3-blocked/ e registre o motivo.
Se finalizar, execute a validação, marque os critérios e mova para kanban/4-done/.
```

### 1.3 Desbloquear e retomar card bloqueado

```text
Considere kanban/instructions.md
O card kanban/3-blocked/<CARD-XXX-slug>.md foi desbloqueado.

Mova-o para kanban/2-doing/, registre a resolução do bloqueio em "Progresso" com data,
e retome a implementação a partir do último passo concluído.
```

---

## 2. Criação de Cards

### 2.1 Criar card a partir de item do backlog

```text
Considere kanban/instructions.md e kanban/9-templates/CARD-001-template.md

Crie um novo card em kanban/1-todo/ a partir do seguinte item do backlog:
- Origem: 0-backlog/backlog.md#<item-referência>
- Título: <título-curto>
- Prioridade: <P0|P1|P2|P3>

Preencha todas as seções obrigatórias: descrição, escopo (inclui/não inclui),
plano de implementação com passos executáveis, critérios de aceite verificáveis
e comandos de validação. Use o próximo ID sequencial disponível.
```

### 2.2 Quebrar item do backlog em múltiplos cards

```text
Considere kanban/instructions.md e kanban/9-templates/CARD-001-template.md

Analise o item "<descrição-do-item>" do backlog e quebre-o em cards menores
e independentes. Cada card deve:
- Ter escopo fechado e implementável isoladamente
- Seguir o template oficial completo
- Ser criado em kanban/1-todo/ com ID sequencial
- Referenciar a origem no backlog

Liste os cards criados ao final.
```

---

## 3. Limpeza e Reorganização do Board

### 3.1 Auditoria completa do board

```text
Considere kanban/instructions.md

Faça uma auditoria completa do board Kanban em kanban/:
1. Liste todos os cards em cada coluna (1-todo, 2-doing, 3-blocked, 4-done).
2. Verifique se cada card possui todos os campos obrigatórios (id, título, prioridade, origem, descrição, escopo, plano, critérios, validação, status, progresso).
3. Verifique se o status do card é consistente com a coluna em que se encontra.
4. Verifique se não há IDs duplicados entre colunas.
5. Verifique se o WIP em 2-doing respeita o limite de 3 cards.
6. Verifique se cards em 3-blocked tem motivo e ação de desbloqueio registrados.
7. Verifique se cards em 4-done tem critérios marcados e validação executada.

Apresente um relatório com problemas encontrados e sugestões de correção.
```

### 3.2 Corrigir inconsistências do board

```text
Considere kanban/instructions.md

A partir da auditoria do board Kanban em kanban/:
1. Corrija cards cujo campo `status` não corresponde a coluna onde estão.
2. Mova cards que estão na coluna errada para a coluna correta conforme seu status.
3. Preencha campos obrigatórios ausentes com valores placeholder marcados como [PENDENTE].
4. Remova duplicatas de ID, mantendo a versão mais recente (pelo progresso).
5. Registre todas as correções feitas na seção "Progresso" de cada card afetado.
```

### 3.3 Limpar cards concluídos antigos

```text
Considere kanban/instructions.md

Analise os cards em kanban/4-done/:
1. Liste todos os cards concluídos com suas datas de conclusão.
2. Identifique cards concluídos há mais de <N> dias.
3. Verifique se os itens correspondentes no backlog estão marcados como concluídos.
4. Sugira quais cards podem ser arquivados (movidos para uma pasta kanban/5-archive/).

Não execute o arquivamento automaticamente — apenas liste e aguarde confirmação.
```

### 3.4 Reorganizar backlog e sincronizar com cards

```text
Considere kanban/instructions.md

Sincronize o backlog (kanban/0-backlog/backlog.md) com o estado atual do board:
1. Identifique itens do backlog que já possuem cards criados (em qualquer coluna).
2. Marque no backlog os itens cujos cards já estão em 4-done com data de conclusão.
3. Identifique itens do backlog sem cards associados e liste-os.
4. Identifique cards órfãos (sem referência válida de origem no backlog) e liste-os.
5. Sugira reordenação do backlog por prioridade.
```

---

## 4. Gestão do Board

### 4.1 Status geral do board (daily/standup)

```text
Considere kanban/instructions.md

Gere um resumo de status do board Kanban para standup:
- Quantos cards em cada coluna (todo, doing, blocked, done).
- Cards em andamento (2-doing): título, último progresso registrado.
- Cards bloqueados (3-blocked): título, motivo do bloqueio.
- Cards prontos para iniciar (1-todo): título e prioridade (ordenados por P0->P3).
- Alertas: WIP excedido, cards parados sem progresso recente.
```

### 4.2 Priorizar próximo card para execução

```text
Considere kanban/instructions.md

Analise os cards em kanban/1-todo/ e recomende o próximo card a ser iniciado,
considerando:
- Prioridade (P0 > P1 > P2 > P3)
- Limite de WIP em 2-doing (máximo 3)
- Dependências ou bloqueios de outros cards
- Complexidade estimada pelo plano de implementação

Justifique a recomendação.
```

### 4.3 Mover card entre colunas

```text
Considere kanban/instructions.md

Mova o card <CARD-XXX-slug>.md de kanban/<coluna-origem>/ para kanban/<coluna-destino>/.
Atualize o campo `status` do card para refletir a nova coluna.
Registre a movimentação na seção "Progresso" com data.
Valide as regras de consistência (ex.: card indo para done precisa ter critérios marcados).
```

---

## 5. Manutenção e Qualidade

### 5.1 Validar qualidade de um card específico

```text
Considere kanban/instructions.md

Avalie a qualidade do card kanban/<coluna>/<CARD-XXX-slug>.md:
- Todos os campos obrigatórios estão preenchidos?
- O plano de implementação tem passos executáveis e ordenados?
- Os critérios de aceite são verificáveis objetivamente?
- A validação inclui comandos reais do projeto?
- O escopo tem limites claros (inclui/não inclui)?

Sugira melhorias concretas se necessário.
```

### 5.2 Padronizar cards existentes conforme template

```text
Considere kanban/instructions.md e kanban/9-templates/CARD-001-template.md

Revise todos os cards existentes no board (todas as colunas) e:
1. Identifique cards que não seguem a estrutura do template oficial.
2. Para cada card fora do padrão, ajuste a estrutura para ficar conforme o template.
3. Preserve o conteúdo original — apenas reorganize e adicione seções ausentes.
4. Registre o ajuste na seção "Progresso" de cada card modificado.
```

### 5.3 Atualizar template oficial

```text
Considere kanban/instructions.md

Revise o template kanban/9-templates/CARD-001-template.md para garantir que:
- Reflete todas as regras e campos obrigatórios de kanban/instructions.md.
- Contém exemplos úteis em cada seção para facilitar o preenchimento.
- Está alinhado com as convenções atuais do projeto.

Faça os ajustes necessários no template.
```

---

## 6. Relatórios e Métricas

### 6.1 Gerar relatório de throughput

```text
Considere kanban/instructions.md

Analise os cards em kanban/4-done/ e gere um relatório de throughput:
- Total de cards concluídos.
- Cards concluídos por período (semanal, se possível inferir pelas datas de progresso).
- Tempo médio de ciclo (da criação/todo até done, baseado nas datas de progresso).
- Distribuição por prioridade (quantos P0, P1, P2, P3 concluídos).
```

### 6.2 Gerar relatório de bloqueios

```text
Considere kanban/instructions.md

Analise o histórico de cards em kanban/3-blocked/ e kanban/4-done/ (que passaram por bloqueio):
- Quantos cards foram bloqueados.
- Motivos de bloqueio mais frequentes.
- Tempo médio de permanência em blocked (se inferível pelo progresso).
- Sugestões para reduzir bloqueios futuros.
```
