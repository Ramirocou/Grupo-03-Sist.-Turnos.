# Historias de usuario Extendidas

_Presentar al menos una historia de usuario representativa por módulo._
_Cada historia debe incluir formato clásico, criterios de aceptación y validación INVEST._

---

## HU-01 — Reservar Turno

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero reservar un turno para un servicio seleccionando una empresa, un profesional, una fecha y un horario disponible, para asegurar mi atención en el momento que mejor se adapte a mis necesidades. |
| Módulo | 3 |
| Requisitos relacionados | RF-09, RF-10, RF-11, RF-12, RF-13, RF-28 (seña), RNF-01 (respuesta < 3s), RNF-03 (HTTPS), RNF-04 (contraseñas cifradas, por el login previo) |
| Excepciones | Si dos personas eligen el mismo horario a la vez, se lo queda el primero que confirme y al segundo se le avisa en pantalla que ya no está libre. Si el profesional no tiene lugares en el mes, se muestra "Sin turnos este mes" Si el servicio requiere seña y el pago no se acredita en 10 minutos, se libera el horario y el turno no se confirma. Si el pago se acredita cuando el horario ya fue tomado por otro cliente, la seña se reembolsa automáticamente (HU-14). |
| Dependencias | Requiere sesión de cliente iniciada (Módulo 1) y disponibilidad cargada por el profesional (HU-04). Si el servicio requiere seña: HU-15 (configuración de la seña) y HU-12 (cobro de la seña). |
| Datos de entrada/salida | Entran: ID Empresa, ID Profesional, ID Servicio, Fecha, Hora, ID Cliente. Salen: ID Turno, estado ("Reservado" o "Pendiente de pago"), orden de envío de correo. |
| Observaciones | Historia núcleo del sistema; genera la entidad Turno y dispara la actualización de disponibilidad. Si el servicio requiere seña, el turno pasa primero por el estado "Pendiente de pago" y se confirma cuando el pago se acredita. |

### Criterios de aceptación

1. El sistema muestra los servicios y profesionales disponibles.
2. El cliente puede elegir fecha y hora libre en un calendario visual.
3. El sistema bloquea horarios ya ocupados.
4. Si el servicio no requiere seña, el sistema guarda la reserva con estado "Reservado" y un ID único.
5. El cliente recibe un correo electrónico de confirmación cuando el turno queda en estado "Reservado".
6. Si el servicio requiere seña, el sistema guarda el turno como "Pendiente de pago", retiene el horario 10 minutos y lo pasa a "Reservado" recién cuando el pago se acredita (HU-12). Durante esos 10 minutos el horario figura como no disponible para los demás clientes (RF-12).

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de que exista disponibilidad cargada (HU-04), sesión iniciada (HU-09) y, si hay seña, de HU-15 y HU-12, pero se implementa y despliega como unidad propia |
| Negociable | Si | El diseño del calendario visual y el canal de confirmación son detalles negociables con el equipo, sin cambiar el objetivo de negocio |
| Valiosa | Si | Es el núcleo del sistema: sin reserva no hay producto. Da valor inmediato al cliente y a la empresa |
| Estimable | Si | Criterios, datos y flujos alternos definidos; se puede estimar sin preguntas abiertas de diseño |
| Pequeña | Si | Cubre un único caso de uso (crear una reserva), resoluble en un sprint |
| Verificable | Si | Cada criterio es verificable con un caso de prueba concreto (reserva exitosa, duplicada, sin cupo, con seña pendiente de pago) |

---

