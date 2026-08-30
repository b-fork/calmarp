---
name: clipping-diario
description: >
  This skill should be used when the user wants to log a new press mention
  into a client's tracking spreadsheet — trigger phrases include "sumá esto
  al clipping de [cliente]", "encontré esta nota de [cliente]", "cargá esto
  a la planilla", "clipping de hoy", or when the user pastes a link,
  screenshot, or description of a media mention and wants it tracked. Reads
  and appends rows to the client's Google Drive spreadsheet (the same source
  the monthly executive-summary skill reads from), classifying each mention
  by tier, reach, and format, and always asks for confirmation before
  writing to the spreadsheet.
metadata:
  version: "0.1.0"
---

# Clipping diario → planilla

Registra una mención de prensa nueva en la planilla de seguimiento del
cliente en Google Drive, para que quede lista cuando se arme el resumen
ejecutivo del mes (ver el skill `resumen-ejecutivo` de este mismo plugin,
que lee de la misma planilla para Vittal).

## Cuándo se usa

El usuario pega un link, describe una nota, o adjunta una captura de una
mención de un cliente en un medio, y quiere que quede cargada. Puede ser
una nota por vez o varias juntas (ej. el clipping del día).

## Antes de cargar nada

1. **Cliente** — a qué cliente pertenece la mención. Si no es obvio del
   contexto, preguntar.
2. **Ubicar la planilla del cliente en Drive** — buscarla con
   `Google Drive:search_files` (por nombre del cliente + "envíos" o
   "clipping" o el año en curso). Si no se encuentra, no inventar una
   estructura de columnas — pedirle el link directo al usuario o preguntar
   si ese cliente todavía no tiene planilla armada.
3. **Leer la planilla existente** con `Google Drive:read_file_content` para
   ver las columnas reales que usa (no todos los clientes tienen las mismas:
   ver el skill `resumen-ejecutivo` para el ejemplo de columnas de Vittal —
   medio, fecha, link, alcance, soporte). Replicar esa estructura, no
   inventar columnas nuevas.

## Clasificar la mención

Para cada nota nueva, extraer o inferir del link/captura/texto que dio el
usuario:

- Medio
- Título de la nota
- Fecha de publicación
- Link
- Tier (según el criterio que ya use ese cliente en la planilla — mirar
  cómo clasificó menciones anteriores del mismo medio si están cargadas)
- Alcance (Nacional/Local) y soporte (online/gráfica/radio/TV), si la
  planilla del cliente usa esas columnas

Si algún dato no se puede determinar con confianza (ej. el tier de un medio
nuevo que no apareció antes en la planilla), dejarlo en blanco y avisarle al
usuario en vez de adivinar.

## Chequeo de duplicados

Antes de agregar la fila, comparar contra las filas existentes (mismo medio
+ mismo título o mismo link) para no cargar la misma nota dos veces. Si
parece un duplicado, avisar y preguntar antes de agregarla igual.

## Confirmar antes de escribir

Mostrarle al usuario la fila que se va a agregar (medio, título, fecha,
tier, link) **antes** de escribirla en la planilla — es una edición de un
documento compartido con el resto del equipo, así que no se escribe sin
confirmación explícita, incluso si el usuario ya aprobó cargar "todo el
clipping de hoy" de entrada. Una vez confirmado, usar
`Google Drive:update_file` (o el método que corresponda para agregar filas)
para escribir.

## Después de cargar

Confirmar qué se agregó (fila y planilla), y no dar por hecho que hay que
regenerar el resumen ejecutivo del mes — eso es un skill aparte que el
usuario dispara cuando lo necesita.
