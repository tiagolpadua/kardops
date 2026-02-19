# KardOps

**KardOps** es una metodologia para coordinar agentes de IA en tareas de ingenieria usando cards Kanban como unidad de trabajo.

Para reglas operacionales detalladas, consulte [instructions.md](instructions.md).

## 1. Objetivo

Estandarizar la ejecucion de trabajo por agentes de IA con:

- trazabilidad de punta a punta
- menor conflicto entre ejecuciones paralelas
- calidad minima verificable antes de concluir cards
- claridad de ownership y handoff entre agentes

## 2. Estructura Kanban

La carpeta `kanban/` usa prefijo numerico para garantizar el ordenamiento:

| Carpeta                | Finalidad                                                      |
| ---------------------- | -------------------------------------------------------------- |
| `0-backlog/backlog.md` | Ideas y demandas aun no comprometidas                          |
| `1-todo/`              | Cards listos para ejecucion                                    |
| `2-doing/`             | Cards en ejecucion activa                                      |
| `3-blocked/`           | Cards bloqueados por dependencia externa o impedimento tecnico |
| `4-done/`              | Cards concluidos y validados                                   |
| `5-archive/`           | Cards concluidos archivados (mas de 30 dias en `4-done/`)      |
| `9-templates/`         | Templates oficiales                                            |

Archivos auxiliares:

- `instructions.md`: reglas operacionales detalladas (fuente de verdad)
- `prompts.md`: catalogo de prompts reutilizables para agentes

## 3. Unidad de Trabajo: Card

Cada card representa una entrega tecnica pequena y verificable. Campos obligatorios y reglas detalladas estan en [instructions.md — seccion 3](instructions.md#3-reglas-obligatorias-para-cards).

Campos opcionales:

- `tamaño`: `P`, `M` o `G` — estimacion de esfuerzo para apoyar decisiones de WIP y planificacion

## 4. Flujo Operacional con Agentes

1. Product/Tech Lead registra en `0-backlog/backlog.md`.
2. Divide en cards tecnicos y crea archivos en `1-todo/` usando template oficial.
3. Orquestador envia un prompt por card al agente (referenciando el archivo del card).
4. **[IMPORTANTE]** Agente mueve el card a `2-doing/` **inmediatamente** al iniciar. Nunca trabajar un card que este en `1-todo/`.
5. Agente implementa, actualiza progreso con timestamps y ejecuta validaciones del card.
6. Si hay impedimento real, mueve a `3-blocked/` con motivo y accion de desbloqueo.
7. Al desbloquear, mueve de `3-blocked/` a `2-doing/`.
8. Antes de finalizar, ejecutar el script de validacion del proyecto. Cada repositorio debe mantener su propio script cubriendo como minimo: analisis estatico y tests automatizados (ver [instructions.md — seccion 13](instructions.md#13-script-de-validación)).
9. Solo si la validacion pasa y los criterios estan marcados, mueve a `4-done/`.

Transiciones de estado, checklists y anti-patrones: ver [instructions.md — secciones 10 a 11](instructions.md#10-transiciones-de-estado-permitidas).

## 5. Uso de `prompts.md` en la Coordinacion

`prompts.md` funciona como inbox de comandos para agentes.

Buenas practicas:

- cada prompt debe apuntar a un card especifico
- evitar prompts amplios sin alcance
- registrar follow-up cuando el agente abra bloqueo
- mantener lenguaje imperativo y objetivo

## 6. Reglas de Paralelismo

Para evitar colision entre agentes:

- un card por agente a la vez
- no permitir el mismo `id` en dos columnas
- la movimentacion es por `mv` (nunca copiar card entre columnas)
- mantener WIP en `2-doing` con maximo 3 cards simultaneos
- priorizar cards independientes en paralelo
- cards con dependencia deben explicitar precondicion en el `Plan de Implementacion`

## 7. Handoff entre Agentes

Cuando un agente termina parcial y otro continua:

- actualizar `Progreso` con el estado actual
- listar exactamente lo que falta
- mantener comandos de validacion reproducibles
- solo mover columna cuando el estado real cambie (todo/doing/blocked/done)

## 8. Control de Secuencia de IDs

El campo `ultimo_id` al final de `0-backlog/backlog.md` registra el ultimo ID utilizado. Antes de crear un nuevo card, verificar este campo y usar el proximo secuencial. Despues de crear, actualizar el campo.

Esto previene colision de IDs cuando multiples agentes crean cards simultaneamente.

## 9. Politica de Archivamiento

Cards en `4-done/` hace mas de 30 dias pueden ser movidos a `5-archive/`. El archivamiento:

- preserva el card integro (sin alteraciones)
- debe ser registrado en el backlog cuando todos los cards del item esten archivados
- puede ser ejecutado via prompt de auditoria (ver `prompts.md` — seccion 3.3)

## 10. Gobernanza para Otros Equipos

Ritual recomendado:

1. Reunion corta de planificacion: prioriza backlog -> todo.
2. Ejecucion asincrona por agentes via cards.
3. Revision diaria de `2-doing` y `3-blocked`.
4. Cierre: validar `4-done` con evidencias.

Indicadores basicos:

- lead time por card
- tasa de bloqueo
- retrabajo (cards reabiertos)
- throughput semanal (`4-done`)

## 11. Referencias Internas

- Reglas operacionales: [instructions.md](instructions.md)
- Template oficial: [kanban/9-templates/CARD-001-template.md](kanban/9-templates/CARD-001-template.md)
- Catalogo de prompts: [prompts.md](prompts.md)
