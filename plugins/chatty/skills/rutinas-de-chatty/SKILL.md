---
name: rutinas-de-chatty
description: Usala cuando el dueño quiera que su Claude le revise el WhatsApp del negocio de forma repetida (un informe diario, un repaso semanal de prospectos, un digest de la cola, "avisame de lo que se me está escapando") o cuando pida armar, ajustar o probar una rutina sobre el conector de Chatty. Entrevista al dueño para que la rutina quede escrita con SUS criterios, le agrega el andamiaje que él no tiene por qué saber que hace falta, y la deja probada contra sus datos reales en `chatty/rutinas/`.
---

# Escribir la rutina del dueño, no darle la de otro

Una rutina buena no es un pedido bien redactado: es conocimiento del negocio escrito. La diferencia entre "buscá buenos prospectos" y una rutina que sirve es que la segunda nombra las dos o tres patologías concretas del pipeline de ESTA empresa, y clasifica por el freno real de cada prospecto en vez de por su nivel de interés. Eso no lo sabés vos y no está en los datos: lo sabe el dueño y nunca lo escribió.

Tu trabajo acá es sacárselo, no reemplazarlo. Si le entregás una rutina genérica va a funcionar la primera vez, se va a leer la segunda, y a la tercera nadie la abre.

## 1. Leé el mapa antes de abrir la boca

Buscá `chatty/mapa.md`. Si no existe, corré primero la skill `mapa-de-chatty` y volvé: sin mapa esta entrevista es un formulario en blanco y las respuestas van a ser "no sé".

Si existe pero tiene más de un mes, decilo antes de empezar y ofrecé refrescarlo. Una rutina construida sobre un inventario viejo promete cosas que la empresa ya no tiene (o se pierde las que consiguió).

Del mapa sacás, sin preguntar nada: el `company_id` para los links, el tamaño y los motivos de la cola, si hay plantillas aprobadas (si no hay, cualquier rutina que quiera alcanzar gente con la ventana cerrada es imposible y hay que decirlo ahí mismo), si hay embudos, listas, audiencias, campañas, archivos, calendario, y cuántas tools tiene realmente esta conexión.

## 2. Preguntá sólo lo que los datos no pueden contestar

Es la disciplina central de esta skill. Cada pregunta que hacés y que el mapa ya contesta te cuesta credibilidad, porque le estás pidiendo al dueño que haga tu trabajo.

**No se pregunta** (se mira): cuántos chats tiene, cuántos están sin responder, qué etapas tiene su embudo, qué plantillas aprobó Meta, si hay campañas a medio mandar, hace cuánto que espera el más viejo.

**Se pregunta** (no está en ningún lado):

- **Qué vende y cuál es el próximo paso concreto que quiere que dé la gente.** Una reunión, un pago, una prueba, una visita. Sin eso no hay forma de decidir qué es "avanzar".
- **Cuáles son sus dos o tres formas de trabarse.** Acá es donde el mapa te salva: no preguntes en abstracto, mostrale lo que viste. "Tenés 180 chats con señal de compra sin seguimiento y 60 que se enfriaron después de que les pasaron el precio: ¿cuál de los dos te duele más?" Un dueño no sabe describir su pipeline, pero sí sabe cuál de dos números lo pone incómodo.
- **Su menú de ofertas.** Es lo más difícil de sacar y lo más valioso de todo, porque es lo que convierte una lista de nombres en una decisión. La pregunta que funciona no es "¿qué ofrecés?" sino **"cuando alguien no avanza, ¿cuáles son los motivos posibles, y qué le ofrecés distinto a cada uno?"**. Buscá que queden dos o tres ramas, cada una con su freno y su oferta. La forma de la respuesta que estás buscando se parece a esto (es de otro negocio, sirve como molde, nunca como contenido):

  - el freno es que necesita algo puntual y la plata no es objeción, entonces se le ofrece el servicio más caro;
  - el freno es el esfuerzo de cambiar y no la plata, entonces se le ofrece algo llave en mano;
  - el freno es el presupuesto, entonces va la propuesta más barata o un seguimiento centrado en el retorno.

  Y una regla que sale de ahí: las ramas tienen que poder dar resultados distintos sin que ninguno sea "peor". Si no queda escrito que la rama llave en mano es otro producto y no un fracaso de la primera, el modelo colapsa las tres en "hacele seguimiento" y la rutina deja de decidir.
- **Quién lee esto y qué hace cuando lo lee.** Cambia el largo, el tono y si hay o no links.
- **La cadencia y el tope** (ver punto 4, que es una sola decisión).

Tres o cuatro preguntas por vez, no un cuestionario. Y cuando el dueño conteste algo vago ("los que están calientes"), devolvele una definición concreta para que la corrija: "¿te sirve que cuente como caliente el que escribió en los últimos siete días y pidió precio?". Es mucho más fácil corregir que redactar.

## 3. El andamiaje se emite siempre, no se pregunta

El dueño no tiene por qué saber que esto hace falta. Va en toda rutina que escribas, sin consultarlo:

