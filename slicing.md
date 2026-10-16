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
