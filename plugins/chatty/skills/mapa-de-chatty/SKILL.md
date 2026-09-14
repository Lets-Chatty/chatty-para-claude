---
name: mapa-de-chatty
description: Usala la primera vez que este Claude trabaja con el conector de Chatty (el WhatsApp del negocio), antes de diseñar cualquier rutina, informe o digest, y cuando el mapa guardado quedó viejo. Arma el retrato del negocio leyendo la configuración (qué vende, cómo está armada la escalera comercial, si hay agente de IA, qué workflows corren y con qué tiempos), el inventario de lo que está realmente cableado (plantillas aprobadas, listas y audiencias, campañas, archivos, agenda, calendario) y recién al final el volumen de la cola. Lo deja escrito en un archivo del dueño, que no es necesariamente el directorio desde donde se la llamó. Es una pasada de SÓLO LECTURA: no manda ni un mensaje.
---

# El mapa de la empresa en Chatty

El conector te da capacidad sobre el WhatsApp de trabajo del dueño y ninguna idea de qué preguntarle. Esta skill arma la foto que falta, y lo hace antes que cualquier otra cosa por una razón práctica: sin mapa, la entrevista que viene después (`rutinas-de-chatty`) es un formulario en blanco.

**Lo que el dueño pide cuando dice "mapeá mi empresa" es un retrato, no una auditoría.** Quiere leer qué vende, cómo está armado el recorrido, dónde vive la información, si tiene IA contratada, qué workflows existen. Si le devolvés cuántos chats esperan y hace cuánto, todo puede ser cierto y aun así no es lo que pidió: le prometiste un mapa y le entregaste un informe de problemas. El volumen entra, pero como contexto y al final.

**El orden es el mensaje.** Un mapa que abre con el diagnóstico se lee como un reproche. Uno que abre con "esto es tu negocio como yo lo leo, corregime" se lee como que lo entendieron, y además le da al dueño lo primero que puede corregir, que es la mitad del motivo por el que este archivo se escribe.

## Regla dura: acá sólo se lee

La mayoría de las tools del conector escriben o le mandan mensajes a clientes reales. Un diagnóstico que manda un WhatsApp es inaceptable: el dueño te dejó entrar a mirar su negocio, no a hablarle a su cartera.

**La lista autorizada no se inventa ni se deduce por el nombre.** Es esta tabla, que espeja el mismo recorte que aplica el servidor cuando una conexión es de sólo lectura. Son estas 17:

| Tool | Qué contesta |
|---|---|
| `embudos_ver` | La escalera comercial: los embudos, sus etapas en orden, y qué dispara entrar a cada una |
| `automatizaciones_ver` | Los workflows, y qué hace cada uno según la descripción que escribió la empresa |
| `plantillas_ver` | Las plantillas que aprobó Meta, con el texto real de cada una |
| `listas_ver` | Listas congeladas y audiencias (filtros con nombre) |
| `campanas_ver` | Campañas en borrador y campañas ya salidas, con a cuántos llegaron |
| `archivos_ver` | La biblioteca de archivos de la empresa |
| `agenda_ver` | Los recordatorios que escribió una persona (no es la cola) |
| `list_pending_drafts` | Borradores de la IA frenados esperando una decisión del dueño |
| `calendar_list_events` | Turnos ya agendados, si hay calendario atado |
| `calendar_check_availability` | Huecos libres reales, si hay calendario atado |
| `pendientes_list` | El tamaño de la cola y su distribución por motivo |
| `pendientes_detail` | Un chat de la cola en profundidad (hilo, estado CRM, ventana de 24h) |
| `pendientes_sweep_now` | Vuelve a barrer ahora en vez de esperar al barrido de la hora |
| `chats_sin_leer` | Qué entró y nadie abrió |
| `chat_ver` | Un chat puntual: contacto, etiquetas, productos, hilo |
| `programados_ver` | Qué mensajes ya están en cola para salirle a un contacto |
| `contactos_previsualizar` | Qué pasaría si importaras unos números, sin importarlos |

**Lo que hace que esto importe de verdad:** el servidor sólo bloquea las que escriben cuando el token de la conexión tiene el permiso `readonly`, y una conexión normal NO lo tiene. O sea que durante el mapa no hay ninguna red abajo: la única barrera sos vos. Tratá la lista como un contrato, no como una sugerencia, y ante la duda de si una tool escribe, no la llames.

