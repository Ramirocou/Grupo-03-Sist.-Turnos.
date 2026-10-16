# Requisitos del sistema

## Requisitos funcionales

### Módulo 1 — Gestión de usuarios.

| ID | Requisito |
|----|-----------|
| RF-01 |El sistema debe permitir registrar usuarios clientes.|
| RF-02 |El sistema debe permitir iniciar sesión mediante correo y contraseña.|
| RF-03 |El sistema debe permitir recuperar contraseñas.|
| RF-04 |El usuario debe poder editar su perfil personal.|

### Módulo 2 — Gestión de empresas

| ID | Requisito |
|----|-----------|
| RF-05 |Las empresas deben poder registrarse en la plataforma.|
| RF-06 |Deben poder configurar horarios de atención.|
| RF-07 |Deben poder administrar los servicios ofrecidos, incluyendo si cada servicio exige seña y de cuánto (monto fijo o porcentaje del precio).|
| RF-08 |Deben poder gestionar profesionales asociados.|

### Módulo 3 — Reserva de turnos.

| ID | Requisito |
|----|-----------|
| RF-09 |El usuario debe poder buscar empresas por categorias.|
| RF-10 |El usuario debe visualizar horarios disponibles.|
| RF-11 | El usuario debe poder reservar un turno; si el servicio exige seña, el turno queda "Pendiente de pago" hasta que el pago se acredita.|
| RF-12 |El sistema debe evitar reservas duplicadas, incluyendo los horarios retenidos por un pago pendiente.|
| RF-13 |El sistema debe generar un identificador único para cada turno.|

### Módulo 4 — Gestión de citas.

| ID | Requisito |
|----|-----------|
| RF-14 |El usuario debe poder cancelar una cita en cualquier momento; si tiene seña, esta se devuelve cuando faltan más de 24 horas y se retiene cuando faltan 24 horas o menos.|
| RF-15 |El usuario debe poder reprogramar una cita; la seña ya pagada pasa al turno nuevo.|
| RF-16 |El sistema debe actualizar automáticamente la disponibilidad|
| RF-17 |La empresa debe visualizar todas las reservas activas, con el estado de la seña de cada una.|

### Módulo 5 — Notificaciones

| ID | Requisito |
|----|-----------|
| RF-18 |El sistema debe enviar confirmaciones automáticas.|
| RF-19 |Debe enviar recordatorios 24 horas antes del turno.|
| RF-20 |Debe informar cancelaciones o modificaciones.|
| RF-21 |Debe notificar cambios realizados por la empresa.|

### Módulo 6 — Reportes

| ID | Requisito |
|----|-----------|
| RF-22 |Generar reportes de turnos atendidos.|
| RF-23 |Generar estadísticas de cancelaciones, incluyendo las señas retenidas.|
| RF-24 |Mostrar indicadores de ocupación.|

### Módulo 7 — Agenda y disponibilidad de profesionales

| ID | Requisito |
|----|-----------|
| RF-25  |El profesional debe poder configurar su propia disponibilidad horaria (días y franjas horarias) dentro de la empresa a la que pertenece.|
| RF-26  |El profesional debe poder consultar su agenda de turnos asignados, filtrando por rango de fechas.|

### Módulo 8 — Señas y pagos
| ID | Requisito |
|----|-----------|
| RF-27 | La empresa debe poder definir, por servicio, si exige seña y de cuánto (monto fijo o porcentaje del precio).|
| RF-28 | El sistema debe cobrar la seña online al reservar un servicio que la exija; el turno queda "Pendiente de pago" con el horario retenido 10 minutos y se confirma solo cuando el pago se acredita.|
| RF-29 | El sistema debe devolver la seña si el cliente cancela con más de 24 horas de anticipación y retenerla si cancela con 24 horas o menos o no se presenta. Si cancela la empresa, siempre se devuelve.|
| RF-30 | El sistema debe devolver automáticamente un pago acreditado cuando el horario ya fue tomado por otra persona, y reintentar las devoluciones rechazadas por la pasarela de pagos.|
| RF-31 | El profesional debe poder registrar si el cliente asistió o estuvo ausente a su turno.|
| RF-32 | Al reprogramar un turno, la seña pagada debe pasar al turno nuevo sin cobrar de nuevo.|


## Requisitos no funcionales

### Rendimiento

| ID | Requisito |
|----|-----------|
| RNF-01 |El sistema debe responder en menos de 3 segundos en al menos el 95% de las solicitudes, con hasta 1000 usuarios concurrentes.|
| RNF-02 |El sistema debe soportar 1000 usuarios concurrentes manteniendo una tasa de solicitudes exitosas de al menos el 99% (menos del 1% de errores o timeouts).|

### Seguridad

| ID | Requisito |
|----|-----------|
| RNF-03 |Toda comunicación debe utilizar HTTPS.|
| RNF-04 |Las contraseñas deben almacenarse cifradas.|
| RNF-05 |Debe existir registro de auditoría para operaciones criticas.|

### Usabilidad

| ID | Requisito |
|----|-----------|
| RNF-06 |Un cliente nuevo debe poder completar una reserva en 4 pasos de interacción o menos y en menos de 2 minutos, siempre que exista disponibilidad para el turno solicitado y los datos ingresados sean válidos.|
| RNF-07 |Debe ser responsiva para móviles y computadoras.|
| RNF-08 |La interfaz debe garantizar una relación de contraste mínima de 4,5:1 para el texto normal, permitir operar todas las funciones mediante el teclado y contar con botones y controles interactivos de al menos 44 × 44 píxeles CSS.|

### Disponibilidad

| ID | Requisito |
|----|-----------|
| RNF-09 |Disponibilidad mínima del 99%.|
| RNF-10 |Copias de seguridad automáticas diarias.|
| RNF-11 |No guardar datos de tarjeta.|

### Confiabilidad

| ID | Requisito |
|----|-----------|
| RNF-12 | Un mismo turno no debe generar más de un cobro, aunque el cliente haga doble clic o reintente el pago.|
