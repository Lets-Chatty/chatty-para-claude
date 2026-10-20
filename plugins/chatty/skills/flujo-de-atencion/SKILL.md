---
name: flujo-de-atencion
description: "Usala cuando el dueño quiera armar, revisar o cambiar cómo se atiende a quien le escribe por WhatsApp, o sea el Flujo de atención de Chatty — «armame el flujo de atención», «¿qué le contesta mi Chatty a alguien nuevo?», «quiero un mensaje de bienvenida», «cargá mi horario y un aviso de que estamos cerrados», «a los que preguntan precios mandales la lista», «que los turnos los conteste la IA», «si no responde, escribile al otro día», «que los mayoristas le queden a Juan», motivos de consulta, seguimientos, probar el flujo antes de prenderlo, activarlo. Mira primero cómo está, entrevista al dueño (horario, por qué le escriben, qué contesta hoy, si tiene agente de IA, qué hace cuando no le responden), lo arma en orden, lo prueba con chats de prueba que no le llegan a nadie y lo activa sólo con su sí explícito. Está en beta: si la empresa no la tiene, lo explica y no insiste."
metadata:
  titulo: "Tu flujo de atención"
  frase: "Armame el flujo de atención."
  resumen: "Te pregunta cómo atendés hoy y lo arma con vos: la bienvenida, el horario, qué se le contesta a cada tipo de consulta y qué pasa si no responden. Lo prueba con chats de prueba y lo prende sólo cuando le decís que sí."
---

# El flujo de atención, armado con el dueño

El Flujo de atención es lo que le pasa a un chat desde que alguien escribe: qué recibe primero, qué se le contesta según por qué escribió, qué pasa si después no responde, y a quién le queda. Lo arma el dueño, en sus palabras; vos lo entrevistás y lo cargás con las tools `flujo_*` y `motivo_crear` del conector.

Las piezas, con los nombres que ve el dueño en Chatty (usá estos y no otros):

- **Recepción**: la **bienvenida** (el primer mensaje a un cliente nuevo, opcional), el **horario de atención** y el **aviso de cerrado** (lo que recibe quien escribe fuera de horario).
- **«Consulta general»**: el camino por defecto, para los chats que no entran en ningún motivo. Es la base: lo que un motivo no define, lo toma de acá.
- **Motivos de consulta**: por qué escribió el cliente («Precios», «Turnos», «Envíos»). Cada motivo define sólo lo que cambia respecto de Consulta general:
  - **lo que les enviamos**: un texto (queda guardado como respuesta rápida) o que conteste el agente de IA;
  - **si no responde**: los seguimientos, y cuándo el chat pasa a «Sin respuesta»;
  - **al entrar**: si el chat se le asigna a una persona apenas entra.
- **Borrador y activo.** El flujo arranca en borrador: los chats reales se atienden como siempre y el flujo corre sólo en las pruebas. **La bienvenida y el horario con su aviso son la excepción: valen enseguida**, también en borrador. Activarlo es lo que hace que los chats reales entren por el flujo.
- Un **motivo nuevo nace «por completar»**: no recibe chats hasta que le cargás lo que les enviamos, y en ese momento queda activo solo.

**A la IA la nombrás por el nombre de SU agente** («Sofía contesta los turnos»), nunca con un nombre genérico ni con nombres internos. Y nada de jerga: ni ids, ni nombres de campos, ni «workflow», ni «canal» (decí «número» o «de dónde vino»).

## 0. Si no está habilitado, no insistas

El flujo de atención está en beta y lo habilita el equipo de Chatty empresa por empresa. Lo vas a saber de dos maneras: `flujo_ver` devuelve `habilitado: false`, o una tool que escribe contesta que no está habilitado. En ese caso:

- Contale cómo está hoy (`flujo_ver` igual lee), decí que el flujo todavía no está habilitado para su empresa y que lo pide a su contacto de Chatty.
- **No reintentes, no busques otra tool que haga lo mismo y no le ofrezcas armarlo «por otro lado».** Nada se cambió.
- Lo que sí podés hacer: dejar la entrevista hecha y escrita, para que el día que lo habiliten se cargue en diez minutos.

Dos rechazos más que no son la beta: si la tool dice que **su usuario no tiene permiso** para cambiar el flujo, lo tiene que hacer un administrador de la empresa; y si **las tools que escriben no aparecen** en tu listado (o la conexión es de sólo lectura), es su plan o su conexión, no un error: decíselo con esas palabras. En los tres casos, `flujo_ver` sigue sirviendo para contarle cómo está.

## 1. Arrancá mirando: `flujo_ver`

