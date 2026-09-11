# Historias de Usuario Extendidas

---

## HU-01 — [Reservar Turno]

| Campo | Detalle |
|-------|---------|
| Historia | Como **cliente**, quiero **seleccionar una fecha, servicio y horario disponible**, para **reservar un turno de forma anticipada**. |

**Criterios de aceptación**

1. El sistema solo debe mostrar y permitir seleccionar horarios que se encuentren con estado "Disponible" para el profesional y servicio seleccionado.
2. Al iniciar el proceso de reserva, el sistema debe bloquear temporalmente el horario (por un tiempo límite de 5 minutos) para evitar que otro usuario lo tome en simultáneo.
3. El usuario debe poder visualizar el resumen del turno (fecha, hora, servicio, costo y profesional) antes de presionar el botón de confirmación final.
4. Una vez confirmado, el sistema debe registrar el turno con estado "Pendiente" o "Confirmado", liberar el bloqueo temporal y enviar un comprobante automático por correo electrónico.

---

## HU-02 — [Cancelar Turno]

| Campo | Detalle |
|-------|---------|
| Historia | Como **cliente**, quiero **cancelar un turno previamente reservado**, para **liberar el espacio en la agenda si no voy a poder asistir**. |

**Criterios de aceptación**

1. El sistema debe permitir al cliente acceder a su listado de turnos y seleccionar la opción de cancelar únicamente en aquellos que se encuentren vigentes.
2. El sistema debe validar que la cancelación se realice respetando el tiempo mínimo de anticipación configurado por el negocio (por ejemplo, 24 horas antes).
3. Si el plazo es menor al permitido, el sistema debe rechazar la cancelación y mostrar un mensaje explicativo.
4. Al completarse con éxito, el horario correspondiente debe volver a la grilla con estado "Disponible" de forma automática.

---

## HU-03 — [Reprogramar Turno]

| Campo | Detalle |
|-------|---------|
| Historia | Como **cliente**, quiero **modificar la fecha y hora de un turno ya agendado**, para **asistir en otro momento sin tener que pasar por todo el proceso de cancelación y nueva reserva**. |

**Criterios de aceptación**

1. El sistema debe ofrecer una opción de reprogramación asociada al turno activo, mostrando la grilla de disponibilidad actual del profesional.
2. El sistema debe validar que el nuevo horario elegido esté libre y cumpla con las reglas de negocio de anticipación.
3. Al confirmar el cambio, el sistema debe liberar el slot horario anterior y bloquear el nuevo slot de manera transaccional.
4. Se debe enviar una notificación actualizada al cliente y al profesional reflejando los nuevos datos de la cita.

---

## HU-04 — [Ver Historial de Turnos]

| Campo | Detalle |
|-------|---------|
| Historia | Como **cliente**, quiero **consultar el listado completo de mis turnos pasados y futuros**, para **llevar un control detallado de mis citas y verificar sus estados**. |

**Criterios de aceptación**

1. El sistema debe mostrar un panel o sección dedicada con los turnos ordenados cronológicamente.
2. Cada ítem del historial debe reflejar claramente el estado actual (Pendiente, Confirmado, Completado, Cancelado, Ausente).
3. El usuario debe contar con filtros básicos para alternar rápidamente entre "Próximos turnos" e "Historial anterior".

---

## HU-05 — [Configurar Disponibilidad de Horarios]

| Campo | Detalle |
|-------|---------|
| Historia | Como **profesional**, quiero **definir mis días, rangos horarios de atención y excepciones**, para **controlar en qué momentos exactos los clientes pueden reservar citas conmigo**. |

**Criterios de aceptación**

1. El sistema debe proveer una interfaz de administración donde el profesional pueda configurar su agenda semanal (horario de apertura, cierre y descansos intermedios).
2. Se debe permitir al profesional marcar fechas específicas como "No laborables" o "Vacaciones", bloqueando automáticamente las reservas en esas fechas.
3. Los cambios realizados en la disponibilidad deben aplicarse únicamente a los slots futuros y no deben afectar los turnos que ya fueron reservados con anterioridad.

---

## HU-06 — [Registrar Asistencia del Cliente]

| Campo | Detalle |
|-------|---------|
| Historia | Como **recepcionista o profesional**, quiero **marcar si el cliente asistió, llegó tarde o faltó a su turno**, para **mantener métricas reales y un registro de comportamiento en la plataforma**. |

**Criterios de aceptación**

1. El panel de gestión diaria debe mostrar los turnos agendados para la fecha actual junto con botones de acción rápida para control de asistencia.
2. El sistema debe permitir registrar estados como: "Presente", "Ausente" o "Cancelado por el negocio".
3. Al registrar una inasistencia (Ausente), el sistema debe sumar un contador en el perfil del cliente para auditoría interna del establecimiento.

---

## Parte B — Los caminos que no salen bien


**Historia elegida:** HU-01 — [Reservar Turno]

| Pregunta | Qué hace el sistema | Quién decide (analista / negocio / técnica) |
|----------|----------------------|-----------------------------------------------|
| ¿Qué pasa si dos clientes intentan reservar el mismo horario exactamente al mismo tiempo? | El sistema otorga el turno al primer request que procesa la base de datos mediante un bloqueo optimista o pesimista, y le muestra un error de "Horario ya no disponible" al segundo, obligándolo a elegir otro. | **Técnica** (define el manejo de concurrencia y bloqueos en la base de datos). |
| ¿Qué pasa si el profesional o servicio es dado de baja mientras el usuario está armando la reserva? | El sistema interrumpe el flujo al intentar confirmar, indicando mediante un alert que "El servicio/profesional ya no se encuentra disponible" y redirige al inicio de la selección. | **Negocio** (establece las reglas sobre qué pasa con los turnos en curso ante bajas administrativas). |
| ¿Qué pasa si el sistema guarda el turno correctamente pero falla el envío del correo de confirmación? | El turno queda guardado exitosamente de todas formas y se visualiza en el panel de "Mis Turnos". El sistema encola la notificación por correo para reintentar su envío en segundo plano. | **Técnica** (diseña la tolerancia a fallos en servicios de mensajería asíncrona sin comprometer la transacción principal). |
| ¿Qué pasa si el usuario aprieta "Confirmar Turno" dos veces muy rápido? | La interfaz deshabilita el botón de forma inmediata tras el primer clic. En el backend, se implementa una validación o token de idempotencia para descartar peticiones duplicadas idénticas en milisegundos. | **Técnica** (implementa la prevención de doble envío en frontend y servidor). |
| ¿Qué pasa si se cae la conexión justo después de confirmar, antes de ver la pantalla de éxito? | El turno se procesa y persiste correctamente en el servidor. Al restablecer la conexión, el usuario ingresa a "Mis Turnos" y comprueba que la reserva figura activa, o bien verifica la recepción del correo. | **Técnica** (manejo de estados resilientes ante fallas de red del cliente). |