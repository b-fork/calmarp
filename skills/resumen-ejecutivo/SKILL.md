---
name: resumen-ejecutivo
description: "Genera el resumen ejecutivo mensual / informe de gestión de RRPP y Prensa que Calma RP entrega a cada uno de sus clientes (ej. Spazios, Vittal, Lendar, Finaer), en formato PPTX. Usar siempre que el usuario pida armar el 'informe mensual', 'resumen ejecutivo', 'reporte de gestión', 'informe de prensa y RRPP' o el 'PPT del mes' para un cliente de Calma RP, incluso si no lo pide explícitamente como 'skill' o 'presentación'. Cubre la portada con el logo del cliente (y el de Calma RP, salvo para Lendar y Finaer), el bloque de KPIs (repercusiones por tier, CPE o VAP, audiencia alcanzada), las secciones de gestión (prensa, RRPP, government affairs, redes, etc.), las notas/publicaciones destacadas — leyéndolas de la planilla de Drive para Vittal, y complementándolas con búsquedas web de coberturas adicionales no listadas por el usuario — y el cierre, todo en la paleta de marca de Calma RP (calmarp.com)."
---

# Resumen ejecutivo mensual — Calma RP

Genera el informe de gestión mensual (RRPP & Prensa) que Calma RP entrega a
cada cliente, como archivo `.pptx`. Esta skill tiene dos modelos de
referencia reales, y el formato varía según el cliente — no forzar el
esqueleto de uno en el otro:

- **Spazios** (real estate, ver `Estructura de slides`): Overall con
  "Contenidos y gestiones" + repercusiones por tier + caja de **CPE**
  (Costo Publicitario Equivalente) y audiencia alcanzada, sourceada de
  **Ejes**. Detalle en secciones "RRPP" y "Government affairs".
- **Vittal** (salud): "Resumen Ejecutivo" con notas gestionadas + tier +
  contenidos, seguido de "¿Qué hicimos este mes?" (bullets de gacetillas y
  contenidos core) y luego "Repercusiones del mes": un párrafo narrativo
  resumiendo lo más destacado, y **una slide por repercusión individual**
  (título de la nota, medio, fecha, link "Ir a la nota", alcance —
  Nacional/Local—, soporte —online/gráfica/radio/etc.). La métrica de
  valor publicitario para Vittal es **VAP** (Valor Actual Publicitario),
  no CPE, y sale de la agencia de clipping **GNG News**, no de Ejes.

**No mezclar las convenciones**: el nombre de la métrica (CPE vs VAP), la
fuente (Ejes vs GNG News u otra agencia de clipping) y el formato de "Top
Publications" (grid de capturas por tier vs. una slide por nota con datos)
dependen del cliente. Si es un cliente nuevo sin modelo previo, preguntarle
al usuario cuál de los dos formatos seguir, o si el cliente tiene su propio
modelo.

## Clientes conocidos

| Cliente | Formato | Métrica / fuente | Logo Calma RP en el deck |
|---|---|---|---|
| Spazios | Variante Spazios | CPE / Ejes | Sí |
| Vittal | Variante Vittal | VAP / GNG News | Sí |
| Lendar | — (sin modelo previo aún) | — | **No, nunca** |
| Finaer | — (sin modelo previo aún) | — | **No, nunca** |

