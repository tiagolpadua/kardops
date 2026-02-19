# KardOps — Prompts

Catalogo de prompts reutilizables para operar el board KardOps via agentes de IA.
Sustituya los placeholders `<...>` por los valores reales antes de usar.

---

## 1. Implementacion de Card

### 1.1 Implementar card del todo

```text
Considere kanban/instructions.md
Implemente el card kanban/1-todo/<CARD-XXX-slug>.md

Siga rigurosamente el plan de implementacion descrito en el card.
Al concluir cada paso, actualice la seccion "Progreso" del card con fecha y resultado.
Al final, ejecute la validacion descrita en el card y marque los criterios de aceptacion atendidos.
Si todos los criterios pasan, actualice el status a `done` y mueva el card a kanban/4-done/.
```

### 1.2 Continuar card en curso

```text
Considere kanban/instructions.md
Continue la implementacion del card kanban/2-doing/<CARD-XXX-slug>.md

Retome desde el ultimo paso registrado en la seccion "Progreso".
Siga el plan de implementacion y actualice el progreso en cada paso concluido.
Si encuentra impedimento, mueva el card a kanban/3-blocked/ y registre el motivo.
Si finaliza, ejecute la validacion, marque los criterios y mueva a kanban/4-done/.
```

### 1.3 Desbloquear y retomar card bloqueado

```text
Considere kanban/instructions.md
El card kanban/3-blocked/<CARD-XXX-slug>.md fue desbloqueado.

Muevalo a kanban/2-doing/, registre la resolucion del bloqueo en "Progreso" con fecha,
y retome la implementacion a partir del ultimo paso concluido.
```

---

## 2. Creacion de Cards

### 2.1 Crear card a partir de item del backlog

```text
Considere kanban/instructions.md y kanban/9-templates/CARD-001-template.md

Cree un nuevo card en kanban/1-todo/ a partir del siguiente item del backlog:
- Origen: 0-backlog/backlog.md#<item-referencia>
- Titulo: <titulo-corto>
- Prioridad: <P0|P1|P2|P3>

Complete todas las secciones obligatorias: descripcion, alcance (incluye/no incluye),
plan de implementacion con pasos ejecutables, criterios de aceptacion verificables
y comandos de validacion. Use el proximo ID secuencial disponible.
```

### 2.2 Dividir item del backlog en multiples cards

```text
Considere kanban/instructions.md y kanban/9-templates/CARD-001-template.md

Analice el item "<descripcion-del-item>" del backlog y dividalo en cards menores
e independientes. Cada card debe:
- Tener alcance cerrado e implementable aisladamente
- Seguir el template oficial completo
- Ser creado en kanban/1-todo/ con ID secuencial
- Referenciar el origen en el backlog

Liste los cards creados al final.
```

---

## 3. Limpieza y Reorganizacion del Board

### 3.1 Auditoria completa del board

```text
Considere kanban/instructions.md

Haga una auditoria completa del board Kanban en kanban/:
1. Liste todos los cards en cada columna (1-todo, 2-doing, 3-blocked, 4-done).
2. Verifique si cada card posee todos los campos obligatorios (id, titulo, prioridad, origen, descripcion, alcance, plan, criterios, validacion, status, progreso).
3. Verifique si el status del card es consistente con la columna en que se encuentra.
4. Verifique si no hay IDs duplicados entre columnas.
5. Verifique si el WIP en 2-doing respeta el limite de 3 cards.
6. Verifique si cards en 3-blocked tienen motivo y accion de desbloqueo registrados.
7. Verifique si cards en 4-done tienen criterios marcados y validacion ejecutada.

Presente un reporte con problemas encontrados y sugerencias de correccion.
```

### 3.2 Corregir inconsistencias del board

```text
Considere kanban/instructions.md

A partir de la auditoria del board Kanban en kanban/:
1. Corrija cards cuyo campo `status` no corresponde a la columna donde estan.
2. Mueva cards que estan en la columna equivocada a la columna correcta conforme su status.
3. Complete campos obligatorios ausentes con valores placeholder marcados como [PENDIENTE].
4. Remueva duplicados de ID, manteniendo la version mas reciente (por el progreso).
5. Registre todas las correcciones hechas en la seccion "Progreso" de cada card afectado.
```

### 3.3 Limpiar cards concluidos antiguos

```text
Considere kanban/instructions.md

Analice los cards en kanban/4-done/:
1. Liste todos los cards concluidos con sus fechas de conclusion.
2. Identifique cards concluidos hace mas de <N> dias.
3. Verifique si los items correspondientes en el backlog estan marcados como concluidos.
4. Sugiera cuales cards pueden ser archivados (movidos a una carpeta kanban/5-archive/).

No ejecute el archivamiento automaticamente — solo liste y aguarde confirmacion.
```

