# KardOps

**KardOps** é uma metodologia para coordenar agentes de IA em tarefas de engenharia usando cards Kanban como unidade de trabalho.

Para regras operacionais detalhadas, consulte [instructions.md](instructions.md).

## 1. Objetivo

Padronizar a execução de trabalho por agentes de IA com:

- rastreabilidade de ponta a ponta
- menor conflito entre execuções paralelas
- qualidade mínima verificável antes de concluir cards
- clareza de ownership e handoff entre agentes

## 2. Estrutura Kanban

A pasta `kanban/` usa prefixo numérico para garantir ordenação:

| Pasta                  | Finalidade                                                      |
| ---------------------- | --------------------------------------------------------------- |
| `0-backlog/backlog.md` | Ideias e demandas ainda não comprometidas                       |
| `1-todo/`              | Cards prontos para execução                                     |
| `2-doing/`             | Cards em execução ativa                                         |
| `3-blocked/`           | Cards bloqueados por dependência externa ou impedimento técnico |
| `4-done/`              | Cards concluídos e validados                                    |
| `5-archive/`           | Cards concluídos arquivados (mais de 30 dias em `4-done/`)      |
| `9-templates/`         | Templates oficiais                                              |

Arquivos auxiliares:

- `instructions.md`: regras operacionais detalhadas (fonte de verdade)
- `prompts.md`: catálogo de prompts reutilizáveis para agentes

## 3. Unidade de Trabalho: Card

Cada card representa uma entrega técnica pequena e verificável. Campos obrigatórios e regras detalhadas estão em [instructions.md — seção 3](instructions.md#3-regras-obrigatórias-para-cards).

Campos opcionais:

- `tamanho`: `P`, `M` ou `G` — estimativa de esforço para apoiar decisões de WIP e planejamento

## 4. Fluxo Operacional com Agentes

1. Product/Tech Lead registra no `0-backlog/backlog.md`.
2. Quebra em cards técnicos e cria arquivos em `1-todo/` usando template oficial.
3. Orquestrador envia um prompt por card para o agente (referenciando o arquivo do card).
4. **[IMPORTANTE]** Agente move card para `2-doing/` **imediatamente** ao iniciar. Nunca trabalhar um card que esteja em `1-todo/`.
5. Agente implementa, atualiza progresso com timestamps e executa validações do card.
6. Se houver impedimento real, move para `3-blocked/` com motivo e ação de desbloqueio.
7. Ao desbloquear, move de `3-blocked/` para `2-doing/`.
8. Antes de finalizar, executar o script de validação do projeto. Cada repositório deve manter seu próprio script cobrindo no mínimo: análise estática e testes automatizados (ver [instructions.md — seção 13](instructions.md#13-script-de-validação)).
9. Apenas se a validação passar e os critérios estiverem marcados, move para `4-done/`.

Transições de estado, checklists e anti-padrões: ver [instructions.md — seções 10 a 11](instructions.md#10-transições-de-estado-permitidas).

## 5. Uso de `prompts.md` na Coordenação

`prompts.md` funciona como inbox de comandos para agentes.

Boas práticas:

- cada prompt deve apontar para um card específico
- evitar prompts amplos sem escopo
- registrar follow-up quando o agente abrir bloqueio
- manter linguagem imperativa e objetiva

## 6. Regras de Paralelismo

Para evitar colisão entre agentes:

- um card por agente por vez
- não permitir mesmo `id` em duas colunas
- movimentação é por `mv` (nunca copiar card entre colunas)
- manter WIP em `2-doing` com no máximo 3 cards simultâneos
- priorizar cards independentes em paralelo
- cards com dependência devem explicitar precondição no `Plano de Implementação`

## 7. Handoff entre Agentes

Quando um agente termina parcial e outro continua:

- atualizar `Progresso` com o estado atual
- listar exatamente o que falta
- manter comandos de validação reproduzíveis
- só mover coluna quando o estado real mudar (todo/doing/blocked/done)

## 8. Controle de Sequência de IDs

O campo `ultimo_id` no final de `0-backlog/backlog.md` registra o último ID utilizado. Antes de criar um novo card, verificar este campo e usar o próximo sequencial. Após criar, atualizar o campo.

Isso previne colisão de IDs quando múltiplos agentes criam cards simultaneamente.

## 9. Política de Arquivamento

Cards em `4-done/` há mais de 30 dias podem ser movidos para `5-archive/`. O arquivamento:

- preserva o card íntegro (sem alterações)
- deve ser registrado no backlog quando todos os cards do item estiverem arquivados
- pode ser executado via prompt de auditoria (ver `prompts.md` — seção 3.3)

## 10. Governança para Outros Times

Ritual recomendado:

1. Reunião curta de planejamento: prioriza backlog -> todo.
2. Execução assíncrona por agentes via cards.
3. Revisão diária de `2-doing` e `3-blocked`.
4. Encerramento: validar `4-done` com evidências.

Indicadores básicos:

- lead time por card
- taxa de bloqueio
- retrabalho (cards reabertos)
- throughput semanal (`4-done`)

## 11. Referências Internas

- Regras operacionais: [instructions.md](instructions.md)
- Template oficial: [kanban/9-templates/CARD-001-template.md](kanban/9-templates/CARD-001-template.md)
- Catálogo de prompts: [prompts.md](prompts.md)
