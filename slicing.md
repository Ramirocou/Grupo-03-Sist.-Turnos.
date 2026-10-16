# Slicing de Épica: Autogestión Integral de Reservas

**Épica:** Como usuario cliente, requiero gestionar integralmente el ciclo de vida de mis reservas mediante la plataforma, con el fin de asegurar mi atención y administrar mis turnos de manera autónoma.
## Parte A — Historias Verticales

### HU-01 — Agendamiento de cita
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero agendar un turno especificando profesional y bloque temporal disponible, con el objetivo de garantizar mi espacio de atención. |

**Criterios de aceptación:**
1. El frontend debe desplegar una interfaz de calendario que permita seleccionar slots de tiempo libres, aplicando un bloqueo transaccional temporal sobre el registro para prevenir colisiones de concurrencia.
2. Tras la confirmación del payload, el backend debe persistir la entidad de reserva en la base de datos y retornar una vista de confirmación con el detalle de la transacción.

---

### HU-02 — Liquidación de seña transaccional
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero procesar el pago de la seña vinculada a mi reserva, para materializar la confirmación definitiva del turno en el sistema |

**Criterios de aceptación:**
1. Una vez que el webhook de la pasarela de pagos notifique la acreditación exitosa, el sistema debe transicionar el estado del turno a "Reservado" de manera automatizada.
2. En caso de expirar el Time-To-Live (TTL) de 10 minutos sin confirmación de pago, el motor de reservas debe liberar el slot bloqueado, restaurando su disponibilidad pública.