### 3.4 Reorganizar backlog y sincronizar con cards

```text
Considere kanban/instructions.md

Sincronice el backlog (kanban/0-backlog/backlog.md) con el estado actual del board:
1. Identifique items del backlog que ya poseen cards creados (en cualquier columna).
2. Marque en el backlog los items cuyos cards ya estan en 4-done con fecha de conclusion.
3. Identifique items del backlog sin cards asociados y listelos.
4. Identifique cards huerfanos (sin referencia valida de origen en el backlog) y listelos.
5. Sugiera reordenacion del backlog por prioridad.
```

---

## 4. Gestion del Board

### 4.1 Status general del board (daily/standup)

```text
Considere kanban/instructions.md

Genere un resumen de status del board Kanban para standup:
- Cuantos cards en cada columna (todo, doing, blocked, done).
- Cards en curso (2-doing): titulo, ultimo progreso registrado.
- Cards bloqueados (3-blocked): titulo, motivo del bloqueo.
- Cards listos para iniciar (1-todo): titulo y prioridad (ordenados por P0->P3).
- Alertas: WIP excedido, cards parados sin progreso reciente.
```

### 4.2 Priorizar proximo card para ejecucion

```text
Considere kanban/instructions.md

Analice los cards en kanban/1-todo/ y recomiende el proximo card a ser iniciado,
considerando:
- Prioridad (P0 > P1 > P2 > P3)
- Limite de WIP en 2-doing (maximo 3)
- Dependencias o bloqueos de otros cards
- Complejidad estimada por el plan de implementacion

Justifique la recomendacion.
```

### 4.3 Mover card entre columnas

```text
Considere kanban/instructions.md

Mueva el card <CARD-XXX-slug>.md de kanban/<columna-origen>/ a kanban/<columna-destino>/.
Actualice el campo `status` del card para reflejar la nueva columna.
Registre la movimentacion en la seccion "Progreso" con fecha.
Valide las reglas de consistencia (ej.: card yendo a done necesita tener criterios marcados).
```

---

## 5. Mantenimiento y Calidad

### 5.1 Validar calidad de un card especifico

```text
Considere kanban/instructions.md

Evalue la calidad del card kanban/<columna>/<CARD-XXX-slug>.md:
- Todos los campos obligatorios estan completos?
- El plan de implementacion tiene pasos ejecutables y ordenados?
- Los criterios de aceptacion son verificables objetivamente?
- La validacion incluye comandos reales del proyecto?
- El alcance tiene limites claros (incluye/no incluye)?

Sugiera mejoras concretas si es necesario.
```

### 5.2 Estandarizar cards existentes conforme template

```text
Considere kanban/instructions.md y kanban/9-templates/CARD-001-template.md

Revise todos los cards existentes en el board (todas las columnas) y:
1. Identifique cards que no siguen la estructura del template oficial.
2. Para cada card fuera del estandar, ajuste la estructura para quedar conforme el template.
3. Preserve el contenido original — solo reorganice y agregue secciones ausentes.
4. Registre el ajuste en la seccion "Progreso" de cada card modificado.
```

### 5.3 Actualizar template oficial

```text
Considere kanban/instructions.md

Revise el template kanban/9-templates/CARD-001-template.md para garantizar que:
- Refleja todas las reglas y campos obligatorios de kanban/instructions.md.
- Contiene ejemplos utiles en cada seccion para facilitar el llenado.
- Esta alineado con las convenciones actuales del proyecto.

Haga los ajustes necesarios en el template.
```

---

## 6. Reportes y Metricas

### 6.1 Generar reporte de throughput

```text
Considere kanban/instructions.md

Analice los cards en kanban/4-done/ y genere un reporte de throughput:
- Total de cards concluidos.
- Cards concluidos por periodo (semanal, si es posible inferir por las fechas de progreso).
- Tiempo medio de ciclo (de la creacion/todo hasta done, basado en las fechas de progreso).
- Distribucion por prioridad (cuantos P0, P1, P2, P3 concluidos).
```

### 6.2 Generar reporte de bloqueos

```text
Considere kanban/instructions.md

Analice el historico de cards en kanban/3-blocked/ y kanban/4-done/ (que pasaron por bloqueo):
- Cuantos cards fueron bloqueados.
- Motivos de bloqueo mas frecuentes.
- Tiempo medio de permanencia en blocked (si es inferible por el progreso).
- Sugerencias para reducir bloqueos futuros.
```
