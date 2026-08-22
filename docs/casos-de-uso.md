# Casos de uso

## Diagrama general

_Incluir el código PlantUML en `diagramas/casos-de-uso.puml`._
_Visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/)._

_Describir brevemente los actores identificados y las relaciones principales (include, extend)._

---

## CU-01 — [Reservar Turno]

| Campo | Detalle |
|-------|---------|
| Identificador | CU-01 |
| Nombre | Reservar Turno |
| Descripción | El cliente, una vez autenticado, selecciona una empresa, un servicio, un profesional y un horario disponible en un calendario visual para reservar un turno. El sistema bloquea el horario, registra la reserva y genera una confirmación por correo. |
| Actores | Principal: Cliente / Secundario: Sistema de Notificaciones. |
| Precondiciones | 1. El cliente posee una cuenta registrada y sesión activa (Módulo 1).
2. La empresa tiene servicios activos y al menos un profesional con disponibilidad cargada (CU-004).
3. Existen horarios disponibles para el servicio/profesional seleccionado. |
| Postcondiciones | Éxito: / Fallo: |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | | |
| 2 | | |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | | |

| Campo | Detalle |
|-------|---------|
| Rendimiento | |
| Frecuencia | |
| Importancia | |
| Urgencia | |

---

## CU-02 — [Nombre]

| Campo | Detalle |
|-------|---------|
| Identificador | CU-02 |
| Nombre | |
| Descripción | |
| Actores | Principal: / Secundario: |
| Precondiciones | |
| Postcondiciones | Éxito: / Fallo: |

### Secuencia normal

| # | Acción (actor) | Reacción (sistema) |
|---|----------------|--------------------|
| 1 | | |
| 2 | | |

### Excepciones

| # | Situación | Respuesta del sistema |
|---|-----------|-----------------------|
| E1 | | |

| Campo | Detalle |
|-------|---------|
| Rendimiento | |
| Frecuencia | |
| Importancia | |
| Urgencia | |
