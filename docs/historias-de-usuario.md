# Historias de usuario Extendidas

_Presentar al menos una historia de usuario representativa por módulo._
_Cada historia debe incluir formato clásico, criterios de aceptación y validación INVEST._

---

## HU-01 — [Reservar Turno]

| Campo | Detalle |
|-------|---------|
| Historia | Como [cliente], quiero [reservar un turno para un servicio seleccionando una empresa, un profesional, una fecha y un horario disponible], para [asegurar mi atención en el momento que mejor se adapte a mis necesidades.]. |
| Módulo 3: |Reserva de turnos |
| Requisitos relacionados | RF-09, RF-10, RF- 11, RF-12, RF-13, RNF-01 (respuesta < 3s), RNF-03 (HTTPS), RNF-04 (contraseñas cifradas, por el login previo)|
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

## HU-02 — [Cancelar Turno]

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
| Independiente | Si | Depende de que exista un turno confirmado (HU-01),pero se puede implementar y desplegar como unidad propia |
| Negociable | Si |  El canal de notificación de cancelación y el mensaje de error son detalles ajustables sin cambiar el objetivo|
| Valiosa | Si |  Libera horarios y da flexibilidad al cliente, evitando ausencias sin aviso |
| Estimable | Si |  Criterios, excepción y datos definidos; no hay preguntas abiertas de diseño |
| Pequeña | Si | Cubre un único caso de uso (cancelar una reserva existente), resoluble en un sprint |
| Verificable | Si | Cada criterio tiene un caso de prueba concreto (cancelación exitosa, cancelación fuera de plazo) |


---

## HU-03 — [Reprogramar Turno]

| Campo | Detalle |
|-------|---------|
| Historia | Como [cliente], quiero [modificar la fecha u horario de un turno reservado], para [adaptarlo a cambios en mi disponibilidad, sin necesidad de cancelar y crear una nueva reserva]. |
| Módulo | 4 |
| Requisitos relacionados | RF-15, RF-16, RNF-01 ( respuesta <3s)|
| Excepciones | Si el horario elegido es tomado por otro cliente antes de confirmar, el sistema avisa "Horario ya no disponible" y pide elegir otro sin perder el turno original. No se permite reprogramar si faltan menos de 24 horas para el turno (mismo límite que la cancelación); el sistema bloquea la acción y explica el motivo.  |
| Dependencias |  HU-01 (el turno debe existir), HU-04 (disponibilidad vigente del profesional), Módulo 5 (Notificaciones). |
| Datos de entrada/salida | Entran: ID Turno, nueva Fecha, nueva Hora. Salen: estado actualizado, fecha anterior (histórico), orden de envío de correo. |
| Observaciones | La operación de reprogramar (liberar horario anterior + reservar el nuevo) debe ejecutarse en una única transacción, en menos de 2 segundos. |

### Criterios de aceptación

1. El cliente puede seleccionar un turno activo propio para reprogramar.
2. El sistema muestra únicamente horarios disponibles del mismo profesional/servicio.
3. El sistema actualiza la reserva con la nueva fecha/hora y libera el horario anterior en la misma operación.
4. El cliente recibe una confirmación por correo electrónico del nuevo horario.
5. El sistema conserva un registro histórico del cambio (fecha anterior y nueva).

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de HU-01 (turno existente) y HU-04 (disponibilidad vigente), pero se implementa como unidad propia |
| Negociable | Si | El límite de 2 segundos y el diseño del flujo de selección de nuevo horario son negociables con el equipo |
| Valiosa | Si | Evita que el cliente tenga que cancelar y volver a reservar, mejorando la experiencia |
| Estimable | Si | Flujo, excepciones y datos de entrada/salida bien definidos |
| Pequeña | Si | Cubre un único caso de uso (mover un turno existente), resoluble en un sprint |
| Verificable | Si | Se puede probar con casos concretos (reprogramación exitosa, conflicto de horario, fuera de plazo) |

---

## HU-04 — [Configurar Disponibilidad]

| Campo | Detalle |
|-------|---------|
| Historia | Como [Profesional], quiero [definir mis días y horarios disponibles para atención], para [que los clientes solo puedan reservar turnos dentro de mi disponibilidad real]. |
| Módulo | 7 |
| Requisitos relacionados | RF-25, RNF-01|
| Excepciones | Si el profesional intenta guardar una franja superpuesta con otra ya cargada, el sistema rechaza el guardado y marca el conflicto. Si hay turnos confirmados en una franja que se intenta eliminar, se muestra una advertencia y se pide confirmación explícita. |
| Dependencias |El profesional debe existir dentro de una empresa|
| Datos de entrada/salida | Entran: ID Profesional, día, hora inicio, hora fin. Salen: franja guardada/actualizada, lista de conflictos si existen. |