## HU-02 — Cancelar Turno

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero cancelar un turno previamente reservado, para liberar el horario cuando no pueda asistir. |
| Módulo | 4 |
| Requisitos relacionados | RF-14, RF-16, RF-17, RF-29 (seña), RNF-01 |
| Excepciones | El turno se puede cancelar hasta el momento de su inicio. Si faltan más de 24 horas, la seña se reembolsa; si faltan 24 horas o menos, la cancelación se permite pero la seña se retiene. Antes de confirmar, el sistema informa al cliente si la seña se reembolsará o se perderá. Si el turno ya estaba cancelado, el sistema informa y actualiza la lista. |
| Dependencias | Tiene que existir un turno confirmado (HU-01). Si el turno tiene seña: HU-12 (pago) y HU-14 (reembolso). |
| Datos de entrada/salida | Entran: ID del Turno, ID del Cliente. Salen: nuevo estado "Cancelado", resultado de la seña (reembolsada, retenida o sin seña), orden de envío de correo. |
| Observaciones | El horario cancelado debe quedar libre en la base de datos al instante |

### Criterios de aceptación


### Criterios de aceptación

1. Dado un turno reservado con más de 24 horas de anticipación, cuando el cliente lo cancela, el sistema registra la cancelación y gestiona la devolución de la seña abonada.
2. Dado un turno reservado con 24 horas o menos de anticipación, cuando el cliente lo cancela, el sistema registra la cancelación y aplica la retención de la seña.
3. Dado un turno reservado, cuando la empresa lo cancela, el sistema gestiona la devolución de la seña independientemente del tiempo restante.
4. Cuando se cancela un turno, el sistema actualiza su estado, libera el horario y actualiza las notificaciones pendientes.
5. Si corresponde una devolución, el sistema conserva la referencia al pago original y registra el resultado de la operación.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Si | Depende de que exista un turno confirmado (HU-01),pero se puede implementar y desplegar como unidad propia |
| Negociable | Si | El canal de notificación y el texto del aviso de reembolso o retención son detalles ajustables sin cambiar el objetivo |
| Valiosa | Si |  Libera horarios y da flexibilidad al cliente, evitando ausencias sin aviso |
| Estimable | Si |  Criterios, excepción y datos definidos; no hay preguntas abiertas de diseño |
| Pequeña | Si | Cubre un único caso de uso (cancelar una reserva existente), resoluble en un sprint |
| Verificable | Si | Cada criterio tiene un caso de prueba concreto (cancelación con reembolso, cancelación tardía con seña retenida) |


---

## HU-03 — Reprogramar Turno

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero modificar la fecha u horario de un turno reservado, para adaptarlo a cambios en mi disponibilidad, sin necesidad de cancelar y crear una nueva reserva. |
| Módulo | 4 |
| Requisitos relacionados | RF-15, RF-16, RF-32 (seña), RNF-01 ( respuesta <3s) |
| Excepciones | Si el horario elegido es tomado por otro cliente antes de confirmar, el sistema avisa "Horario ya no disponible" y pide elegir otro sin perder el turno original. No se permite reprogramar si faltan menos de 24 horas para el turno (mismo límite que la cancelación); el sistema bloquea la acción y explica el motivo. Si el turno tiene seña pagada, esta se traslada al nuevo turno sin generar un nuevo cobro. |
| Dependencias | HU-01 (el turno debe existir), HU-04 (disponibilidad vigente del profesional), Módulo 5 (Notificaciones), HU-12 (seña pagada). |
| Datos de entrada/salida | Entran: ID Turno, nueva Fecha, nueva Hora. Salen: estado actualizado, fecha anterior (histórico), seña trasladada (si corresponde), orden de envío de correo. |
| Observaciones | La operación de reprogramar (liberar horario anterior + reservar el nuevo) debe ejecutarse en una única transacción, en menos de 2 segundos. |

### Criterios de aceptación

1. El cliente puede seleccionar un turno activo propio para reprogramar.
2. El sistema muestra únicamente horarios disponibles del mismo profesional/servicio.
3. El sistema actualiza la reserva con la nueva fecha/hora y libera el horario anterior en la misma operación.
4. El cliente recibe una confirmación por correo electrónico del nuevo horario.
5. El sistema conserva un registro histórico del cambio (fecha anterior y nueva).
6. Si el turno tiene seña pagada, el sistema la traslada al nuevo turno sin cobrar nuevamente ni reembolsar.

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

