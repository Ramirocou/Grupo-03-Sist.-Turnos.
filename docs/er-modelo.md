
# Modelo Entidad-Relación

## 1. Objetivo

El modelo Entidad-Relación representa los datos necesarios para el Sistema de Turnos. Incluye la administración de usuarios, empresas, categorías, profesionales, servicios, reservas, disponibilidad, notificaciones, reprogramaciones y pagos de señas.

El diseño busca mantener la integridad de los datos, evitar duplicaciones, conservar el historial de operaciones y garantizar que las reservas y los pagos estén vinculados correctamente.

## 2. Diagrama

El diagrama se encuentra en `diagramas/er.puml` y utiliza la sintaxis de PlantUML.

Las entidades representan los conceptos principales del sistema. Las claves primarias identifican cada registro y las claves foráneas permiten relacionar las entidades.

## 3. Descripción de las entidades

### 3.1. USUARIO

Representa una cuenta que permite acceder al sistema.

- `usuario_id` (PK): identificador único de la cuenta.
- `email` (UK): correo electrónico único utilizado para iniciar sesión.
- `password_hash`: resultado de una función segura de hash aplicada a la contraseña.
- `rol`: tipo de cuenta, que puede ser CLIENTE, EMPRESA, PROFESIONAL o ADMIN.
- `estado`: situación de la cuenta, por ejemplo, activa o inactiva.
- `token_recuperacion_hash`: hash del token temporal para recuperar la contraseña.
- `token_recuperacion_expira`: fecha y hora de vencimiento del token.
- `fecha_creacion`: fecha y hora de creación de la cuenta.

Las contraseñas no deben almacenarse en texto plano ni mediante cifrado reversible. La recuperación debe utilizar tokens temporales, de un solo uso y con vencimiento.

### 3.2. CLIENTE

Representa a una persona que utiliza el sistema para buscar empresas y reservar turnos.

- `cliente_id` (PK): identificador único del cliente.
- `usuario_id` (FK, UK): cuenta asociada al cliente.
- `nombre`: nombre del cliente.
- `apellido`: apellido del cliente.
- `telefono`: teléfono de contacto.

El correo electrónico se obtiene de la cuenta asociada en USUARIO. De esta manera, se evita guardar el mismo correo en dos entidades.

### 3.3. EMPRESA

Representa un establecimiento que ofrece servicios y administra reservas.

- `empresa_id` (PK): identificador único de la empresa.
- `usuario_id` (FK, UK): cuenta asociada a la empresa.
- `nombre`: nombre comercial.
- `descripcion`: información general del establecimiento.
- `telefono`: teléfono de contacto.
- `direccion`: dirección del establecimiento.
- `estado`: situación de la empresa en la plataforma.

### 3.4. CATEGORIA

Representa una clasificación utilizada para buscar empresas.

- `categoria_id` (PK): identificador único de la categoría.
- `nombre` (UK): nombre único de la categoría.
- `descripcion`: explicación de la categoría.

### 3.5. EMPRESA_CATEGORIA

Resuelve la relación de muchos a muchos entre empresas y categorías.

- `empresa_id` (PK, FK): empresa clasificada.
- `categoria_id` (PK, FK): categoría asignada.

Una empresa puede pertenecer a varias categorías y una categoría puede incluir varias empresas.

### 3.6. PROFESIONAL

Representa a una persona que atiende turnos dentro de una empresa.

- `profesional_id` (PK): identificador único del profesional.
- `usuario_id` (FK, UK): cuenta asociada al profesional.
- `empresa_id` (FK): empresa a la que pertenece.
- `nombre`: nombre del profesional.
- `apellido`: apellido del profesional.
- `telefono`: teléfono de contacto.
- `estado`: indica si está habilitado para recibir reservas.

### 3.7. SERVICIO

Representa una prestación ofrecida por una empresa.

- `servicio_id` (PK): identificador único del servicio.
- `empresa_id` (FK): empresa que ofrece el servicio.
- `nombre`: nombre de la prestación.
- `descripcion`: detalle del servicio.
- `duracion_minutos`: duración prevista, expresada en minutos.
- `precio`: precio total del servicio.
- `requiere_senia`: indica si se debe abonar una seña.
- `tipo_senia`: forma de calcular la seña, mediante un importe fijo o un porcentaje.
- `valor_senia`: importe fijo o porcentaje configurado, según el tipo de seña.
- `estado`: indica si el servicio está disponible.

