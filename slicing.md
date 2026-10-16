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
| Historia | Como usuario cliente, requiero procesar el pago de la seña vinculada a mi reserva, para materializar la confirmación definitiva del turno en el sistema. |

**Criterios de aceptación:**
1. Una vez que el webhook de la pasarela de pagos notifique la acreditación exitosa, el sistema debe transicionar el estado del turno a "Reservado" de manera automatizada.
2. En caso de expirar el Time-To-Live (TTL) de 10 minutos sin confirmación de pago, el motor de reservas debe liberar el slot bloqueado, restaurando su disponibilidad pública.

---

### HU-03 — Despacho de comprobantes y recordatorios
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero recibir confirmaciones y recordatorios asíncronos vía correo electrónico, para disponer de respaldo documental y mitigar el riesgo de ausentismo. |

**Criterios de aceptación:**
1. El servicio de mensajería debe encolar y despachar un correo con el comprobante transaccional en el instante en que el turno adquiera el estado "Reservado".
2. El sistema debe ejecutar un proceso batch que dispare un recordatorio al cliente exactamente 24 horas antes del timestamp de inicio de la cita.

---

### HU-04 — Modificación de timestamp de reserva
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero alterar el timestamp de un turno previamente consolidado, para ajustar la reserva ante variaciones en mi agenda personal sin requerir un nuevo alta. |

**Criterios de aceptación:**
1. El motor de reservas debe ejecutar una transacción atómica que consolide el nuevo slot horario y libere simultáneamente el registro anterior en la base de datos.
2. Ante turnos que posean un pago de seña asociado, la lógica de negocio debe transferir el saldo acreditado al nuevo registro sin detonar eventos de cobro adicionales.

---

### HU-05 — Anulación de entidad de reserva
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero anular una reserva confirmada, para liberar el slot de agenda de la compañía ante la imposibilidad de asistencia. |

**Criterios de aceptación:**
1. Al procesar el request de anulación, el backend debe mutar el estado de la entidad a "Cancelado" y purgar el bloqueo sobre el slot horario instantáneamente.
2. Previo al commit de anulación, la interfaz de usuario debe renderizar un modal de advertencia detallando si la lógica de negocio aplicará un reembolso o la retención de los fondos de seña.

---

### HU-06 — Restitución automatizada de fondos
| Campo | Detalle |
|-------|---------|
| Historia | Como usuario cliente, requiero la restitución automática de los fondos de la seña al cancelar dentro del margen permitido, para salvaguardar mi capital. |

**Criterios de aceptación:**
1. El sistema debe invocar la API de la pasarela para efectuar un rollback financiero si la diferencia entre el timestamp actual y el de la cita supera las 24 horas.
2. Toda solicitud de reembolso exitosa debe generar un registro de auditoría interno con metadatos de timestamp y monto, disparando consecuentemente una notificación al usuario.

## Parte B — Los caminos que no salen bien

**Historia de usuario base:** HU-01 — Agendamiento de cita.

| Pregunta | Qué hace el sistema | Quién decide (analista / negocio / técnica) y Justificación |
|----------|----------------------|-------------------------------------------------------------|
| ¿Qué pasa si dos clientes intentan reservar exactamente al mismo tiempo? | El sistema le da el turno al primero que la base de datos llega a procesar. Al segundo le muestra un aviso de que el horario ya se ocupó y le pide elegir otro. | **Técnica:** El equipo de desarrollo es quien configura los bloqueos en la base de datos para asegurar que no se pisen los registros al guardarlos. |
| ¿Qué pasa si dan de baja al profesional mientras el cliente está haciendo la reserva? | El sistema frena la reserva antes de confirmar, tira una alerta diciendo que el profesional ya no está disponible y manda al usuario al inicio. | **Negocio:** El dueño del producto define esta regla. Es una decisión comercial priorizar que no se vendan turnos de alguien que ya no trabaja, aunque le corte el proceso al cliente. |
| ¿Qué pasa si se guarda bien el turno pero se cae el servicio de correos? | El turno queda guardado perfectamente. El sistema deja el mail en una cola de espera y lo vuelve a intentar mandar por atrás sin que el usuario se dé cuenta. | **Técnica:** Los desarrolladores arman la arquitectura así para que la caída de un servicio externo (como el mail) no te tire abajo el guardado del turno. |
| ¿Qué pasa si el cliente le da doble clic muy rápido al botón de confirmar? | La interfaz web bloquea el botón después del primer clic. Si igual llega el segundo clic al servidor, este lo descarta porque detecta que es la misma petición repetida. | **Técnica:** Es responsabilidad de los programadores atajar estos envíos dobles tanto en la pantalla como en el servidor para no crear turnos duplicados por error. |
| ¿Qué pasa si se corta el internet del cliente justo después de confirmar? | El servidor procesa el turno de todas formas. Cuando al cliente le vuelve el internet y entra a su perfil, va a ver que su turno está ahí guardado. | **Técnica:** El equipo técnico arma el flujo para que el guardado no dependa de que al cliente le cargue la última pantalla, evitando que se pierdan datos por un microcorte. |
