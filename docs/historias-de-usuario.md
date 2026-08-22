# Historias de usuario Extendidas

_Presentar al menos una historia de usuario representativa por módulo._
_Cada historia debe incluir formato clásico, criterios de aceptación y validación INVEST._

---

## HU-01 — [Reservar Turno]

| Campo | Detalle |
|-------|---------|
| Historia | Como [cliente], quiero [reservar un turno para un servicio seleccionando una empresa, un profesional, una fecha y un horario disponible], para [asegurar mi atención en el momento que mejor se adapte a mis necesidades.]. |
| Módulo 3: |Reserva de turnos |
| Requisitos relacionados | RF-09, RF-10, RF- 11, RF-12, RF-13, RNF-01, RNF-03, RNF-04 |
| Excepciones | Si dos personas eligen el mismo horario a la vez, se lo queda el primero que confirme y al segundo se le avisa en pantalla que ya no está libre. Si el profesional no tiene lugares en el mes, se muestra "Sin turnos este mes" |
| Dependencias | Requiere sesión de cliente iniciada (Módulo 1) y disponibilidad cargada por el profesional (HU-04). |
| Datos de entrada/salida | Entran: ID Empresa, ID Profesional, Fecha, Hora, ID Cliente. Salen: ID Turno, estado, orden de envío de correo. |
| Observaciones | Historia núcleo del sistema; genera la entidad Turno y dispara la actualización de disponibilidad. |

### Criterios de aceptación

1. El sistema muestra los servicios y profesionales disponibles.
2. El cliente puede elegir fecha y hora libre en un calendario visual.
3. El sistema bloquea horarios ya ocupados.
4. El sistema guarda la reserva con estado "Reservado" y un ID único.
5. El cliente recibe un correo electrónico de confirmación.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Si | Depende de que exista disponibilidad cargada (HU-04) y sesión iniciada (Módulo 1), pero se implementa y despliega como unidad propia |
| Negociable | Si | El diseño del calendario visual y el canal de confirmación son detalles negociables con el equipo, sin cambiar el objetivo de negocio |
| Valiosa | Si | Es el núcleo del sistema: sin reserva no hay producto. Da valor inmediato al cliente y a la empresa |
| Estimable | Si | Criterios, datos y flujos alternos definidos; se puede estimar sin preguntas abiertas de diseño |
| Pequeña | Si | Cubre un único caso de uso (crear una reserva), resoluble en un sprint |
| Verificable | Si | Cada criterio es verificable con un caso de prueba concreto (reserva exitosa, duplicada, sin cupo) |

---

## HU-02 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [cliente], quiero [cancelar un turno previamente reservado], para [liberar el horario cuando no pueda asistir]. |
| Módulo | 4 |
| Requisitos relacionados | RF-14, RF-16, RF-17, RNF-01 |
| Excepciones | No se puede cancelar si faltan menos de 24 horas para el turno; si lo intenta, el sistema frena la acción y muestra un mensaje de error explicando el límite de tiempo. |
| Dependencias | Tiene que existir un turno confirmado HU01|
| Datos de entrada/salida | Entran: ID del Turno, ID del Cliente. Salen: nuevo estado "Cancelado", orden de envío de correo |
| Observaciones | El horario cancelado debe quedar libre en la base de datos al instante |

### Criterios de aceptación

1. El cliente ve sus turnos activos.
2. El cliente puede elegir un turno para cancelar.
3. El sistema cambia el estado del turno a "Cancelado".
4. El horario vuelve a estar libre de inmediato.
5. El cliente recibe un correo electrónico de cancelación.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | |
| Negociable | Si | |
| Valiosa | Si | |
| Estimable | Si | |
| Pequeña | Si | |
| Verificable | Si | |


---

## HU-03 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [rol], quiero [acción], para [objetivo]. |
| Módulo | |
| Requisitos relacionados | RF-XX, RF-XX |
| Excepciones | |
| Dependencias | |
| Datos de entrada/salida | |
| Observaciones | |

### Criterios de aceptación

1. 
2. 
3. 

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | | |
| Negociable | | |
| Valiosa | | |
| Estimable | | |
| Pequeña | | |
| Verificable | | |

---

## HU-04 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [rol], quiero [acción], para [objetivo]. |
| Módulo | |
| Requisitos relacionados | RF-XX, RF-XX |
| Excepciones | |
| Dependencias | |
| Datos de entrada/salida | |
| Observaciones | |

### Criterios de aceptación

1. 
2. 
3. 

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | | |
| Negociable | | |
| Valiosa | | |
| Estimable | | |
| Pequeña | | |
| Verificable | | |

---

## HU-05 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [rol], quiero [acción], para [objetivo]. |
| Módulo | |
| Requisitos relacionados | RF-XX, RF-XX |
| Excepciones | |
| Dependencias | |
| Datos de entrada/salida | |
| Observaciones | |

### Criterios de aceptación

1. 
2. 
3. 

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | | |
| Negociable | | |
| Valiosa | | |
| Estimable | | |
| Pequeña | | |
| Verificable | | |

---

## HU-06 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [rol], quiero [acción], para [objetivo]. |
| Módulo | |
| Requisitos relacionados | RF-XX, RF-XX |
| Excepciones | |
| Dependencias | |
| Datos de entrada/salida | |
| Observaciones | |

### Criterios de aceptación

1. 
2. 
3. 

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | | |
| Negociable | | |
| Valiosa | | |
| Estimable | | |
| Pequeña | | |
| Verificable | | |

---

## HU-07 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [rol], quiero [acción], para [objetivo]. |
| Módulo | |
| Requisitos relacionados | RF-XX, RF-XX |
| Excepciones | |
| Dependencias | |
| Datos de entrada/salida | |
| Observaciones | |

### Criterios de aceptación

1. 
2. 
3. 

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | | |
| Negociable | | |
| Valiosa | | |
| Estimable | | |
| Pequeña | | |
| Verificable | | |

---

## HU-08 — [Nombre de la historia]

| Campo | Detalle |
|-------|---------|
| Historia | Como [rol], quiero [acción], para [objetivo]. |
| Módulo | |
| Requisitos relacionados | RF-XX, RF-XX |
| Excepciones | |
| Dependencias | |
| Datos de entrada/salida | |
| Observaciones | |

### Criterios de aceptación

1. 
2. 
3. 

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | | |
| Negociable | | |
| Valiosa | | |
| Estimable | | |
| Pequeña | | |
| Verificable | | |

---