`pendientes_sweep_now` es la única de la lista que escribe, y está adentro a propósito: escribe en nuestra propia cola y no le manda nada a nadie. Lo peor que puede hacer es mostrarle al dueño un chat que ya estaba ahí.

**Si alguna de las 17 no aparece en tu listado de tools, eso es información y no un error.** El servidor esconde del listado lo que esta empresa no puede usar: las dos de calendario no se ofrecen cuando la empresa no tiene un calendario de Google atado, y hay familias enteras que dependen de lo que la empresa tenga contratado. Anotalo en el mapa como "no disponible", no lo llames igual para ver qué pasa, y no le prometas al dueño una rutina que dependa de eso.

## El retrato del negocio sale de la configuración, no de leer conversaciones

Este es el hallazgo que gobierna toda la skill, y es contraintuitivo, así que va explícito.

La tentación es leer una muestra de chats para entender de qué vive la empresa. **Está mal por dos motivos**: diez conversaciones no son la empresa (y el porcentaje que saques de ahí se va a leer después como si lo fuera), y encima estarías paseando por contenido de clientes reales para contestar una pregunta que no lo necesita.

**Casi todo el retrato sale de dos llamadas, sin abrir una sola conversación: `automatizaciones_ver()` y `embudos_ver()`.** No son dos listas de nombres. Son la operación del negocio escrita por la propia empresa:

- **`automatizaciones_ver()` trae, por workflow, una `descripcion` que escribió la empresa**, y ahí suele estar la lógica comercial completa: qué se manda, en qué orden, con qué etiqueta se gatilla, a las cuántas horas del paso anterior, dentro de qué ventana horaria, qué agente de IA se asigna, qué se frena cuando contesta un humano y qué se encadena después. Leer eso entero, workflow por workflow, es la parte más rendidora del mapa entero. Lo único que no vas a encontrar ahí es el TEXTO literal que le llega al cliente, que vive en otro servicio; todo lo demás está.
- **`embudos_ver()` trae las etapas en orden, y ese orden ES la escalera comercial**: los nombres que la empresa le puso a cada peldaño dicen cómo entiende su propio recorrido, desde el primer contacto hasta el cierre o el descarte. Y `al_entrar` más `puede_enviar_mensajes` te dicen cuáles de esos peldaños son en realidad acciones hacia afuera y no anotaciones internas.

Con eso solo se establece, sin preguntarle nada al dueño: qué vende y en qué formatos, si vende por cohortes o camadas con fecha, cuántos peldaños tiene el recorrido y cómo los llama, si tiene un agente de IA contratado y con qué nombre, los tiempos exactos de cada seguimiento, en qué horario le escribe a la gente, qué lo frena, y qué puñado de etiquetas gobierna todo el circuito.

Cuando la descripción de un workflow esté vacía o sea un nombre críptico, eso también es un dato del mapa: anotalo como "sin describir" y preguntáselo al dueño, no lo adivines.

## ⚠️ La configuración explica los números, así que se lee ANTES del volumen

No es sólo una cuestión de orden narrativo: leer el volumen sin haber leído la configuración te hace decir cosas falsas con cara de hallazgo.

El caso que lo muestra: una cola con cientos de chats bajo un motivo que suena a abandono, y la etapa terminal del embudo de esa empresa llamándose, con todas las letras, casi igual que el motivo. Los chats no estaban abandonados: estaban en el final diseñado del recorrido, adonde el cierre automático de la propia empresa los manda cuando se acaba la escalera. Contarle al dueño que su equipo dejó colgada a esa gente, cuando lo que estabas viendo era su embudo funcionando como él lo armó, es de los errores más caros que se pueden cometer acá, porque destruye la credibilidad de todo el resto del informe.

**La regla:** antes de interpretar cualquier número, buscá si la configuración ya lo explica. Si el nombre de una etapa, un workflow o una etiqueta se parece sospechosamente a lo que el número dice, lo más probable es que estés mirando un diseño, no una falla. Y si no podés descartarlo, escribilo como pregunta para el dueño en vez de como diagnóstico.

## La pasada, en orden y con topes

Una empresa grande tiene mucha data y vos pagás cada token que traés. Todo lo que sigue está pensado para traer agregados, no montañas de filas.

**1. Contá tus propias tools antes de llamar a ninguna.** Cuántas tenés y cuáles de las 17 faltan. Eso ya es la primera línea del inventario. El total te va a dar menos que las que el servidor tiene registradas, y está bien: esconde del listado lo que esta empresa no puede usar. Escribí el número que contaste vos, nunca uno de memoria ni de otro documento.

