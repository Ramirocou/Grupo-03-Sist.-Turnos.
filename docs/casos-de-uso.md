# Casos de uso

## Diagrama general

_Incluir el código PlantUML en `diagramas/casos-de-uso.puml`._
_Visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/)._

_Describir brevemente los actores identificados y las relaciones principales (include, extend)._

---

## CU-00 — [Nombre]

| Campo | Detalle |
|-------|---------|
| Identificador | CU-02 |
| Nombre | |
| Descripción | |
| Actores | Principal: / Secundario: |
| Precondiciones | |
| Postcondiciones | Éxito: / Fallo: |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | | |
| 2 | | |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | | |

| Campo | Detalle |
|-------|---------|
| Rendimiento | |
| Frecuencia | |
| Importancia | |
| Urgencia | |

---

## CU-01 — Reservar Turno

| Campo | Detalle |
|-------|---------|
| Identificador | CU-01 |
| Nombre | Reservar Turno |
| Descripción | El cliente, una vez autenticado, selecciona una empresa, un servicio, un profesional y un horario disponible en un calendario visual para reservar un turno. El sistema bloquea el horario, registra la reserva y genera una confirmación por correo. |
| Actores | Principal: Cliente / Secundario: Sistema de Notificaciones |
| Precondiciones | 1. El cliente posee una cuenta registrada y sesión activa (Módulo 1).<br>2. La empresa tiene servicios activos y al menos un profesional con disponibilidad cargada (CU-004).<br>3. Existen horarios disponibles para el servicio/profesional seleccionado. |
| Postcondiciones | Éxito: 1. El turno queda registrado con estado "Reservado" y un ID único.<br>2. El horario reservado deja de estar disponible para otros clientes.<br>3. Se envía una notificación de confirmación por correo. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente accede al módulo de reservas. | El sistema valida la sesión activa y muestra las empresas/servicios disponibles. |
| 2 | El cliente selecciona una empresa y un servicio. | El sistema muestra los profesionales asociados. |
| 2.1 | Si no existen profesionales disponibles. | El sistema informa la situación y solicita una nueva selección. |
| 3 | El cliente selecciona un profesional. | El sistema muestra el calendario visual con los horarios disponibles. |
| 3.1 | Si no existen horarios disponibles en el mes. | El sistema muestra "Sin turnos este mes" y ofrece cambiar de profesional. |
| 4 | El cliente selecciona fecha y horario. | El sistema valida la disponibilidad y bloquea el horario temporalmente. |
| 5 | El cliente confirma la reserva. | El sistema registra el turno con ID único, estado "Reservado", y envía la confirmación por correo. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Dos clientes eligen el mismo horario simultáneamente. | El sistema confirma al primero que finaliza y al segundo le informa en pantalla que el horario ya no está disponible, sin perder su selección de empresa/servicio. |
| E2 | La sesión del cliente expira durante el proceso. | El sistema cancela la operación y solicita autenticación nuevamente. |
| E3 | Falla el envío de la notificación. | El sistema confirma la reserva igualmente y registra el error de envío. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Cada paso debe completarse en menos de 3 segundos; la confirmación final (paso 5) en menos de 2 segundos. |
| Frecuencia | Se estima una media de 200 reservas diarias. |
| Importancia | Vital |
| Urgencia | Inmediata |

---

## CU-02 — Cancelar Turno

| Campo | Detalle |
|-------|---------|
| Identificador | CU-02 |
| Nombre | Cancelar Turno |
| Descripción | El cliente solicita la cancelación de un turno propio previamente reservado. El sistema valida el plazo mínimo de 24hs, libera el horario y notifica la cancelación. |
| Actores | Principal: Cliente / Secundario: Sistema de Notificaciones |
| Precondiciones | 1. Cliente autenticado.<br>2. Existe un turno con estado "Reservado" asociado al cliente. |
| Postcondiciones | Éxito: 1. El turno queda en estado "Cancelado".<br>2. El horario vuelve a estar disponible de inmediato.<br>3. Se registra y notifica la cancelación. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente accede a "Mis Turnos". | El sistema muestra los turnos activos del cliente. |
| 2 | El cliente selecciona el turno a cancelar. | El sistema muestra el detalle de la reserva y valida que falten más de 24hs. |
| 3 | El cliente confirma la cancelación. | El sistema actualiza el estado a "Cancelado", libera el horario y envía la notificación de cancelación. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Faltan menos de 24hs para el turno. | El sistema bloquea la cancelación y muestra un mensaje explicando el límite de tiempo. |
| E2 | El turno ya fue cancelado previamente. | El sistema informa la situación y actualiza la lista de turnos activos. |
| E3 | La sesión expira durante el proceso. | El sistema cancela la operación y solicita autenticación. |
| E4 | Falla el envío de la notificación. | El sistema completa la cancelación igualmente y registra el error. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Máximo 2 segundos por paso; el horario debe liberarse en la base de datos al instante. |
| Frecuencia | Aproximadamente 30 cancelaciones diarias. |
| Importancia | Importante |
| Urgencia | Hay presión |