## HU-04 — Configurar Disponibilidad

| Campo | Detalle |
|-------|---------|
| Historia | Como profesional, quiero definir mis días y horarios disponibles para atención, para que los clientes solo puedan reservar turnos dentro de mi disponibilidad real. |
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

## HU-05 — Consultar agenda

| Campo | Detalle |
|-------|---------|
| Historia | Como profesional, quiero visualizar mis turnos programados en un rango de fechas, para organizar mi jornada laboral. |
| Módulo | 7 |
| Requisitos relacionados | RF-26, RF-31, RNF-01 (respuesta <3s) |
| Excepciones | Si falla la carga de datos (error de conexión), el sistema muestra un mensaje de error y permite reintentar sin perder el filtro de fecha aplicado. |
| Dependencias | HU-01 (deben existir turnos programados) y HU-13 (registro de asistencia desde la agenda). |
| Datos de entrada/salida | Entran: ID Profesional, rango de fechas (opcional). Salen: lista de turnos con fecha, hora, cliente, servicio y estado. |
| Observaciones | Desde cada turno ya iniciado, el profesional puede registrar la asistencia del cliente (HU-13); los estados "Atendido" y "Ausente" se ven en la agenda. |


### Criterios de aceptación

1. El sistema muestra los turnos del profesional para el día actual por defecto, con opción de filtrar por rango de fechas.
2. Cada turno visualizado indica fecha, hora, cliente, servicio y estado.
3. Los turnos cancelados se muestran diferenciados visualmente.
4. Si no hay turnos en el rango consultado, se muestra el mensaje "Sin turnos programados".
5. La información se actualiza automáticamente si se registra una nueva reserva o cancelación mientras la agenda está abierta.
6. El profesional puede abrir un turno ya iniciado y registrar la asistencia del cliente (HU-13).

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de HU-01 (deben existir turnos programados), pero se implementa y despliega como unidad propia |
| Negociable | Si | El filtro de fechas por defecto y el estilo visual de diferenciación son ajustables |
| Valiosa | Si | Le permite al profesional organizar su jornada laboral |
| Estimable | Si | Criterios y datos definidos, sin ambigüedad de diseño |
| Pequeña | Si | Cubre un único caso de uso (visualizar turnos en un rango), resoluble en un sprint |
| Verificable | Si | Casos de prueba claros (con turnos, sin turnos, error de conexión) |

---

## HU-06 — Gestionar servicios

| Campo | Detalle |
|-------|---------|
| Historia | Como administrador de una empresa, quiero registrar, modificar y desactivar los servicios ofrecidos, para mantener actualizada la oferta disponible para los clientes. |
| Módulo | 2 |
| Requisitos relacionados | RF-07, RNF-05 |
| Excepciones | Si se intenta guardar un servicio sin nombre, precio o duración, el sistema rechaza el guardado y señala los campos faltantes. Si el nombre está duplicado, informa el conflicto y no guarda el cambio. |
| Dependencias | Módulo 2 (la empresa debe existir). |
| Observaciones | Los servicios registrados aquí son un prerrequisito para HU-07 (asociar servicios a profesionales) y para HU-01 (los servicios activos son la base de la búsqueda de reserva) — ambas historias dependen de esta, no al revés. La configuración de la seña de cada servicio se hace en HU-15. |
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

## HU-07 — Gestionar Profesionales

| Campo | Detalle |
|-------|---------|
| Historia | Como administrador de una empresa, quiero registrar y administrar los profesionales que prestan servicios en la agenda, para asignarlos correctamente a las reservas de clientes. |
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

