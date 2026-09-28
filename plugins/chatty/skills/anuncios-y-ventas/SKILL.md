---
name: anuncios-y-ventas
description: Usala cuando el dueño pregunte por sus anuncios o campañas de Meta (Facebook, Instagram, click-to-WhatsApp) y lo que le dejan — qué anuncios traen más leads o consultas, qué anuncio vende, cuáles traen ventas, costo por lead, costo por venta, ROAS, si los que más leads traen son los que más venden, en qué anuncio poner la plata o cuál apagar. Cruza lo que pasó en el WhatsApp del negocio (conector de Chatty, por anuncio) con el gasto de cada anuncio (conector de Meta Ads), unidos por el id del anuncio, y termina en una tabla, un veredicto y sugerencias concretas. Sin mandarle al dueño a buscar nada en Chatty.
---

# Qué anuncios traen leads y cuáles traen ventas

La pregunta que el dueño hace es casi siempre la misma, con otras palabras: *«¿qué anuncios me traen más consultas, cuáles terminan en venta, cuánto me sale cada una, y coinciden?»*. Se contesta con dos o tres llamadas y una cuenta. Lo difícil no es la cuenta: es no inventar lo que no está y no mandar al dueño a buscar lo que las tools ya tienen.

**Regla que no se negocia: nunca le pidas al dueño que se fije algo en Chatty** (el nombre de una fuente, qué etapa es la venta, cuántos llegaron a tal paso). Si lo necesitás, está en el conector. Lo único que el dueño sabe y los datos no dicen es **qué cuenta como venta en su negocio**, y eso se le pregunta UNA vez, bien armado, y queda guardado.

## 1. Qué tipo de cuenta es

`cuenta_ver()` te dice de dónde salen las ventas antes de buscarlas:

- **`chatty_completo`**: la empresa usa embudos o carga ventas en Chatty. La venta es una etapa, una etiqueta o un registro de venta.
- **`solo_conector`**: no hay nada de eso cargado. La venta sale de leer las conversaciones (paso 3b), y no hay nada que preguntarle al dueño: el informe cuenta solo lo que marques.

Mirá también `definicion_de_venta`: si ya está, el dueño ya contestó la pregunta de la venta y no se la volvés a hacer. En una cuenta `solo_conector` viene con origen `auto_solo_conector`: cuentan las ventas cargadas en Chatty y la etiqueta «Venta (detectada por Claude)», sin pregunta de por medio.

## 2. El informe del período

`anuncios_resultados` con el período que nombró el dueño (`dias`, o `desde`/`hasta`). Si no nombró ninguno, 30 días, y decí cuál usaste. Si vas a cruzar con Meta, pasá en `tz` la zona horaria de la cuenta publicitaria (ver paso 4) para que los dos lados cuenten los mismos días.

Qué trae cada fila de `anuncios`, y cómo leerla:

- **`leads_nuevos`**: chats que empezaron en el período, atribuidos al **primer** anuncio que los trajo. Una plantilla de reenganche posterior no le roba el lead al anuncio.
- **`leads_que_vuelven`**: gente que ya había escrito antes y volvió por un anuncio en el período. Meta también la cuenta como conversación, así que para el costo por lead se suma.
- **`etapas`**: cuántos de esos leads llegaron **alguna vez** a cada etapa del embudo (no dónde están hoy). Los nombres de las etapas están en `candidatas_de_venta`, con el mismo `id`.
- **`ventas`** e **`ingresos`**: según la definición de venta. Los ingresos vienen por moneda: nunca sumes monedas distintas.

Y además:

- **`sin_anuncio`**: lo que no vino de un anuncio identificado (orgánico, un anuncio que Meta no dijo cuál fue, links propios, plantillas…). **Nunca lo repartas entre los anuncios**: mostralo aparte.
- **`cobertura.pct_con_ad_id`**: qué parte de los leads nuevos tiene anuncio identificado. **Va en la primera línea del informe.** Con 95% el cruce es sólido; con 60% decilo antes de sacar conclusiones.
- **`siguiente_paso`**: si viene, hacelo antes de mostrar números.
- **`truncado`**: si vino, algo se recortó para entrar (los anuncios más chicos, por ejemplo). Decilo.

## 3. Si todavía no se sabe qué es una venta

### 3a. Cuenta `chatty_completo`: una sola pregunta

Con `venta_requiere_definicion: true` las ventas vienen en `null`. Armá **una** pregunta de opción múltiple con `candidatas_de_venta`, cada opción con su nombre y cuántos chats tiene, y en palabras del negocio:

> Para contar ventas necesito saber qué es una venta para vos. En este período veo: la etapa «Seña pagada» (12 chats), la etapa «Presupuesto enviado» (85), la etiqueta «Cliente» (40). ¿Cuál de estas es una venta cerrada?

Con la respuesta, `venta_definir(tipo, ids)` y volvé a llamar a `anuncios_resultados`. Una pregunta, no un cuestionario; y si el dueño duda entre dos, mostrá los dos resultados (se puede pedir el informe con una definición distinta sin guardarla) y que elija viendo los números.

### 3b. Cuenta `solo_conector`, o ninguna candidata tiene datos: leer

Las ventas salen de las conversaciones, y leerlas es tu trabajo, no el del dueño:

