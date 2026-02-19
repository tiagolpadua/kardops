# KardOps — Prompts

Catálogo de prompts reutilizables para operar el board KardOps vía agentes de IA.
Sustituya los placeholders `<...>` por los valores reales antes de usar.

---

## 1. Implementación de Card

### 1.1 Implementar card del todo

```text
Considere kanban/instructions.md
Implemente el card kanban/1-todo/<CARD-XXX-slug>.md

Siga rigurosamente el plan de implementación descrito en el card.
Al concluir cada paso, actualice la sección "Progreso" del card con fecha y resultado.
Al final, ejecute la validación descrita en el card y marque los criterios de aceptación atendidos.
Si todos los criterios pasan, actualice el status a `done` y mueva el card a kanban/4-done/.
```

### 1.2 Continuar card en curso

```text
Considere kanban/instructions.md
Continue la implementación del card kanban/2-doing/<CARD-XXX-slug>.md

Retome desde el último paso registrado en la sección "Progreso".
Siga el plan de implementación y actualice el progreso en cada paso concluido.
Si encuentra impedimento, mueva el card a kanban/3-blocked/ y registre el motivo.
Si finaliza, ejecute la validación, marque los criterios y mueva a kanban/4-done/.
```

### 1.3 Desbloquear y retomar card bloqueado

```text
Considere kanban/instructions.md
El card kanban/3-blocked/<CARD-XXX-slug>.md fue desbloqueado.

Muévalo a kanban/2-doing/, registre la resolución del bloqueo en "Progreso" con fecha,
y retome la implementación a partir del último paso concluido.
```

---

## 2. Creación de Cards

### 2.1 Crear card a partir de ítem del backlog

```text
Considere kanban/instructions.md y kanban/9-templates/CARD-001-template.md

Cree un nuevo card en kanban/1-todo/ a partir del siguiente ítem del backlog:
- Origen: 0-backlog/backlog.md#<item-referencia>
- Título: <titulo-corto>
- Prioridad: <P0|P1|P2|P3>

Complete todas las secciones obligatorias: descripción, alcance (incluye/no incluye),
plan de implementación con pasos ejecutables, criterios de aceptación verificables
y comandos de validación. Use el próximo ID secuencial disponible.
```

### 2.2 Dividir ítem del backlog en múltiples cards

```text
Considere kanban/instructions.md y kanban/9-templates/CARD-001-template.md

Analice el ítem "<descripcion-del-item>" del backlog y divídalo en cards menores
e independientes. Cada card debe:
- Tener alcance cerrado e implementable aisladamente
- Seguir el template oficial completo
- Ser creado en kanban/1-todo/ con ID secuencial
- Referenciar el origen en el backlog

Liste los cards creados al final.
```

---

## 3. Limpieza y Reorganización del Board

### 3.1 Auditoría completa del board

```text
Considere kanban/instructions.md

Haga una auditoría completa del board Kanban en kanban/:
1. Liste todos los cards en cada columna (1-todo, 2-doing, 3-blocked, 4-done).
2. Verifique si cada card posee todos los campos obligatorios (id, título, prioridad, origen, descripción, alcance, plan, criterios, validación, status, progreso).
3. Verifique si el status del card es consistente con la columna en que se encuentra.
4. Verifique si no hay IDs duplicados entre columnas.
5. Verifique si el WIP en 2-doing respeta el límite de 3 cards.
6. Verifique si cards en 3-blocked tienen motivo y acción de desbloqueo registrados.
7. Verifique si cards en 4-done tienen criterios marcados y validación ejecutada.

Presente un reporte con problemas encontrados y sugerencias de corrección.
```

### 3.2 Corregir inconsistencias del board

```text
Considere kanban/instructions.md

A partir de la auditoría del board Kanban en kanban/:
1. Corrija cards cuyo campo `status` no corresponde a la columna donde están.
2. Mueva cards que están en la columna equivocada a la columna correcta conforme su status.
3. Complete campos obligatorios ausentes con valores placeholder marcados como [PENDIENTE].
4. Remueva duplicados de ID, manteniendo la versión más reciente (por el progreso).
5. Registre todas las correcciones hechas en la sección "Progreso" de cada card afectado.
```

### 3.3 Limpiar cards concluidos antiguos

```text
Considere kanban/instructions.md

Analice los cards en kanban/4-done/:
1. Liste todos los cards concluidos con sus fechas de conclusión.
2. Identifique cards concluidos hace más de <N> días.
3. Verifique si los ítems correspondientes en el backlog están marcados como concluidos.
4. Sugiera cuáles cards pueden ser archivados (movidos a una carpeta kanban/5-archive/).

No ejecute el archivamiento automáticamente — solo liste y aguarde confirmación.
```