## HU-08 — Recibir recordatorios

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero recibir un recordatorio automático 24 horas antes de mi turno confirmado, para evitar olvidos y asistir en el horario programado. |
| Módulo | 5 |
| Requisitos relacionados | RF-19, RF-20, RNF-01 |
| Excepciones |Ver criterios 4 y 5 (supresión ante cancelación, recálculo ante reprogramación). Si falla el envío, queda registrado el error sin bloquear el resto del proceso.|
| Dependencias |Módulo 4 (estado del turno: HU-02, HU-03) y servicio externo de envío de emails. |
| Datos de entrada/salida | Entran: ID Turno, estado, fecha/hora del turno, email del cliente. Salen: registro de envío (exitoso/fallido) con fecha y hora. |

### Criterios de aceptación

1. El sistema debe generar un recordatorio automático 24hs antes de cada turno confirmado.
2. El recordatorio debe enviarse por email a la dirección registrada del cliente.
3. El mensaje debe incluir fecha, hora, servicio y nombre del profesional asignado.
4. No deben enviarse recordatorios de turnos cancelados ni de turnos en estado "Pendiente de pago".
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


## HU-09 — Registrarse e iniciar sesión

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero registrarme con mi correo y una contraseña e iniciar sesión, para acceder a la plataforma y gestionar mis turnos de forma personal y segura. |
| Módulo | 1 |
| Requisitos relacionados | RF-01, RF-02, RNF-01 (respuesta < 3s), RNF-03 (HTTPS), RNF-04 (contraseñas cifradas) |
| Excepciones | Si el correo ya está registrado, el sistema rechaza el alta e invita a iniciar sesión. Si las credenciales son incorrectas, el sistema muestra un mensaje genérico sin indicar cuál de los dos datos falló. Tras 5 intentos fallidos consecutivos, el acceso se bloquea 15 minutos. Si la sesión expira por inactividad, el sistema pide autenticarse nuevamente. |
| Dependencias | Ninguna (historia base). Es prerrequisito de HU-01, HU-02 y HU-03 (sesión de cliente). El alta de profesionales se hace desde HU-07; recuperar contraseña (RF-03) y editar perfil (RF-04) quedan fuera de esta historia. |
| Datos de entrada/salida | Entran: nombre, apellido, correo, contraseña (registro); correo, contraseña (inicio de sesión). Salen: ID Cliente, sesión activa, mensaje de error si corresponde. |
| Observaciones | Historia base del Módulo 1: los casos de uso CU-01, CU-02 y CU-03 la dan por cumplida en sus precondiciones ("sesión activa"). |

### Criterios de aceptación

1. El cliente puede registrarse con nombre, apellido, correo y contraseña; el correo debe ser único en el sistema.
2. La contraseña debe tener al menos 8 caracteres y se almacena cifrada, nunca en texto plano.
3. Con correo y contraseña válidos, el sistema inicia sesión y lleva al cliente al módulo de reservas.
4. Con credenciales inválidas, el sistema rechaza el acceso con un mensaje genérico y no revela cuál dato es incorrecto.
5. La sesión expira tras 30 minutos de inactividad y el sistema solicita autenticarse nuevamente.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Si | No depende de otras historias; son las demás las que dependen de esta |
| Negociable | Si | La política de contraseñas, el tiempo de expiración y la verificación del correo son ajustables sin cambiar el objetivo |
| Valiosa | Si | Sin cuentas no hay turnos asociados a un cliente; habilita todo el flujo de reservas |
| Estimable | Si | Datos, reglas de validación y excepciones definidos |
| Pequeña | Si | Recuperar contraseña (RF-03) y editar perfil (RF-04) se separan en otras historias; resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (registro exitoso, correo duplicado, credenciales inválidas, sesión expirada) |

---

## HU-10 — Consultar registro de auditoría

