# Modelo Entidad-Relación

## Diagrama
@startuml
class EMPRESA {
  id_empresa
  nombre
  rubro
}

class SERVICIO {
  id_servicio
  nombre
  precio
}

class PROFESIONAL {
  id_profesional
  nombre
  especialidad
}

class TURNO {
  id_turno
  fecha_hora
  estado
}

class CLIENTE {
  id_cliente
  nombre
  telefono
}

'
EMPRESA "1" -- "N" SERVICIO : Ofrece
EMPRESA "1" -- "N" PROFESIONAL : Tiene
PROFESIONAL "1" -- "N" TURNO : Atiende
CLIENTE "1" -- "N" TURNO : Reserva
@enduml 

//www.plantuml.com/plantuml/png/RP2nRiCW441tlW9vwPIXQwTO4PKifNQ4tQxH1SuA4WRhW2vL_xt4AWb3Da3lwN6t3n4efiKoPI44BZvEKemj_s6S6tt6PIKCa2_EBnV2VA1q8Sz-sRym2ldL7RgX607fsoZZQsKbLAPoJd9u5sCtzEsnQ2lv6OFn3cnjObYp2jPeq0z_-fJzFHWJkLjjHdM5yzKJR5u4NHKFnqxqaoYEiWPTnBeKqUBitUQyiBA6vhNXknrl-kOnw1iVPaA5pw1giinDkOAiUglHROWCjJ5J6bdvuXD4ucshmFOPJ8lz0m00

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
