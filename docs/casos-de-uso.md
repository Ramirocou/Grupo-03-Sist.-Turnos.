# Casos de uso

## Diagrama general

Hay dos diagramas en `diagramas/` (visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/)):

- `diagramas/casos-de-uso.puml`: cadena de trazabilidad RF / RNF → HU → CU de cada caso de uso.
- `diagramas/casos-de-uso-actores.puml`: diagrama de casos de uso con actores y relaciones `extend`.

### Actores

| Actor | Tipo | Casos de uso en los que participa |
|-------|------|-----------------------------------|
| Cliente | Principal | CU-01, CU-02, CU-03, CU-09 |
| Profesional | Principal | CU-04, CU-05, CU-12 |
| Administrador de empresa | Principal | CU-06, CU-07, CU-11 |
| Sistema (proceso automático) | Principal | CU-08, CU-10 |
| Servicio de notificaciones | Secundario (externo) | CU-01, CU-02, CU-03, CU-08, CU-09, CU-10 |
| Pasarela de pagos | Secundario (externo) | CU-09, CU-10 |

### Relaciones principales

| Relación | Condición |
|----------|-----------|
| CU-09 Pagar seña `extend` CU-01 Reservar Turno | Solo si el servicio exige seña. |
| CU-10 Reembolsar seña `extend` CU-02 Cancelar Turno | Solo si el turno tenía seña y faltaban más de 24 horas (o cancela la empresa). |
| CU-10 Reembolsar seña `extend` CU-09 Pagar seña | Solo si el pago se acredita cuando el horario ya fue tomado por otra persona. |
| CU-11 Configurar seña `extend` CU-06 Gestionar Servicios | Solo para servicios que exigen seña. |
| CU-12 Registrar asistencia `extend` CU-05 Consultar Agenda | Solo sobre turnos ya iniciados. |

Todas son `extend` y no `include` porque la funcionalidad adicional ocurre solo en una condición; el caso base es completo sin ella.

---

## CU-01 — Reservar Turno

| Campo | Detalle |
|-------|---------|
| Identificador | CU-01 |
| Nombre | Reservar Turno |
| Descripción | El cliente, una vez autenticado, selecciona una empresa, un servicio, un profesional y un horario disponible en un calendario visual para reservar un turno. Si el servicio no exige seña, el turno queda reservado y se confirma por correo. Si exige seña, el turno queda "Pendiente de pago" con el horario retenido 10 minutos y se confirma recién cuando el pago se acredita (CU-09). |
| Actores | Principal: Cliente / Secundarios: Sistema de Notificaciones; Pasarela de pagos (solo si el servicio exige seña) |
| Precondiciones | 1. El cliente posee una cuenta registrada y sesión activa (Módulo 1).<br>2. La empresa tiene servicios activos y al menos un profesional con disponibilidad cargada (CU-04).<br>3. Existen horarios disponibles para el servicio/profesional seleccionado. |
| Postcondiciones | Éxito sin seña: 1. El turno queda registrado con estado "Reservado" y un ID único.<br>2. El horario deja de estar disponible para otros clientes.<br>3. Se envía la confirmación por correo.<br><br>Éxito con seña: 1. El turno queda "Pendiente de pago" con un ID único y el horario retenido 10 minutos (no disponible para otros clientes).<br>2. Al acreditarse el pago (CU-09), el turno pasa a "Reservado" y se envía la confirmación con el comprobante.<br><br>Fallo: si el pago no se acredita en 10 minutos, el turno pasa a "Vencido", el horario se libera y el turno no se confirma. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente accede al módulo de reservas. | El sistema valida la sesión activa y muestra las empresas/servicios disponibles, indicando cuáles exigen seña. |
| 2 | El cliente selecciona una empresa y un servicio. | El sistema muestra los profesionales asociados. |
| 2.1 | Si no existen profesionales disponibles. | El sistema informa la situación y solicita una nueva selección. |
| 3 | El cliente selecciona un profesional. | El sistema muestra el calendario visual con los horarios disponibles. Los horarios ocupados y los retenidos por un pago pendiente figuran como no disponibles. |
| 3.1 | Si no existen horarios disponibles en el mes. | El sistema muestra "Sin turnos este mes" y ofrece cambiar de profesional. |
| 4 | El cliente selecciona fecha y horario. | El sistema valida la disponibilidad y bloquea el horario temporalmente. |
| 5 | El cliente revisa el resumen y confirma la reserva. | El sistema muestra el resumen del turno (si el servicio exige seña, también el monto y las condiciones de reembolso) y registra el turno con un ID único. |
| 5.1 | Si el servicio no exige seña. | El sistema guarda el turno como "Reservado", deja el horario ocupado y envía la confirmación por correo. |
| 5.2 | Si el servicio exige seña. | El sistema guarda el turno como "Pendiente de pago", retiene el horario 10 minutos y deriva al cliente a CU-09 (Pagar seña). Cuando el pago se acredita, pasa el turno a "Reservado" y envía la confirmación con el comprobante. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | Dos clientes eligen el mismo horario simultáneamente. | El sistema confirma al primero que finaliza y al segundo le informa en pantalla que el horario ya no está disponible, sin perder su selección de empresa/servicio. |
| E2 | La sesión del cliente expira durante el proceso. | El sistema cancela la operación y solicita autenticación nuevamente. |
| E3 | Falla el envío de la notificación. | El sistema confirma la reserva igualmente y registra el error de envío. |
| E4 | El pago de la seña no se acredita dentro de los 10 minutos. | El sistema pasa el turno a "Vencido", libera el horario y avisa que la reserva no se confirmó. |
| E5 | El pago se acredita cuando el horario ya fue tomado por otro cliente. | El sistema no confirma el turno, inicia el reembolso automático de la seña (CU-10) y avisa al cliente. |
| E6 | La pasarela de pagos no responde. | El sistema informa el error y el turno no se confirma; el horario se libera al vencer la retención de 10 minutos. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Cada paso debe completarse en menos de 3 segundos; la confirmación final (paso 5) en menos de 2 segundos. El cliente nuevo debe poder completar la reserva en 4 pasos o menos y en menos de 2 minutos (RNF-06). |
| Frecuencia | Se estima una media de 200 reservas diarias. |
| Importancia | Vital |
| Urgencia | Inmediata |

