---
name: mapa-de-chatty
description: Usala la primera vez que este Claude trabaja con el conector de Chatty (el WhatsApp del negocio), antes de diseñar cualquier rutina, informe o digest, y cuando el mapa guardado quedó viejo. Arma la foto de la empresa (volumen de la cola, motivos, embudos, ritmo) y el inventario de lo que está realmente cableado (plantillas aprobadas, listas y audiencias, campañas, archivos, agenda, calendario) y lo deja escrito en `chatty/mapa.md`. Es una pasada de SÓLO LECTURA: no manda ni un mensaje.
---

# El mapa de la empresa en Chatty

El conector te da capacidad sobre el WhatsApp de trabajo del dueño y ninguna idea de qué preguntarle. Esta skill arma la foto que falta, y lo hace antes que cualquier otra cosa por una razón práctica: sin mapa, la entrevista que viene después (`rutinas-de-chatty`) es un formulario en blanco. Si le preguntás en frío "¿cuáles son tus dos formas de trabarte?", contesta "no sé". Si le decís "tenés 340 chats esperando respuesta hace más de cinco días, y de tus doce etiquetas se usan tres", la misma pregunta se vuelve contestable.

## Regla dura: acá sólo se lee

De las 45 tools del conector, 28 escriben o le mandan mensajes a clientes reales. Un diagnóstico que manda un WhatsApp es inaceptable: el dueño te dejó entrar a mirar su negocio, no a hablarle a su cartera.

**La lista autorizada no se inventa ni se deduce por el nombre.** Es esta tabla, que espeja el mismo recorte que aplica el servidor cuando una conexión es de sólo lectura. Son estas 17:

| Tool | Qué contesta |
|---|---|
| `pendientes_list` | El tamaño de la cola y su distribución por motivo |
| `pendientes_detail` | Un chat de la cola en profundidad (hilo, estado CRM, ventana de 24h) |
| `pendientes_sweep_now` | Vuelve a barrer ahora en vez de esperar al barrido de la hora |
| `chats_sin_leer` | Qué entró y nadie abrió |
| `chat_ver` | Un chat puntual: contacto, etiquetas, productos, hilo |
| `list_pending_drafts` | Borradores de la IA frenados esperando una decisión del dueño |
| `embudos_ver` | Los embudos, sus etapas, y qué dispara entrar a cada una |
| `automatizaciones_ver` | Qué workflows existen (los nombres, no el texto que mandan) |
| `programados_ver` | Qué mensajes ya están en cola para salirle a un contacto |
| `listas_ver` | Listas congeladas y audiencias (filtros con nombre) |
| `campanas_ver` | Campañas en borrador y campañas ya salidas, con a cuántos llegaron |
| `plantillas_ver` | Las plantillas que aprobó Meta, con el texto real de cada una |
| `archivos_ver` | La biblioteca de archivos de la empresa |
| `agenda_ver` | Los recordatorios que escribió una persona (no es la cola) |
| `contactos_previsualizar` | Qué pasaría si importaras unos números, sin importarlos |
| `calendar_list_events` | Turnos ya agendados, si hay calendario atado |
| `calendar_check_availability` | Huecos libres reales, si hay calendario atado |

**Lo que hace que esto importe de verdad:** el servidor sólo bloquea las otras 28 cuando el token de la conexión tiene el permiso `readonly`, y una conexión normal NO lo tiene. O sea que durante el mapa no hay ninguna red abajo: la única barrera sos vos. Tratá la lista como un contrato, no como una sugerencia, y ante la duda de si una tool escribe, no la llames.

`pendientes_sweep_now` es la única de la lista que escribe, y está adentro a propósito: escribe en nuestra propia cola y no le manda nada a nadie. Lo peor que puede hacer es mostrarle al dueño un chat que ya estaba ahí.