**2. La escalera comercial.** `embudos_ver()` una vez, sin `funnel_id`: te da los embudos, las etapas en orden, y para cada etapa `al_entrar` con sus efectos y `puede_enviar_mensajes`. Leé los nombres de las etapas como lo que son, la forma en que la empresa entiende su recorrido, y reconstruí la escalera de punta a punta. Mirá también `configuracion_leida`: en `false` la etapa no se pudo leer entera, así que "no tiene automatizaciones" no quedó establecido.

**3. Cómo se persigue a la gente.** `automatizaciones_ver()`, y leé las descripciones enteras, no los nombres. De acá salen los tiempos, las ventanas horarias, los disparadores, los frenos, el agente de IA si lo hay, y el encadenamiento entre workflows. Es la llamada más barata y la más informativa de toda la pasada.

**4. El inventario, una llamada cada uno:** `plantillas_ver()`, `listas_ver()`, `campanas_ver()`, `archivos_ver()`, `agenda_ver()`, `list_pending_drafts(limit=50)`.

Esto es la mitad menos obvia del trabajo y la que más cambia lo que después se puede diseñar: una tool puede existir y no servir para nada porque falta la configuración. Sin plantillas aprobadas no hay forma de escribirle a nadie con la ventana de 24 horas cerrada, y una rutina de reactivación es imposible. Sin embudos creados, "higiene del embudo" no existe. Sin audiencias guardadas, cada envío arranca de cero. Anotá cada cosa como "hay / no hay / hay pero", con el número.

**5. El tamaño de la cola, recién ahora.** `pendientes_list(limit=5, only_new=False)`. El truco es que los agregados viajan en `summary` (`summary.total` / `total_matching` y `summary.by_reason`), no en las filas, así que pedir cinco filas te da los mismos números que pedir doscientas y cuesta cuarenta veces menos. Leé siempre `returned` y `truncated` antes de decir un número en voz alta: `truncated: true` significa que las filas son un pedazo, nunca que la cola es del tamaño de lo que podés contar.

Los cinco motivos (`nunca_respondido`, `lead_sin_respuesta`, `silencio_post_info`, `sin_cierre`, `senal_compra_sin_seguimiento`) se solapan: un chat puede traer varios, así que los conteos por motivo no suman el total. No los sumes.

⚠️ `nunca_respondido` NO quiere decir "nunca les respondimos": quiere decir que su último mensaje quedó sin respuesta y que además no hay ninguna automatización corriendo sobre ese chat. Escribilo así en el mapa. Decirle a un dueño que su equipo abandonó a gente que en realidad atendió durante un año es la clase de error que se descubre abriendo un solo chat, y te quema la credibilidad de todo el resto del informe. Y antes de escribir el número, contrastalo con la escalera que leíste en el paso 2, que es donde suele estar la explicación.

**6. Lo que entró y nadie abrió.** `chats_sin_leer(limit=40)`. Si vuelve exactamente 40, lo que sabés es "40 o más", no "40". Decilo así.

**7. El `company_id`.** Hace falta para armar links clickeables en las rutinas que vienen después (`https://app.letschatty.com/inbox?area=waiting-agent&company_id=<company_id>&chatId=<chat_id>`). El conector resuelve la empresa solo, así que casi nunca te lo dice de frente: buscalo en la salida cruda de `chat_ver` o de `pendientes_detail`, y si no está, pedile al dueño que pegue cualquier URL de su inbox, que lo lleva en el query string. **No lo inventes ni lo deduzcas**: un link con el id equivocado abre la app en otra empresa o en nada, y el dueño lo descubre clickeando.

**8. Sólo si hacía falta:** `pendientes_sweep_now()` cuando la cola se ve rara o vacía y el dueño dice que no puede ser, y después repetí el paso 5. `chat_ver(chat_id, tail=20)` sobre un puñado de chats cuando una pregunta concreta lo pide.

## Dónde vive la información: qué se ve desde acá y qué no

Esta sección existe para que no le prometas al dueño algo que este conector no puede dar. Lo que está afuera del alcance:

