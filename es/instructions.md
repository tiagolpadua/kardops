# KardOps — Instrucciones Operacionales

Este documento define las reglas operacionales de la metodologia KardOps para gestion de tareas via archivos Markdown dentro de `kanban/`.

## 1. Estructura

- `0-backlog/backlog.md`: fuente de verdad de lo que aun necesita ser hecho en el proyecto.
- `1-todo/`: cards listos para iniciar.
- `2-doing/`: cards en curso.
- `3-blocked/`: cards temporalmente bloqueados por dependencia externa, falla no relacionada o impedimento tecnico.
- `4-done/`: cards concluidos y validados.
- `5-archive/`: cards concluidos archivados (mas de 30 dias en `4-done/`).
- `9-templates/`: modelos oficiales de card.

## 2. Flujo de Trabajo

1. Registrar demandas en `0-backlog/backlog.md`.
2. Dividir backlog en cards menores e independientes.
3. Crear un archivo `.md` por card en `1-todo/` usando template oficial.
4. **[IMPORTANTE]** Al iniciar ejecucion, **MOVER INMEDIATAMENTE** el card a `2-doing/`.
   - Actualizar el status en el card a `status: doing`.
   - Nunca trabajar un card que este en `1-todo/`.
5. Implementar la tarea y actualizar el card con progreso objetivo (fecha + resultado).
6. Si hay impedimento real, mover a `3-blocked/` y registrar causa/bloqueo.
7. Al desbloquear, mover de `3-blocked/` a `2-doing/`.
8. Antes de finalizar, ejecutar el script de validacion del proyecto. Cada repositorio debe mantener su propio script de validacion en la raiz, cubriendo como minimo: analisis estatico y tests automatizados. El formato del script es libre (shell script, Makefile, npm script, etc.).
9. Solo si la validacion pasa y los criterios estan marcados, actualizar status a `status: done` y mover a `4-done/`.

**FLUJO OBLIGATORIO: `1-todo/` -> `2-doing/` -> `4-done/` (o `3-blocked/` durante)**

**ANTI-PATRON: NO mover directo de `1-todo/` a `4-done/` sin pasar por `2-doing/`**

## 3. Reglas Obligatorias para Cards

Todo card debe contener:

- `id`: identificador unico (ej.: `CARD-001`).
- `titulo`: objetivo corto y claro.
- `prioridad`: `P0`, `P1`, `P2` o `P3`.
- `origen`: referencia rastreable (backlog, analisis tecnico, issue, u otro documento del proyecto).
- `descripcion`: contexto funcional/tecnico.
- `alcance`: lo que entra y lo que no entra.
- `plan de implementacion`: pasos tecnicos en orden.
- `criterios de aceptacion`: checklist verificable.
- `validacion`: comandos/tests que comprueban la conclusion.
- `status`: `todo`, `doing`, `blocked` o `done`.
- `progreso`: log temporal corto y objetivo.

Campos opcionales:

- `tamaño`: `P`, `M` o `G` — estimacion de esfuerzo para apoyar decisiones de WIP y planificacion.

## 4. Reglas de Consistencia

- **El flujo correcto es siempre: `1-todo/` -> `2-doing/` -> `4-done/` (o `3-blocked/` durante la ejecucion).**
- **NUNCA mover directamente de `1-todo/` a `4-done/`.**
- El mismo `id` no puede existir en mas de una columna al mismo tiempo.
- La movimentacion es por `mv` (nunca copiar card entre columnas).
- Card en `2-doing` debe tener:
  - `status: doing`
  - progreso actualizado regularmente con timestamps
- Card en `4-done` debe tener:
  - `status: done`
  - criterios de aceptacion marcados
  - validacion obligatoria marcada
  - fecha de conclusion en el progreso
- Card en `3-blocked` debe explicitar:
  - `status: blocked`
  - motivo del bloqueo
  - accion necesaria para desbloquear
  - timestamp de cuando fue bloqueado

## 5. Convencion de Nombre de Archivo

Patron: `kanban/<columna>/CARD-XXX-slug-corto.md`

Ejemplo: `kanban/1-todo/CARD-012-ajustar-sync-offline.md`

## 6. Control de Secuencia de IDs

El campo `ultimo_id` al final de `0-backlog/backlog.md` registra el ultimo ID utilizado. Antes de crear un nuevo card, verificar este campo y usar el proximo secuencial. Despues de crear, actualizar el campo.

## 7. Template Oficial

Template oficial: `kanban/9-templates/CARD-001-template.md`

Proceso recomendado:

1. Duplicar el template en `1-todo/` con nuevo nombre.
2. Actualizar `id`, titulo, prioridad y origen.
3. Completar descripcion, alcance, plan, criterios y validacion.

## 8. Calidad Minima por Card