| Campo | Detalle |
|-------|---------|
| Historia | Como administrador de una empresa, quiero consultar el registro de auditoría de las operaciones críticas realizadas en mi empresa, para saber quién hizo cada cambio y cuándo, y poder aclarar errores o reclamos. |
| Módulo | 2 |
| Requisitos relacionados | RNF-05 (registro de auditoría de operaciones críticas), RF-07, RF-08, RNF-01 (respuesta < 3s), RNF-03 (HTTPS) |
| Excepciones | Si no hay registros para los filtros elegidos, se muestra "Sin registros de auditoría". Si un usuario sin rol de administrador intenta acceder, el sistema deniega el acceso y no muestra datos. Si falla la carga, el sistema muestra un error y permite reintentar sin perder los filtros aplicados. |
| Dependencias | HU-06 y HU-07 (generan los registros de auditoría) y HU-09 (sesión y rol de administrador). |
| Datos de entrada/salida | Entran: ID Empresa, rango de fechas, usuario (opcional), tipo de operación (opcional). Salen: lista de registros con fecha y hora, usuario, tipo de operación, entidad afectada (servicio o profesional) y detalle del cambio. |
| Observaciones | En esta versión se auditan solo las operaciones de servicios y profesionales (HU-06 y HU-07). Sumar cancelaciones, reprogramaciones y cambios de disponibilidad queda para una iteración posterior. |

### Criterios de aceptación

1. El administrador ve un listado de registros ordenado del más reciente al más antiguo, con fecha y hora, usuario, operación y entidad afectada.
2. El administrador puede filtrar los registros por rango de fechas, por usuario y por tipo de operación.
3. Los registros son de solo lectura: el sistema no permite editarlos ni eliminarlos.
4. Cada administrador solo accede a los registros de su propia empresa.
5. Si no hay registros para los filtros aplicados, se muestra el mensaje "Sin registros de auditoría".

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de que HU-06 y HU-07 generen los registros y de HU-09 para el rol, pero se despliega como unidad propia |
| Negociable | Si | Los filtros disponibles, la paginación y el formato de presentación son negociables |
| Valiosa | Si | Da trazabilidad sobre los cambios del catálogo y del personal; hoy los registros se generan pero nadie puede verlos |
| Estimable | Si | Alcance acotado a dos tipos de operación, con datos de entrada/salida definidos |
| Pequeña | Si | Cubre un único caso de uso (consultar y filtrar registros), resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (consulta con filtros, sin resultados, acceso de usuario sin rol, empresa ajena) |

---

## HU-11 — Generar reportes de gestión

| Campo | Detalle |
|-------|---------|
| Historia | Como administrador de una empresa, quiero generar reportes de gestión sobre los turnos de mi empresa, para conocer la actividad del negocio y tomar decisiones sobre agendas y recursos. |
| Módulo | 6 |
| Requisitos relacionados | RF-22, RF-23 (modificado: suma señas retenidas), RF-24 |
| Excepciones | Sin definir. |
| Dependencias | HU-01, HU-02, HU-03 y HU-04 (turnos y disponibilidad), HU-13 (asistencia) y HU-12 (señas cobradas), a confirmar. |
| Datos de entrada/salida | Sin definir (período, filtros y formato de salida pendientes). |
| Observaciones | **Historia en refinamiento: todavía no cumple el DoR.** Preguntas abiertas: (1) cómo se calcula el indicador de ocupación; (2) si "turno atendido" equivale al estado "Atendido" que registra HU-13; (3) en qué formato se entrega el reporte (pantalla, PDF o Excel); (4) si RF-22, RF-23 y RF-24 van en una sola historia o se dividen en tres; (5) las estadísticas de cancelaciones (RF-23) pasan a incluir las señas retenidas; falta definir si también se informan los montos cobrados y reembolsados. |

### Criterios de aceptación

