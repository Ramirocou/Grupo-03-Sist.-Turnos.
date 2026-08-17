# Modelo Entidad-Relación

## Diagrama

_Incluir el código PlantUML en `diagramas/er.puml`._
_Visualizar en [plantuml.com](https://www.plantuml.com/plantuml/uml/)._

## Entidades

| Entidad | Descripción | Relaciones clave |
|---------|-------------|-----------------|
| | | |
| | | |
| | | |

## Descripción de atributos principales

_Para cada entidad, describir brevemente los atributos más relevantes y su propósito._

### [Empresa]

- `Id_empresa` (PK): 
- `Nombre`:
- `Rubro`:

### [Servicio]

- `Id_servicio` (PK):
- `id_empresa` (FK): 
- `nombre`:
- `precio`:

### [Profesional]

- `Id_profesional` (PK):
- `id_empresa` (FK):
- `nombre`:
- `especialidad`:


### [Turno]

- `Id_turno` (PK):
- `id_profesional` (FK) :
- `id_cliente` (FK) :
- `fecha_hora`:
- `estado`:

### [Cliente]

- `id_cliente` (PK): 
- `nombre`:
- `teléfono`:
## Decisiones de diseño

● Duración Dinámica basada en el Servicio: En lugar de estructurar la base de datos con bloques de tiempo fijos (ej. siempre de 30 minutos), la tabla TURNO se calcula dinámicamente usando la columna duracion_minutos de la tabla SERVICIO. Esto permite que un profesional ofrezca un servicio de 15 minutos y seguidamente uno de 60 minutos sin romper la consistencia de la agenda. 
● Estrategia Multitenancy Dinámica: Cada tabla crítica (Profesional, Servicio, Turno) cuenta con una clave foránea empresa_id. Esto garantiza el aislamiento lógico de los datos, impidiendo de forma estricta que la Empresa A acceda a las agendas o clientes de la Empresa B.


### Decisión 1 — [Título]

### Decisión 2 — [Título]