### Criterios de aceptación

1. El profesional puede registrar días de la semana y franjas horarias (hora inicio - hora fin).
2. El sistema valida que no se superpongan franjas horarias en el mismo día.
3. La disponibilidad configurada se refleja de inmediato en las búsquedas de los clientes.
4. El profesional puede modificar o eliminar una franja ya configurada.
5. Si se reduce la disponibilidad y existen turnos ya confirmados en el horario eliminado, el sistema advierte y no cancela esos turnos automáticamente.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de que el profesional exista dentro de una empresa (Módulo 2), pero se despliega como unidad propia |
| Negociable | Si | La forma de presentar el calendario y el mensaje de advertencia son detalles negociables |
| Valiosa | Si | Sin esta historia no hay base real para que HU-01 funcione correctamente |
| Estimable | Si | Reglas de validación (superposición, turnos confirmados) y datos bien definidos |
| Pequeña | Si | Cubre un único caso de uso (cargar/editar franjas horarias), resoluble en un sprint |
| Verificable | Si | Cada criterio es verificable (franja superpuesta rechazada, advertencia ante turnos confirmados) |

---

## HU-05 — [Consultar agenda]

| Campo | Detalle |
|-------|---------|
| Historia | Como [profesional], quiero [visualizar mis turnos programados en un rango de fechas], para [organizar mi jornada labora]. |
| Módulo | 7 |
| Requisitos relacionados | RF-26, RNF-01 (respuesta <3s)|
| Excepciones | Si falla la carga de datos (error de conexión), el sistema muestra un mensaje de error y permite reintentar sin perder el filtro de fecha aplicado. |
| Dependencias | HU-01 (Deben existir turnos programados). |
| Datos de entrada/salida | 	
Entran: ID Profesional, rango de fechas (opcional). Salen: lista de turnos con fecha, hora, cliente, servicio y estado. |


### Criterios de aceptación

1. El sistema muestra los turnos del profesional para el día actual por defecto, con opción de filtrar por rango de fechas.
2. Cada turno visualizado indica fecha, hora, cliente, servicio y estado.
3. Los turnos cancelados se muestran diferenciados visualmente.
4. Si no hay turnos en el rango consultado, se muestra el mensaje "Sin turnos programados".
5. La información se actualiza automáticamente si se registra una nueva reserva o cancelación mientras la agenda está abierta.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Si | Solo requiere que existan turnos programados; no depende de otras historias en ejecución |
| Negociable | Si | El filtro de fechas por defecto y el estilo visual de diferenciación son ajustables |
| Valiosa | Si | Le permite al profesional organizar su jornada laboral |
| Estimable | Si | Criterios y datos definidos, sin ambigüedad de diseño |
| Pequeña | Si | Cubre un único caso de uso (visualizar turnos en un rango), resoluble en un sprint |
| Verificable | Si | Casos de prueba claros (con turnos, sin turnos, error de conexión) |

---

## HU-06 — [Gestionar servicios]

| Campo | Detalle |
|-------|---------|
| Historia | Como [administrador de una empresa], quiero [registrar, modificar y desactivar los servicios ofrecidos], para [mantener actualizada la oferta disponible para los clientes]. |
| Módulo | 2 |
| Requisitos relacionados | RF-07, RNF-05|
| Excepciones | Si se intenta guardar un servicio sin nombre, precio o duración, el sistema rechaza el guardado y señala los campos faltantes. Si el nombre está duplicado, informa el conflicto y no guarda el cambio. |
| Dependencias | Módulo 2 (la empresa debe existir). |
| Observaciones | Los servicios registrados aquí son un prerrequisito para HU-07 (asociar servicios a profesionales) y para HU-01 (los servicios activos son la base de la búsqueda de reserva) — ambas historias dependen de esta, no al revés. |
| Datos de entrada/salida | Entran: ID Empresa, nombre, descripción, precio, duración (minutos). Salen: ID Servicio, estado (activo/inactivo). |

### Criterios de aceptación

