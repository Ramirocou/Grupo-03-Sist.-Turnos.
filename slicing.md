# Slicing de Épica: Autogestión de Reservas

**Épica:** Como cliente, quiero gestionar el ciclo completo de mis reservas de forma online (agendar, pagar, modificar y cancelar), para asegurar mi atención y administrar mis tiempos.

## Parte A — Historias Verticales

### HU-01 — Reservar Turno

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero reservar un turno para un servicio seleccionando una empresa, un profesional, una fecha y un horario disponible, para asegurar mi atención en el momento que mejor se adapte a mis necesidades. |

### Criterios de aceptación

1. El sistema muestra los servicios y profesionales disponibles.
2. El cliente puede elegir fecha y hora libre en un calendario visual.

---

### HU-02 — Cancelar Turno

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero cancelar un turno previamente reservado, para liberar el horario cuando no pueda asistir. |

### Criterios de aceptación

1. Dado un turno reservado con más de 24 horas de anticipación, cuando el cliente lo cancela, el sistema registra la cancelación y gestiona la devolución de la seña abonada.
2. Dado un turno reservado con 24 horas o menos de anticipación, cuando el cliente lo cancela, el sistema registra la cancelación y aplica la retención de la seña.

---

### HU-03 — Reprogramar Turno

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero modificar la fecha u horario de un turno reservado, para adaptarlo a cambios en mi disponibilidad, sin necesidad de cancelar y crear una nueva reserva. |

### Criterios de aceptación

1. El cliente puede seleccionar un turno activo propio para reprogramar.
2. El sistema muestra únicamente horarios disponibles del mismo profesional/servicio.

---

### HU-04 — Recibir recordatorios

| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero recibir un recordatorio automático 24 horas antes de mi turno confirmado, para evitar olvidos y asistir en el horario programado. |

### Criterios de aceptación

1. El sistema debe generar un recordatorio automático 24hs antes de cada turno confirmado.
2. El recordatorio debe enviarse por email a la dirección registrada del cliente.

### HU-04 — Reprogramar Turno.
| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero modificar la fecha u horario de un turno reservado, para adaptarlo a cambios en mi disponibilidad, sin necesidad de cancelar y crear una nueva reserva. |

**Criterios de aceptación:**
1. El sistema actualiza la reserva con la nueva fecha/hora y libera el horario anterior en la misma operación..
2. Si el turno tiene seña pagada, el sistema la traslada al nuevo turno sin cobrar nuevamente ni reembolsar.

---

### HU-05 — Cancelar Turno.
| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero cancelar un turno previamente reservado, para liberar el horario cuando no pueda asistir.. |

**Criterios de aceptación:**
1. Dado un turno reservado con más de 24 horas de anticipación, cuando el cliente lo cancela, el sistema registra la cancelación y gestiona la devolución de la seña abonada..
2. Dado un turno reservado con 24 horas o menos de anticipación, cuando el cliente lo cancela, el sistema registra la cancelación y aplica la retención de la seña..

---

### HU-06 — Reembolsar seña.
| Campo | Detalle |
|-------|---------|
| Historia | Como cliente, quiero que se me reembolse la seña cuando corresponda, para no perder dinero por causas ajenas a mí. |

**Criterios de aceptación:**
1. El sistema reembolsa la seña automáticamente en tres casos: el cliente cancela con más de 24 horas de anticipación, la empresa cancela el turno, o el pago se acredita cuando el horario ya fue tomado por otra persona..
2. Cada reembolso queda registrado con fecha, monto y motivo.

## Parte B — Los caminos que no salen bien

**Historia de usuario base:** HU-01 — Reservar Turno.

| Pregunta | Qué hace el sistema | Quién decide (analista / negocio / técnica) y Justificación |
|----------|----------------------|-------------------------------------------------------------|
| ¿Qué pasa si dos personas eligen el mismo horario a la vez? | Se lo queda el primero que confirme y al segundo se le avisa en pantalla que ya no está libre. | **Técnica:** El equipo de desarrollo define el manejo de concurrencia y los bloqueos en la base de datos para garantizar que no se pisen ni se dupliquen los registros. |
| ¿Qué pasa si dan de baja al profesional mientras el cliente está haciendo la reserva? | El sistema frena la reserva antes de confirmar, tira una alerta diciendo que el profesional ya no está disponible y manda al usuario al inicio de la búsqueda. | **Negocio:** El dueño del producto define esta regla. Es una decisión comercial priorizar que no se vendan turnos de alguien que ya no trabaja, interrumpiendo el flujo del cliente para evitar un problema mayor después. |
| ¿Qué pasa si se guarda bien el turno pero falla el envío del correo electrónico de confirmación? | El turno queda guardado en estado "Reservado" perfectamente. El sistema encola el correo y vuelve a intentar mandarlo por atrás sin que el usuario se dé cuenta. | **Técnica:** Los desarrolladores arman la arquitectura con tolerancia a fallos, separando el guardado del turno de la API de correos para que la caída de un servicio externo no rompa la transacción principal. |
| ¿Qué pasa si el cliente hace doble clic muy rápido en el botón de confirmar reserva? | La interfaz web bloquea el botón después del primer clic. Si igual llega el segundo clic al servidor, este lo descarta porque detecta que es la misma petición repetida. | **Técnica:** Es responsabilidad de los programadores atajar estos envíos dobles tanto en la pantalla (frontend) como en el servidor (backend) para no generar turnos duplicados por error. |
| ¿Qué pasa si se corta el internet del cliente justo después de confirmar? | El servidor procesa el turno de todas formas. Cuando al cliente le vuelve el internet y entra a su perfil con su sesión iniciada, va a ver que su turno está ahí guardado. | **Técnica:** El equipo técnico arma el flujo para que el guardado no dependa de que al cliente le cargue la última pantalla, evitando que se pierdan datos confirmados por un microcorte. |
