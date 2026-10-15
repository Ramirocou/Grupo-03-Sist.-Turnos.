
# Modelo Entidad-Relación

## 1. Objetivo

El modelo Entidad-Relación representa la estructura de datos necesaria para el Sistema de Turnos. Define las entidades, sus atributos, las claves primarias, las claves foráneas y las relaciones que permiten gestionar empresas, clientes, profesionales, servicios, turnos, disponibilidad, días no laborables, notificaciones y reprogramaciones.

El modelo busca mantener la integridad de los datos, evitar duplicaciones innecesarias y permitir que las reservas puedan consultarse y administrarse de forma consistente.

## 2. Diagrama

El diagrama se encuentra en `diagramas/er.puml` y está escrito en PlantUML.

El archivo `.puml` es la fuente del diagrama. Este documento contiene su explicación y las decisiones de diseño.

## 3. Descripción de las entidades

### 3.1. EMPRESA

Representa a una empresa o establecimiento que utiliza el sistema de turnos.

- `empresa_id` (PK): identificador único de la empresa.
- `nombre`: nombre comercial del establecimiento.
- `email`: correo electrónico de contacto.
- `telefono`: teléfono de contacto.
- `direccion`: dirección del establecimiento.
- `estado`: indica si la empresa se encuentra activa o inactiva.

### 3.2. CLIENTE

Representa a una persona que solicita y reserva turnos.

- `cliente_id` (PK): identificador único del cliente.
- `nombre`: nombre del cliente.
- `apellido`: apellido del cliente.
- `email`: correo electrónico del cliente, utilizado para enviar el comprobante y las comunicaciones correspondientes.
- `telefono`: número de contacto del cliente.

### 3.3. PROFESIONAL

Representa a una persona que presta los servicios ofrecidos por una empresa.

- `profesional_id` (PK): identificador único del profesional.
- `empresa_id` (FK): empresa a la que pertenece.
- `nombre`: nombre del profesional.
- `apellido`: apellido del profesional.
- `email`: correo electrónico de contacto.
- `telefono`: número de contacto.
- `estado`: indica si el profesional está activo o inactivo para recibir reservas.

### 3.4. SERVICIO

Representa cada servicio que una empresa ofrece a sus clientes.

- `servicio_id` (PK): identificador único del servicio.
- `empresa_id` (FK): empresa que ofrece el servicio.
- `nombre`: nombre del servicio.
- `descripcion`: detalle de las características del servicio.
- `duracion_minutos`: duración prevista del servicio, expresada en minutos.
- `precio`: precio del servicio.
- `estado`: indica si el servicio está disponible para reservar.

### 3.5. TURNO

Representa una reserva realizada por un cliente para un servicio con un profesional.

- `turno_id` (PK): identificador único del turno.
- `empresa_id` (FK): empresa que administra la reserva.
- `cliente_id` (FK): cliente que realizó la reserva.
- `profesional_id` (FK): profesional asignado al turno.
- `servicio_id` (FK): servicio reservado.
- `fecha`: fecha prevista del turno.
- `hora_inicio`: hora de inicio de la reserva.
- `hora_fin`: hora de finalización prevista.
- `estado`: situación actual del turno.
- `fecha_creacion`: fecha y hora en que se registró la reserva.

Los estados permitidos son Pendiente, Confirmado, Completado, Cancelado y Ausente.

El sistema debe validar que el estado asignado pertenezca a esta lista. También debe comprobar que el horario solicitado esté disponible y que no exista una reserva incompatible para el profesional.

### 3.6. DISPONIBILIDAD

Representa los horarios habituales en los que un profesional puede atender.

- `disponibilidad_id` (PK): identificador único del horario de disponibilidad.
- `profesional_id` (FK): profesional al que corresponde el horario.
- `dia_semana`: día de la semana al que corresponde la disponibilidad, del 1 al 7.
- `hora_inicio`: hora desde la cual el profesional puede atender.
- `hora_fin`: hora hasta la cual puede atender.
- `activo`: indica si esa configuración de disponibilidad está habilitada.

Esta entidad permite definir diferentes horarios para cada día de la semana sin tener que crear una estructura fija para cada posible franja horaria.

### 3.7. DIA_NO_LABORABLE

