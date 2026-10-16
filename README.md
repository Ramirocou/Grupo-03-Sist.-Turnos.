# Sistema de Turnos — Grupo Oozma Kappa

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
_El sistema cubrirá los siguientes procesos:_

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
11. Cobro y devolución de señas al reservar, mediante una pasarela de pagos externa.
12. Registro de la asistencia del cliente por parte del profesional. 

_El sistema no cubrirá los siguientes procesos:_

 * Gestión contable de las empresas. 

 * Facturación electrónica. 

 * Administración de recursos humanos. 

 * Sistemas médicos específicos (historias clínicas). 

 * Procesamiento de pagos online en general: el único pago que procesa el sistema es la seña de la reserva (cobro y devolución).

    
### 1.3 Contexto tecnológico y decisiones relevantes 
_Durante el relevamiento se identificaron las siguientes necesidades:_

 * Acceso desde dispositivos móviles y computadoras. 

 * Actualización en tiempo real de disponibilidad. 

 * Notificaciones automáticas mediante correo electrónico y aplicación móvil. 

 * Soporte para múltiples empresas dentro de la misma plataforma. 

 * Escalabilidad para incorporar nuevos rubros en el futuro. 

_La solución estará compuesta por:_

 * Aplicación móvil para clientes. 

 * Plataforma web para empresas. 

 * Base de datos centralizada. 

 * Sistema de notificaciones automáticas. 
 
 * API de integración con calendarios externos.
   
 * Integración con pasarela de pagos externa, que procesa los cobros y las devoluciones de señas. El sistema nunca guarda datos de tarjeta.

Decisión de alcance (cambio del 09/10/26): el cliente pidió que la reserva exija una seña, por lo que el pago online pasa a formar parte de la primera versión, limitado a cobrar y devolver señas. El detalla esta en la issue #1. 

| Entrega | Fecha | Qué incluye | Archivos |
|---|---|---|---|
| Inicio del proyecto | 12/08/2026 | Repositorio, nombre y número del grupo | `README.md` |
| Descripción del sistema | 14/08/2026 | Origen, alcance y contexto del proyecto | `README.md` |
| Modelo de datos | 16/08/2026 y 17/08/2026 | Entidades y relaciones del modelo ER | `docs/er-modelo.md` |
| Requisitos y stakeholders | 17/08/2026 y 22/08/2026 | Requisitos funcionales y no funcionales, módulo agregado e identificación de stakeholders | `docs/requisitos.md`, `docs/stakeholders.md` |
| Historias de usuario | 14/08/2026 al 02/09/2026 | Historias con criterios de aceptación e INVEST, y correcciones | `docs/historias-de-usuario.md` |
| Definition of Ready | 22/08/2026 y 02/09/2026 | Checklist DoR | `DoR.md` |
| Casos de uso | 22/08/2026 y 18/09/2026 | Casos de uso del 1 al 8 y diagrama | `docs/casos-de-uso.md` |
| Diseño de UI | | Pantallas y wireframes | `docs/diseño-ui.md`, `diagramas/wireframes/` |
| Devoluciones del docente | 28/08/2026 al 28/09/2026 | Devoluciones y revisiones del profesor | `feedback/` |
| Cambio de alcance | 09/10/2026 | Seña al reservar | Issue #1 |
| Ajustes por devolución del docente | 15/10/2026 | Historias de usuario, README y DoR actualizados | `docs/historias-de-usuario.md`, `README.md`, `DoR.md`, `slicing.md` |


## Estructura del repositorio

```
/
├── README.md
├── integrantes.md
├── RECURSOS.md         ← leer antes de empezar: prerrequisitos, cheatsheet de git, recursos
├── DoR.md
├── slicing.md
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
├── cuestionario/
└── feedback/

## Instrucciones operativas

- Un integrante del grupo es responsable de subir los cambios al repositorio.
- Completar `integrantes.md` antes de la primera entrega.
- Mantener los archivos en la carpeta correspondiente según la estructura indicada.
- Los diagramas deben entregarse en formato PlantUML (`.puml`). Se pueden visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/).