1. `ventas_candidatas` con el mismo período: trae un lote de chats que hablan de pago, transferencia, comprobante o seña, o que mandaron un adjunto, con sus últimos mensajes.
2. Leé cada uno y decidí: compró o no. Si compró, la frase que lo muestra va en `evidencia`, textual. El monto sólo si la conversación lo dice; si lo decís, con la moneda. No infieras montos.
3. `ventas_marcar` con **todos** los veredictos del lote, también los que no compraron: así no vuelven.
4. Repetí desde 1 hasta que `devueltos` sea 0.
5. `anuncios_resultados` de nuevo. En una cuenta `solo_conector` ya cuenta lo que marcaste; en una `chatty_completo` sin candidatas con datos, guardá antes `venta_definir(tipo="deteccion")`.

**Lo que marcás queda escrito en el Chatty del dueño**, a la vista, no en una libreta tuya:

- compró, con monto, en una cuenta `solo_conector` → una **venta cargada** en ese chat, sin producto, con el nombre «Venta detectada por Claude» y la frase que lo muestra;
- compró sin monto, o en una cuenta `chatty_completo` (ahí las ventas las cargan ellos) → la etiqueta **«Venta (detectada por Claude)»**;
- no compró → la etiqueta **«Sin venta (revisado por Claude)»**, para no volver a leerlo salvo que el cliente escriba de nuevo.

Nada de eso le llega a ningún cliente, y todo se deshace desde el chat (borrar la venta, sacar la etiqueta). Contale al dueño lo que dice `resumen` en la respuesta, tal cual: tiene que saber que va a encontrar esas ventas y etiquetas en su Chatty. Si borra una venta que cargaste, esa venta no se vuelve a cargar; si una que cargaste no era, no la borres vos: decile que la borre desde el chat.

Si los candidatos son muchos, avisale al dueño cuántos vas leyendo, y si hay casos dudosos marcalos como no compró y listalos aparte para que él confirme: una venta inventada ensucia el costo por venta de un anuncio que quizás no lo merece, y además queda cargada en su Chatty.

## 4. El gasto, del conector de Meta Ads

El gasto no está en Chatty: está en Meta. El plugin trae declarado el conector oficial de Meta Ads; si sus tools no aparecen es porque el dueño no lo autorizó, y no hay nada roto.

1. La cuenta publicitaria: la tool que lista las cuentas del dueño. Si tiene varias, preguntá cuál (o usá todas y decí que sumaste). Si la cuenta dice su zona horaria, usala en `tz` del paso 2; si no, decí qué zona usaste.
2. El gasto **a nivel anuncio**, para el mismo período exacto que el informe (`ventana.desde` / `ventana.hasta`), con el id del anuncio, su nombre, el gasto y, si está, las conversaciones iniciadas que cuenta Meta como control.
3. **Uní por id de anuncio, nunca por nombre.** Es común tener varios anuncios distintos que se llaman igual.

Las cuentas:

- **Costo por lead** = gasto ÷ (`leads_nuevos` + `leads_que_vuelven`).
- **Costo por venta** = gasto ÷ `ventas`. Con 0 ventas decí «sin ventas atribuidas», nunca infinito ni cero.
- **ROAS** = ingresos ÷ gasto, sólo si hay ingresos y en la misma moneda que el gasto. Si las monedas no coinciden, no conviertas: mostrá los dos números.

Lo que no cuadra también se informa, sin corregirlo:

- **Gasto en Meta y 0 leads en Chatty** → posible pérdida de atribución (Meta a veces no dice de qué anuncio vino un chat; mirá cuántos hay en el balde de anuncio no identificado).
- **Un anuncio en Chatty que no está en la cuenta** → es de otra cuenta publicitaria.
- **Mucha diferencia entre las conversaciones que cuenta Meta y los leads de Chatty** → decilo, con los dos números.

**Sin el conector de Meta**: informá leads y ventas por anuncio igual, y decí que el costo necesita que conecte Meta Ads. No inventes gasto ni lo estimes.

## 5. Lo que le entregás

Una primera línea con el período y la cobertura. Después una tabla, ordenada por gasto (o por leads, si no hay gasto):

| Anuncio | Gasto | Leads (nuevos + vuelven) | Costo por lead | Llegaron a [etapa clave] | Ventas | Costo por venta |
|---|---|---|---|---|---|---|

Usá el titular del anuncio para nombrarlo, no el id, y dejá el id disponible si lo pide. Aparte, una línea con lo que vino sin anuncio identificado.

**El veredicto de «¿coinciden?»**, en una frase: ¿los que más leads traen son los que más venden? Mirá la tasa de conversión (ventas ÷ leads) de los dos o tres anuncios de más volumen. El caso típico, y el más útil de mostrar, es el anuncio que trae muchísimos leads baratos y casi no vende, al lado de uno más caro por lead que convierte el triple.

**Sugerencias concretas**, dos o tres, atadas a los números de esta tabla: qué anuncio escalar, cuál revisar o apagar, dónde mirar el mensaje (si un anuncio trae leads que nunca pasan de la primera etapa, el problema puede estar en qué promete el anuncio y no en la atención). Con pocas ventas (menos de cinco por anuncio) decilo: son señales, no conclusiones.

Si no hay anuncios en el período, o la cuenta no hace publicidad en Meta, decilo así, sin armar una tabla vacía.
