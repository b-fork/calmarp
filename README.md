# Calma RP

Plugin interno con los skills que usa el equipo de Calma RP (agencia
boutique de Prensa y RRPP) para el trabajo del día a día con clientes.

## Componentes

### Skills

| Skill | Qué hace |
|---|---|
| `resumen-ejecutivo` | Genera el informe de gestión mensual de RRPP y Prensa de un cliente, en PPTX. Dos formatos de referencia (Spazios: CPE/Ejes; Vittal: VAP/GNG News), logo de Calma RP en el deck salvo para Lendar y Finaer. |
| `clipping-diario` | Carga una mención de prensa nueva en la planilla de seguimiento del cliente en Google Drive, clasificada por tier/alcance/soporte, con confirmación antes de escribir. |
| `brief-vocero` | Arma una hoja de preparación de una página para un vocero antes de una entrevista: mensajes clave, preguntas probables e incómodas, frases puente, temas a evitar. |

## Setup

No requiere variables de entorno propias. `clipping-diario` y la lectura de
la planilla de Vittal en `resumen-ejecutivo` usan el conector de Google
Drive del usuario — tiene que estar conectado en Claude para que estas
partes funcionen.

## Uso

Cada skill se activa solo al detectar el pedido correspondiente en el chat
(ver la `description` de cada `SKILL.md`) — no hace falta invocarlos por
nombre. Ejemplos:

- "Armá el informe de julio para Spazios" → `resumen-ejecutivo`
- "Sumá esta nota a la planilla de Vittal" → `clipping-diario`
- "Necesito un brief para la entrevista de mañana con Clarín" → `brief-vocero`

## Clientes conocidos

Ver la sección "Clientes conocidos" en `skills/resumen-ejecutivo/SKILL.md`
para el estado de cada cliente (formato de reporte, fuente de métricas, si
lleva o no el logo de Calma RP). Se actualiza a medida que se suman
clientes nuevos o cambian sus convenciones.