1. El administrador puede registrar un servicio con nombre, descripción, precio y duración en minutos.
2. El administrador puede modificar los datos de un servicio existente.
3. El administrador puede desactivar un servicio; uno desactivado no aparece en las búsquedas de clientes, pero no afecta turnos ya reservados con ese servicio.
4. El sistema no permite registrar dos servicios con el mismo nombre dentro de la misma empresa.
5. Todos los cambios quedan registrados con fecha y usuario que los realizó.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de que la empresa exista (Módulo 2), pero se implementa como unidad propia |
| Negociable | Si | Los campos exactos del formulario de alta son negociables sin cambiar el objetivo |
| Valiosa | Si | Mantiene actualizada la oferta que ven los clientes al reservar |
| Estimable | Si | Reglas de validación (nombre duplicado, campos obligatorios) bien definidas |
| Pequeña | Si | Cubre un único caso de uso (alta/edición/baja de servicios), resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (alta exitosa, nombre duplicado, campos faltantes) |

---

## HU-07 — [Gestionar Profesionales]

| Campo | Detalle |
|-------|---------|
| Historia | Como [administrador de una empresa], quiero [registrar y administrar los profesionales que prestan servicios en la agenda], para [asignarlos correctamente a las reservas de clientes]. |
| Módulo | 2 |
| Requisitos relacionados | RF-08, RNF-05 (registro de auditoría de operaciones críticas) |
| Excepciones | Ver criterio 5 (advertencia ante turnos futuros pendientes al desactivar). Si se intenta registrar sin servicio asociado, el sistema rechaza el alta y señala el campo faltante. |
| Dependencias | Módulo de Servicios (HU-06, alta de servicios) y Módulo de Agenda/Turnos. |
| Datos de entrada/salida | Entran: nombre, especialidad, lista de IDs de servicios. Salen: ID Profesional, estado (activo/inactivo). |

### Criterios de aceptación

1. Debe ser posible registrar un profesional con nombre, especialidad y servicios que ofrece.
2. Debe ser posible modificar los datos de un profesional ya registrado.
3. Debe ser posible asociar uno o más servicios existentes a cada profesional.
4. Debe ser posible desactivar un profesional; uno desactivado no puede recibir nuevos turnos, pero sus turnos ya confirmados se mantienen sin cambios.
5. Si se intenta desactivar un profesional con turnos futuros pendientes, el sistema debe advertir al administrador antes de confirmar la baja.
6. No se puede registrar un profesional sin al menos un servicio asociado.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de que existan servicios ya cargados (HU-06), pero se despliega como unidad propia|
| Negociable | Si | La forma de asociar servicios (selección múltiple, búsqueda, etc.) es negociable |
| Valiosa | Si | Permite asignar correctamente profesionales a las reservas de clientes |
| Estimable | Si | Reglas claras (mínimo un servicio asociado, advertencia ante turnos pendientes) |
| Pequeña | Si | Cubre un único caso de uso (alta/edición/baja de profesionales), resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (alta sin servicio, baja con turnos pendientes) |

---

## HU-08 — [Recibir recordatorios]

| Campo | Detalle |
|-------|---------|
| Historia | Como [cliente], quiero [recibir un recordatorio automático 24 horas antes de mi turno confirmado], para [evitar olvidos y asistir en el horario programado]. |
| Módulo | 5 |
| Requisitos relacionados | RF-19, RF-20, RNF-01 |
| Excepciones |Ver criterios 4 y 5 (supresión ante cancelación, recálculo ante reprogramación). Si falla el envío, queda registrado el error sin bloquear el resto del proceso.|
| Dependencias |Módulo 4 (estado del turno: HU-02, HU-03) y servicio externo de envío de emails. |
| Datos de entrada/salida | Entran: ID Turno, estado, fecha/hora del turno, email del cliente. Salen: registro de envío (exitoso/fallido) con fecha y hora. |

### Criterios de aceptación

1. El sistema debe generar un recordatorio automático 24hs antes de cada turno confirmado.
2. El recordatorio debe enviarse por email a la dirección registrada del cliente.
3. El mensaje debe incluir fecha, hora, servicio y nombre del profesional asignado.
4. No deben enviarse recordatorios de turnos cancelados.
5. Si un turno se reprograma, el recordatorio debe recalcularse según la nueva fecha/hora.
6. El envío (exitoso o fallido) debe quedar registrado en el sistema con fecha y hora.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende del estado del turno (HU-02, HU-03) y de un servicio externo de envío de emails |
| Negociable | Si | El contenido exacto del mensaje y el canal (email vs. otro) son negociables |
| Valiosa | Si | Reduce ausencias por olvido, beneficiando a cliente y profesional |
| Estimable | Si | Reglas de recálculo y supresión bien definidas, sin ambigüedad |
| Pequeña | Si | Cubre un único caso de uso (generación y envío de recordatorio), resoluble en un sprint |
| Verificable | Si | Casos de prueba claros (envío exitoso, turno cancelado, turno reprogramado, fallo de envío) |

---


