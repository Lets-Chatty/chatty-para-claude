---
name: optimizar-por-conversiones
description: Usala cuando el dueño quiera que Meta optimice sus anuncios por lo que pasa de verdad en su WhatsApp, y no sólo por conversaciones iniciadas — «que Meta aprenda de mis ventas», «optimizar por compras», «configurar la API de Conversiones», «mandarle a Meta las ventas y los leads buenos», «mis anuncios traen consultas pero no venden», «¿llegan los eventos a Meta?», «marcá este lead como bueno». Revisa si los eventos de compra y de lead calificado llegan a Meta, los activa, los alimenta con las ventas y los leads buenos reales, y prepara el cambio en la campaña con el conector de Meta Ads, siempre en borrador y con el OK del dueño. No promete resultados ni números mínimos que no pueda verificar.
---

# Que Meta optimice por tus ventas reales

Un anuncio que lleva a WhatsApp, por defecto, se optimiza para conseguir **conversaciones**: Meta aprende a traer gente que escribe, no gente que compra. La API de Conversiones sirve para cambiar eso: Chatty le avisa a Meta cuándo una consulta terminó en venta (**compra**) o cuándo el dueño la marcó como buena (**lead calificado**). La optimización que se pudo comprobar es la de **compras**: Meta usa esas ventas para aprender qué gente le conviene al negocio. Los leads buenos se envían igual y quedan a la vista, pero no hay un objetivo de WhatsApp comprobado para optimizar por ellos.

Tres cosas tienen que ser ciertas antes de prometer nada, y las tres se miran en el paso 1:

1. **Los eventos tienen que existir y llegar.** Una compra sale a Meta cuando se carga una venta en el chat; un lead calificado, cuando se lo marca como bueno.
2. **Sólo cuentan los chats que entraron por un anuncio de click-to-WhatsApp.** Meta atribuye el evento al clic. Una consulta orgánica, o por un link suelto, no le sirve para optimizar un anuncio. Si casi todo lo que vende el negocio entra así, esto no va a mover la aguja, y hay que decirlo.
3. **Meta decide si habilita la optimización.** Pide un mínimo de eventos recientes y depende de la cuenta. No des un número de memoria: el administrador de anuncios de Meta es quien lo muestra.

**Nunca le pidas al dueño que se fije algo en Chatty.** Todo lo que necesitás está en el conector. Y no mandes eventos a mano ni inventes ventas para llegar a un mínimo: una venta o un lead bueno falso le enseña a Meta a buscar la gente equivocada, que es peor que no optimizar.

## 1. Diagnóstico (siempre primero)

Llamá, en este orden:

- `whatsapp_conexion_ver`: sin número conectado no hay chats ni eventos. Si no está conectado, decíselo y frená acá: lo primero es conectar el WhatsApp, y la herramienta te explica cómo.
- `conversiones_meta_estado`: dice si la empresa tiene dataset, qué tipo de eventos salieron en los últimos 7 y 30 días, qué porcentaje trae el clic del anuncio, y trae un veredicto en palabras. Leelo así:

| El veredicto dice… | Qué significa y qué hacés |
|---|---|
| Sin dataset | Todavía no se activó. Paso 2. |
| Sin eventos | Está activo pero no salió nada: faltan ventas o leads buenos cargados. Paso 3. |
| Eventos sin clic de anuncio | Salen eventos, pero de chats que no vinieron de un anuncio: no sirven para optimizar. Decilo y preguntá si el negocio hace anuncios a WhatsApp. |
| Los eventos salen con el acceso de Chatty | Funciona, pero la empresa debería reconectar su WhatsApp para que salgan con el suyo. |
| Hay señal | Seguí al paso 4. |
| No se pudo consultar | **No es lo mismo que «no hay eventos».** Decí que no pudiste verlo y que se reintente. No saques conclusiones. |

Si alguna de estas tools no aparece, es porque el plan de la persona no la incluye o no tiene permiso: decíselo con esas palabras y no insistas.

## 2. Activar

`conversiones_meta_activar` deja listo el lugar al que Chatty le manda los eventos a Meta. Lo hace un **administrador de la empresa**; si la persona no lo es, la tool lo va a decir, y le explicás que lo tiene que hacer un administrador. Es seguro repetirla: si ya estaba activo, lo dice y no toca nada.

Si falla por el WhatsApp (no conectado, o sin el permiso de Meta para eventos), explicale qué falta sin culparla: suele resolverse reconectando el WhatsApp.

