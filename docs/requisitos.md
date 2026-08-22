# Requisitos del sistema

## Requisitos funcionales

### Módulo 1 — [Gestión de usuarios.]

| ID | Requisito |
|----|-----------|
| RF-01 |El sistema debe permitir registrar usuarios clientes.|
| RF-02 |El sistema debe permitir iniciar sesión mediante correo y contraseña.|
| RF-03 |El sistema debe permitir recuperar contraseñas.|
| RF-04 |El usuario debe poder editar su perfil personal.|

### Módulo 2 — [Gestión de empresas]

| ID | Requisito |
|----|-----------|
| RF-05 |Las empresas deben poder registrarse en la plataforma.|
| RF-06 |Deben poder configurar horarios de atención.|
| RF-07 |Deben poder administrar los servicios ofrecidos.|
| RF-08 |Deben poder gestionar profesionales asociados.|

### Módulo 3 — [Reserva de turnos.]

| ID | Requisito |
|----|-----------|
| RF-09 |El usuario debe poder buscar empresas por categorias|
| RF-10 |El usuario debe visualizar horarios disponibles|
| RF-11 |El usuario debe poder reservar un turno|
| RF-12 |El sistema debe evitar reservas duplicadas|
| RF-13 |El sistema debe generar un identificador único para cada turno|

### Módulo 4 — [Gestión de citas.]

| ID | Requisito |
|----|-----------|
| RF-14 |El usuario debe poder cancelar una cita|
| RF-15 |El usuario debe poder reprogramar una cita|
| RF-16 |El sistema debe actualizar automáticamente la disponibilidad|
| RF-17 |La empresa debe visualizar todas las reservas activas|

### Módulo 5 — [Notificaciones]

| ID | Requisito |
|----|-----------|
| RF-18 |El sistema debe enviar confirmaciones automáticas|
| RF-19 |Debe enviar recordatorios 24 horas antes del turno|
| RF-20 |Debe informar cancelaciones o modificaciones|
| RF-21 |Debe notificar cambios realizados por la empresa|

### Módulo 6 — [Reportes]

| ID | Requisito |
|----|-----------|
| RF-22 |Generar reportes de turnos atendidos|
| RF-23 |Generar estadísticas de cancelaciones|
| RF-24 |Mostrar indicadores de ocupación|

### Módulo 7 — [Agenda y disponibilidad de profesionales]

| ID | Requisito |
|----|-----------|
| RF-25 |El profesional debe poder configurar su propia disponibilidad horaria (días y franjas horarias) dentro de la empresa a la que pertenece.|
| RF-26 |El profesional debe poder consultar su agenda de turnos asignados, filtrando por rango de fechas|


## Requisitos no funcionales

### Rendimiento

| ID | Requisito |
|----|-----------|
| RNF-01 |El sistema debe responder en menos de 3 segundos|
| RNF-02 |Debe soportar al menos 1000 usuarios concurrentes|

### Seguridad

| ID | Requisito |
|----|-----------|
| RNF-03 |Toda comunicación debe utilizar HTTPS|
| RNF-04 |Las contraseñas deben almacenarse cifradas|
| RNF-05 |Debe existir registro de auditoría para operaciones criticas|

### Usabilidad

| ID | Requisito |
|----|-----------|
| RNF-06 |La interfaz debe ser intuitiva|
| RNF-07 |Debe ser responsiva para móviles y computadoras|
| RNF-08 |Debe cumplir criterios básicos de accesibilidad|

### Disponibilidad

| ID | Requisito |
|----|-----------|
| RNF-09 |Disponibilidad mínima del 99%|
| RNF-10 |Copias de seguridad automáticas diarias|
