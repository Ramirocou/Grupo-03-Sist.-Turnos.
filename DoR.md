# Definition of Ready (DoR)

_Antes de que una historia entre a desarrollo, tiene que pasar un filtro: el Definition of
Ready. Es un acuerdo del equipo sobre qué condiciones mínimas debe cumplir una historia para
considerarse "lista para trabajar". Si no las cumple, vuelve a refinamiento._

---

## Checklist del equipo

_Entre 6 y 10 ítems. Cada uno redactado como una condición verificable ("la historia tiene
criterios de aceptación escritos"), no como un deseo ("la historia está bien definida")._

## Checklist del equipo

| # | Ítem | Justificación (qué problema evita) |
|---|------|--------------------------------------|
| 1 | ¿Tiene al menos 2 criterios de aceptación verificables (se responde sí/no sin discutir)? | Evita que "listo" sea subjetivo; sin criterios, dev y QA discuten qué significa terminar la historia. |
| 2 | ¿El actor/rol está identificado sin ambigüedad? | Evita implementar permisos o flujos para el usuario equivocado. |
| 3 | ¿Hay al menos un flujo alternativo o excepción documentado en la propia historia? | Evita que el desarrollo solo contemple el "camino feliz" y se descubran casos borde recién en QA. |
| 4 | ¿Están listadas las dependencias con otras historias o módulos? | Evita bloqueos a mitad de sprint por depender de algo que todavía no existe. |
| 5 | ¿Hay al menos un requisito no funcional relevante asociado a esta historia puntual? | Evita entregar algo funcionalmente correcto pero inutilizable (lento, inseguro, etc.). |
| 6 | ¿El equipo puede estimar el esfuerzo sin preguntas abiertas de diseño? | Evita que el equipo "adivine" el alcance y sub/sobre-estime la tarea. |
| 7 | ¿Están especificados los datos de entrada/salida necesarios? | Evita idas y vueltas entre frontend/backend por formatos de datos no acordados. |
| 8 | ¿Hay una forma concreta de verificar que la historia está terminada? | Evita cierres prematuros de historias que en realidad no cumplen lo pedido. |

---

## Aplicación a tres historias propias

_Elijan TRES historias de usuario de su propio trabajo del primer semestre y pásenlas por su
propia checklist. Es esperable —y deseable— que alguna no pase._

### Historia 1 — HU-01 Reservar Turno

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 | Sí | — |
| 2 | Sí | — |
| 3 | Sí | — |
| 4 | Sí | — |
| 5 | Sí | - |
| 6 | Sí | — |
| 7 | Sí | — |
| 8 | Sí | — |

---

### Historia 2 — HU-03 Reprogramar Turno

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 | Sí | — |
| 2 | Sí | — |
| 3 | Sí | — |
| 4 | Sí | — |
| 5 | Sí | — |
| 6 | Sí | — |
| 7 | Sí | — |
| 8 | Sí | — |

**8 de 8 → pasa.**

---

### Historia 3 — HU-05 Consultar Agenda

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 | Sí | — |
| 2 | Sí | — |
| 3 | Sí | — |
| 4 | Sí | - |
| 5 | Si | - |
| 6 | Sí | — |
| 7 | Sí | - |
| 8 | Sí | - |

---