1. El administrador puede elegir un rango de fechas y generar el reporte de turnos atendidos de su empresa.
2. El reporte muestra la cantidad y el porcentaje de turnos cancelados en el período elegido.
3. El reporte muestra un indicador de ocupación por profesional (fórmula a definir).

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Consume datos que generan HU-01, HU-02, HU-03, HU-04, HU-12 y HU-13, pero ninguna historia depende de esta |
| Negociable | Si | El formato, los filtros y la presentación de los reportes son negociables |
| Valiosa | Si | Le permite a la empresa medir ausentismo, cancelaciones y uso de su agenda |
| Estimable | No | Hay preguntas abiertas de diseño (cálculo de ocupación, formato de salida, montos de señas) |
| Pequeña | No | Agrupa tres requisitos (RF-22, RF-23, RF-24); conviene dividirla en un reporte por historia |
| Verificable | Parcial | Los criterios 1 y 2 se pueden probar; el 3 no, hasta definir la fórmula de ocupación |

---

## HU-12 — Pagar seña al reservar

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero pagar online la seña del turno que estoy reservando, para confirmar mi reserva. |
| Módulo | 8 |
| Requisitos relacionados | RF-12, RF-28, RF-30, RNF-01 (respuesta < 3s), RNF-03 (HTTPS), RNF-11 (no almacenar datos de tarjeta), RNF-12 (sin cobros duplicados) |
| Excepciones | Si el pago es rechazado, el sistema lo informa y permite reintentar mientras dure la retención del horario. Si el pago no se acredita en 10 minutos, se libera el horario y el turno no se confirma. Si el pago se acredita cuando otra persona ya tomó el horario, la seña se reembolsa automáticamente (HU-14) y se avisa al cliente. Si el cliente reintenta o hace doble clic, no se genera un segundo cobro. Si la pasarela de pagos no responde, el sistema informa el error y el turno no se confirma. |
| Dependencias | HU-01 (flujo de reserva), HU-15 (servicio configurado con seña), HU-09 (sesión de cliente) y la pasarela de pagos (sistema externo, nuevo stakeholder). |
| Datos de entrada/salida | Entran: ID Turno, monto de la seña, medio de pago (los datos de tarjeta se ingresan en la pasarela). Salen: ID Pago, estado del pago (pendiente, acreditado, rechazado o reembolsado), estado del turno, comprobante por correo. |
| Observaciones | El monto de la seña lo define cada empresa por servicio en HU-15 (monto fijo o porcentaje del precio). El sistema guarda solo el ID de transacción de la pasarela, el monto y el estado. |

### Criterios de aceptación

1. Antes de pagar, el cliente ve el monto de la seña y las condiciones de reembolso (más de 24 horas antes: se reembolsa; 24 horas o menos: se retiene).
2. Al iniciar el pago, el turno queda en estado "Pendiente de pago" y el horario se retiene durante 10 minutos.
3. Cuando el pago se acredita, el turno pasa a "Reservado" y el cliente recibe la confirmación con el comprobante por correo.
4. Si el pago no se acredita dentro de los 10 minutos, el horario vuelve a estar disponible y el turno no se confirma.
5. El sistema no almacena datos de tarjeta; guarda solo el ID de transacción, el monto y el estado del pago.
6. Un mismo turno no puede generar dos cobros: reintentar o repetir la operación no duplica el pago.
7. Si el pago se acredita cuando el horario ya fue tomado por otra persona, el sistema inicia el reembolso automático (HU-14).
8. Durante los 10 minutos de retención, los demás clientes ven el horario como no disponible y no pueden seleccionarlo, igual que un horario ya reservado (RF-12).

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de HU-01, HU-15 y HU-09, y de la pasarela externa, pero se implementa como unidad propia |
| Negociable | Si | La pasarela elegida, los medios de pago y los textos del flujo son negociables |
| Valiosa | Si | Reduce el ausentismo: el cliente se compromete con el turno y la empresa asegura un ingreso |
| Estimable | Parcial | El flujo está definido, pero falta elegir la pasarela de pagos |
| Pequeña | Si | Cubre un único caso de uso (cobrar la seña de una reserva); el reembolso se separa en HU-14 |
| Verificable | Si | Casos de prueba concretos (pago exitoso, rechazado, vencido a los 10 minutos, doble clic, pago tardío) |