### 3.4 Reorganizar backlog y sincronizar con cards

```text
Considere kanban/instructions.md

Sincronice el backlog (kanban/0-backlog/backlog.md) con el estado actual del board:
1. Identifique ítems del backlog que ya poseen cards creados (en cualquier columna).
2. Marque en el backlog los ítems cuyos cards ya están en 4-done con fecha de conclusión.
3. Identifique ítems del backlog sin cards asociados y lístelos.
4. Identifique cards huérfanos (sin referencia válida de origen en el backlog) y lístelos.
5. Sugiera reordenación del backlog por prioridad.
```

---

## 4. Gestión del Board

### 4.1 Status general del board (daily/standup)

```text
Considere kanban/instructions.md

Genere un resumen de status del board Kanban para standup:
- Cuántos cards en cada columna (todo, doing, blocked, done).
- Cards en curso (2-doing): título, último progreso registrado.
- Cards bloqueados (3-blocked): título, motivo del bloqueo.
- Cards listos para iniciar (1-todo): título y prioridad (ordenados por P0->P3).
- Alertas: WIP excedido, cards parados sin progreso reciente.
```

### 4.2 Priorizar próximo card para ejecución

```text
Considere kanban/instructions.md

Analice los cards en kanban/1-todo/ y recomiende el próximo card a ser iniciado,
considerando:
- Prioridad (P0 > P1 > P2 > P3)
- Límite de WIP en 2-doing (máximo 3)
- Dependencias o bloqueos de otros cards
- Complejidad estimada por el plan de implementación

Justifique la recomendación.
```

### 4.3 Mover card entre columnas

```text
Considere kanban/instructions.md

Mueva el card <CARD-XXX-slug>.md de kanban/<columna-origen>/ a kanban/<columna-destino>/.
Actualice el campo `status` del card para reflejar la nueva columna.
Registre la movimentación en la sección "Progreso" con fecha.
Valide las reglas de consistencia (ej.: card yendo a done necesita tener criterios marcados).
```

---

## 5. Mantenimiento y Calidad

### 5.1 Validar calidad de un card específico

```text
Considere kanban/instructions.md

Evalúe la calidad del card kanban/<columna>/<CARD-XXX-slug>.md:
- Todos los campos obligatorios están completos?
- El plan de implementación tiene pasos ejecutables y ordenados?
- Los criterios de aceptación son verificables objetivamente?
- La validación incluye comandos reales del proyecto?
- El alcance tiene límites claros (incluye/no incluye)?

Sugiera mejoras concretas si es necesario.
```

### 5.2 Estandarizar cards existentes conforme template

```text
Considere kanban/instructions.md y kanban/9-templates/CARD-001-template.md

Revise todos los cards existentes en el board (todas las columnas) y:
1. Identifique cards que no siguen la estructura del template oficial.
2. Para cada card fuera del estándar, ajuste la estructura para quedar conforme al template.
3. Preserve el contenido original — solo reorganice y agregue secciones ausentes.
4. Registre el ajuste en la seccion "Progreso" de cada card modificado.
```

### 5.3 Actualizar template oficial

```text
Considere kanban/instructions.md

Revise el template kanban/9-templates/CARD-001-template.md para garantizar que:
- Refleja todas las reglas y campos obligatorios de kanban/instructions.md.
- Contiene ejemplos útiles en cada sección para facilitar el llenado.
- Está alineado con las convenciones actuales del proyecto.

Haga los ajustes necesarios en el template.
```

---

## 6. Reportes y Métricas

### 6.1 Generar reporte de throughput

```text
Considere kanban/instructions.md

Analice los cards en kanban/4-done/ y genere un reporte de throughput:
- Total de cards concluidos.
- Cards concluidos por período (semanal, si es posible inferir por las fechas de progreso).
- Tiempo medio de ciclo (de la creación/todo hasta done, basado en las fechas de progreso).
- Distribución por prioridad (cuántos P0, P1, P2, P3 concluidos).
```

### 6.2 Generar reporte de bloqueos

```text
Considere kanban/instructions.md

Analice el histórico de cards en kanban/3-blocked/ y kanban/4-done/ (que pasaron por bloqueo):
- Cuántos cards fueron bloqueados.
- Motivos de bloqueo más frecuentes.
- Tiempo medio de permanencia en blocked (si es inferible por el progreso).
- Sugerencias para reducir bloqueos futuros.
```