- Plan de implementacion con pasos ejecutables (no solo decision vaga).
- Criterios de aceptacion pasibles de verificacion objetiva.
- Validacion con comandos reales del proyecto.
- Alcance con limites claros para evitar crecimiento indefinido.

## 9. Politica de WIP y Bloqueos

- WIP recomendado en `2-doing`: maximo 3 cards simultaneos.
- Cards bloqueados no deben quedarse en `2-doing`.
- Bloqueo por falla no relacionada debe ser registrado en `progreso` y en el item de validacion.

## 10. Transiciones de Estado Permitidas

**Flujo Normal (Camino Feliz):**

```text
1-todo/ (status: todo)
  | [al iniciar ejecucion]
2-doing/ (status: doing)
  | [despues de completar, testear y validar]
4-done/ (status: done)
```

**Flujo con Bloqueo:**

```text
1-todo/ (status: todo)
  | [al iniciar ejecucion]
2-doing/ (status: doing)
  | [al encontrar impedimento real]
3-blocked/ (status: blocked)
  | [al desbloquear]
2-doing/ (status: doing)
  | [despues de completar, testear y validar]
4-done/ (status: done)
```

**Transiciones NO Permitidas:**

- `1-todo/` -> `4-done/` (saltar `2-doing/`)
- `1-todo/` -> `3-blocked/` (bloquear sin iniciar)
- `4-done/` -> `2-doing/` (reabrir sin motivo explicito)
- Dejar card en `1-todo/` mientras esta siendo trabajado

## 11. Checklists de Transicion

### todo -> doing

Antes de mover:

- [ ] Lei completamente el card, incluyendo plan de implementacion
- [ ] Entiendo todos los pasos del plan
- [ ] Identifique los pre-requisitos y dependencias
- [ ] Tengo claros cuales son los criterios de aceptacion
- [ ] Identifique los comandos de validacion

Al mover:

- [ ] Movi el archivo de `1-todo/` a `2-doing/`
- [ ] Actualice `status: doing` en el card
- [ ] Agregue timestamp de inicio en la seccion de progreso

### doing -> blocked

Al mover:

- [ ] Movi el archivo de `2-doing/` a `3-blocked/`
- [ ] Actualice `status: blocked` en el card
- [ ] Complete la seccion `## Bloqueo` con motivo, impacto y accion para desbloquear
- [ ] Agregue timestamp del bloqueo en la seccion de progreso

### blocked -> doing

Al mover:

- [ ] Movi el archivo de `3-blocked/` a `2-doing/`
- [ ] Actualice `status: doing` en el card
- [ ] Registre la resolucion del bloqueo en la seccion de progreso con fecha

### doing -> done

Antes de mover:

- [ ] Implemente todos los pasos del plan
- [ ] Ejecute validaciones locales con exito
- [ ] Marque todos los criterios de aceptacion como [x]
- [ ] Ejecute todos los tests (lint, build, unit tests, etc)
- [ ] Agregue timestamps de conclusion en la seccion de progreso
- [ ] Actualice `status: done` en el card

Al mover:

- [ ] Movi el archivo de `2-doing/` a `4-done/`
- [ ] Verifique que `status: done`
- [ ] Verifique que todos los criterios estan marcados [x]
- [ ] Verifique que todas las validaciones fueron ejecutadas [x]

## 12. Actualizacion del Backlog

- Al crear cards a partir del backlog, mantener trazabilidad (`origen`).
- Cuando todos los cards de un item esten en `4-done`, marcar item como concluido en `0-backlog/backlog.md` con fecha.
- No borrar historico de backlog; preferir marcacion de conclusion.

## 13. Script de Validacion

Cada repositorio que adopta esta metodologia debe mantener un script de validacion en la raiz. El nombre y formato del script son libres (shell script, Makefile, npm script, etc.). El script debe cubrir, como minimo:

1. **Analisis estatico / linting** — detectar errores de codigo y violaciones de estilo.
2. **Tests automatizados** — ejecutar la suite de tests del proyecto.

El script puede incluir etapas adicionales conforme la necesidad del proyecto (cobertura, build, verificacion de tipos, etc.), siempre que las dos etapas minimas esten presentes.

Si el proyecto aun no posee el script, crearlo es pre-requisito antes de mover cualquier card a `4-done/`.

## 14. Politica de Archivamiento

Cards en `4-done/` hace mas de 30 dias pueden ser movidos a `5-archive/`. El archivamiento:

- preserva el card integro (sin alteraciones)
- debe ser registrado en el backlog cuando todos los cards del item esten archivados
- puede ser ejecutado via prompt de auditoria (ver `prompts.md` — seccion 3.3)

## 15. Definicion de Listo (DoD)

Un card solo esta listo cuando:

1. Implementacion concluida.
2. Criterios de aceptacion marcados.
3. Script de validacion del proyecto ejecutado con exito.
4. Card actualizado a `status: done`.
5. Card movido a `4-done/`.