- **El texto literal de los pasos de un workflow.** Vive en otro servicio de Chatty que este conector no habla. La `descripcion` te dice qué hace el workflow, no qué lee el cliente. Cuando importe la palabra exacta, preguntale al dueño.
- **Cuántos chats hay en cada etapa del embudo.** `embudos_ver` da la estructura, no la población. Se lo pedís al dueño o que lo mire en su pantalla de CRM.
- **Un catálogo de etiquetas.** No hay agregado de etiquetas: las etiquetas aparecen de a un chat, dentro de `chat_ver` o `pendientes_detail`. Las que sí podés nombrar con certeza son las que aparecen como disparadores o efectos en workflows y etapas, y esas valen mucho justamente porque son las que gobiernan el circuito.
- **Un catálogo de productos.** Los productos aparecen por chat (en el contacto que devuelve `chat_ver`), no como lista de la empresa. Lo que sí podés nombrar es lo que aparezca nombrado en la configuración.
- **Las plantillas más allá de cuarenta.** `plantillas_ver` confiesa un tope de 40 textos: las de más vienen sin texto y la propia respuesta lo aclara. De las pendientes de aprobación y las rechazadas sólo viaja el conteo.
- **Una vista global de lo programado.** `programados_ver` es por chat, así que no sirve como agregado; sirve antes de afirmar que un chat puntual está resuelto.
- **Cuánta gente sigue disponible en una lista guardada.** `cuantos_congelados` es la cuenta del día que se guardó, no la de hoy.
- **Mover o cancelar un turno del calendario.** Acá se mira nada más. No hay ninguna tool de este conector que toque un turno agendado.
- **El tamaño real de lo sin leer.** `chats_sin_leer` está capado del lado del cliente: te da "n o más", nunca un total.

## Agregados, no muestras

Una llamada a `chat_ver` sobre un chat cualquiera no dice nada de la empresa, y sin embargo suena a que sí. El riesgo real no es el costo: es que un porcentaje sacado de diez chats se lee igual que el de la empresa entera y después nadie lo vuelve a verificar.

La regla: si un número viene de menos que todo, se escribe con la muestra pegada al lado ("en 10 chats leídos de la cola, 7 tenían la etiqueta X") y nunca como una distribución. Y si el dato no se puede agregar con estas 17 tools, se dice, con el listado de arriba a mano.

## Dónde queda el mapa

⚠️ **El directorio desde el que te llamaron casi nunca es el lugar.** El mapa es un documento comercial del dueño; la sesión, en cambio, puede estar parada en cualquier lado. Ya pasó dos veces: la corrida arrancó desde un repo de código que no tiene nada que ver con la empresa mapeada, y el retrato comercial quedó guardado adentro de ese repo. Nadie borra un archivo así, y medio año después es un archivo que nadie entiende por qué está ahí. **Escribir en el cwd es una decisión, no el default.**

Esto no es una receta con pasos fijos, es un criterio con un orden:

**1. Mirá la libreta antes que nada.** `~/chatty/donde-viven-los-mapas.md` es el único lugar que no depende de dónde estés parado. Si ya hay una línea para esta empresa, escribí ahí y no preguntes nada: evitar esa pregunta es todo el motivo por el que la libreta existe.

**2. Si no hay línea, fijate si el directorio donde estás tiene algo que ver con la empresa que acabás de mapear.** Miralo de verdad: el nombre del directorio, el README, el remoto de git, de qué habla lo que hay adentro. Si es el proyecto del negocio, el cwd está bien. Si es un repo de desarrollo de otra cosa, el proyecto de otro cliente, o una carpeta de paso, no lo está.

**3. Cuando no lo está, proponé un lugar y preguntá UNA sola vez.** La sugerencia por defecto es `~/chatty/`, que es donde ya vive la libreta y no le ensucia el repo a nadie. Al dueño casi siempre le da igual cuál sea mientras no quede perdido; lo que no le da igual es que se lo preguntes en cada corrida.

**4. Anotá la respuesta en la libreta**, la haya elegido él o la hayas sugerido vos y él aceptado. Una línea por empresa, y si el archivo no existe, crealo:

```markdown
# Dónde viven los mapas de Chatty
| Empresa | Archivo | Relevado |
|---|---|---|
| Panadería El Sol | ~/chatty/mapa-panaderia-el-sol.md | 2026-09-14 |
```

**El nombre del archivo lleva a la empresa adentro: `mapa-<empresa>.md`.** Dos motivos, los dos prácticos: dos empresas distintas no se pisan, y un archivo suelto se reconoce sin abrirlo. Un `mapa.md` pelado, tres meses después, no le dice nada a nadie. Si te encontrás un `chatty/mapa.md` viejo de una corrida anterior, tratalo como el mapa de esta empresa si el contenido coincide, y aprovechá para renombrarlo y anotarlo en la libreta.

