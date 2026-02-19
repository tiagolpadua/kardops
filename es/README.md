# KardOps

**KardOps** es una metodología para coordinar agentes de IA en tareas de ingeniería usando cards Kanban como unidad de trabajo.

Para reglas operacionales detalladas, consulte [instructions.md](instructions.md).

## 1. Objetivo

Estandarizar la ejecución de trabajo por agentes de IA con:

- trazabilidad de punta a punta
- menor conflicto entre ejecuciones paralelas
- calidad mínima verificable antes de concluir cards
- claridad de ownership y handoff entre agentes

## 2. Estructura Kanban

La carpeta `kanban/` usa prefijo numérico para garantizar el ordenamiento:

| Carpeta                | Finalidad                                                      |
| ---------------------- | -------------------------------------------------------------- |
| `0-backlog/backlog.md` | Ideas y demandas aún no comprometidas                          |
| `1-todo/`              | Cards listos para ejecución                                    |
| `2-doing/`             | Cards en ejecución activa                                      |
| `3-blocked/`           | Cards bloqueados por dependencia externa o impedimento técnico |
| `4-done/`              | Cards concluidos y validados                                   |
| `5-archive/`           | Cards concluidos archivados (más de 30 días en `4-done/`)      |
| `9-templates/`         | Templates oficiales                                            |

Archivos auxiliares:

- `instructions.md`: reglas operacionales detalladas (fuente de verdad)
- `prompts.md`: catálogo de prompts reutilizables para agentes

## 3. Unidad de Trabajo: Card

Cada card representa una entrega técnica pequeña y verificable. Campos obligatorios y reglas detalladas están en [instructions.md — sección 3](instructions.md#3-reglas-obligatorias-para-cards).

Campos opcionales:

- `tamaño`: `P`, `M` o `G` — estimación de esfuerzo para apoyar decisiones de WIP y planificación

## 4. Flujo Operacional con Agentes

1. Product/Tech Lead registra en `0-backlog/backlog.md`.
2. Divide en cards técnicos y crea archivos en `1-todo/` usando template oficial.
3. Orquestador envía un prompt por card al agente (referenciando el archivo del card).
4. **[IMPORTANTE]** Agente mueve el card a `2-doing/` **inmediatamente** al iniciar. Nunca trabajar un card que esté en `1-todo/`.
5. Agente implementa, actualiza progreso con timestamps y ejecuta validaciones del card.
6. Si hay impedimento real, mueve a `3-blocked/` con motivo y acción de desbloqueo.
7. Al desbloquear, mueve de `3-blocked/` a `2-doing/`.
8. Antes de finalizar, ejecutar el script de validación del proyecto. Cada repositorio debe mantener su propio script cubriendo como mínimo: análisis estático y tests automatizados (ver [instructions.md — sección 13](instructions.md#13-script-de-validación)).
9. Solo si la validación pasa y los criterios están marcados, mueve a `4-done/`.

Transiciones de estado, checklists y anti-patrones: ver [instructions.md — secciones 10 a 11](instructions.md#10-transiciones-de-estado-permitidas).

## 5. Uso de `prompts.md` en la Coordinación

`prompts.md` funciona como inbox de comandos para agentes.

Buenas prácticas:

- cada prompt debe apuntar a un card específico
- evitar prompts amplios sin alcance
- registrar follow-up cuando el agente abra bloqueo
- mantener lenguaje imperativo y objetivo

## 6. Reglas de Paralelismo

Para evitar colisión entre agentes:

- un card por agente a la vez
- no permitir el mismo `id` en dos columnas
- la movimentación es por `mv` (nunca copiar card entre columnas)
- mantener WIP en `2-doing` con máximo 3 cards simultáneos
- priorizar cards independientes en paralelo
- cards con dependencia deben explicitar precondición en el `Plan de Implementación`

## 7. Handoff entre Agentes

Cuando un agente termina parcial y otro continúa:

- actualizar `Progreso` con el estado actual
- listar exactamente lo que falta
- mantener comandos de validación reproducibles
- solo mover columna cuando el estado real cambie (todo/doing/blocked/done)

## 8. Control de Secuencia de IDs

El campo `ultimo_id` al final de `0-backlog/backlog.md` registra el último ID utilizado. Antes de crear un nuevo card, verificar este campo y usar el próximo secuencial. Después de crear, actualizar el campo.

Esto previene colisión de IDs cuando múltiples agentes crean cards simultáneamente.

## 9. Política de Archivamiento

Cards en `4-done/` hace más de 30 días pueden ser movidos a `5-archive/`. El archivamiento:

- preserva el card íntegro (sin alteraciones)
- debe ser registrado en el backlog cuando todos los cards del item estén archivados
- puede ser ejecutado vía prompt de auditoría (ver `prompts.md` — sección 3.3)

## 10. Gobernanza para Otros Equipos

Ritual recomendado:

1. Reunión corta de planificación: prioriza backlog -> todo.
2. Ejecución asíncrona por agentes vía cards.
3. Revisión diaria de `2-doing` y `3-blocked`.
4. Cierre: validar `4-done` con evidencias.

Indicadores básicos:

- lead time por card
- tasa de bloqueo
- retrabajo (cards reabiertos)
- throughput semanal (`4-done`)

## 11. Referencias Internas

- Reglas operacionales: [instructions.md](instructions.md)
- Template oficial: [kanban/9-templates/CARD-001-template.md](kanban/9-templates/CARD-001-template.md)
- Catálogo de prompts: [prompts.md](prompts.md)