Representa una fecha en la que una empresa o un profesional no puede atender.

- `dia_no_laborable_id` (PK): identificador único del registro.
- `empresa_id` (FK): empresa a la que corresponde el día no laborable.
- `profesional_id` (FK, opcional): profesional afectado cuando el cierre corresponde solamente a una persona.
- `fecha`: día en el que no habrá atención.
- `descripcion`: motivo o detalle del cierre.

Si `profesional_id` no tiene valor, el registro representa un día no laborable para toda la empresa. Si tiene valor, se aplica al profesional indicado.

### 3.8. NOTIFICACION

Representa una comunicación relacionada con un turno, como un recordatorio o un aviso de confirmación.

- `notificacion_id` (PK): identificador único de la notificación.
- `turno_id` (FK): turno al que corresponde la comunicación.
- `canal`: medio utilizado para enviar la notificación, por ejemplo, email.
- `fecha_envio`: fecha y hora en la que se intentó o registró el envío.
- `estado`: resultado del envío, según los estados definidos por el sistema.

La entidad permite registrar las comunicaciones asociadas a una reserva y consultar si se encuentran pendientes, enviadas o fallidas, siempre que esos valores estén definidos en la implementación.

### 3.9. REPROGRAMACION

Registra los cambios de fecha y hora realizados sobre un turno.

- `reprogramacion_id` (PK): identificador único del registro de reprogramación.
- `turno_id` (FK): turno que fue reprogramado.
- `fecha_anterior`: fecha que tenía el turno antes del cambio.
- `hora_anterior`: hora de inicio anterior.
- `fecha_nueva`: nueva fecha asignada.
- `hora_nueva`: nueva hora de inicio.
- `fecha_reprogramacion`: fecha y hora en la que se realizó el cambio.

Cada reprogramación genera un registro nuevo. De esta manera, se conserva el historial de cambios y no se pierde la información de las fechas y horas anteriores.

## 4. Relaciones y cardinalidades

- EMPRESA — PROFESIONAL: una empresa puede tener varios profesionales; cada profesional pertenece a una empresa.
- EMPRESA — SERVICIO: una empresa puede ofrecer varios servicios; cada servicio pertenece a una empresa.
- EMPRESA — TURNO: una empresa puede administrar varios turnos; cada turno corresponde a una empresa.
- EMPRESA — DIA_NO_LABORABLE: una empresa puede registrar varios días no laborables.
- CLIENTE — TURNO: un cliente puede reservar varios turnos; cada turno corresponde a un cliente.
- PROFESIONAL — TURNO: un profesional puede atender varios turnos; cada turno tiene un profesional asignado.
- SERVICIO — TURNO: un servicio puede estar asociado a varios turnos; cada turno corresponde a un servicio.
- PROFESIONAL — DISPONIBILIDAD: un profesional puede tener varios registros de disponibilidad.
- PROFESIONAL — DIA_NO_LABORABLE: un profesional puede tener varios días no laborables particulares.
- TURNO — NOTIFICACION: un turno puede tener cero o varias notificaciones.
- TURNO — REPROGRAMACION: un turno puede no haber sido reprogramado o tener múltiples reprogramaciones.

Las claves foráneas permiten vincular los registros relacionados y evitar referencias a entidades inexistentes.

## 5. Reglas de integridad

1. Cada entidad debe tener una clave primaria que identifique unívocamente sus registros.
2. Las claves foráneas deben referenciar registros existentes.
3. Un turno debe tener asociados una empresa, un cliente, un profesional y un servicio.
4. El servicio reservado debe pertenecer a la misma empresa que administra el turno.
5. El profesional debe estar habilitado para atender el servicio y pertenecer a la empresa correspondiente.
6. La hora de inicio debe ser anterior a la hora de fin.
7. La duración prevista del turno debe respetar la duración del servicio, salvo que los requisitos permitan excepciones.
8. No se deben aceptar reservas fuera de la disponibilidad del profesional, en días no laborables ni en horarios ocupados.
9. El estado de un turno debe pertenecer a los valores permitidos.
10. Las reprogramaciones deben conservar la fecha y la hora anteriores.
11. Las notificaciones deben quedar asociadas al turno correspondiente.
12. El correo electrónico del cliente debe estar disponible cuando sea necesario enviar el comprobante por ese medio.