Si el servicio no exige seña, `tipo_senia` y `valor_senia` deben quedar vacíos. Si exige seña, ambos campos son obligatorios.

Cuando `tipo_senia` sea FIJA, `valor_senia` representa un importe monetario. Cuando sea PORCENTAJE, representa un porcentaje válido del precio del servicio.

### 3.8. PROFESIONAL_SERVICIO

Representa los servicios que cada profesional está habilitado para realizar.

- `profesional_id` (PK, FK): profesional asociado.
- `servicio_id` (PK, FK): servicio que puede realizar.

La clave primaria compuesta evita duplicar la misma asociación.

La lógica del sistema debe comprobar que el profesional y el servicio pertenezcan a la misma empresa.

### 3.9. TURNO

Representa una reserva solicitada por un cliente.

- `turno_id` (PK): identificador único del turno.
- `empresa_id` (FK): empresa que administra la reserva.
- `cliente_id` (FK): cliente que solicita el turno.
- `profesional_id` (FK): profesional asignado.
- `servicio_id` (FK): servicio reservado.
- `fecha`: fecha prevista del turno.
- `hora_inicio`: hora de inicio.
- `hora_fin`: hora de finalización prevista.
- `estado`: estado actual de la reserva.
- `fecha_creacion`: fecha y hora de creación.
- `fecha_vencimiento_pago`: límite para completar el pago de una seña cuando corresponda.

Los estados permitidos son:

- `Pendiente de pago`: se retiene temporalmente el horario mientras se espera el pago de la seña.
- `Reservado`: el turno está confirmado y reservado.
- `Completado`: el servicio fue atendido.
- `Cancelado`: el turno fue cancelado.
- `Ausente`: el cliente no se presentó.
- `Vencido`: venció el plazo de pago y se liberó el horario.

Si el servicio no requiere seña, el turno puede pasar directamente a Reservado. Si la requiere, debe permanecer Pendiente de pago hasta la acreditación del pago o el vencimiento del plazo.

El sistema debe impedir turnos superpuestos para un mismo profesional y validar la disponibilidad antes de confirmar la reserva.

### 3.10. DISPONIBILIDAD

Representa los horarios habituales de atención de un profesional.

- `disponibilidad_id` (PK): identificador único del registro.
- `profesional_id` (FK): profesional al que corresponde.
- `dia_semana`: día de la semana, del 1 al 7.
- `hora_inicio`: inicio de la franja.
- `hora_fin`: final de la franja.
- `activo`: indica si la franja está habilitada.

La disponibilidad se guarda como registros para permitir horarios diferentes por día y modificar la agenda sin cambiar la estructura de la base de datos.

Las franjas no deben superponerse de manera incompatible. La lógica de reservas debe verificar también los turnos ya ocupados.

### 3.11. DIA_NO_LABORABLE

Representa una fecha en la que no se puede atender.

- `dia_no_laborable_id` (PK): identificador único.
- `empresa_id` (FK): empresa a la que corresponde.
- `profesional_id` (FK, opcional): profesional afectado cuando la excepción es individual.
- `fecha`: fecha sin atención.
- `descripcion`: motivo o detalle.

Si `profesional_id` está vacío, el día no laborable se aplica a toda la empresa. Si tiene valor, se aplica al profesional indicado.

### 3.12. NOTIFICACION

Representa las comunicaciones vinculadas a un turno.

- `notificacion_id` (PK): identificador único.
- `turno_id` (FK): turno relacionado.
- `canal`: medio de envío, por ejemplo, email.
- `tipo`: tipo de comunicación, como confirmación, recordatorio, cancelación o modificación.
- `fecha_programada`: fecha y hora previstas para el envío.
- `fecha_envio`: fecha y hora del intento o envío registrado.
- `estado`: resultado de la notificación, por ejemplo, pendiente, enviada o fallida.

Los recordatorios deben programarse para 24 horas antes del turno. Si el turno se cancela o se reprograma, las notificaciones pendientes deben actualizarse para evitar comunicaciones incorrectas.

### 3.13. REPROGRAMACION