**Por defecto, todos los informes llevan el isologo de Calma RP** (chico,
como firma de agencia — ver [Estructura de slides](#estructura-de-slides))
junto con el logo del cliente. **Lendar y Finaer son la única excepción
confirmada**: para esos dos, no va el logo de Calma RP en ningún lado del
deck (ni portada, ni header interior, ni cierre) — el resto de los clientes
sí lo lleva. Si aparece un cliente nuevo sin instrucción explícita, incluir
el logo de Calma RP por defecto (el comportamiento estándar) y preguntar
solo si el usuario indica lo contrario.

Lendar y Finaer todavía no tienen un modelo de referencia cargado en esta
skill — al armarles el primer informe, preguntar al usuario qué variante seguir
(Spazios o Vittal) o pedir un modelo/ejemplo previo de esos clientes.

## Antes de empezar — reunir estos datos

Cada informe es **por cliente y por mes**. Antes de generar el PPT, confirmar
con el usuario (o inferir de lo que ya pegó/adjuntó en el chat):

1. **Cliente y período** — nombre del cliente, mes y año (ej. "Spazios,
   Julio 2026").
2. **Logo del cliente** — archivo de imagen. Va SIEMPRE en la portada y,
   en tamaño chico, en el encabezado de cada slide interior (ver
   [Estructura de slides](#estructura-de-slides)). Si el usuario no lo adjuntó
   en este chat, pedirlo antes de armar la portada — no dejar la portada
   sin logo ni usar un logo de otro cliente. El logo de **Calma RP va
   también**, chico, salvo para Lendar y Finaer (ver
   [Clientes conocidos](#clientes-conocidos)).
3. **Foto de tapa** — una imagen del cliente/proyecto para la portada
   (en el modelo: foto del edificio). Si no hay una, usar un fondo sólido
   en la paleta de Calma RP en vez de inventar o buscar una imagen del
   cliente.
4. **Datos de gestión del mes** — lo que el usuario pega en el chat o sube como
   archivo (Word/Excel/notas): contenidos y gestiones realizadas, agenda de
   government affairs, propuestas de eventos/PR, redes, etc. Las secciones
   reales varían por cliente (un cliente de real estate tiene "Government
   affairs" y "Municipalidad"; otro puede no tenerlo) — **no fuerces
   secciones que el cliente no tiene**, seguí lo que el usuario provea.
5. **KPIs de repercusiones** — cantidad total de menciones/repercusiones y
   su desglose por tier (Tier 1/2/3 según el medio).
6. **Valor publicitario y audiencia** — el nombre de la métrica y su fuente
   varían por cliente (ver arriba: CPE/Ejes para Spazios, VAP/GNG News para
   Vittal). Pueden llegar como PDF/Excel exportado de la agencia de
   clipping, captura de pantalla, o texto pegado por el usuario. Si el cliente
   no tiene esta métrica, omitir el bloque en vez de completarlo con
   estimaciones propias — nunca inventes CPE, VAP ni audiencia.
7. **Notas publicadas / capturas** — para la sección de repercusiones
   individuales o "Top Publications":
   - **Vittal**: las notas publicadas viven en una **planilla de Google
     Drive compartida** (no en el chat). Buscarla con
     `Google Drive:search_files` (por título "Vittal" o contenido del mes,
     ej. "Vittal Envíos PR 2026") y leerla con `read_file_content` antes de
     pedirle nada al usuario. Si la búsqueda no la encuentra, pedirle el link
     directo en vez de reconstruir los datos de memoria — no inventar
     medios, fechas ni links de notas.
   - **Otros clientes**: si el usuario no adjuntó capturas, dejar los datos de
     texto (medio, título, link) y avisar que faltan las capturas, en vez
     de inventar el diseño de una nota que no viste.
   - **Búsqueda web complementaria**: además de los links de notas que
     el usuario provea (pegados, Drive, etc.), usar `web_search` para buscar
     coberturas adicionales del cliente en medios durante el mes del
     informe (ej. `"[Cliente]" [mes] [año]`, o el nombre de un vocero/tema
     puntual que el usuario haya mencionado). El objetivo es **sumar notas que
     el usuario no haya listado**, no reemplazar ni validar las que ya dio.
     - Antes de agregar un resultado encontrado por web search a la
       sección de repercusiones, chequear que no sea un duplicado de algo
       ya provisto (mismo medio + mismo tema).
     - Clasificar cada hallazgo nuevo (tier, alcance, soporte) con el
       mismo criterio que el resto de las notas del mes, si el usuario ya
       definió esos criterios; si no, dejarlo sin clasificar y avisar.
     - Antes de incluir un hallazgo de búsqueda web en el PPT final,
       mostrárselo al usuario (medio, título, fecha, link) para que lo
       confirme — no agregarlo directo al deck sin su visto bueno, porque
       una búsqueda puede traer notas de otro proyecto del cliente, texto
       ambiguo, o coincidencias de nombre que no son coberturas reales.
     - Si la búsqueda no encuentra nada adicional, seguir con lo que el usuario
       proveyó — no forzar resultados de baja relevancia solo para sumar
       más notas.

Si falta el logo del cliente o los datos del mes, pedirlos antes de generar
el archivo — no completar el informe con contenido de relleno.

## Estructura de slides

Dos esqueletos reales según el cliente (ver arriba). Usar el que corresponda
al cliente; si es nuevo, preguntar cuál seguir.

### Variante Spazios (real estate / KPIs consolidados)

1. **Portada** — Foto de tapa a página completa con overlay oscuro sutil.
   Título centrado: "INFORME DE GESTIÓN / RRPP & PRENSA / [CLIENTE]" y debajo
   "[MES] [AÑO]". **Logo del cliente** en la portada (arriba, centrado o
   esquina — nunca ausente). **Logo de Calma RP** chico en una esquina de
   la portada (salvo Lendar/Finaer, ver [Clientes conocidos](#clientes-conocidos)).
2. **Overall (KPIs del mes)** — encabezado con nombre del cliente arriba a
   la izquierda (texto chico) y logo del cliente arriba a la derecha, en
   cada slide interior. Logo de Calma RP chico en el footer (salvo
   Lendar/Finaer). Tres bloques en la misma slide:
   - *Contenidos y gestiones*: bullets de lo realizado en el mes.
   - *Repercusiones*: número total grande + desglose "X en Tier 1 / Y en
     Tier 2 / Z en Tier 3".
   - *CPE y audiencia* (solo si aplica): caja destacada con el monto y la
     audiencia alcanzada, cada uno con una línea explicando qué mide la
     métrica.
3. **Detalle por área** (una o más slides, según el cliente) — secciones
   con subtítulo + bullets: puede ser "RRPP" (propuestas de contenido,
   eventos, PR), "Government affairs", "Redes", "Influencers", etc. Seguir
   los datos que el usuario provea; no todos los clientes tienen las mismas
   áreas.
4. **Top Publications** — una slide por tier relevante (al menos Tier 1).
   Grid de capturas de las notas reales, agrupadas visualmente por medio
   (si hay logo del medio, incluirlo). Si no hay capturas, listar medio +
   título + link en su lugar.
5. **Cierre** — imagen a página completa (idealmente del cliente) con
   "MUCHAS GRACIAS" o el cierre que el usuario prefiera.

### Variante Vittal (salud / repercusión por slide)

1. **Portada** — igual que en la variante Spazios: título "Reporte mensual
   / PR - [MES] [AÑO]" y **logo del cliente** siempre presente, más el
   **logo de Calma RP** chico (salvo Lendar/Finaer).
2. **Resumen Ejecutivo** — nota al pie con la fuente de la métrica de valor
   publicitario (ej. "*VAP Valor Actual Publicitario (Fuente agencia
   clipping, GNG News)"), y debajo: notas gestionadas (número grande) +
   desglose por tier + cantidad de contenidos.
3. **¿Qué hicimos este mes?** — bullets de gacetillas/contenidos core del
   mes, y un bullet aparte de difusión/gestión de agenda periodística.
4. **Repercusiones del mes** — slide divisoria simple (solo título).
5. **Repercusiones gestionadas del mes** — párrafo narrativo que resume qué
   salió, en qué medios, y cuántos medios adicionales replicaron cada tema.
6. **Una slide por repercusión** — leída de la planilla de Drive (ver
   [Antes de empezar](#antes-de-empezar--reunir-estos-datos)): título de la
   nota ("Destacada: ..."), medio, fecha, link "Ir a la nota", alcance
   (Nacional/Local), soporte (online/gráfica/radio/etc.).
7. **Cierre** — igual que en la variante Spazios.

## Paleta de colores — Calma RP

Extraída del isologo de Calma RP (`assets/calmarp-logo.png`) y de
calmarp.com. Es una paleta neutra y cálida, deliberadamente sobria — no
inventar colores saturados fuera de esta lista:

| Uso | Color | Hex |
|---|---|---|
| Texto principal / marca (títulos, "Calma RP") | Taupe cálido | `5F5654` |
| Texto de cuerpo | Gris carbón | `2E2A28` |
| Fondo principal | Blanco | `FFFFFF` |
| Fondo secundario / cards | Beige claro | `EDE7E3` |
| Líneas divisorias / texto secundario | Gris medio | `8C8481` |

**El color de acento para destacar KPIs (la caja de CPE/audiencia en el
modelo original usa azul) se reemplaza por el color de marca del cliente**
cuando se conoce (ej. el verde de Spazios), no por un azul genérico. Si no
se conoce el color del cliente, usar el taupe `5F5654` como acento.

Tipografía: un sans-serif geométrico y limpio (Century Gothic o similar es
lo que usa el modelo original) para títulos; un sans-serif neutro (Arial,
Calibri) para cuerpo de texto y bullets.

## Cómo generarlo

1. Leer `/mnt/skills/public/pptx/SKILL.md` (skill de pptx) antes de escribir
   código — trae las gotchas de pptxgenjs (layout 16:9, colores sin `#`,
   bullets, etc.) que aplican acá también.
2. Confirmar los inputs de la sección anterior. Si falta el logo del
   cliente o los datos del mes, pedirlos antes de seguir. Para **Vittal**,
   buscar primero la planilla de notas publicadas en Google Drive
   (`Google Drive:search_files` → `read_file_content`) antes de pedirle
   nada al usuario — es la fuente de la sección de repercusiones, no algo que
   él vaya a pegar en el chat.
3. Complementar con `web_search` (ver
   [Antes de empezar, punto 7](#antes-de-empezar--reunir-estos-datos)) para
   encontrar coberturas del mes que el usuario no haya listado, y confirmarlas
   con él antes de sumarlas al deck.
4. Construir el PPTX con `pptxgenjs`, `pres.layout = 'LAYOUT_WIDE'` (16:9,
   igual que el modelo: 720×405pt ≈ 13.33"×7.5"), siguiendo la estructura de
   slides de arriba y la paleta de colores de Calma RP.
5. En cada slide interior, insertar el logo del cliente en la esquina
   superior derecha (chico) y el nombre del cliente en texto chico arriba a
   la izquierda, replicando el header del modelo. Insertar también el logo
   de Calma RP chico (esquina inferior, como firma de agencia) en portada y
   slides interiores — **excepto para Lendar y Finaer**, donde no va en
   ningún lado del deck.
6. Correr la QA de la skill de pptx: `markitdown` para chequear contenido
   (sin texto de relleno tipo "Lorem" o "[insert]"), `scripts/office/validate.py`,
   y conversión a imágenes para inspección visual de cada slide (overflow de
   texto, logo del cliente bien posicionado, contraste de texto).
7. Guardar el archivo final como `[Cliente] - Informe de Gestión [Mes] [Año].pptx`
   y entregarlo con `present_files`.

## Assets incluidos en esta skill

- `assets/calmarp-logo.png` — isologo de Calma RP. Se usa como referencia
  de paleta de marca Y se inserta chico en el deck (portada + footer
  interior) en todos los clientes **excepto Lendar y Finaer**. El logo
  grande y protagonista de la portada es siempre el del **cliente**.
