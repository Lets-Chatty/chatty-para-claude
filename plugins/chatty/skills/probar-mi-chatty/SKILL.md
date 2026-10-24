---
name: probar-mi-chatty
description: Usala cuando el dueño quiera ver cómo respondería su Chatty sin que le llegue nada a un cliente real — «probá qué pasa si alguien pregunta por precios», «¿mi agente contesta bien esto?», «simulá un cliente que escribe», «¿en qué etapa cae una consulta nueva?», «quiero testear mi configuración antes de prenderla». Explica que es una simulación (no envía nada a WhatsApp), cómo hacerla con las tools de chats de prueba, cómo leer lo que respondió y qué efectos colaterales avisar.
metadata:
  titulo: "Probá tu Chatty sin clientes reales"
  frase: "Simulá un cliente que pregunta precios."
  resumen: "Escribe como si fuera un cliente y te muestra qué contesta tu Chatty, sin que le llegue nada a nadie por WhatsApp."
---

# Probar cómo respondería tu Chatty

El dueño quiere ver a su Chatty en acción sin esperar a que escriba un cliente de verdad. Para eso existen los **chats de prueba**: la persona que escribe es inventada, y todo lo que responde la empresa queda guardado en ese chat sin salir a ningún lado.

## Lo que tenés que dejar claro (siempre, antes de la primera prueba)

1. **Es una simulación.** El mensaje no pasa por WhatsApp, no lo recibe ningún cliente y nada de lo que responda Chatty se envía.
2. **Pero la configuración corre de verdad.** El mensaje entra por el mismo camino que uno real: el agente de IA, los flujos, las etiquetas y los embudos de la empresa se ejecutan. Por eso una prueba **puede dejar etiquetas, mover el chat de etapa o consumir créditos de IA**. Decíselo, y recordale que esos chats de prueba existen en su bandeja.
3. **Es lo mismo que «Test chats» de Chatty Labs**, hecho desde acá: no hace falta que entre a ninguna pantalla.

## Cómo se hace

1. **Empezá una conversación nueva**: `prueba_chat_enviar` con el mensaje que escribiría el cliente, sin `chat_id`. Crea una persona de prueba nueva (empieza de cero, sin historia).
2. **Esperá unos segundos y leé la respuesta**: `chat_ver` con el `chat_id` que te devolvió. La IA responde de forma asincrónica; si todavía no hay respuesta, esperá y volvé a leer. **No reenvíes el mensaje**: duplicaría la prueba.
3. **Seguí la conversación**: `prueba_chat_enviar` con el mismo `chat_id` y el siguiente mensaje del cliente.
4. **Para retomar una prueba anterior**: `pruebas_ver` lista los chats de prueba de la empresa.

Una prueba útil casi nunca es un solo mensaje. Armá un recorrido corto de dos o tres turnos que se parezca a lo real (primero consulta, después una objeción o una pregunta concreta) y mirá cómo se sostiene.

## Qué mirar cuando lees el resultado

- **La respuesta**: ¿dice lo que el dueño querría que diga? ¿el tono es el suyo? ¿inventa algo (precios, horarios) que no está en su configuración?
- **Quién respondió**: si contestó el agente de IA, un flujo o nadie. Que nadie conteste también es un resultado: suele ser que el agente está apagado o en modo sugerencias, o que otra regla lo frena.
- **Lo que quedó en el chat**: etiquetas, etapa del embudo, productos. Las ves al leerlo con `chat_ver`.
- **Qué falló y por qué**: si la respuesta es mala, decí en qué parte de la configuración habría que mirar (instrucciones del agente, contexto, ejemplos), sin tocarla vos sin que el dueño lo pida.

## Límites (decíselos si los pisa)

- **Archivos: sólo desde un link.** El «cliente» de prueba puede mandar una imagen, un video, un audio o un documento (por ejemplo, una foto de un comprobante o un PDF), pasando `archivo_url` en `prueba_chat_enviar`: un link https público o la dirección de un archivo que ya está en la biblioteca de Chatty (`archivos_ver`). Un archivo que el dueño tiene en su computadora o adjunto en esta conversación no puede viajar por acá: pedile que lo suba a la biblioteca de Chatty (o a un link) y usá esa dirección. El texto que escribas va de pie de foto; un audio no lleva pie, así que el texto llega como mensaje aparte. Topes: imagen 5 MB, audio y video 16 MB, documento 25 MB.
- **Necesita un WhatsApp conectado.** Sin número, la tool lo dice y no inventa nada: ahí el camino es conectarlo primero.
- **Sólo escribe en chats de prueba.** Nunca en una conversación real, aunque el dueño te pase el id de una: la tool lo rechaza.
- **No es un ensayo del envío.** Sirve para ver qué decide y qué dice tu Chatty. No prueba cómo se vería el mensaje en el teléfono de un cliente ni la entrega de Meta.
- **Esta tool puede no estar en tu plan.** Si no figura en el listado, es información sobre la cuenta, no un error: decile al dueño que forma parte del plan completo.