Conserva el historial de cambios de fecha y hora.

- `reprogramacion_id` (PK): identificador único.
- `turno_id` (FK): turno modificado.
- `fecha_anterior`: fecha previa al cambio.
- `hora_anterior`: hora de inicio previa.
- `fecha_nueva`: nueva fecha.
- `hora_nueva`: nueva hora de inicio.
- `fecha_reprogramacion`: fecha y hora en que se realizó el cambio.
- `senia_transferida`: indica si la seña se mantiene aplicada a la reserva reprogramada.
- `observacion`: detalle opcional del cambio.

Cada reprogramación genera un registro nuevo. No se deben sobrescribir los datos históricos.

### 3.14. PAGO

Representa las operaciones económicas relacionadas con un turno.

- `pago_id` (PK): identificador único de la operación.
- `turno_id` (FK): turno al que corresponde.
- `pago_origen_id` (FK, opcional): pago original al que se vincula una devolución.
- `tipo_operacion`: identifica si es un PAGO o una DEVOLUCION.
- `concepto`: motivo de la operación, como SENIA o AJUSTE.
- `monto`: importe de la operación.
- `moneda`: moneda utilizada, por ejemplo, ARS.
- `estado`: estado de la operación, como PENDIENTE, APROBADO, RECHAZADO o ANULADO.
- `proveedor`: pasarela de pago utilizada.
- `referencia_proveedor`: identificador de la operación en la pasarela, si existe.
- `clave_idempotencia`: identificador único que permite evitar el procesamiento duplicado de una misma operación.
- `fecha_creacion`: fecha y hora de registro.
- `fecha_procesamiento`: fecha y hora en que se obtuvo el resultado.
- `motivo`: detalle de rechazo, devolución o ajuste.

Cada intento de pago debe poder identificarse. Las devoluciones deben registrarse como operaciones separadas y referenciar el pago original.

La confirmación de un pago debe depender de la respuesta verificada de la pasarela. El sistema no debe considerar acreditado un pago solamente porque el cliente haya regresado a la página de confirmación.

## 4. Relaciones y cardinalidades

- USUARIO — CLIENTE: una cuenta puede tener un perfil de cliente; cada perfil corresponde a una cuenta.
- USUARIO — EMPRESA: una cuenta puede administrar un perfil de empresa.
- USUARIO — PROFESIONAL: una cuenta puede corresponder a un profesional.
- EMPRESA — CATEGORIA: relación muchos a muchos resuelta mediante EMPRESA_CATEGORIA.
- EMPRESA — PROFESIONAL: una empresa puede tener varios profesionales.
- EMPRESA — SERVICIO: una empresa puede ofrecer varios servicios.
- PROFESIONAL — SERVICIO: relación muchos a muchos resuelta mediante PROFESIONAL_SERVICIO.
- EMPRESA — TURNO: una empresa puede administrar varios turnos.
- CLIENTE — TURNO: un cliente puede solicitar varios turnos.
- PROFESIONAL — TURNO: un profesional puede atender varios turnos.
- SERVICIO — TURNO: un servicio puede estar asociado a varios turnos.
- PROFESIONAL — DISPONIBILIDAD: un profesional puede configurar varias franjas.
- EMPRESA — DIA_NO_LABORABLE: una empresa puede registrar varios días no laborables.
- PROFESIONAL — DIA_NO_LABORABLE: un profesional puede tener excepciones particulares.
- TURNO — NOTIFICACION: un turno puede generar varias notificaciones.
- TURNO — REPROGRAMACION: un turno puede tener cero o varias reprogramaciones.
- TURNO — PAGO: un turno puede tener varias operaciones de pago.
- PAGO — PAGO: una devolución puede referenciar un pago original.

## 5. Reglas de integridad

