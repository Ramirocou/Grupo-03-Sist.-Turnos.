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