Siempre primero, antes de preguntar nada. Contale al dueño, en criollo y en pocas líneas:

- si está en **borrador** o **activo**, y qué significa eso para sus clientes hoy;
- qué recibe alguien nuevo (bienvenida), el horario y si hay aviso de cerrado;
- qué pasa con Consulta general, y qué motivos hay, con su estado (los «por completar» todavía no reciben chats);
- qué falta para poder lanzarlo (`para_lanzarlo`).

No muestres los `ref`: son para vos, para nombrar un motivo en la próxima llamada cuando hay dos parecidos. Si ya hay un flujo armado, el trabajo es ajustar lo que el dueño quiera, no rehacerlo: preguntale qué quiere cambiar antes de proponer nada.

## 2. Entrevistá antes de crear nada

Lo que sigue no está en ningún dato: lo sabe el dueño. Tres o cuatro preguntas por vez, no un formulario, y cuando conteste algo vago, devolvele una propuesta concreta para que la corrija («¿te sirve que a los que preguntan precio les mandemos la lista y, si en 3 horas no contestan, un "¿pudiste verla?"?»). Es más fácil corregir que redactar.

1. **Horario.** Qué días y en qué franjas atiende, si corta al mediodía, y qué quiere que reciba alguien que escribe fuera de horario. Si atiende las 24 h, no hay aviso de cerrado.
2. **Por qué le escriben.** De acá salen los motivos: **tres o cuatro para empezar**, los que más se repiten. Pedile cómo lo pregunta un cliente de verdad («¿cuánto sale?», «¿tienen turno el sábado?»): esas frases ayudan a la IA a reconocer el motivo. Más motivos se suman después, cuando el flujo ya ande.
3. **Qué contesta hoy a cada cosa.** Si ya lo escribe siempre igual, ese texto es lo que les enviamos. Si ya tiene respuestas rápidas cargadas en Chatty, se pueden usar por su nombre.
4. **Si tiene un agente de IA, y cuál.** Si lo tiene, para qué motivos quiere que conteste la IA y para cuáles un texto fijo. Si tiene varios agentes, que elija cuál; no lo adivines.
5. **Qué hace cuando no le contestan.** Si les vuelve a escribir, cuándo y qué les dice; si prefiere que la IA haga el seguimiento; o si no hace nada. Y cuánto espera para darlo por «Sin respuesta».
6. **Si algún tema le tiene que quedar a una persona puntual** apenas entra (los mayoristas a Juan, los reclamos a Ana), o repartirse entre el equipo.

Si el dueño no sabe por qué le escriben, no inventes motivos: con Consulta general sola el flujo ya funciona. Ofrecele arrancar así y sumar motivos después, mirando qué preguntan.

Antes de cargar, mostrale el plan entero en una tabla corta (qué recibe cada motivo, qué pasa si no responde, a quién queda) y esperá su OK. **Cada texto que le va a llegar a un cliente se confirma con el dueño antes de guardarlo**, palabra por palabra.

## 3. Armalo en este orden

El orden importa: de arriba hacia abajo, cada paso se apoya en el anterior.

1. **Bienvenida**: `flujo_bienvenida_configurar` con `texto` (o `respuesta_rapida`, o `sin_bienvenida`). ⚠️ **Vale enseguida para los chats nuevos reales**, aunque el flujo esté en borrador. Decíselo antes de guardarla.
2. **Horario y aviso de cerrado**: `flujo_horario_configurar` con `dias` (todos los que atiende, porque reemplaza el horario entero), `aviso_texto` y, si no es Buenos Aires, `zona_horaria`. ⚠️ **También vale enseguida.** Sin aviso, el horario queda guardado pero fuera de horario no se avisa nada: si la tool lo dice, contáselo.
3. **Consulta general**: `flujo_respuesta_configurar` con `motivo="Consulta general"`. Es lo único obligatorio para poder lanzar el flujo.
4. **Los motivos**: `motivo_crear` (nombre corto, como lo diría el dueño, y sus frases), y enseguida `flujo_respuesta_configurar` para ese motivo. Hasta ese segundo paso el motivo está «por completar» y no recibe chats; cuando la respuesta queda cargada, la tool te dice que quedó activo. Antes de crear, mirá en `flujo_ver` si ya hay uno parecido: mejor configurar ése que tener dos.
5. **Los seguimientos**: `flujo_seguimientos_configurar`, primero en Consulta general y después en los motivos que necesiten otros. Los minutos se cuentan desde que se le envió la respuesta (60 = 1 h, 1440 = 1 día). ⚠️ Los motivos que no tienen seguimientos propios **heredan los de Consulta general**: la tool te dice a cuáles más les llega (`tambien_les_llega_a`), y eso se le cuenta al dueño.
6. **Asignar al entrar**, sólo si el dueño lo pidió: `flujo_asignar_al_entrar` con una persona (por nombre o email) o por turnos. Si avisa que el reparto por turnos no está configurado, decíselo: el chat quedaría esperando.