---

## CU-03 — Reprogramar Turno

| Campo | Detalle |
|-------|---------|
| Identificador | CU-03 |
| Nombre | Reprogramar Turno |
| Descripción | El cliente modifica la fecha u horario de un turno ya reservado, seleccionando una nueva disponibilidad del mismo profesional/servicio, en una única operación transaccional. |
| Actores | Principal: Cliente / Secundario: Sistema de Notificaciones |
| Precondiciones | 1. Cliente autenticado.<br>2. Existe un turno con estado "Reservado" asociado al cliente.<br>3. Faltan más de 24hs para el turno original. |
| Postcondiciones | Éxito: 1. El turno queda actualizado con la nueva fecha/hora.<br>2. El horario anterior se libera automáticamente.<br>3. Queda un registro histórico del cambio (fecha anterior y nueva). / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente accede a "Mis Turnos". | El sistema muestra los turnos activos. |
| 2 | El cliente selecciona el turno a reprogramar. | El sistema muestra el detalle y valida que falten más de 24hs. |
| 3 | El cliente solicita reprogramación. | El sistema muestra el calendario con horarios disponibles del mismo profesional/servicio. |
| 4 | El cliente selecciona un nuevo horario. | El sistema valida la disponibilidad del nuevo horario. |
| 5 | El cliente confirma el cambio. | El sistema ejecuta en una sola transacción: libera el horario anterior, reserva el nuevo, guarda el histórico y envía notificación de modificación. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El nuevo horario elegido ya fue ocupado por otro cliente. | El sistema solicita seleccionar otro horario, sin perder el turno original. |
| E2 | Faltan menos de 24hs para el turno. | El sistema bloquea la reprogramación y explica el límite de tiempo. |
| E3 | La sesión expira durante el proceso. | El sistema solicita autenticación nuevamente, sin aplicar cambios parciales. |
| E4 | Falla el envío de la notificación. | El sistema registra el error y conserva la modificación ya aplicada. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | La transacción completa (liberar + reservar) debe ejecutarse en menos de 2 segundos. |
| Frecuencia | Aproximadamente 20 reprogramaciones diarias. |
| Importancia | Importante |
| Urgencia | Hay presión |

---

## CU-04 — Configurar Disponibilidad

| Campo | Detalle |
|-------|---------|
| Identificador | CU-04 |
| Nombre | Configurar Disponibilidad |
| Descripción | El profesional define o modifica sus días y franjas horarias de atención dentro de la empresa a la que pertenece, para que el sistema solo ofrezca esos horarios en las búsquedas de los clientes. |
| Actores | Principal: Profesional / Secundario: — |
| Precondiciones | 1. El profesional está registrado y activo dentro de una empresa (CU-007).<br>2. El profesional tiene sesión activa. |
| Postcondiciones | Éxito: 1. La disponibilidad configurada queda almacenada por día y franja horaria.<br>2. Los nuevos horarios se reflejan en las búsquedas de reserva de los clientes en menos de 5 segundos. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El profesional accede a "Mi disponibilidad". | El sistema muestra las franjas ya configuradas, si existen. |
| 2 | El profesional agrega o edita una franja (día, hora inicio, hora fin). | El sistema valida que no se superponga con otra franja del mismo día. |
| 3 | El profesional guarda los cambios. | El sistema almacena la disponibilidad y la propaga a las búsquedas de clientes. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | La franja ingresada se superpone con otra ya cargada. | El sistema rechaza el guardado y marca el conflicto. |
| E2 | El profesional intenta eliminar una franja con turnos ya confirmados. | El sistema muestra una advertencia y solicita confirmación explícita, sin cancelar los turnos existentes. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | La propagación de cambios hacia las búsquedas de clientes debe completarse en menos de 5 segundos. |
| Frecuencia | Se estima que cada profesional actualiza su disponibilidad algunas veces por semana. |
| Importancia | Vital (prerrequisito de CU-001 y CU-003) |
| Urgencia | Hay presión |

---

## CU-05 — Consultar Agenda

| Campo | Detalle |
|-------|---------|
| Identificador | CU-05 |
| Nombre | Consultar Agenda |
| Descripción | El profesional visualiza sus turnos asignados dentro de un rango de fechas, para organizar su jornada laboral. |
| Actores | Principal: Profesional / Secundario: — |
| Precondiciones | 1. El profesional está autenticado.<br>2. El profesional tiene o no turnos asignados (la agenda puede estar vacía). |
| Postcondiciones | Éxito: 1. Se muestra la lista de turnos del rango consultado, actualizada. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El profesional accede a "Mi Agenda". | El sistema muestra los turnos del día actual por defecto. |
| 2 | El profesional selecciona un rango de fechas (opcional). | El sistema filtra y muestra los turnos con fecha, hora, cliente, servicio y estado. |
| 3 | El profesional consulta el detalle de un turno. | El sistema muestra la información completa del turno seleccionado. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | No hay turnos en el rango consultado. | El sistema muestra el mensaje "Sin turnos programados". |
| E2 | Falla la carga de datos (error de conexión). | El sistema muestra un mensaje de error y permite reintentar sin perder el filtro aplicado. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | La consulta debe responder en menos de 3 segundos, incluso con alto volumen histórico de turnos. |
| Frecuencia | Se estima que cada profesional consulta su agenda varias veces al día. |
| Importancia | Importante |
| Urgencia | Normal |

