# Chatty para Claude

El conector de Chatty le da a tu Claude la capacidad de operar el WhatsApp del negocio: cuarenta y pico de herramientas sobre la cola de pendientes, los embudos, las plantillas, las campañas y la agenda. Este plugin le da el criterio, que es la otra mitad: qué preguntarle a esos datos, qué campos mienten si los leés rápido, y cómo escribir rutinas que sigan sirviendo al mes siguiente.

## Antes de instalar

Necesitás el conector de Chatty ya conectado en tu Claude. El plugin no trae herramientas ni credenciales: es criterio sobre las herramientas que ya tenés. Sin el conector se instala igual y no tiene nada sobre qué trabajar.

No esperes un número redondo de herramientas: el servidor esconde del listado lo que tu empresa no puede usar (el calendario, por ejemplo, si no tenés uno atado), así que tu conexión va a ofrecer menos de las que hay registradas. Contarlas es la primera línea del mapa, y el faltante es información sobre tu cuenta, no una conexión rota.

## Instalación

```
/plugin marketplace add Lets-Chatty/chatty-para-claude
/plugin install chatty@chatty-para-claude
```

## Qué hacer primero

**1. Corré el mapa.** Pedile a Claude que arme el mapa de tu empresa en Chatty. Es una pasada de sólo lectura (no manda ni un mensaje) que deja escrito en un archivo tuyo el tamaño real de la cola, los motivos por los que cada chat está ahí, tus embudos, y el inventario de lo que está realmente cableado: plantillas aprobadas, listas, audiencias, campañas, archivos, calendario.

Leelo y corregilo a mano donde algo no sea así. Ese archivo le gana a lo que Claude deduzca, y la mitad de su valor es que vos lo edites.

Si la sesión estaba parada en una carpeta que no tiene nada que ver con tu negocio, Claude te va a proponer dónde guardarlo y te lo va a preguntar una sola vez: la respuesta queda anotada en `~/chatty/donde-viven-los-mapas.md`, que es lo que hace que no te lo vuelva a preguntar y lo que le permite encontrar el mapa la próxima vez, sin importar desde dónde arranques.

**2. Después armá la primera rutina.** Pedile una rutina sobre el conector (un informe diario de la cola, un repaso semanal de prospectos, lo que se te esté escapando). Claude te va a entrevistar para que la rutina quede escrita con TUS criterios, no con los de otro negocio, y la va a correr contra tus datos reales antes de dártela por buena. Queda al lado del mapa, en castellano, y se toca cuando quieras.

El orden importa: sin mapa, la entrevista de la rutina es un formulario en blanco.

## Qué trae

- **`mapa-de-chatty`** arma la foto de la empresa y el inventario de lo que se puede usar. Sólo lectura.
- **`rutinas-de-chatty`** convierte esa foto en rutinas que trabajan: entrevista, andamiaje que vos no tenés por qué saber que hace falta, y una corrida de prueba contra tus datos.

## El gasto de los anuncios: el conector de Meta (opcional, y no es de Chatty)

Chatty sabe todo lo que pasó en la conversación y nada de lo que costó traerla. El gasto de los anuncios vive en Meta, y es lo único grande que a Chatty le falta para que la pregunta "cuánto me sale una venta" tenga respuesta.

Meta publica su propio servidor para esto, hospedado por Meta, en `https://mcp.facebook.com/ads`. Este plugin lo trae declarado para ahorrarte el paso, pero **es de Meta y no de Chatty**: Chatty no ve tus anuncios, no guarda nada de eso, y si algo falla de ese lado no lo podemos arreglar nosotros. Se autoriza en el navegador con tu propia cuenta de Meta Business, y no tenés que generar ni pegar ningún token en ningún lado.

**Si no lo autorizás no se rompe nada.** Queda ahí sin conectar, figura como pendiente de autenticación en `/mcp`, y sus herramientas simplemente no aparecen. Las skills tratan esa ausencia igual que la del calendario: es información sobre tu cuenta, no un error, y nunca se te promete un informe que dependa de algo que no está.

Para agregarlo a mano, o para sacarlo:

```
claude mcp add --transport http meta-ads https://mcp.facebook.com/ads
claude mcp remove meta-ads
```

Si tu empresa usa una app propia de Meta, Meta documenta esta otra forma, que además pide configurar la redirect URL en los ajustes de Facebook Login for Business:

```
claude mcp add --transport http --client-id <META_APP_ID> meta-ads https://mcp.facebook.com/ads
```

⚠️ **Esa URL es la única.** Buscando vas a encontrar varios servidores de terceros con nombres parecidos, que te piden generar un token de tu cuenta publicitaria y pegárselo. Ninguno es de Meta. Conectar la cuenta de anuncios al lugar equivocado es la peor cosa que puede salir mal acá, así que si la dirección no es exactamente la de arriba, no sigas.

Todavía no hay una skill que cruce el gasto con las ventas: falta una pieza del lado de Chatty para que la atribución no quede coja. Por ahora el conector queda disponible y explicado, que es lo honesto.
