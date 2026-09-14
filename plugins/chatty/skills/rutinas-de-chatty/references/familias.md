# Las familias de rutinas

Una familia no es una plantilla: es una pregunta del negocio, más lo que hay que sacarle al dueño para poder contestarla, más lo que este conector puede y no puede ver. Se escribe una por vez y se prueba antes de ofrecer la siguiente.

La de prospectos de alto potencial está trabajada aparte, en `ejemplo-prospectos.md`, y es por donde se arranca.

## La cola de hoy

**Contesta:** qué hago antes del mediodía.

Es la rutina diaria, y su virtud entera es el tope duro: tres a cinco ítems, nunca más. Un digest diario largo se deja de leer en una semana. Abre con qué cambió desde ayer (entró, se movió, sigue clavado) y cada ítem trae el link y una línea de por qué está ahí.

**Se pregunta:** qué cuenta como urgente para él (no para vos), a qué hora la lee, y si prefiere que le diga qué hacer o sólo a quién mirar.

**Se lee con:** `pendientes_list(only_new=True)`, que es justo lo nuevo desde el último aviso, más `pendientes_detail` sobre los pocos que entran. `chats_sin_leer` si lo que le importa es lo que nadie abrió.

**Ojo:** la cola y la agenda (`agenda_ver`) son dos listas con dos vidas distintas y no se mezclan ni se suman. La cola la escribe un barrido y se resuelve sola cuando el chat deja de calificar; la agenda la escribió una persona, tiene fecha, y no se resuelve nunca sola.

## Cómo contesta mi agente de IA

**Contesta:** si lo que la IA está diciendo en nombre del negocio está bien.

⚠️ **Esta familia es distinta en especie a todas las demás y se escribe distinto.** El resto lee datos y los ordena. Esta emite un juicio sobre el trabajo de la empresa. Si el dueño no escribió antes qué considera una buena respuesta, vas a inventar la vara y a aplicarla con total seguridad, y un criterio inventado suena razonable de una forma en que un número inventado no.

**Por eso arranca al revés:** antes de escribir una línea de rutina, el dueño escribe su estándar. Cuatro o cinco reglas propias, concretas, del tipo "nunca damos un precio sin preguntar el volumen", "siempre ofrecemos la visita", "no prometemos plazos de entrega". Eso queda en el archivo de la rutina y es contra ESO que se mide, citándolo. Si el dueño no lo escribe, la rutina no se escribe: decíselo así, no la hagas igual con criterios tuyos.

**Se lee con:** `list_pending_drafts` (los borradores frenados esperando su decisión, que es exactamente donde la IA todavía no habló y se puede corregir barato) y `pendientes_detail` para ver cómo siguió la conversación.

**Lo que no podés ver:** el texto que manda un workflow. `automatizaciones_ver` y `embudos_ver` te dan los nombres y qué se dispara, no lo que el cliente lee. Cuando importe el contenido, preguntáselo al dueño en vez de deducirlo del nombre del workflow.

## De dónde viene la plata

**Contesta:** qué fuente o qué anuncio trae conversaciones que avanzan, y cuál trae ruido.

Es la familia que más cambia decisiones fuera de Chatty (dónde poner la plata de publicidad), y la que más depende de que la empresa tenga la casa ordenada: sin etiquetas o embudos que distingan el origen, no hay nada que agregar.

**Se pregunta:** qué cuenta como "avanzó" para él (pidió precio, agendó, pagó), y cómo distingue hoy de dónde viene cada chat.

**Se lee con:** `pendientes_list` por motivo y `pendientes_detail` para el estado CRM de cada chat, más `campanas_ver` cuando lo que se compara son campañas propias.

**Ojo con el techo:** no hay agregado por fuente ni por anuncio en estas 17 tools, así que todo número sale de una muestra acotada y se escribe como muestra ("de los 20 que miré"), nunca como distribución de la empresa. Si el dueño quiere esto de verdad y en serio, la respuesta honesta es que se mira en las pantallas de Chatty, no acá.

**Y el otro techo, el que más duele: acá no está lo que costó.** Chatty ve la conversación, no la factura de publicidad. El gasto vive en Meta y llega por el conector de Meta, que es de Meta, se autoriza aparte con la cuenta de Meta Business del dueño, y puede perfectamente no estar autorizado. Si no está, sus tools no figuran en tu listado, y eso es información y no una falla: decíselo y no le prometas un costo por venta. Un número de rentabilidad construido sobre un gasto supuesto es peor que no darle ninguno, porque se usa para decidir dónde poner la plata.

## Higiene del embudo

**Contesta:** qué está guardado en el lugar equivocado.

Chats parados hace semanas en una etapa que no es de cierre, gente marcada como ganada que sigue preguntando, etapas que nadie usa. Es la menos glamorosa y la que más ordena.

**Se pregunta:** cuánto tiempo en cada etapa ya es demasiado, y qué etapas considera él de verdad vivas.

**Se lee con:** `embudos_ver()` para la estructura, `pendientes_list` y `pendientes_detail` para el estado de los chats.

⚠️ **Acá es donde una rutina de lectura se puede convertir sin querer en una acción hacia afuera.** Mover un chat de etapa puede disparar las automatizaciones de la etapa de destino, y algunas mandan un mensaje: `embudos_ver` lo marca con `puede_enviar_mensajes`. Y cuando `configuracion_leida` viene en `false`, la etapa no se pudo leer entera, así que "no manda nada" no quedó establecido y hay que tratarla como si mandara. Una rutina de higiene propone movimientos y los ejecuta sólo con el dueño confirmando, uno por uno.

## La ventana de 24 horas

**Contesta:** a quién le podés escribir gratis todavía, y a quién ya le hace falta una plantilla paga.

Es la familia más barata de construir y la que más plata ahorra, porque el dato viene servido: cada fila de `pendientes_list` trae `free_text_window`, con `open: true` y las horas que quedan, `open: false` cuando la ventana se cerró, u `open: null` cuando no se pudo determinar, que se reporta como "no sé" y nunca como "gratis".

**Se pregunta:** a quién vale la pena alcanzar antes de que se cierre la ventana (o sea, qué hace que un chat sea recuperable para él).

**Se lee con:** `pendientes_list` para las ventanas y `plantillas_ver` para los que ya se cerraron, porque ahí se le dice al dueño POR NOMBRE qué plantilla aprobada le sirve, con el texto real a la vista, en vez de mandarlo a adivinar entre nombres crípticos.

**Si el mapa dice cero plantillas aprobadas, esta rutina se corta a la mitad** y hay que decirlo de entrada: se puede listar a quién se le está por cerrar la ventana, pero a los que ya se cerró no hay forma de alcanzarlos. Ese es un hallazgo para el dueño, no un detalle técnico.