---

## CU-02 — Cancelar Turno

| Campo | Detalle |
|-------|---------|
| Identificador | CU-02 |
| Nombre | Cancelar Turno |
| Descripción | El cliente solicita la cancelación de un turno propio previamente reservado, en cualquier momento hasta su inicio. El sistema libera el horario, notifica la cancelación y, si el turno tenía seña, la devuelve cuando faltan más de 24 horas o la retiene cuando faltan 24 horas o menos. |
| Actores | Principal: Cliente / Secundarios: Sistema de Notificaciones; Pasarela de pagos (solo si hay una seña para devolver) |
| Precondiciones | 1. Cliente autenticado.<br>2. Existe un turno con estado "Reservado" asociado al cliente, que todavía no comenzó. |
| Postcondiciones | Éxito: 1. El turno queda en estado "Cancelado".<br>2. El horario vuelve a estar disponible de inmediato.<br>3. Si el turno tenía seña: queda reembolsada (más de 24 horas) o retenida (24 horas o menos), y se conserva la referencia al pago original.<br>4. Se registra y notifica la cancelación. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente accede a "Mis Turnos". | El sistema muestra los turnos activos del cliente. |
| 2 | El cliente selecciona el turno a cancelar. | El sistema muestra el detalle de la reserva. Si tiene seña, informa antes de confirmar si se reembolsará (faltan más de 24 horas) o se perderá (faltan 24 horas o menos). |
| 3 | El cliente confirma la cancelación. | El sistema actualiza el estado a "Cancelado", libera el horario, actualiza las notificaciones pendientes y envía la notificación de cancelación. |
| 3.1 | Si el turno tenía seña y faltaban más de 24 horas. | El sistema inicia el reembolso de la seña (CU-10) y lo informa en la notificación. |
| 3.2 | Si el turno tenía seña y faltaban 24 horas o menos. | El sistema permite la cancelación, retiene la seña y registra la retención. |
| 3.3 | Si el turno no tenía seña. | El sistema completa la cancelación sin ninguna operación de pago. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El turno ya comenzó. | El sistema bloquea la cancelación y muestra un mensaje explicando que ya no se puede cancelar. |
| E2 | El turno ya fue cancelado previamente. | El sistema informa la situación y actualiza la lista de turnos activos. |
| E3 | La sesión expira durante el proceso. | El sistema cancela la operación y solicita autenticación. |
| E4 | Falla el envío de la notificación. | El sistema completa la cancelación igualmente y registra el error. |
| E5 | La pasarela rechaza el reembolso. | La cancelación se completa igualmente; el reembolso queda como "Reembolso pendiente" y se reintenta (CU-10). |

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
| Descripción | El cliente modifica la fecha u horario de un turno ya reservado, seleccionando una nueva disponibilidad del mismo profesional/servicio, en una única operación transaccional. Si el turno tiene seña pagada, esta pasa al turno nuevo sin cobrar de nuevo. |
| Actores | Principal: Cliente / Secundario: Sistema de Notificaciones |
| Precondiciones | 1. Cliente autenticado.<br>2. Existe un turno con estado "Reservado" asociado al cliente.<br>3. Faltan más de 24hs para el turno original. |
| Postcondiciones | Éxito: 1. El turno queda actualizado con la nueva fecha/hora.<br>2. El horario anterior se libera automáticamente.<br>3. Queda un registro histórico del cambio (fecha anterior y nueva).<br>4. Si tenía seña pagada, esta queda aplicada al turno nuevo, sin un nuevo cobro ni reembolso.<br>5. Las notificaciones pendientes (recordatorio) se recalculan con la nueva fecha/hora. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente accede a "Mis Turnos". | El sistema muestra los turnos activos. |
| 2 | El cliente selecciona el turno a reprogramar. | El sistema muestra el detalle y valida que falten más de 24hs. |
| 3 | El cliente solicita reprogramación. | El sistema muestra el calendario con horarios disponibles del mismo profesional/servicio. |
| 4 | El cliente selecciona un nuevo horario. | El sistema valida la disponibilidad del nuevo horario. |
| 5 | El cliente confirma el cambio. | El sistema ejecuta en una sola transacción: libera el horario anterior, reserva el nuevo, guarda el histórico, traslada la seña pagada (si corresponde) sin generar un nuevo cobro y envía notificación de modificación. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El nuevo horario elegido ya fue ocupado por otro cliente. | El sistema avisa "Horario ya no disponible" y solicita seleccionar otro horario, sin perder el turno original. |
| E2 | Faltan menos de 24hs para el turno. | El sistema bloquea la reprogramación y explica el límite de tiempo. |
| E3 | La sesión expira durante el proceso. | El sistema solicita autenticación nuevamente, sin aplicar cambios parciales. |
| E4 | Falla el envío de la notificación. | El sistema registra el error y conserva la modificación ya aplicada. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | La transacción completa (liberar + reservar + trasladar la seña) debe ejecutarse en menos de 2 segundos. |
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
| Precondiciones | 1. El profesional está registrado y activo dentro de una empresa (CU-07).<br>2. El profesional tiene sesión activa. |
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
| Importancia | Vital (prerrequisito de CU-01 y CU-03) |
| Urgencia | Hay presión |