---

## HU-13 — Registrar asistencia del cliente

| Campo | Detalle |
|-------|---------|
| Historia | Como profesional, quiero registrar si el cliente asistió o no a su turno, para definir qué pasa con la seña y mantener el historial de atención. |
| Módulo | 7 |
| Requisitos relacionados | RF-31, RNF-01 (respuesta < 3s), RNF-05 (registro de auditoría de operaciones críticas) |
| Excepciones | No se puede registrar la asistencia de un turno que todavía no comenzó ni de uno cancelado. Si la asistencia ya fue registrada, el profesional puede corregirla y el cambio queda registrado con fecha y usuario. Si el profesional no registra nada dentro de las 24 horas posteriores al fin del turno, el sistema lo pasa a "Atendido" y la seña queda aplicada al servicio. |
| Dependencias | HU-01 (turno existente), HU-05 (agenda desde donde se registra) y HU-12 (seña pagada). |
| Datos de entrada/salida | Entran: ID Turno, ID Profesional, asistencia (asistió o ausente). Salen: estado del turno ("Atendido" o "Ausente"), resultado de la seña (aplicada o retenida), fecha y hora del registro. |
| Observaciones | Reglas a confirmar con el cliente: (1) si el cliente asistió, la seña queda aplicada al pago del servicio; si estuvo ausente, se retiene; (2) solo el profesional del turno marca al ausente, hasta 24 horas después del fin del turno; (3) si no marca nada, se considera "Atendido", para que el cliente no pierda la seña sin que nadie lo confirme. |

### Criterios de aceptación

1. El profesional puede marcar un turno propio como "Atendido" o "Ausente" desde su agenda, desde la hora de inicio del turno y hasta 24 horas después de su hora de fin.
2. Solo se puede registrar la asistencia de turnos propios en estado "Reservado".
3. Si el profesional marca "Atendido", la seña queda aplicada al servicio; si marca "Ausente", la seña se retiene.
4. El registro guarda fecha, hora y profesional que lo realizó.
5. El nuevo estado se refleja en la agenda (HU-05) y queda disponible para los reportes (HU-11).
6. Si el profesional no registra la asistencia dentro de las 24 horas posteriores al fin del turno, el sistema lo pasa a "Atendido" y la seña queda aplicada al servicio.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Se usa desde la agenda (HU-05) y se apoya en HU-01 y HU-12, pero se despliega como unidad propia |
| Negociable | Si | La forma de marcar (botones, menú) y los estados adicionales son negociables |
| Valiosa | Si | Define el destino de la seña y habilita el reporte de turnos atendidos |
| Estimable | Si | Estados, reglas y datos definidos; la regla de la seña se confirma sin cambiar el esfuerzo |
| Pequeña | Si | Cubre un único caso de uso (registrar asistencia de un turno), resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (asistió, ausente, turno futuro, turno cancelado, corrección, sin registro dentro del plazo) |

---

## HU-14 — Reembolsar seña

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero que se me reembolse la seña cuando corresponda, para no perder dinero por causas ajenas a mí. |
| Módulo | 8 |
| Requisitos relacionados | RF-29, RF-30, RNF-03 (HTTPS), RNF-05 (registro de auditoría de operaciones críticas), RNF-12 (sin cobros duplicados) |
| Excepciones | Si la pasarela rechaza el reembolso, este queda en estado "Reembolso pendiente" y el sistema lo reintenta. Si un pago ya fue reembolsado, el sistema no genera un segundo reembolso. |
| Dependencias | HU-12 (pago de la seña), HU-02 (cancelación del cliente) y la pasarela de pagos. La cancelación por parte de la empresa no está cubierta por ninguna historia actual. |
| Datos de entrada/salida | Entran: ID Pago, motivo del reembolso. Salen: estado del reembolso (pendiente o reembolsado), monto, fecha, correo al cliente. |
| Observaciones | La política de reintentos (cada cuánto y cuántas veces) está pendiente de definir. |