---

## CU-06 — Gestionar Servicios

| Campo | Detalle |
|-------|---------|
| Identificador | CU-06 |
| Nombre | Gestionar Servicios |
| Descripción | El administrador de una empresa registra, modifica o desactiva los servicios que la empresa ofrece a sus clientes. |
| Actores | Principal: Administrador de empresa / Secundario: — |
| Precondiciones | 1. El administrador está autenticado y tiene una empresa registrada. |
| Postcondiciones | Éxito: 1. El catálogo de servicios queda actualizado (alta, modificación o baja).<br>2. El cambio queda registrado en la auditoría con fecha y usuario. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El administrador accede a "Gestión de servicios". | El sistema muestra el listado de servicios de la empresa. |
| 2 | El administrador da de alta un servicio (nombre, descripción, precio, duración) o edita/desactiva uno existente. | El sistema valida los datos ingresados. |
| 3 | El administrador confirma la operación. | El sistema guarda el cambio, actualiza el catálogo visible para los clientes y registra la auditoría. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Faltan campos obligatorios (nombre, precio o duración). | El sistema rechaza el guardado y señala los campos faltantes. |
| E2 | El nombre del servicio ya existe en la misma empresa. | El sistema informa el conflicto y no guarda el cambio. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | — |
| Frecuencia | Actualización de catálogo de baja frecuencia (semanal o menor). |
| Importancia | Importante |
| Urgencia | Normal |

---

## CU-07 — Gestionar Profesionales

| Campo | Detalle |
|-------|---------|
| Identificador | CU-07 |
| Nombre | Gestionar Profesionales |
| Descripción | El administrador de una empresa registra, modifica, asocia servicios y desactiva a los profesionales que prestan servicios en la agenda de la empresa. |
| Actores | Principal: Administrador de empresa / Secundario: — |
| Precondiciones | 1. El administrador está autenticado.<br>2. Existe al menos un servicio activo para poder asociarlo al profesional (CU-006). |
| Postcondiciones | Éxito: 1. El profesional queda registrado, modificado o desactivado según la operación.<br>2. Los cambios se reflejan en las reservas futuras. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El administrador accede a "Gestión de profesionales". | El sistema muestra el listado de profesionales de la empresa. |
| 2 | El administrador da de alta un profesional (nombre, especialidad, servicios asociados) o edita uno existente. | El sistema valida que tenga al menos un servicio asociado. |
| 3 | El administrador confirma la operación. | El sistema guarda el profesional y lo habilita para recibir turnos. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Se intenta registrar un profesional sin ningún servicio asociado. | El sistema rechaza el alta y señala el campo faltante. |
| E2 | Se intenta desactivar un profesional con turnos futuros pendientes. | El sistema muestra una advertencia y solicita confirmación explícita; los turnos ya confirmados no se cancelan. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | — |
| Frecuencia | Baja frecuencia (altas/bajas ocasionales de personal). |
| Importancia | Vital (prerrequisito de CU-001 y CU-004) |
| Urgencia | Normal |

---

## CU-08 — Recibir Recordatorios

| Campo | Detalle |
|-------|---------|
| Identificador | CU-08 |
| Nombre | Recibir Recordatorios |
| Descripción | El sistema, de forma automática, genera y envía un recordatorio al cliente 24 horas antes de cada turno confirmado. |
| Actores | Principal: Sistema (proceso automático / job programado) / Secundario: Cliente; Servicio de Notificaciones (email) |
| Precondiciones | 1. Existe un turno con estado "Reservado" cuya fecha/hora está a 24hs de distancia. |
| Postcondiciones | Éxito: 1. El cliente recibe el recordatorio por correo electrónico.<br>2. El envío (exitoso o fallido) queda registrado con fecha y hora. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El job programado se ejecuta periódicamente. | El sistema identifica los turnos confirmados a 24hs de su fecha/hora. |
| 2 | El sistema arma el mensaje con fecha, hora, servicio y profesional. | El servicio de notificaciones envía el correo al cliente. |
| 3 | — | El sistema registra el envío exitoso con fecha y hora. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El turno fue cancelado antes de las 24hs. | El sistema no genera ni envía el recordatorio. |
| E2 | El turno fue reprogramado. | El sistema recalcula el recordatorio según la nueva fecha/hora. |
| E3 | Falla el envío del correo. | El sistema registra el error con fecha y hora, sin bloquear el resto del proceso. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | El job debe dispararse de forma confiable dentro de la ventana de 24hs ± 5 minutos. |
| Frecuencia | Depende del volumen de turnos confirmados; acorde a las ~200 reservas diarias de CU-001. |
| Importancia | Importante |
| Urgencia | Normal |

---