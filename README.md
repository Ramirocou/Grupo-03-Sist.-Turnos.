# [Sistema de Turnos] — Grupo [Oozma Kappa]

> Materia: Diseño de Sistemas Web — Analista Funcional de Sistemas  
> Institución: Terciario Urquiza — Rosario  
> Docente: Pedernera Pablo  
> Cuatrimestre: 2.° 2026

## Integrantes

Ver [integrantes.md](integrantes.md)

## 1. Descripción del sistema y contexto real. 
### 1.1 Origen del proyecto 

Actualmente, gran cantidad de empresas de servicios continúan administrando sus turnos mediante agendas físicas, llamadas telefónicas, mensajes de WhatsApp o planillas manuales. Estos métodos presentan problemas frecuentes como superposición de citas, pérdida de información, dificultades para coordinar horarios y altos niveles de ausentismo por falta de recordatorios. 

Ante esta situación surge la necesidad de desarrollar una plataforma digital integral que permita centralizar la gestión de turnos y citas en un único sistema accesible desde dispositivos móviles y navegadores web. 

La propuesta consiste en una solución tecnológica orientada a conectar clientes y empresas de servicios mediante una plataforma moderna que facilite la reserva, modificación, cancelación y seguimiento de turnos en tiempo real.

### 1.2 Alcance del sistema 

1. Registro y autenticación de usuarios. 
2. Búsqueda de empresas y servicios disponibles. 
3. Consulta de horarios disponibles. 
4. Reserva de turnos. 
5. Confirmación automática de citas. 
6. Gestión de cancelaciones y reprogramaciones. 
7. Envío de recordatorios automáticos. 
8. Administración de agendas por parte de las empresas. 
9. Consulta del historial de turnos. 
10. Generación de reportes de gestión.

_El sistema cubrirá los siguientes procesos:._

 * Gestión contable de las empresas. 

 * Facturación electrónica. 

 * Administración de recursos humanos. 

 * Sistemas médicos específicos (historias clínicas). 

 * Procesamiento de pagos online en la primera versión. 

### 1.3 Contexto tecnológico y decisiones relevantes 
_Durante el relevamiento se identificaron las siguientes necesidades:._

 * Acceso desde dispositivos móviles y computadoras. 

 * Actualización en tiempo real de disponibilidad. 

 * Notificaciones automáticas mediante correo electrónico y aplicación móvil. 

 * Soporte para múltiples empresas dentro de la misma plataforma. 

 * Escalabilidad para incorporar nuevos rubros en el futuro. 

_La solución estará compuesta por::_

 * Aplicación móvil para clientes. 

 * Plataforma web para empresas. 

 * Base de datos centralizada. 

 * Sistema de notificaciones automáticas. 
 
 * API de integración con calendarios externos.

## 2. Identificación de Stakeholders. 

### 2.1 Cliente o usuario final 
_Es la persona que utiliza la plataforma para reservar, modificar o cancelar turnos. (Es el principal beneficiario del sistema. Su experiencia determinará el éxito o fracaso de la plataforma.)._

### 2.2 Empresa prestadora de servicios 
_Representa a clínicas, consultorios, peluquerías, gimnasios, institutos educativos, estudios profesionales y demás organizaciones que ofrecen servicios mediante turnos. (Gestionan la disponibilidad horaria, los servicios ofrecidos y la atención al cliente.)._

### 2.3 Profesional o empleado 
_Es la persona encargada de brindar el servicio reservado.(Su agenda debe mantenerse actualizada para evitar conflictos y sobrecarga de trabajo)._

### 2.4 Administrador de la plataforma 
_Responsable de supervisar el funcionamiento general del sistema. (Gestiona incidencias, altas de empresas, configuraciones globales y monitoreo del servicio.)._

### 2.5 Servicio de notificaciones 
_Sistema externo encargado de enviar correos electrónicos y notificaciones push. (Permite reducir ausencias mediante recordatorios automáticos.)._

### 2.6 Servicio de calendario externo 
_Sistemas como Google Calendar o Microsoft Outlook. (Facilitan la sincronización automática de agendas.)._

|    Stakeholders |    Tipo       |   Impacto   |
|-----------------|---------------|-------------|
|Cliente          |Usuario externo|Alto         |
|Empresa          |Cliente corporativo|Alto     |
|Profesional      |Usuario interno|Alto         |
|Administrador    |Interno        |Alto         |
|Servicio de Norificaciones|Sistema externo|Medio|
|Calendario Externo|Sistema externo|Medio       |

## 3. Requisitos funcionales y no funcionales. 

