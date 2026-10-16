# Slicing de Épica: Autogestión Integral de Reservas

**Épica:** Como usuario cliente, requiero gestionar integralmente el ciclo de vida de mis reservas mediante la plataforma, con el fin de asegurar mi atención y administrar mis turnos de manera autónoma[cite: 3].

## Parte A — Historias Verticales[cite: 3]

### HU-01 — Agendamiento de cita
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero agendar un turno especificando profesional y bloque temporal disponible, con el objetivo de garantizar mi espacio de atención. |

**Criterios de aceptación:**
1. El frontend debe desplegar una interfaz de calendario que permita seleccionar slots de tiempo libres, aplicando un bloqueo transaccional temporal sobre el registro para prevenir colisiones de concurrencia[cite: 3].
2. Tras la confirmación del payload, el backend debe persistir la entidad de reserva en la base de datos y retornar una vista de confirmación con el detalle de la transacción[cite: 3].
