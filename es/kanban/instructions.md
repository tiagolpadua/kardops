# KardOps — Instrucciones Operacionales

Este documento define las reglas operacionales de la metodología KardOps para gestión de tareas vía archivos Markdown dentro de `kanban/`.

## 1. Estructura

- `0-backlog/backlog.md`: fuente de verdad de lo que aún necesita ser hecho en el proyecto.
- `1-todo/`: cards listos para iniciar.
- `2-doing/`: cards en curso.
- `3-blocked/`: cards temporalmente bloqueados por dependencia externa, falla no relacionada o impedimento técnico.
- `4-done/`: cards concluidos y validados.
- `5-archive/`: cards concluidos archivados (más de 30 días en `4-done/`).
- `9-templates/`: modelos oficiales de card.

## 2. Flujo de Trabajo

1. Registrar demandas en `0-backlog/backlog.md`.
2. Dividir backlog en cards menores e independientes.
3. Crear un archivo `.md` por card en `1-todo/` usando template oficial.
4. **[IMPORTANTE]** Al iniciar ejecución, **MOVER INMEDIATAMENTE** el card a `2-doing/`.
   - Actualizar el status en el card a `status: doing`.
   - Nunca trabajar un card que esté en `1-todo/`.
5. Implementar la tarea y actualizar el card con progreso objetivo (fecha + resultado).
6. Si hay impedimento real, mover a `3-blocked/` y registrar causa/bloqueo.
7. Al desbloquear, mover de `3-blocked/` a `2-doing/`.
8. Antes de finalizar, ejecutar el script de validación del proyecto. Cada repositorio debe mantener su propio script de validación en la raíz, cubriendo como mínimo: análisis estático y tests automatizados. El formato del script es libre (shell script, Makefile, npm script, etc.).
9. Solo si la validación pasa y los criterios están marcados, actualizar status a `status: done` y mover a `4-done/`.

**FLUJO OBLIGATORIO: `1-todo/` -> `2-doing/` -> `4-done/` (o `3-blocked/` durante)**

**ANTI-PATRÓN: NO mover directo de `1-todo/` a `4-done/` sin pasar por `2-doing/`**

## 3. Reglas Obligatorias para Cards

Todo card debe contener:

- `id`: identificador único (ej.: `CARD-001`).
- `titulo`: objetivo corto y claro.
- `prioridad`: `P0`, `P1`, `P2` o `P3`.
- `origen`: referencia rastreable (backlog, análisis técnico, issue, u otro documento del proyecto).
- `descripcion`: contexto funcional/técnico.
- `alcance`: lo que entra y lo que no entra.
- `plan de implementacion`: pasos técnicos en orden.
- `criterios de aceptacion`: checklist verificable.
- `validacion`: comandos/tests que comprueban la conclusión.
- `status`: `todo`, `doing`, `blocked` o `done`.
- `progreso`: log temporal corto y objetivo.

Campos opcionales:

- `tamaño`: `P`, `M` o `G` — estimación de esfuerzo para apoyar decisiones de WIP y planificación.

## 4. Reglas de Consistencia

- **El flujo correcto es siempre: `1-todo/` -> `2-doing/` -> `4-done/` (o `3-blocked/` durante la ejecución).**
- **NUNCA mover directamente de `1-todo/` a `4-done/`.**
- El mismo `id` no puede existir en más de una columna al mismo tiempo.
- La movimentación es por `mv` (nunca copiar card entre columnas).
- Card en `2-doing` debe tener:
  - `status: doing`
  - progreso actualizado regularmente con timestamps
- Card en `4-done` debe tener:
  - `status: done`
  - criterios de aceptación marcados
  - validación obligatoria marcada
  - fecha de conclusión en el progreso
- Card en `3-blocked` debe explicitar:
  - `status: blocked`
  - motivo del bloqueo
  - acción necesaria para desbloquear
  - timestamp de cuando fue bloqueado

## 5. Convención de Nombre de Archivo

Patrón: `kanban/<columna>/CARD-XXX-slug-corto.md`

Ejemplo: `kanban/1-todo/CARD-012-ajustar-sync-offline.md`

## 6. Control de Secuencia de IDs

El campo `ultimo_id` al final de `0-backlog/backlog.md` registra el último ID utilizado. Antes de crear un nuevo card, verificar este campo y usar el próximo secuencial. Después de crear, actualizar el campo.

## 7. Template Oficial

Template oficial: `kanban/9-templates/CARD-001-template.md`

Proceso recomendado:

1. Duplicar el template en `1-todo/` con nuevo nombre.
2. Actualizar `id`, titulo, prioridad y origen.
3. Completar descripcion, alcance, plan, criterios y validacion.

## 8. Calidad Mínima por Card