Lo que les enviamos en un motivo se elige de UNA forma: un `texto`, una `respuesta_rapida` existente, `que_responda_la_ia` (con su `agente` y, si hace falta, una `indicacion` corta: «preguntá primero la zona»), o `como_consulta_general`.

**Lo que no se hace desde acá**, y se dice en vez de improvisarlo: poner etiquetas o mover el chat de etapa al entrar, el detalle de «Cuando responde» más allá de qué agente sigue la charla, pausar la atención, desactivar un motivo y volver el flujo a borrador. Todo eso se hace en Chatty, en la pantalla «Flujo de atención».

Después de cada cambio, la respuesta de la tool dice en qué quedó (el motivo, si el flujo sigue en borrador). Si una tool devuelve `ok: false`, leé el `motivo` y contáselo con esas palabras; si dice `se_guardo` con un `que_hacer`, hacé eso y no otra cosa.

## 4. Probalo antes de activarlo

`flujo_probar` escribe como si fuera un cliente sobre un **chat de prueba** (un número que empieza con 00000): corre el flujo de verdad, también en borrador, pero **no le llega a nadie por WhatsApp**, no se le asigna a nadie y no cuenta en las métricas. La IA sí gasta créditos: decíselo antes de una tanda larga.

- Probá **un caso por motivo**, con una frase que se parezca a como escribe un cliente real, **y uno que no encaje en ninguno** (tiene que caer en Consulta general).
- Usá `flujo_probar` y no la tool de chats de prueba de siempre: ésa prueba la atención de hoy, y con el flujo en borrador no pasaría por él.
- Sin `chat_id` es un cliente nuevo: así se ve la bienvenida y en qué motivo entra. Con el `chat_id` que te devolvió, seguís la misma charla.
- La IA tarda unos segundos: si la respuesta dice que puede llegar más, volvé a llamar con el `chat_id` y sin `mensaje`. **No reenvíes el mensaje**: duplicarías la prueba.
- Si el número de WhatsApp no está conectado, la prueba no se puede hacer: decíselo y ayudalo a conectarlo primero.

Mostrale al dueño cada caso así: qué escribió el «cliente», qué recibió en orden, y en qué motivo quedó (`donde_esta`). Si un caso cayó en el motivo equivocado o la respuesta no es la que él quería, ajustá con él (más frases en el motivo, otra indicación para la IA, otro texto) y volvé a probar. Las pruebas viejas se pueden retomar con su `chat_id`.

## 5. Activar: sólo con un sí explícito

1. Llamá a `flujo_activar` **sin** `confirmo`. No cambia nada: dice si se puede activar, qué falta, y qué va a pasar.
2. Contale al dueño, con esas palabras, **qué cambia al activar**: desde el próximo mensaje, todos los chats de sus clientes entran por el flujo (bienvenida, horario, Consulta general, los motivos activos y sus seguimientos), y donde el flujo decide algo, deja de aplicar lo que hoy pasa según de dónde vino el chat. Si quedan motivos «por completar», que sepa que no van a recibir chats.
3. Si no lo probaron, sugerí probarlo antes.
4. **Sólo con un «sí» explícito del dueño a esa pregunta**, llamá con `confirmo: true`. Un «dale, armalo» del principio no es un sí a activarlo, ni lo es un «se ve bien» después de una prueba. Ante la duda, preguntá.

Volver a borrador no se hace desde acá: se hace en Chatty, en «Flujo de atención». Decíselo al activar, así sabe que hay vuelta atrás.

## 6. Cómo cerrás

Tres o cuatro líneas, sin jerga:

1. **Cómo quedó**: activo o en borrador, y qué recibe hoy un cliente nuevo.
2. **Qué armaste**: los motivos, con lo que recibe cada uno y qué pasa si no responde.
3. **Qué quedó para hacer en Chatty**, si algo (etiquetas al entrar, un motivo para desactivar).
4. **Qué mirar en unos días**: si los chats caen en el motivo que corresponde. Si muchos caen en Consulta general, ahí hay un motivo que falta, o frases que sumar.

Un ejemplo de entrevista y del plan que sale de ella está en `references/ejemplo.md`. Es de otro negocio: sirve como molde, nunca como contenido.