---

## CU-05 — Consultar Agenda

| Campo | Detalle |
|-------|---------|
| Identificador | CU-05 |
| Nombre | Consultar Agenda |
| Descripción | El profesional visualiza sus turnos asignados dentro de un rango de fechas, para organizar su jornada laboral. Desde la agenda puede registrar la asistencia de los turnos ya iniciados (CU-12). |
| Actores | Principal: Profesional / Secundario: — |
| Precondiciones | 1. El profesional está autenticado.<br>2. El profesional tiene o no turnos asignados (la agenda puede estar vacía). |
| Postcondiciones | Éxito: 1. Se muestra la lista de turnos del rango consultado, actualizada, con los turnos cancelados diferenciados y los estados "Atendido" y "Ausente" visibles. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El profesional accede a "Mi Agenda". | El sistema muestra los turnos del día actual por defecto. |
| 2 | El profesional selecciona un rango de fechas (opcional). | El sistema filtra y muestra los turnos con fecha, hora, cliente, servicio y estado. Los turnos cancelados se muestran diferenciados visualmente. |
| 3 | El profesional consulta el detalle de un turno. | El sistema muestra la información completa del turno seleccionado. |
| 4 | El profesional abre un turno ya iniciado para registrar la asistencia del cliente. | El sistema deriva a CU-12 (Registrar asistencia) y, al volver, muestra el nuevo estado en la agenda. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | No hay turnos en el rango consultado. | El sistema muestra el mensaje "Sin turnos programados". |
| E2 | Falla la carga de datos (error de conexión). | El sistema muestra un mensaje de error y permite reintentar sin perder el filtro aplicado. |
| E3 | Se registra una nueva reserva o cancelación mientras la agenda está abierta. | El sistema actualiza la lista automáticamente. |

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
| Descripción | El administrador de una empresa registra, modifica o desactiva los servicios que la empresa ofrece a sus clientes. La configuración de la seña de cada servicio se realiza en CU-11. |
| Actores | Principal: Administrador de empresa / Secundario: — |
| Precondiciones | 1. El administrador está autenticado y tiene una empresa registrada. |
| Postcondiciones | Éxito: 1. El catálogo de servicios queda actualizado (alta, modificación o baja).<br>2. Un servicio desactivado deja de aparecer en las búsquedas de clientes, sin afectar los turnos ya reservados.<br>3. El cambio queda registrado en la auditoría con fecha y usuario. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El administrador accede a "Gestión de servicios". | El sistema muestra el listado de servicios de la empresa, con su condición de seña. |
| 2 | El administrador da de alta un servicio (nombre, descripción, precio, duración) o edita/desactiva uno existente. | El sistema valida los datos ingresados. |
| 3 | El administrador confirma la operación. | El sistema guarda el cambio, actualiza el catálogo visible para los clientes y registra la auditoría. |
| 4 | El administrador desea que el servicio exija seña. | El sistema deriva a CU-11 (Configurar seña por servicio). |

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
| Precondiciones | 1. El administrador está autenticado.<br>2. Existe al menos un servicio activo para poder asociarlo al profesional (CU-06). |
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
| Importancia | Vital (prerrequisito de CU-01 y CU-04) |
| Urgencia | Normal |