## 3. Alimentarlo con lo que de verdad pasó

- **Compras:** salen solas cuando se carga una venta **con monto**. Si el negocio no carga ventas en Chatty, se leen de las conversaciones con el flujo de la skill `anuncios-y-ventas` (`ventas_candidatas` y `ventas_marcar`). Sólo las que tienen monto generan el evento de compra; una venta marcada sin monto deja una etiqueta y no avisa a Meta.
- **Leads buenos:** `lead_calificar` marca un chat como bueno, neutro o malo. Sólo «bueno» le avisa a Meta, y sólo sirve si el chat vino de un anuncio.

Antes de calificar:

- **Marcá sólo lo que la conversación respalda** (pidió precio y confirmó interés, pidió turno, mandó el comprobante). Ante la duda, no lo marques bueno, y listale los dudosos al dueño para que decida.
- **Para más de unos pocos chats, mostrale la lista y esperá su OK.** Cada «bueno» entrena a Meta, y no se puede retirar del todo un evento ya enviado.
- Cada vez que marques, decile qué efecto tuvo: «lo marqué bueno; como vino de un anuncio, Meta recibe el evento» o «lo marqué bueno, pero no vino de un anuncio, así que sirve para tus registros y no para Meta».

Los eventos pueden tardar en aparecer del lado de Meta. No te asustes ni lo des por perdido a los pocos minutos: volvé a mirar `conversiones_meta_estado` más tarde.

## 4. Cambiar la campaña (con el conector de Meta Ads)

Esto lo hace el conector de Meta Ads, no el de Chatty. Si sus tools no aparecen es porque el dueño no lo autorizó: decí que sin eso podés dejar los eventos funcionando pero no tocar la campaña.

1. **Mirá cómo está la campaña hoy**: cuenta, campaña, conjunto de anuncios, con qué objetivo y a qué destino (WhatsApp). Preguntá cuál si hay varias.
2. **El objetivo de optimización que corresponde es «compras por mensajería»** (en la API, `MESSAGING_PURCHASE_CONVERSION`), con destino WhatsApp y la página de Facebook del negocio como `promoted_object`. No lleva el dataset ni ningún id de Chatty: el vínculo con los eventos lo hace Meta por la cuenta de WhatsApp. Lo que se comprobó creando conjuntos de anuncios de prueba (pausados, ya borrados):
   - **Campaña de interacción (`OUTCOME_ENGAGEMENT`)**: Meta aceptó «compras por mensajería».
   - **Campaña de ventas (`OUTCOME_SALES`)**: Meta lo rechazó («el objetivo no está disponible con el objetivo de tu campaña», error 2490408), aunque su propia lista de objetivos válidos lo incluye. **No te fíes de esa lista**: la validación real es crear el conjunto.
   - **Campaña de clientes potenciales (`OUTCOME_LEADS`) con «leads de calidad»** y destino WhatsApp: rechazado. Ese objetivo es para formularios, no para chats de WhatsApp. Para WhatsApp, en esa campaña solo sirve optimizar por conversaciones.
   - **«Conversiones del sitio web» (`OFFSITE_CONVERSIONS`)** pide un píxel o una app: no sirve para eventos que vienen del chat.

   Esa prueba mostró que Meta acepta crear el conjunto; **no mostró que entregue ni que aprenda**, porque eso depende de que lleguen suficientes eventos. Si al crear el conjunto Meta rechaza el objetivo en la cuenta del dueño, decile el motivo tal cual lo dice Meta y proponé quedarte en optimizar por conversaciones: los eventos igual se siguen acumulando y se pueden medir, y Meta puede habilitarlo cuando junte volumen. **Para los leads buenos no encontré un objetivo propio de WhatsApp que se pueda elegir:** lo único que se comprobó como objetivo es el de compras. Los leads buenos igual se envían y quedan a la vista del dueño en el administrador de eventos de Meta, pero no le prometas que Meta optimice por ellos.