1. Cada clave primaria debe identificar un registro de forma única.
2. Las claves foráneas deben referenciar registros existentes.
3. El email de USUARIO debe ser único.
4. Las contraseñas deben almacenarse mediante hash seguro y nunca en texto plano.
5. Un turno debe vincular una empresa, un cliente, un profesional y un servicio.
6. El profesional y el servicio deben pertenecer a la empresa indicada en el turno.
7. El profesional debe estar asociado al servicio mediante PROFESIONAL_SERVICIO.
8. La hora de inicio debe ser anterior a la hora de fin.
9. La duración del turno debe respetar la duración configurada para el servicio.
10. No se deben admitir reservas superpuestas para un mismo profesional.
11. No se deben ofrecer horarios fuera de la disponibilidad ni durante días no laborables.
12. Los servicios que exigen seña deben tener configurado el tipo y el valor de la seña.
13. Un turno con seña pendiente debe tener un vencimiento definido.
14. Al vencer el plazo de pago, el turno debe pasar a Vencido y liberar el horario.
15. El turno se confirma como Reservado solamente cuando se cumplen las condiciones de reserva.
16. Las operaciones de pago deben utilizar claves de idempotencia para evitar duplicaciones.
17. Las devoluciones deben referenciar el pago original y no superar el importe efectivamente acreditado y disponible para devolución.
18. La política de cancelación debe aplicarse de forma consistente según quién cancela y el tiempo restante.
19. Cada reprogramación debe conservar la fecha y hora anteriores y las nuevas.
20. Las notificaciones deben actualizarse cuando un turno cambia de estado o de horario.
21. Los estados de turno, pago y notificación deben validarse contra los valores definidos por el sistema.

## 6. Decisiones de diseño

### Decisión 1. USUARIO como entidad común

Se separan los datos de autenticación de los perfiles de cliente, empresa y profesional.

Alternativa descartada: repetir correo y contraseña en cada entidad.

Justificación: centralizar las credenciales evita duplicaciones y permite gestionar el acceso y la recuperación de contraseñas de forma consistente. La aplicación debe validar que el rol de la cuenta corresponda al perfil utilizado.

### Decisión 2. Empresa identificada directamente en TURNO

Se incorpora `empresa_id` en TURNO.

Alternativa descartada: obtener la empresa recorriendo solamente las relaciones del profesional o del servicio.

Justificación: facilita las consultas y permite identificar directamente a la empresa responsable. La aplicación debe verificar que las claves foráneas correspondan a la misma empresa.

### Decisión 3. Relación explícita entre TURNO y SERVICIO

Se incorpora `servicio_id` en TURNO.

Alternativa descartada: registrar únicamente cliente, profesional y horario.

Justificación: permite saber qué prestación fue reservada y consultar su precio y duración.

### Decisión 4. Duración en SERVICIO

Se incorpora `duracion_minutos`.

Alternativa descartada: definir una duración única para todos los servicios o guardar solamente la hora de inicio.

Justificación: permite calcular la hora de finalización y comprobar los conflictos de agenda.

### Decisión 5. Categorías mediante una entidad independiente

Se utiliza CATEGORIA y la tabla asociativa EMPRESA_CATEGORIA.

Alternativa descartada: almacenar varias categorías en una única columna de EMPRESA.

Justificación: una empresa puede pertenecer a varias categorías y cada categoría puede agrupar varias empresas, sin duplicar datos.

### Decisión 6. Asociación entre PROFESIONAL y SERVICIO

Se utiliza PROFESIONAL_SERVICIO.

Alternativa descartada: asumir que todos los profesionales pueden realizar todos los servicios de su empresa.

Justificación: permite definir explícitamente qué prestaciones puede realizar cada profesional.

### Decisión 7. Disponibilidad mediante registros

Se utiliza DISPONIBILIDAD para almacenar los días y las franjas horarias.

Alternativa descartada: programar franjas fijas directamente en el código.

Justificación: permite modificar horarios y administrar diferentes franjas según el día de la semana.

### Decisión 8. Días no laborables independientes

Se utiliza DIA_NO_LABORABLE para registrar cierres generales y excepciones individuales.

Alternativa descartada: eliminar o modificar permanentemente la disponibilidad habitual.

Justificación: permite mantener la agenda normal y registrar excepciones sin perder la configuración original.

### Decisión 9. Estados del turno definidos

Se establecen los estados Pendiente de pago, Reservado, Completado, Cancelado, Ausente y Vencido.

Alternativa descartada: aceptar cualquier texto como estado o utilizar indistintamente Confirmado y Reservado.

Justificación: un conjunto definido evita inconsistencias y representa el ciclo de vida de la reserva, incluida la espera de pago y el vencimiento de la retención temporal.