### 3.1 Requisitos funcionales 

    Módulo 1: Gestión de usuarios. 

|ID      |Requisito           |
|--------|---------------------|
|RF-01   |El sistema debe permitir registrar usuarios clientes.
|RF-02   |El sistema debe permitir iniciar sesión mediante correo y contraseña.
|RF-03   |El sistema debe permitir recuperar contraseñas.|
|RF-04   |El usuario debe poder editar su perfil personal.|

    Módulo 2: Gestión de empresas.

|ID      |Requisito           |
|--------|---------------------|
|RF-05   |Las empresas deben poder registrarse en la plataforma.
|RF-06   |Deben poder configurar horarios de atención.|
|RF-07   |Deben poder administrar los servicios ofrecidos.|
|RF-08   |Deben poder gestionar profesionales asociados.|

    Módulo 3: Reserva de turnos. 

|ID      |Requisito           |
|--------|---------------------|
|RF-09   |El usuario debe poder buscar empresas por categoría.|
|RF-10   |El usuario debe visualizar horarios disponibles.|
|RF-11   |El usuario debe poder reservar un turno.|
|RF-12   |El sistema debe evitar reservas duplicadas.|
|RF-13   |El sistema debe generar un identificador único para cada turno.

    Módulo 4: Gestión de citas. 

|ID      |Requisito           |
|--------|---------------------|
|RF-14   |El usuario debe poder cancelar una cita.|
|RF-15   |El usuario debe poder reprogramar una cita.|
|RF-16   |El sistema debe actualizar automáticamente la disponibilidad.
|RF-17   |La empresa debe visualizar todas las reservas activas.

    Módulo 5: Notificaciones. 

|ID      |Requisito           |
|--------|---------------------|
|RF-18   |El sistema debe enviar confirmaciones automáticas.|
|RF-19   |Debe enviar recordatorios 24 horas antes del turno.|
|RF-20   |Debe informar cancelaciones o modificaciones.|
|RF-21   |Debe notificar cambios realizados por la empresa.


    Módulo 6: Reportes.

|ID      |Requisito           |
|--------|---------------------|
|RF-22   |Generar estadísticas de cancelaciones.|
|RF-23   |Generar estadísticas de cancelaciones.|
|RF-24   |Mostrar indicadores de ocupación.|

## 3.2 Requisitos no funcionales 

    Rendimiento
|ID      |Requisito           |
|--------|---------------------|
|RNF-01   |El sistema debe responder en menos de 3 segundos.|
|RNF-02   |Debe soportar al menos 1000 usuarios concurrentes.|

    Seguridad
|ID      |Requisito           |
|--------|---------------------|
|RNF-03   |Toda comunicación debe utilizar HTTPS.|
|RNF-04   |Las contraseñas deben almacenarse cifradas.|
|RNF-05   |Debe existir registro de auditoría para operaciones críticas.|

    Usabilidad
|ID      |Requisito           |
|--------|---------------------|
|RNF-06   |La interfaz debe ser intuitiva.|
|RNF-07   |Debe ser responsiva para móviles y computadoras.|
|RNF-08   |Debe cumplir criterios básicos de accesibilidad.|

    Disponibilidad
|ID      |Requisito           |
|--------|---------------------|
|RNF-09   |Disponibilidad mínima del 99%.|
|RNF-10   |Copias de seguridad automáticas diarias.|



## Caso de estudio

_Nombre del organismo o empresa comitente y contexto del problema que el sistema resuelve._

## Entregas

| Entrega | Descripción | Fecha | Estado |
|---------|-------------|-------|--------|
| EP-01 | Presentación preliminar | | |
| EP-02 | | | |
| Final | Versión definitiva | | |

## Estructura del repositorio

```
/
├── README.md
├── integrantes.md
├── RECURSOS.md         ← leer antes de empezar: prerrequisitos, cheatsheet de git, recursos
├── docs/
│   ├── requisitos.md
│   ├── historias-de-usuario.md
│   ├── casos-de-uso.md
│   ├── er-modelo.md
│   ├── diseño-ui.md
│   └── stakeholders.md
├── diagramas/
│   ├── casos-de-uso.puml
│   ├── er.puml
│   └── wireframes/
└── cuestionario/
```

## Instrucciones operativas

- Un integrante del grupo es responsable de subir los cambios al repositorio.
- Completar `integrantes.md` antes de la primera entrega.
- Mantener los archivos en la carpeta correspondiente según la estructura indicada.
- Los diagramas deben entregarse en formato PlantUML (`.puml`). Se pueden visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/).