---

## CU-08 — Recibir Recordatorios

| Campo | Detalle |
|-------|---------|
| Identificador | CU-08 |
| Nombre | Recibir Recordatorios |
| Descripción | El sistema, de forma automática, genera y envía un recordatorio al cliente 24 horas antes de cada turno confirmado. |
| Actores | Principal: Sistema (proceso automático / job programado) / Secundario: Cliente; Servicio de Notificaciones (email) |
| Precondiciones | 1. Existe un turno con estado "Reservado" cuya fecha/hora está a 24hs de distancia. Los turnos "Pendiente de pago" no generan recordatorio. |
| Postcondiciones | Éxito: 1. El cliente recibe el recordatorio por correo electrónico.<br>2. El envío (exitoso o fallido) queda registrado con fecha y hora. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El job programado se ejecuta periódicamente. | El sistema identifica los turnos confirmados ("Reservado") a 24hs de su fecha/hora. |
| 2 | El sistema arma el mensaje con fecha, hora, servicio y profesional. | El servicio de notificaciones envía el correo al cliente. |
| 3 | — | El sistema registra el envío exitoso con fecha y hora. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El turno fue cancelado antes de las 24hs. | El sistema no genera ni envía el recordatorio. |
| E2 | El turno fue reprogramado. | El sistema recalcula el recordatorio según la nueva fecha/hora. |
| E3 | Falla el envío del correo. | El sistema registra el error con fecha y hora, sin bloquear el resto del proceso. |
| E4 | El turno está en estado "Pendiente de pago". | El sistema no genera ni envía el recordatorio. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | El job debe dispararse de forma confiable dentro de la ventana de 24hs ± 5 minutos. |
| Frecuencia | Depende del volumen de turnos confirmados; acorde a las ~200 reservas diarias de CU-01. |
| Importancia | Importante |
| Urgencia | Normal |

---

## CU-09 — Pagar Seña