**Si alguna de las 17 no aparece en tu listado de tools, eso es información y no un error.** El servidor esconde del listado lo que esta empresa no puede usar: las dos de calendario no se ofrecen cuando la empresa no tiene un calendario de Google atado, y hay familias enteras que dependen de lo que la empresa tenga contratado. Anotalo en el mapa como "no disponible", no lo llames igual para ver qué pasa, y no le prometas al dueño una rutina que dependa de eso.

## La pasada, en orden y con topes

Una empresa grande tiene mucha data y vos pagás cada token que traés. Todo lo que sigue está pensado para traer agregados, no montañas de filas.

**1. Contá tus propias tools antes de llamar a ninguna.** Cuántas tenés y cuáles de las 17 faltan. Eso ya es la primera línea del inventario.

**2. El tamaño real de la cola.** `pendientes_list(limit=5, only_new=False)`. El truco es que los agregados viajan en `summary` (`summary.total` / `total_matching` y `summary.by_reason`), no en las filas, así que pedir cinco filas te da los mismos números que pedir doscientas y cuesta cuarenta veces menos. Leé siempre `returned` y `truncated` antes de decir un número en voz alta: `truncated: true` significa que las filas son un pedazo, nunca que la cola es del tamaño de lo que podés contar.

Los cinco motivos (`nunca_respondido`, `lead_sin_respuesta`, `silencio_post_info`, `sin_cierre`, `senal_compra_sin_seguimiento`) se solapan: un chat puede traer varios, así que los conteos por motivo no suman el total. No los sumes.

⚠️ `nunca_respondido` NO quiere decir "nunca les respondimos": quiere decir que su último mensaje quedó sin respuesta y que además no hay ninguna automatización corriendo sobre ese chat. Escribilo así en el mapa. Decirle a un dueño que su equipo abandonó a gente que en realidad atendió durante un año es la clase de error que se descubre abriendo un solo chat, y te quema la credibilidad de todo el resto del informe.

**3. Lo que entró y nadie abrió.** `chats_sin_leer(limit=40)`. Si vuelve exactamente 40, lo que sabés es "40 o más", no "40". Decilo así.

**4. La estructura comercial.** `embudos_ver()` una vez, sin `funnel_id`: te da los embudos, las etapas en orden, y para cada etapa `al_entrar` con sus efectos y `puede_enviar_mensajes`. Ese último campo es oro para el mapa, porque marca qué movimientos de CRM son en realidad acciones hacia afuera. Y mirá `configuracion_leida`: en `false` la etapa no se pudo leer entera, así que "no tiene automatizaciones" no quedó establecido.

**5. Qué automatizaciones existen.** `automatizaciones_ver()`. Trae nombres e ids, no el texto que le llega al cliente: ese vive en otro servicio que este conector no habla. Cuando importe qué va a leer la persona, preguntale al dueño en vez de adivinar por el nombre del workflow.

**6. El inventario, una llamada cada uno:** `plantillas_ver()`, `listas_ver()`, `campanas_ver()`, `archivos_ver()`, `agenda_ver()`, `list_pending_drafts(limit=50)`.

Esto es la mitad menos obvia del trabajo y la que más cambia lo que después se puede diseñar: una tool puede existir y no servir para nada porque falta la configuración. Sin plantillas aprobadas no hay forma de escribirle a nadie con la ventana de 24 horas cerrada, y una rutina de reactivación es imposible. Sin embudos creados, "higiene del embudo" no existe. Sin audiencias guardadas, cada envío arranca de cero. Anotá cada cosa como "hay / no hay / hay pero", con el número.

**7. El `company_id`.** Hace falta para armar links clickeables en las rutinas que vienen después (`https://app.letschatty.com/inbox?area=waiting-agent&company_id=<company_id>&chatId=<chat_id>`). El conector resuelve la empresa solo, así que casi nunca te lo dice de frente: buscalo en la salida cruda de `chat_ver` o de `pendientes_detail`, y si no está, pedile al dueño que pegue cualquier URL de su inbox, que lo lleva en el query string. **No lo inventes ni lo deduzcas**: un link con el id equivocado abre la app en otra empresa o en nada, y el dueño lo descubre clickeando.