Las validaciones deben implementarse en la lógica del sistema y, cuando corresponda, reforzarse mediante restricciones de la base de datos.

## 6. Decisiones de diseño

### Decisión 1. Identificadores propios y claves foráneas

Se utiliza una clave primaria propia para cada entidad y claves foráneas para representar sus relaciones.

Alternativa descartada: identificar los registros mediante nombres, correos electrónicos u otros datos descriptivos.

Justificación: estos datos pueden cambiar o repetirse. Un identificador estable facilita las relaciones y mantiene la integridad referencial.

### Decisión 2. Identificación de la empresa en TURNO

Se incorpora `empresa_id` en TURNO para identificar directamente a la empresa que administra cada reserva.

Alternativa descartada: obtener la empresa únicamente recorriendo las relaciones entre el profesional o el servicio y la empresa.

Justificación: la referencia explícita facilita las consultas y permite validar que el turno, el servicio y el profesional correspondan a la misma empresa. Como contrapartida, es necesario validar la consistencia entre las claves foráneas para evitar asociaciones incorrectas.

### Decisión 3. Relación explícita entre TURNO y SERVICIO

Se incorpora `servicio_id` como clave foránea en TURNO.

Alternativa descartada: registrar únicamente al cliente, al profesional y el horario.

Justificación: sin la referencia al servicio, el sistema no podría determinar qué prestación reservó el cliente ni utilizar correctamente su duración y precio.

### Decisión 4. Duración del servicio

Se incorpora `duracion_minutos` en SERVICIO.

Alternativa descartada: guardar únicamente la hora de inicio o definir una duración fija para todos los servicios.

Justificación: cada servicio puede requerir un tiempo diferente. La duración permite calcular el horario de finalización y comprobar la disponibilidad del profesional.

### Decisión 5. Disponibilidad mediante registros

Se utiliza DISPONIBILIDAD para almacenar el día de la semana y las horas de inicio y fin de cada franja de atención.

Alternativa descartada: definir un conjunto fijo de franjas horarias directamente en el código o crear una columna por cada horario posible.

Justificación: una entidad independiente permite agregar, modificar o desactivar horarios sin cambiar la estructura del modelo. También facilita que cada profesional tenga distintas franjas según el día de la semana.

### Decisión 6. Días no laborables separados

Se utiliza DIA_NO_LABORABLE para registrar cierres generales o excepciones particulares.

Alternativa descartada: eliminar temporalmente la disponibilidad habitual del profesional cuando existe un feriado o un cierre excepcional.

Justificación: la disponibilidad habitual y las excepciones son conceptos diferentes. Separarlos permite conservar el horario normal y registrar los días en los que no debe ofrecerse atención.

### Decisión 7. Historial de reprogramaciones

Se utiliza REPROGRAMACION para registrar cada cambio de fecha y hora.

Alternativa descartada: sobrescribir directamente los datos del turno sin conservar los valores anteriores.

Justificación: mantener un historial permite conocer las modificaciones realizadas y cumplir el criterio de aceptación que exige conservar la fecha anterior y la nueva.

### Decisión 8. Notificaciones independientes

Se utiliza NOTIFICACION para registrar los avisos vinculados a los turnos.

Alternativa descartada: almacenar únicamente un indicador de recordatorio enviado dentro de TURNO.

Justificación: un turno puede generar distintas comunicaciones. Una entidad independiente permite registrar el canal, la fecha y el estado de cada envío.

### Decisión 9. Estados explícitos del turno

Se define un conjunto cerrado de estados para TURNO: Pendiente, Confirmado, Completado, Cancelado y Ausente.

Alternativa descartada: aceptar cualquier texto como estado.

Justificación: limitar los valores evita inconsistencias y facilita las búsquedas, los filtros y las validaciones del flujo de atención.

### Decisión 10. Correo electrónico del cliente

Se incorpora `email` en CLIENTE.

Alternativa descartada: depender exclusivamente del teléfono o solicitar el correo nuevamente cada vez que se reserva.

Justificación: disponer del correo asociado al cliente facilita el envío del comprobante y las comunicaciones previstas por los requisitos.
