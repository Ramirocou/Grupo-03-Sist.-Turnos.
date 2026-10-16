# Definition of Ready (DoR)

_Antes de que una historia entre a desarrollo, tiene que pasar un filtro: el Definition of
Ready. Es un acuerdo del equipo sobre qué condiciones mínimas debe cumplir una historia para
considerarse "lista para trabajar". Si no las cumple, vuelve a refinamiento._

---

## Checklist del equipo

_Entre 6 y 10 ítems. Cada uno redactado como una condición verificable ("la historia tiene
criterios de aceptación escritos"), no como un deseo ("la historia está bien definida")._

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

### Historia 1 — HU-03 Reprogramar Turno

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 — Criterios de aceptación | Sí | — |
| 2 — Actor identificado | Sí | — |
| 3 — Flujo alternativo | Sí | — |
| 4 — Dependencias | Sí | — |
| 5 — RNF asociado | No | El RNF-01 pide responder en menos de 3 s, pero la observación de la historia exige menos de 2 s para la misma operación. Son dos valores para lo mismo; falta fijar un único tiempo (por ejemplo, RNF-01 < 3 s por paso y < 2 s para la transacción completa). |
| 6 — Estimable sin dudas de diseño | Sí | — |
| 7 — Datos de entrada/salida | Sí | — |
| 8 — Forma de verificar | Sí | — |

**7 de 8 → no pasa.**

---

### Historia 2 — HU-05 Consultar Agenda

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 — Criterios de aceptación | Sí | — |
| 2 — Actor identificado | Sí | — |
| 3 — Flujo alternativo | Sí | — |
| 4 — Dependencias | Sí | — |
| 5 — RNF asociado | Sí | — |
| 6 — Estimable sin dudas de diseño | No | El criterio 5 dice que la agenda "se actualiza automáticamente", pero no define cómo: si es en tiempo real (push) o con refresco periódico. Es una pregunta de diseño abierta que cambia mucho el esfuerzo. |
| 7 — Datos de entrada/salida | Sí | — |
| 8 — Forma de verificar | Sí | — |

**7 de 8 → no pasa.**

---

### Historia 3 — HU-11 Generar reportes de gestión

| Ítem (según checklist) | ¿Pasa? | Qué le falta (si no pasa) |
|-------------------------|--------|-----------------------------|
| 1 — Criterios de aceptación | Sí | — |
| 2 — Actor identificado | Sí | — |
| 3 — Flujo alternativo | No | Las excepciones están sin definir (por ejemplo, qué se muestra si no hay turnos en el período). |
| 4 — Dependencias | Sí | — |
| 5 — RNF asociado | No | No tiene ningún RNF asociado (tiempo de generación, volumen de datos). |
| 6 — Estimable sin dudas de diseño | No | Falta definir cómo se calcula el indicador de ocupación, si "atendido" equivale al estado que registra HU-13, si el reporte incluye los montos de señas y si RF-22, RF-23 y RF-24 van en una o en tres historias. |
| 7 — Datos de entrada/salida | No | Período, filtros y formato de salida (pantalla, PDF o Excel) sin definir. |
| 8 — Forma de verificar | No | El criterio 3 (indicador de ocupación) no se puede probar mientras no exista la fórmula. |

**3 de 8 → no pasa.**

—-