### Decisión 10. Historial de reprogramación

Se utiliza REPROGRAMACION para conservar los cambios.

Alternativa descartada: sobrescribir directamente la fecha y hora anteriores.

Justificación: permite reconstruir las modificaciones realizadas y registrar si la seña se mantuvo aplicada a la reserva.

### Decisión 11. Notificaciones independientes

Se utiliza NOTIFICACION para registrar las comunicaciones vinculadas a cada turno.

Alternativa descartada: guardar solamente un indicador de recordatorio dentro de TURNO.

Justificación: una reserva puede generar varias comunicaciones, con diferentes fechas, tipos y resultados.

### Decisión 12. Configuración de señas por servicio

Se incorporan `requiere_senia`, `tipo_senia` y `valor_senia` en SERVICIO.

Alternativa descartada: utilizar un importe único de seña para todos los servicios.

Justificación: permite que cada empresa defina si exige una seña y cómo se calcula, respetando las reglas de negocio.

### Decisión 13. Pagos como entidad independiente

Se utiliza PAGO para registrar los intentos de cobro, las acreditaciones y las devoluciones.

Alternativa descartada: guardar solamente un indicador de pago dentro de TURNO.

Justificación: una reserva puede tener varios intentos de pago y operaciones de devolución. La entidad independiente conserva la trazabilidad de cada operación.

### Decisión 14. Idempotencia de pagos

Se incorpora `clave_idempotencia` para identificar de manera única cada operación lógica.

Alternativa descartada: confiar solamente en que el cliente no presione dos veces el botón de pago.

Justificación: las solicitudes pueden repetirse por errores de conexión, reintentos o notificaciones duplicadas. La clave permite evitar el procesamiento repetido de una misma operación.

### Decisión 15. Contraseñas y recuperación

Se almacenan hashes de contraseñas y de tokens de recuperación, con vencimientos.

Alternativa descartada: guardar contraseñas o tokens de recuperación permanentes en texto plano.

Justificación: reduce el riesgo de exposición de credenciales y limita la validez de los mecanismos de recuperación.

## 7. Reglas de negocio para señas, cancelaciones y reprogramaciones

### Reserva con seña

1. Si el servicio no requiere seña, el turno puede quedar Reservado después de validar la disponibilidad.
2. Si requiere seña, el horario se retiene durante diez minutos mientras se espera la acreditación.
3. Si el pago se acredita dentro del plazo y el horario continúa disponible para esa reserva, el turno pasa a Reservado.
4. Si el pago no se acredita dentro del plazo, el turno pasa a Vencido y el horario se libera.
5. Si se recibe una notificación de pago tardía, el sistema debe verificar el estado real de la operación y no confirmar un turno cuyo horario ya fue asignado a otra reserva. Si corresponde, debe iniciar el procedimiento de devolución.

### Cancelación

1. Si el cliente cancela con más de 24 horas de anticipación, corresponde devolver la seña abonada.
2. Si el cliente cancela cuando faltan 24 horas o menos, la seña se retiene.
3. Si la empresa cancela, corresponde devolver la seña independientemente del tiempo restante.
4. En todos los casos, el sistema debe registrar la cancelación, actualizar la disponibilidad y gestionar las notificaciones correspondientes.
5. La devolución debe registrarse como una operación separada vinculada al pago original.

### Reprogramación

1. Debe verificarse la disponibilidad del nuevo horario antes de confirmar el cambio.
2. La fecha y hora anteriores deben conservarse en REPROGRAMACION.
3. El nuevo horario debe quedar vinculado al mismo turno.
4. Si la política permite transferir la seña, debe conservarse su relación con la reserva y registrarse la transferencia en el historial.
5. Las notificaciones pendientes deben actualizarse para reflejar la nueva fecha y hora.

## 8. Alcance y limitaciones

El modelo representa la estructura de información y las relaciones necesarias para las funciones descritas.

La autenticación, la autorización por roles, las validaciones de horarios, la integración con la pasarela de pago, los plazos de retención, las devoluciones y el envío de notificaciones requieren reglas de aplicación adicionales.

Las restricciones que dependen de varias entidades o de operaciones concurrentes deben garantizarse en la lógica del sistema y, cuando sea posible, reforzarse mediante restricciones y transacciones de la base de datos.