**8. Sólo si hacía falta:** `pendientes_sweep_now()` cuando la cola se ve rara o vacía y el dueño dice que no puede ser, y después repetí el paso 2. `chat_ver(chat_id, tail=20)` sobre un puñado de chats cuando una pregunta concreta lo pide.

## Agregados, no muestras

Una llamada a `chat_ver` sobre un chat cualquiera no dice nada de la empresa, y sin embargo suena a que sí. El riesgo real no es el costo: es que un porcentaje sacado de diez chats se lee igual que el de la empresa entera y después nadie lo vuelve a verificar.

La regla: si un número viene de menos que todo, se escribe con la muestra pegada al lado ("en 10 chats leídos de la cola, 7 tenían la etiqueta X") y nunca como una distribución. Y si el dato no se puede agregar con estas 17 tools, se dice.

Dos casos concretos donde el techo se nota, y conviene tenerlos escritos antes de prometer nada:

- **Cuántos chats hay en cada etapa del embudo.** `embudos_ver` te da la estructura, no la población. Preguntáselo al dueño o pedile que lo mire en la pantalla de CRM.
- **Qué etiquetas se usan de verdad y cuáles están muertas.** No hay agregado de etiquetas acá: las etiquetas de un chat salen de `chat_ver` o `pendientes_detail`, de a uno. Lo honesto es pedirle al dueño la lista de las que considera vivas y contrastarla con lo que hayas visto, marcado como muestra.

## Dónde queda el mapa

En el proyecto del cliente, en `chatty/mapa.md`, **con la fecha adentro**. No se vuelve a derivar en cada sesión: cuesta plata, y sobre todo el dueño tiene que poder corregir a mano lo que entendiste mal, que es la mitad del valor de escribirlo.

Estructura mínima del archivo:

```markdown
# Mapa de <empresa> en Chatty
Relevado el <fecha>. Corregilo a mano cuando algo no sea así: este archivo le gana a lo que yo deduzca.

## Identidad
- company_id: <id> (de dónde salió)
- Tools disponibles en la conexión: <n>. No disponibles: <cuáles y qué significa>

## La cola
- Total: <n> (only_new=False). Truncado: sí/no
- Por motivo: <motivo: n> ... (se solapan, no suman)
- Sin leer: <n> (o "40 o más")

## Estructura comercial
- Embudos y etapas, marcando las etapas que al entrar pueden mandar un mensaje
- Workflows existentes (sólo nombres: el texto no se puede leer desde acá)

## Inventario (qué está realmente cableado)
- Plantillas aprobadas: <n> (o ninguna, y qué implica)
- Listas / audiencias: <n> / <n>
- Campañas: <n> en borrador, <n> salidas
- Archivos: <n>
- Agenda: <n> recordatorios
- Borradores de IA frenados: <n>, el más viejo de <cuándo>
- Calendario: atado / no atado

## Lo que no pude establecer
<cada cosa, con por qué y a quién preguntársela>

## Correcciones del dueño
<acá escribe él>
```

## Cuándo rehacerlo

- Cuando pasó más de un mes, porque el inventario cambia solo (alguien aprueba una plantilla, alguien arma un embudo).
- Cuando una rutina empieza a devolver cosas raras: suele ser el mapa viejo, no la rutina.
- Cuando el dueño cuenta un cambio de operación (sumó gente, prendió la IA, cambió de embudo).
- Nunca "por las dudas" al principio de cada sesión: para eso se escribió el archivo.

## Cómo se cierra

Mostrale el mapa al dueño en dos o tres frases, no el archivo entero, y pedile que corrija lo que esté mal. Después ofrecele `rutinas-de-chatty`, que es lo que convierte esta foto en algo que trabaja.

El detalle de qué dice y qué no dice cada tool de lectura (los campos que mienten si los leés rápido) está en `references/lo-que-cada-tool-dice.md`. Leelo cuando vayas a apoyar una conclusión en un campo puntual.