| Campo | Detalle |
|-------|---------|
| Identificador | CU-09 |
| Nombre | Pagar Seña |
| Descripción | El cliente paga online la seña del turno que está reservando, a través de la pasarela de pagos externa. El pago confirma la reserva. El sistema nunca guarda datos de tarjeta. |
| Actores | Principal: Cliente / Secundarios: Pasarela de pagos; Sistema de Notificaciones |
| Precondiciones | 1. El cliente tiene sesión activa.<br>2. El turno está en estado "Pendiente de pago" con el horario retenido (CU-01, paso 5.2).<br>3. El servicio tiene la seña configurada (CU-11). |
| Postcondiciones | Éxito: 1. El pago queda registrado con ID de transacción de la pasarela, monto y estado "Aprobado".<br>2. El turno pasa a "Reservado".<br>3. El cliente recibe la confirmación con el comprobante por correo.<br><br>Fallo: si el pago no se acredita dentro de los 10 minutos, el turno pasa a "Vencido" y el horario se libera. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El cliente revisa el monto de la seña y las condiciones de reembolso, y elige pagar. | El sistema muestra el monto, las condiciones (más de 24 horas: se reembolsa; 24 horas o menos: se retiene) y el tiempo restante de retención del horario. |
| 2 | El cliente inicia el pago. | El sistema registra la operación como "Pendiente" con una clave de idempotencia y abre el formulario de la pasarela de pagos. |
| 3 | El cliente completa el pago en la pasarela (los datos de tarjeta se ingresan allí). | La pasarela procesa el pago y comunica el resultado al sistema. |
| 4 | — | El sistema verifica la respuesta de la pasarela, registra ID de transacción, monto y estado, pasa el turno a "Reservado" y envía la confirmación con el comprobante. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | La pasarela rechaza el pago. | El sistema informa el rechazo y permite reintentar mientras dure la retención del horario. |
| E2 | El pago no se acredita dentro de los 10 minutos. | El sistema pasa el turno a "Vencido", libera el horario y el turno no se confirma. |
| E3 | El pago se acredita cuando el horario ya fue tomado por otra persona. | El sistema no confirma el turno, inicia el reembolso automático (CU-10) y avisa al cliente. |
| E4 | El cliente hace doble clic o reintenta la operación. | El sistema no genera un segundo cobro: la clave de idempotencia identifica la misma operación. |
| E5 | La pasarela de pagos no responde. | El sistema informa el error y el turno no se confirma. |
| E6 | Falla el envío del comprobante por correo. | El sistema mantiene el turno como "Reservado" y registra el error de envío. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Cada paso debe completarse en menos de 3 segundos (RNF-01). El horario se retiene 10 minutos. |
| Frecuencia | A definir: ocurre solo en servicios con seña; como máximo, las ~200 reservas diarias de CU-01. |
| Importancia | Vital |
| Urgencia | Inmediata |

---

## CU-10 — Reembolsar Seña

| Campo | Detalle |
|-------|---------|
| Identificador | CU-10 |
| Nombre | Reembolsar Seña |
| Descripción | El sistema devuelve automáticamente la seña pagada cuando corresponde: el cliente cancela con más de 24 horas de anticipación, la empresa cancela el turno, o el pago se acredita cuando el horario ya fue tomado por otra persona. Si el cliente cancela con 24 horas o menos, o no se presenta, la seña no se devuelve. |
| Actores | Principal: Sistema (proceso automático) / Secundarios: Pasarela de pagos; Cliente (destinatario del reembolso); Sistema de Notificaciones |
| Precondiciones | 1. Existe un pago de seña acreditado, sin reembolso previo.<br>2. Se produjo uno de los eventos que habilitan el reembolso (CU-02 con más de 24 horas, cancelación por la empresa, o CU-09 E3). |
| Postcondiciones | Éxito: 1. El reembolso queda registrado como una operación separada, vinculada al pago original, con fecha, monto y motivo.<br>2. El cliente recibe un correo con el monto y el motivo.<br><br>Fallo parcial: si la pasarela lo rechaza, queda en "Reembolso pendiente" y el sistema lo reintenta hasta que se concrete. |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | Un evento habilita el reembolso. | El sistema verifica que el pago original esté acreditado, que no haya sido reembolsado y que el monto no supere lo acreditado. |
| 2 | — | El sistema solicita la devolución a la pasarela de pagos, con una clave de idempotencia, y registra la operación referenciando el pago original. |
| 3 | La pasarela confirma la devolución. | El sistema registra el estado, la fecha, el monto y el motivo, y envía el correo al cliente. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | La pasarela rechaza el reembolso. | El sistema lo deja en "Reembolso pendiente" y lo reintenta. La política de reintentos (frecuencia y cantidad) está pendiente de definir. |
| E2 | El pago ya fue reembolsado. | El sistema no genera un segundo reembolso. |
| E3 | El cliente canceló con 24 horas o menos, o estuvo ausente. | No se ejecuta este caso de uso: la seña queda retenida y se registra. |
| E4 | Falla el envío del correo. | El reembolso queda igualmente registrado y se guarda el error de envío. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Cada paso debe completarse en menos de 3 segundos (RNF-01). |
| Frecuencia | Menor que las cancelaciones diarias de CU-02 (~30), ya que solo aplican las cancelaciones con seña y más de 24 horas, las canceladas por la empresa y los pagos tardíos. |
| Importancia | Importante |
| Urgencia | Hay presión |

