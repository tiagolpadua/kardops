# KardOps

**KardOps** e uma metodologia para coordenar agentes de IA em tarefas de engenharia usando cards Kanban como unidade de trabalho.

Para regras operacionais detalhadas, consulte [instructions.md](instructions.md).

## 1. Objetivo

Padronizar a execucao de trabalho por agentes de IA com:

- rastreabilidade de ponta a ponta
- menor conflito entre execucoes paralelas
- qualidade minima verificavel antes de concluir cards
- clareza de ownership e handoff entre agentes

## 2. Estrutura Kanban

A pasta `kanban/` usa prefixo numerico para garantir ordenacao:

| Pasta                  | Finalidade                                                      |
| ---------------------- | --------------------------------------------------------------- |
| `0-backlog/backlog.md` | Ideias e demandas ainda nao comprometidas                       |
| `1-todo/`              | Cards prontos para execucao                                     |
| `2-doing/`             | Cards em execucao ativa                                         |
| `3-blocked/`           | Cards bloqueados por dependencia externa ou impedimento tecnico |
| `4-done/`              | Cards concluidos e validados                                    |
| `5-archive/`           | Cards concluidos arquivados (mais de 30 dias em `4-done/`)      |
| `9-templates/`         | Templates oficiais                                              |

Arquivos auxiliares:

- `instructions.md`: regras operacionais detalhadas (fonte de verdade)
- `prompts.md`: catalogo de prompts reutilizaveis para agentes

## 3. Unidade de Trabalho: Card

Cada card representa uma entrega tecnica pequena e verificavel. Campos obrigatorios e regras detalhadas estao em [instructions.md — secao 3](instructions.md#3-regras-obrigatórias-para-cards).

Campos opcionais:

- `tamanho`: `P`, `M` ou `G` — estimativa de esforco para apoiar decisoes de WIP e planejamento

## 4. Fluxo Operacional com Agentes

1. Product/Tech Lead registra no `0-backlog/backlog.md`.
2. Quebra em cards tecnicos e cria arquivos em `1-todo/` usando template oficial.
3. Orquestrador envia um prompt por card para o agente (referenciando o arquivo do card).
4. **[IMPORTANTE]** Agente move card para `2-doing/` **imediatamente** ao iniciar. Nunca trabalhar um card que esteja em `1-todo/`.
5. Agente implementa, atualiza progresso com timestamps e executa validacoes do card.
6. Se houver impedimento real, move para `3-blocked/` com motivo e acao de desbloqueio.
7. Ao desbloquear, move de `3-blocked/` para `2-doing/`.
8. Antes de finalizar, executar o script de validacao do projeto. Cada repositorio deve manter seu proprio script cobrindo no minimo: analise estatica e testes automatizados (ver [instructions.md — secao 13](instructions.md#13-script-de-validação)).
9. Apenas se a validacao passar e os criterios estiverem marcados, move para `4-done/`.

Transicoes de estado, checklists e anti-padroes: ver [instructions.md — secoes 10 a 11](instructions.md#10-transições-de-estado-permitidas).

## 5. Uso de `prompts.md` na Coordenacao

`prompts.md` funciona como inbox de comandos para agentes.

Boas praticas:

- cada prompt deve apontar para um card especifico
- evitar prompts amplos sem escopo
- registrar follow-up quando o agente abrir bloqueio
- manter linguagem imperativa e objetiva

## 6. Regras de Paralelismo

Para evitar colisao entre agentes:

- um card por agente por vez
- nao permitir mesmo `id` em duas colunas
- movimentacao e por `mv` (nunca copiar card entre colunas)
- manter WIP em `2-doing` com no maximo 3 cards simultaneos
- priorizar cards independentes em paralelo
- cards com dependencia devem explicitar precondicao no `Plano de Implementacao`

## 7. Handoff entre Agentes

Quando um agente termina parcial e outro continua:

- atualizar `Progresso` com o estado atual
- listar exatamente o que falta
- manter comandos de validacao reproduziveis
- so mover coluna quando o estado real mudar (todo/doing/blocked/done)

## 8. Controle de Sequencia de IDs

O campo `ultimo_id` no final de `0-backlog/backlog.md` registra o ultimo ID utilizado. Antes de criar um novo card, verificar este campo e usar o proximo sequencial. Apos criar, atualizar o campo.

Isso previne colisao de IDs quando multiplos agentes criam cards simultaneamente.

## 9. Politica de Arquivamento

Cards em `4-done/` ha mais de 30 dias podem ser movidos para `5-archive/`. O arquivamento:

- preserva o card integro (sem alteracoes)
- deve ser registrado no backlog quando todos os cards do item estiverem arquivados
- pode ser executado via prompt de auditoria (ver `prompts.md` — secao 3.3)

## 10. Governanca para Outros Times

Ritual recomendado:

1. Reuniao curta de planejamento: prioriza backlog -> todo.
2. Execucao assincrona por agentes via cards.
3. Revisao diaria de `2-doing` e `3-blocked`.
4. Encerramento: validar `4-done` com evidencias.

Indicadores basicos:

- lead time por card
- taxa de bloqueio
- retrabalho (cards reabertos)
- throughput semanal (`4-done`)

## 11. Referencias Internas

- Regras operacionais: [instructions.md](instructions.md)
- Template oficial: [kanban/9-templates/CARD-001-template.md](kanban/9-templates/CARD-001-template.md)
- Catalogo de prompts: [prompts.md](prompts.md)
