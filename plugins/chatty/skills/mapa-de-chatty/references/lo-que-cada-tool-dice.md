# Lo que cada tool de lectura dice, y lo que no

Referencia para cuando vayas a apoyar una afirmación del mapa en un campo concreto. Todo lo de acá describe lo que documenta cada tool del conector: si alguna vez algo no coincide, gana lo que diga la descripción de la tool que tengas a la vista.

## `chats_sin_leer(limit)`

Previews de los chats sin leer, ordenados por lo más reciente, con nombre, texto del último mensaje y fecha. Está capado del lado del cliente: si pediste 40 y vinieron 40, lo que sabés es "40 o más". Por default mira la última semana; `ventana.mas_viejos` dice cuántos sin leer quedaron afuera por ser más viejos.

- `ventana_24h` por fila es una frase, no un campo para repetir. Abierta quiere decir que una respuesta de texto sale directo, sin plantilla, pero **no es gratis sin más**: desde el 1 de octubre de 2026 Meta da gratis las primeras 1.000 respuestas del mes por número de WhatsApp de la empresa y después cobra cada una a la tarifa del país del cliente, la misma que la de un mensaje de utilidad (lo cobra Meta, no Chatty; [precios oficiales](https://developers.facebook.com/documentation/business-messaging/whatsapp/pricing)). Cuántas lleva la empresa en el mes no lo dice ninguna tool: no lo inventes. Cerrada quiere decir que la ventana de 24 horas de WhatsApp se cerró y alcanzar a esa persona cuesta una plantilla paga que además tiene que aceptar. Si no se sabe, se dice "no sé", no se asume que está abierta.
- Sin leer no es lo mismo que sin contestar: un chat que alguien abrió y no contestó no aparece acá.

## `chat_ver(chat_id, tail)`

Contacto (etiquetas, productos, ventas) más el hilo. Caro y puntual: sirve para contestar una pregunta concreta, no para caracterizar a la empresa. Las etiquetas y los productos vienen del contacto, o sea de a un chat: no hay catálogo de etiquetas ni de productos en ninguna de las de lectura, así que lo que se puede nombrar con certeza es lo que aparezca en la configuración.

## `embudos_ver(funnel_id)`

Por etapa: id, nombre, orden, si cierra el embudo, y `al_entrar` con los efectos concretos y los ids resueltos a nombres.

**Las etapas en orden SON la escalera comercial de la empresa**, y los nombres que les puso dicen cómo entiende su propio recorrido, desde el primer contacto hasta el cierre o el descarte. Leelas como retrato del negocio y no sólo como estructura de datos: es de las dos llamadas que más cuentan del mapa. Ojo con la etapa terminal, porque suele explicar un pico raro de chats que parecen abandonados.

- `puede_enviar_mensajes: true` quiere decir que mover un chat ahí puede terminar en un mensaje al cliente. Tratá ese movimiento como una acción hacia afuera.
- `configuracion_leida: false` quiere decir que no se pudo leer la configuración entera, así que "no tiene automatizaciones" NO quedó establecido. En ese caso `puede_enviar_mensajes` viene en `true` a propósito: suponer que el silencio es seguro es exactamente como se le manda un mensaje a alguien sin querer. Las etapas que fallaron están en `resolucion_incompleta`.
- Dice QUÉ workflow engancha una etapa, no el texto que ese workflow manda. `true` significa "le puede llegar un mensaje", no "le va a llegar".

## `automatizaciones_ver()`

Los workflows que existen, con id, nombre y descripción. **Es mucho más que una lista de nombres, y leerla como una lista de nombres es el error más caro del mapa.** La `descripcion` la escribió la propia empresa, y ahí suele estar la lógica comercial entera: qué se manda, en qué orden, con qué etiqueta se gatilla, a las cuántas horas del paso anterior, en qué ventana horaria, qué agente de IA se asigna, qué frena la secuencia y qué workflow se encadena después. Junto con `embudos_ver`, es la mitad del retrato del negocio, y sale sin abrir una sola conversación de un cliente.

Lo único que NO está es el texto literal que le llega al cliente: el contenido de los pasos vive en otro servicio de Chatty que este conector no habla. O sea que la descripción te dice qué hace el workflow, no qué lee la persona. Cuando importe la palabra exacta, preguntale al dueño en vez de adivinar por el nombre.

Una descripción vacía o un nombre críptico también son un dato: anotalo como "sin describir" y preguntalo, no lo completes vos.

## `programados_ver(chat_id)`

Lo que ya está en cola para salirle a ese contacto. Es por chat, no hay vista global, así que no sirve como agregado del mapa: úsala antes de afirmar que un chat está resuelto.

## `listas_ver(audiencia_id)`

Dos cosas distintas bajo el mismo techo: `tipo: "lista"` es un conjunto congelado de chats que no cambia nunca, y `tipo: "audiencia"` es un filtro con nombre que se vuelve a correr y mañana puede traer gente distinta. Para el mapa cuentan por separado, porque habilitan rutinas distintas.

`la_cree_yo` dice si la creaste vos, y sólo esas se pueden editar. `cuantos_congelados` es la cuenta del día que se guardó, no la de hoy: para saber cuántos siguen disponibles hay que volver a llamar pasando ese `audiencia_id`, y eso sólo vale sobre una de tipo lista.

## `campanas_ver()`

Separa `en_borrador` (frenadas esperando una decisión) de `enviadas` (todo lo demás: programadas, saliendo, pausadas, terminadas, falladas, canceladas). En las dos, `apuntaba_a` y `le_llego_a`: en un borrador es la proyección de hoy y puede cambiar, porque la ventana de 24 horas de cada chat se recalcula recién al mandar; en una ya salida es lo que pasó.

Un borrador viejo sin mandar es un hallazgo del mapa, no un detalle: alguien dejó algo a mitad de camino.

## `plantillas_ver()`

Las aprobadas por Meta que Chatty puede enviar, con el texto real (encabezado, cuerpo con sus huecos, pie y botones). Los huecos pueden ser numerados (`{{1}}`, `{{2}}`) o con nombre (`{{nombre}}`): las que tienen nombre traen `formato_variables: "con_nombre"`, `nombres_variables` y `como_completarlas`, y se mandan con `variables` como diccionario `{nombre: valor}` en vez de como lista. Las aprobadas que Chatty no puede mandar (carrusel, multimedia no soportada) salen aparte con el motivo, y no se le ofrecen al dueño como opción. De pendientes y rechazadas sólo viaja el conteo.

Tope confesado de 40 textos: las filas de más vienen sin texto y la propia respuesta lo aclara en `ojo`. No completa variables ni manda nada.

**Cero plantillas aprobadas es de los datos más importantes del mapa**: sin plantillas no se puede alcanzar a nadie con la ventana cerrada, y eso mata de entrada cualquier rutina de reactivación.

## `archivos_ver(buscar, tipo)`

La biblioteca de la empresa con la URL de cada archivo. `tipo` acepta imagen, audio, video o documento. No sube nada.

## `agenda_ver(chat_id)`

Los recordatorios que escribió una persona, con fecha. La agenda la escribe alguien, tiene fecha, y no se resuelve nunca sola. No la mezcles con los chats sin leer al contestar ni sumes sus números.

## `list_pending_drafts(limit)`

Los borradores de la IA frenados esperando la decisión del dueño, con `cintia_id`, `customer`, `situation`, `draft_preview` y `created_at`. Para el mapa importan dos números: cuántos hay y de cuándo es el más viejo. Un borrador de hace tres semanas dice algo del negocio que ninguna otra tool dice.

## `contactos_previsualizar(pegado, contactos)`

Qué pasaría si importaras esos números, sin importar nada. No tiene lugar en el mapa (necesita que alguien traiga una lista), pero conviene saber que existe: es el paso obligatorio antes de cualquier import, clasifica fila por fila en `new`, `existing`, `duplicate`, `possibly_wrong` e `invalid`, y el `motivo` de cada fila ya viene redactado en castellano para mostrarlo tal cual. Mirá siempre `sin_telefono`: las líneas de las que no se pudo sacar ningún número desaparecen antes de contarse.

## `calendar_list_events(date_from, date_to)` y `calendar_check_availability(date_from, date_to, duration_minutes)`

Existen sólo si la empresa tiene un calendario de Google atado. Si no lo tiene, el servidor las saca del listado a propósito, porque una capacidad que está en el catálogo y falla al usarse es peor que una que no se ofrece. Que falten es un dato del inventario.

Cuando están: `calendar_check_availability` devuelve huecos libres reales con `start`/`end` exactos en la zona horaria del negocio, y esos strings se copian tal cual, nunca se construye ni se ajusta una fecha a mano. Mirar es todo lo que se puede hacer con un turno ya agendado: no hay ninguna tool acá que mueva ni cancele uno, así que no le prometas a nadie un cambio de turno.