---

## CU-11 — Configurar Seña por Servicio

| Campo | Detalle |
|-------|---------|
| Identificador | CU-11 |
| Nombre | Configurar Seña por Servicio |
| Descripción | El administrador de una empresa define, para cada servicio, si exige seña y de cuánto: un monto fijo o un porcentaje del precio. El sistema no define un valor por defecto. |
| Actores | Principal: Administrador de empresa / Secundario: — |
| Precondiciones | 1. El administrador está autenticado.<br>2. El servicio existe (CU-06). |
| Postcondiciones | Éxito: 1. La configuración queda guardada (exige seña, tipo y valor) y el sistema muestra la seña resultante.<br>2. Los turnos ya reservados no se ven afectados.<br>3. El cambio queda registrado en la auditoría con fecha y usuario. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El administrador selecciona un servicio en "Gestión de servicios". | El sistema muestra la configuración de seña actual. |
| 2 | El administrador indica si el servicio exige seña. Si la exige, elige monto fijo o porcentaje e ingresa el valor. | El sistema valida el valor y muestra la seña resultante. |
| 3 | El administrador guarda la configuración. | El sistema guarda los cambios y registra la auditoría. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El servicio exige seña y no se indica el valor. | El sistema rechaza el guardado y señala el campo faltante. |
| E2 | El valor es menor o igual a 0, el porcentaje es mayor a 100 o el monto fijo supera el precio. | El sistema rechaza el guardado y señala el error. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Menos de 3 segundos (RNF-01). |
| Frecuencia | Baja (cuando la empresa define o ajusta sus condiciones). |
| Importancia | Vital (prerrequisito de CU-09 y de la rama con seña de CU-01) |
| Urgencia | Hay presión |

---

## CU-12 — Registrar Asistencia del Cliente

| Campo | Detalle |
|-------|---------|
| Identificador | CU-12 |
| Nombre | Registrar Asistencia del Cliente |
| Descripción | El profesional registra desde su agenda si el cliente asistió ("Atendido") o estuvo ausente ("Ausente") a un turno propio. El registro define el destino de la seña: si asistió, queda aplicada al servicio; si estuvo ausente, se retiene. |
| Actores | Principal: Profesional / Secundario: — |
| Precondiciones | 1. El profesional está autenticado.<br>2. El turno es propio, está en estado "Reservado" y ya comenzó.<br>3. No pasaron más de 24 horas desde el fin del turno. |
| Postcondiciones | Éxito: 1. El turno queda "Atendido" o "Ausente".<br>2. La seña queda aplicada (Atendido) o retenida (Ausente).<br>3. El registro guarda fecha, hora y profesional.<br>4. El nuevo estado se refleja en la agenda y queda disponible para los reportes. / Fallo: N/A |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | El profesional abre un turno ya iniciado desde su agenda (CU-05). | El sistema muestra el turno con las acciones "Atendido" y "Ausente" habilitadas. |
| 2 | El profesional marca "Atendido" o "Ausente". | El sistema valida el turno y el plazo, actualiza el estado, define el destino de la seña, registra fecha, hora y profesional, y actualiza la agenda. |
| 2.1 | Si la asistencia ya había sido registrada. | El sistema permite corregirla y registra el cambio con fecha y usuario. |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | El turno todavía no comenzó, o está cancelado. | El sistema bloquea el registro y explica el motivo. |
| E2 | El profesional no registra nada dentro de las 24 horas posteriores al fin del turno. | El sistema pasa el turno a "Atendido" y la seña queda aplicada al servicio. |
| E3 | El turno no es del profesional o no está en estado "Reservado". | El sistema no permite el registro. |

| Campo | Detalle |
|-------|---------|
| Rendimiento | Menos de 3 segundos (RNF-01). |
| Frecuencia | Una vez por turno realizado; acorde a las ~200 reservas diarias de CU-01. |
| Importancia | Importante |
| Urgencia | Normal |