El archivo va **con la fecha adentro**. No se vuelve a derivar en cada sesión: cuesta plata, y sobre todo el dueño tiene que poder corregir a mano lo que entendiste mal, que es la mitad del valor de escribirlo.

El archivo sigue el mismo orden que la pasada, y por el mismo motivo: primero el negocio, después el volumen.

```markdown
# Mapa de <empresa> en Chatty
Relevado el <fecha>. Corregilo a mano cuando algo no sea así: este archivo le gana a lo que yo deduzca.

## 1. Qué vende y cómo lo vende
<Todo esto sale de la configuración, no de leer conversaciones.>
- Qué vende: <productos, programas, servicios, camadas con fecha si las hay>
- La escalera comercial: <las etapas en orden, con el nombre que les puso la empresa, y qué significa cada peldaño>
- Agente de IA: <hay / no hay; si hay, cómo se llama y en qué parte del recorrido entra>
- Cómo persigue a la gente: <los seguimientos con sus tiempos exactos, uno por uno>
- Ventana horaria: <en qué horario le escribe a la gente>
- Frenos: <qué detiene una secuencia; por ejemplo, que conteste un humano>
- Las etiquetas que gobiernan el recorrido: <las que aparecen como disparador o efecto>

## 2. Qué está cableado
<Una capacidad puede existir y no servir porque falta la configuración. Esto decide qué rutinas son posibles.>
- Tools disponibles en la conexión: <n>. No disponibles: <cuáles y qué implica>
- Plantillas aprobadas: <n> (o ninguna, y qué implica)
- Listas / audiencias: <n> / <n>
- Campañas: <n> en borrador, <n> salidas
- Archivos: <n>
- Agenda: <n> recordatorios
- Borradores de IA frenados: <n>, el más viejo de <cuándo>
- Calendario: atado / no atado
- Workflows: <n>

## 3. Qué dispara qué
<La sección de cuidado: dónde una acción que parece administrativa le escribe a un cliente.>
- Etapas que al entrar pueden mandar un mensaje: <cuáles, de cuántas en total>
- Etapas inertes: <cuáles>
- Etapas con `configuracion_leida: false`: <cuáles; ahí no quedó establecido que sean inertes>

## 4. Volumen (contexto, no titular)
- Total en la cola: <n> (only_new=False). Truncado: sí/no
- Por motivo: <motivo: n> ... (se solapan, no suman)
- Sin leer: <n> (o "40 o más")
- Qué de esto explica la configuración: <por ejemplo, un motivo que coincide con el final diseñado del embudo>

## 5. Lo que no pude establecer
<cada cosa, con por qué y a quién preguntársela>

## 6. Correcciones del dueño
<acá escribe él>
```

## Identidad

El `company_id` va en el archivo con de dónde salió, arriba de todo o al pie, como te quede más limpio. Lo importante es que esté y que no sea inventado.

## Cuándo rehacerlo

- Cuando pasó más de un mes, porque el inventario cambia solo (alguien aprueba una plantilla, alguien arma un embudo).
- Cuando una rutina empieza a devolver cosas raras: suele ser el mapa viejo, no la rutina.
- Cuando el dueño cuenta un cambio de operación (sumó gente, prendió la IA, cambió de embudo).
- Nunca "por las dudas" al principio de cada sesión: para eso se escribió el archivo.

## Cómo se cierra

Contale el retrato primero, en dos o tres frases y hablado, no el archivo entero: qué vende su empresa, cómo está armado el recorrido, si tiene IA y cómo persigue a la gente, **todo presentado como lectura de su propia configuración** ("esto es lo que leo en cómo tenés armado Chatty, corregime lo que esté mal"). Que la primera reacción posible del dueño sea corregirte, no defenderse.

Recién después, y en una o dos frases más, lo que te llamó la atención del volumen, y siempre atado a lo anterior cuando la configuración lo explique.

Cerrá diciéndole la ruta exacta donde quedó el archivo, pidiéndole que corrija ahí lo que esté mal, y ofreciendo `rutinas-de-chatty`, que es lo que convierte esta foto en algo que trabaja.

El detalle de qué dice y qué no dice cada tool de lectura (los campos que mienten si los leés rápido) está en `references/lo-que-cada-tool-dice.md`. Leelo cuando vayas a apoyar una conclusión en un campo puntual.