### Criterios de aceptación

1. El sistema reembolsa la seña automáticamente en tres casos: el cliente cancela con más de 24 horas de anticipación, la empresa cancela el turno, o el pago se acredita cuando el horario ya fue tomado por otra persona.
2. Si el cliente cancela con 24 horas o menos de anticipación, o no asiste, el sistema no reembolsa la seña.
3. El cliente recibe un correo con el monto y el motivo del reembolso.
4. Si la pasarela rechaza el reembolso, este queda como "Reembolso pendiente" y el sistema lo reintenta hasta que se concrete.
5. Un mismo pago no puede reembolsarse más de una vez.
6. Cada reembolso queda registrado con fecha, monto y motivo.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de HU-12 y de la pasarela externa, pero se despliega como unidad propia |
| Negociable | Si | El texto del correo y el formato del comprobante son negociables |
| Valiosa | Si | Protege al cliente y evita reclamos por cobros indebidos |
| Estimable | Parcial | Falta definir la política de reintentos ante reembolsos rechazados |
| Pequeña | Si | Cubre un único caso de uso (devolver una seña), resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (cancelación con anticipación, cancelación tardía, pago tardío, reembolso rechazado) |

---

## HU-15 — Configurar seña por servicio

| Campo | Detalle |
|-------|---------|
| Historia | Como administrador de una empresa, quiero definir qué servicios exigen seña y de cuánto, para asegurar los turnos que más me importan según cada servicio. |
| Módulo | 2 |
| Requisitos relacionados | RF-27, RF-07 (modificado: el servicio incluye la seña), RNF-01 (respuesta < 3s), RNF-05 (registro de auditoría de operaciones críticas) |
| Excepciones | Si el servicio exige seña y no se indica el valor, el sistema rechaza el guardado y señala el campo faltante. Si el porcentaje es menor o igual a 0 o mayor a 100, o el monto fijo es mayor al precio del servicio, el sistema rechaza el guardado. |
| Dependencias | HU-06 (el servicio debe existir) y HU-09 (sesión del administrador). Es prerrequisito de HU-12 y de la rama con seña de HU-01. |
| Datos de entrada/salida | Entran: ID Servicio, exige seña (sí/no), tipo de seña (monto fijo o porcentaje), valor. Salen: configuración guardada y monto de seña resultante. |
| Observaciones | Cada empresa decide el monto o el porcentaje por servicio; el sistema no define un valor por defecto. |

### Criterios de aceptación

1. El administrador puede indicar, por servicio, si exige seña o no.
2. Si la exige, el administrador elige entre un monto fijo o un porcentaje del precio, y el sistema muestra la seña resultante.
3. El sistema rechaza valores inválidos: vacío, menor o igual a 0, porcentaje mayor a 100 o monto mayor al precio.
4. Un servicio que no exige seña se reserva como hasta ahora, sin pasar por el pago.
5. Cambiar la configuración no afecta a los turnos ya reservados, y el cambio queda registrado con fecha y usuario.

### Validación INVEST

| Criterio | ¿Se cumple? | Observación |
|----------|-------------|-------------|
| Independiente | Parcial | Depende de que el servicio exista (HU-06) y de la sesión (HU-09), pero se despliega como unidad propia |
| Negociable | Si | La forma de cargar el valor (monto o porcentaje, vista previa) es negociable |
| Valiosa | Si | Cada empresa decide qué turnos asegura con seña; sin esto no hay cobro |
| Estimable | Si | Reglas de validación y datos definidos |
| Pequeña | Si | Cubre un único caso de uso (configurar la seña de un servicio), resoluble en un sprint |
| Verificable | Si | Casos de prueba concretos (con seña fija, con porcentaje, valor inválido, servicio sin seña) |

---