1. **Idioma de salida explícito.** "Escribí el informe entero en castellano." Sin eso la salida se vuelve inglesa apenas la rutina tiene instrucciones en inglés.
2. **Links clickeables, con el `company_id` ya adentro.** `https://app.letschatty.com/inbox?area=waiting-agent&company_id=<el del mapa>&chatId=<chat_id>` y la orden explícita de no mostrar el `chat_id` crudo. Esto es lo que separa un informe de una herramienta: sin link el dueño lee y no hace nada.
3. **Tiempo relativo, nunca timestamps.** "Hace 12 días", no "2026-08-30T14:03". La urgencia comercial es relativa, y un timestamp obliga a hacer la cuenta a mano.
4. **El esquema, columna por columna**, con qué va en cada una y la instrucción de no inventar: el dato que no está se escribe "no especificado". Es la regla que evita que una tabla de ocho columnas se llene de suposiciones con cara de dato.
5. **Un orden con opinión.** Por tamaño y urgencia, por plata en juego, por cuánto hace que espera. Una tabla ordenada decide; una tabla sin orden sólo lista.
6. **El caso vacío, con permiso explícito de volver sin nada.** "Si no hay ninguno que cumpla estos criterios, decilo en una línea y listo." Es la instrucción más sofisticada de todas y la que más se omite: sin ella, a un modelo al que le pediste ocho columnas se las vas a encontrar llenas.
7. **Qué está prohibido inferir.** Nombrá los campos que salen de texto libre de un chat (cuánto factura, cuántas consultas recibe, qué presupuesto tiene) y dejá escrito que si el cliente no lo dijo, va "no especificado".
8. **Una ventana temporal concreta.** "Reciente" no es una fecha: cada corrida mira algo distinto y nadie se entera. Escribí "los últimos 30 días" o "desde la última corrida".
9. **Memoria entre corridas.** La rutina lee y después actualiza un archivo de estado (`chatty/rutinas/<nombre>.estado.md`) con qué marcó y cuándo, y el informe abre con **qué cambió desde la última vez**. Sin esto el mismo prospecto aparece idéntico cinco lunes seguidos y el digest deja de leerse. Es, de lejos, lo que más define si la rutina sobrevive al mes.
10. **Confirmación antes de cualquier acción hacia afuera.** Si la rutina termina publicando en Slack, mandando un WhatsApp o disparando una campaña, tiene que pedir la confirmación del dueño en el momento, con el texto a la vista. Puede parecer de más en una rutina que el dueño escribió para sí mismo, pero no lo es: una acción que sale de la empresa se confirma.
11. **Sólo lectura salvo que el dueño haya pedido lo contrario, explícitamente.** Una rutina de informe se escribe con las 17 tools de lectura que lista `mapa-de-chatty`. Si la rutina manda algo, nombrá exactamente qué tool lo manda y a quién le llega.

## 4. Cadencia y tope son la misma decisión

Preguntala siempre, temprano, porque cambia la forma entera de la rutina. Si no se pregunta, todas las rutinas salen con forma de semanal y la diaria nace muerta.

| Cadencia | Tope | Qué pide | Qué NO pide |
|---|---|---|---|
| Diaria | Duro, 3 a 5 ítems | "Qué hago antes del mediodía", qué cambió desde ayer | Razonamiento largo, tablas anchas, historia |
| Semanal | 10 a 15 | Profundidad, el porqué de cada caso, próximo paso | Repetir lo de ayer |
| Mensual | Sin tope de ítems, sí de secciones | Tendencia: qué se movió respecto del mes pasado | Detalle chat por chat |

Un tope duro es una instrucción de la rutina, no una sugerencia: "como máximo cinco, si hay más quedate con los cinco más urgentes y decí cuántos quedaron afuera".

## 5. Terminá con una corrida de prueba

Corré la rutina recién escrita contra los datos reales del dueño, mostrale la salida, y preguntale exactamente esto: **"¿estos son los que vos hubieras marcado?"**.

No es una formalidad. Es lo único que valida la rutina, porque el único que sabe quiénes son los correctos es él. Lo que sale de ahí suele ser un ajuste de criterio (falta un motivo, sobra uno, el orden está al revés), y se corrige en el archivo en el momento, delante del dueño, para que vea que la rutina es suya y se toca.

Si la prueba vuelve vacía, no la arregles bajando el criterio: confirmá con él que está bien que esté vacía. Una rutina que a veces no encuentra nada es una rutina que funciona.

## Dónde queda

En `chatty/rutinas/<nombre>.md`, escrita como un prompt autosuficiente: nada de "como hablamos recién". Quien la corre dentro de dos meses puede ser una sesión en blanco o una tarea programada, y tiene que alcanzar con el archivo. Al lado, su `<nombre>.estado.md`, que escribe la propia rutina.

Arriba de todo del archivo, tres líneas: qué contesta, cada cuánto se corre, y de qué fecha es el mapa sobre el que se escribió.

## Las familias

Arrancá siempre por **prospectos de alto potencial**: es la que más plata mueve, la que mejor se entrevista, y la que tiene el ejemplo trabajado en `references/ejemplo-prospectos.md` (leelo antes de escribir la primera). Las otras familias, con lo que cada una pregunta y lo que ninguna puede hacer sin configuración previa, están en `references/familias.md`: la cola de hoy, cómo contesta mi agente de IA, de dónde viene la plata (por fuente y por anuncio), higiene del embudo, y la ventana de 24 horas de WhatsApp.

Nombrale al dueño las que le sirven, para que vea que esto se combina y no es un informe suelto. Pero escribí una sola por vez y probala: cinco rutinas sin probar valen menos que una que él ya corrigió.

⚠️ **"Cómo contesta mi agente de IA" es distinta en especie a todas las demás y no se escribe como las otras.** El resto lee datos y los ordena; esa emite un juicio sobre el trabajo de la empresa. Si el dueño no escribió antes qué considera una buena respuesta, vas a inventar la vara y aplicarla con total seguridad. Es la misma trampa que tapa el "no especificado", pero más cara: un número inventado se nota, un criterio inventado suena razonable. Esa familia arranca obligando al dueño a escribir su propio estándar, y si no lo escribe, no se escribe la rutina.