- Plan de implementacion con pasos ejecutables (no solo decisión vaga).
- Criterios de aceptacion pasibles de verificación objetiva.
- Validacion con comandos reales del proyecto.
- Alcance con límites claros para evitar crecimiento indefinido.

## 9. Política de WIP y Bloqueos

- WIP recomendado en `2-doing`: máximo 3 cards simultáneos.
- Cards bloqueados no deben quedarse en `2-doing`.
- Bloqueo por falla no relacionada debe ser registrado en `progreso` y en el item de validacion.

## 10. Transiciones de Estado Permitidas

**Flujo Normal (Camino Feliz):**

```text
1-todo/ (status: todo)
  | [al iniciar ejecución]
2-doing/ (status: doing)
  | [después de completar, testear y validar]
4-done/ (status: done)
```

**Flujo con Bloqueo:**

```text
1-todo/ (status: todo)
  | [al iniciar ejecución]
2-doing/ (status: doing)
  | [al encontrar impedimento real]
3-blocked/ (status: blocked)
  | [al desbloquear]
2-doing/ (status: doing)
  | [después de completar, testear y validar]
4-done/ (status: done)
```

**Transiciones NO Permitidas:**

- `1-todo/` -> `4-done/` (saltar `2-doing/`)
- `1-todo/` -> `3-blocked/` (bloquear sin iniciar)
- `4-done/` -> `2-doing/` (reabrir sin motivo explícito)
- Dejar card en `1-todo/` mientras está siendo trabajado

## 11. Checklists de Transición

### todo -> doing

Antes de mover:

- [ ] Leí completamente el card, incluyendo plan de implementacion
- [ ] Entiendo todos los pasos del plan
- [ ] Identifiqué los pre-requisitos y dependencias
- [ ] Tengo claros cuáles son los criterios de aceptacion
- [ ] Identifiqué los comandos de validacion

Al mover:

- [ ] Moví el archivo de `1-todo/` a `2-doing/`
- [ ] Actualicé `status: doing` en el card
- [ ] Agregué timestamp de inicio en la seccion de progreso

### doing -> blocked

Al mover:

- [ ] Moví el archivo de `2-doing/` a `3-blocked/`
- [ ] Actualicé `status: blocked` en el card
- [ ] Completé la seccion `## Bloqueo` con motivo, impacto y accion para desbloquear
- [ ] Agregué timestamp del bloqueo en la seccion de progreso

### blocked -> doing

Al mover:

- [ ] Moví el archivo de `3-blocked/` a `2-doing/`
- [ ] Actualicé `status: doing` en el card
- [ ] Registré la resolucion del bloqueo en la seccion de progreso con fecha

### doing -> done

Antes de mover:

- [ ] Implementé todos los pasos del plan
- [ ] Ejecuté validaciones locales con éxito
- [ ] Marqué todos los criterios de aceptacion como [x]
- [ ] Ejecuté todos los tests (lint, build, unit tests, etc)
- [ ] Agregué timestamps de conclusión en la seccion de progreso
- [ ] Actualicé `status: done` en el card

Al mover:

- [ ] Moví el archivo de `2-doing/` a `4-done/`
- [ ] Verifiqué que `status: done`
- [ ] Verifiqué que todos los criterios están marcados [x]
- [ ] Verifiqué que todas las validaciones fueron ejecutadas [x]

## 12. Actualización del Backlog

- Al crear cards a partir del backlog, mantener trazabilidad (`origen`).
- Cuando todos los cards de un item estén en `4-done`, marcar item como concluido en `0-backlog/backlog.md` con fecha.
- No borrar histórico de backlog; preferir marcación de conclusión.

## 13. Script de Validación

Cada repositorio que adopta esta metodología debe mantener un script de validación en la raíz. El nombre y formato del script son libres (shell script, Makefile, npm script, etc.). El script debe cubrir, como mínimo:

1. **Análisis estático / linting** — detectar errores de código y violaciones de estilo.
2. **Tests automatizados** — ejecutar la suite de tests del proyecto.

El script puede incluir etapas adicionales conforme la necesidad del proyecto (cobertura, build, verificación de tipos, etc.), siempre que las dos etapas mínimas estén presentes.

Si el proyecto aún no posee el script, crearlo es pre-requisito antes de mover cualquier card a `4-done/`.

## 14. Política de Archivamiento

Cards en `4-done/` hace más de 30 días pueden ser movidos a `5-archive/`. El archivamiento:

- preserva el card íntegro (sin alteraciones)
- debe ser registrado en el backlog cuando todos los cards del item estén archivados
- puede ser ejecutado vía prompt de auditoría (ver `prompts.md` — seccion 3.3)

## 15. Definición de Listo (DoD)

Un card solo está listo cuando:

1. Implementación concluida.
2. Criterios de aceptacion marcados.
3. Script de validación del proyecto ejecutado con éxito.
4. Card actualizado a `status: done`.
5. Card movido a `4-done/`.
