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
_El sistema cubrirá los siguientes procesos:._

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

_El sistema no cubrirá los siguientes procesos:._

 * Gestión contable de las empresas. 

 * Facturación electrónica. 

 * Administración de recursos humanos. 

 * Sistemas médicos específicos (historias clínicas). 

 * Procesamiento de pagos online en general: el único pago que procesa el sistema es la seña de la reserva (cobro y devolución).

    
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
   
 * Integración con pasarela de pagos externa, que proceso los cobros y las devoluciones de cosas y las devoluciones de señas. El sistema nuca guarda datos de tarjeta.

Decisión de alcance (cambio del 09/10/26): el cliente pidió que la reserva exija una seña, por lo que el pago online pasa a formar parte de la primera versión, limitado a cobrar y devolver señas. El detalla esta en la issue #1. 


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