3. **Mostrá el antes y el después** (qué se optimizaba, qué se va a optimizar, qué no cambia) y esperá el OK explícito. **No actives nada sin ese OK**: activar publica y empieza a gastar.
4. **Avisale de un costo escondido**: cambiar el objetivo de un conjunto que ya está andando puede reiniciar la fase de aprendizaje de Meta, y los resultados de los primeros días van a ser peores antes de mejorar. Si el conjunto es bueno hoy, preguntale si prefiere duplicarlo y probar en paralelo.
5. **Fijá a mano todo lo que se pueda y no dejes que Meta decida sin preguntarle al dueño.** Antes de armar el borrador preguntale lo que falte, **una sola vez y en una lista corta**, y mostrá cada elección en el resumen. Lo que el dueño no definió no lo deduzcas: preguntalo.
   - **Destino: solo WhatsApp.** Un único destino (`WHATSAPP`), nunca el de varias apps de mensajería (Messenger, Instagram Direct y WhatsApp juntos), que Meta ofrece aparte. Con un solo destino el clic siempre abre el chat de WhatsApp del negocio.
   - **Página de Facebook:** cuál. Se listan con la herramienta de páginas de la cuenta publicitaria.
   - **Número de WhatsApp:** cuál. El número **se configura en la Página, no en el anuncio**: la Página tiene que tener un número verificado vinculado. Sin eso el anuncio se crea, pero falla al activarlo con un aviso de reconectar el WhatsApp a la Página o a Instagram; si pasa, decile al dueño que lo vincule y verifique en los ajustes de WhatsApp de la Página, y no recrees nada. Si hay más de un número, Meta documenta que el conjunto acepta indicar cuál (`whatsapp_phone_number` dentro del `promoted_object`, junto con la página); **no lo comprobé**, así que verificá en la respuesta de Meta que quedó el número que corresponde y, si no, decíselo.
   - **Cuenta de Instagram:** cuál. Se listan con la herramienta de cuentas de Instagram de la cuenta publicitaria y se fija en el anuncio (`instagram_user_id`). Sin ninguna cuenta indicada, el anuncio no se muestra en Instagram: si el dueño quiere Instagram, tiene que haber una; si no la quiere, no pases ninguna.
   - **Público:** lugares, edades, género y, si los hay, públicos propios o similares de la cuenta. Intereses **solo con ids reales** que el dueño te dé: nunca los inventes. Al definir el público, apagá Advantage+ Audience (`targeting_automation.advantage_audience` en 0); si queda prendido, Meta toma las edades que pongas como una sugerencia y no como un límite. Es decisión del dueño: decile que apagarlo le da control pero le deja a Meta menos margen para encontrar gente, y que el costo puede subir.
   - **Ubicaciones:** dónde se muestra es otra cosa que adónde lleva el clic. Un anuncio con destino WhatsApp puede aparecer en Facebook e Instagram y el clic igual va a WhatsApp. Preguntá si quiere ubicaciones fijas (qué plataformas y qué lugares) o que las elija Meta, y explicale que fijarlas puede encarecer.
   - **Mejoras automáticas de Meta con IA sobre el anuncio** (retoques de imagen, optimización de textos, animación de imagen): apagalas explícitamente al crear el anuncio, una por una, y no confíes en el valor por defecto. Meta puede sumar mejoras que no conozco: mirá lo que devuelve la herramienta y, si queda alguna activa, avisale al dueño. El mensaje de bienvenida del chat no se fija desde acá: rige el que tenga la Página.
   - **Declaración de contenido hecho con IA:** preguntale al dueño si el anuncio tiene imágenes o videos generados o editados con IA. La declaración es su responsabilidad: **nunca la completes ni la infieras por él**.
   - **Presupuesto:** monto y si es diario o total. No lo deduzcas.
   - **Objetivo de optimización:** el del punto 2.

   Cerrá con una tabla de dos columnas: **«Lo fijé a mano»** y **«Lo que sigue automático»**. Si algo no se puede apagar con las herramientas, decíselo con esas palabras en vez de dar a entender que quedó todo bajo control.
6. **En una campaña que ya existe, no toques el presupuesto, el público ni los anuncios** salvo que el dueño lo pida.

## 5. Lo que le entregás

Cuatro líneas, en este orden, sin jerga:

1. **Dónde estamos:** si los eventos llegan, cuántos en los últimos 30 días y qué parte viene de anuncios.
2. **Qué hiciste:** qué activaste, qué ventas o leads marcaste (y cuáles quedaron dudosos esperando su decisión).
3. **Qué queda en borrador y qué hace falta para publicarlo.**
4. **Cuándo volver a mirar**, con un plazo realista (días, no horas), y qué mirar: otra vez `conversiones_meta_estado` y, del lado de Meta, si la optimización por compras o por leads quedó habilitada.

No prometas que el costo por venta va a bajar. Lo que sí podés decir es que Meta va a tener información real para aprender, y que el efecto, si lo hay, se ve en las semanas siguientes.
