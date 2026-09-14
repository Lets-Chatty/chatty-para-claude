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

Meta publica su propio servidor para esto, hospedado por Meta, en `https://mcp.facebook.com/ads`. Este plugin lo trae declarado para ahorrarte el paso, pero **es de Meta y no de Chatty**: Chatty no ve tus anuncios, no guarda nada de eso, y si algo falla de ese lado no lo podemos arreglar nosotros.

**Se autoriza con tu propia cuenta, y no necesitás ser desarrollador ni tener una app de Meta.** Meta lista a Claude Code entre los agentes soportados y la conexión es la de cualquier servidor remoto: la primera vez que se use, se abre el diálogo de Facebook Login for Business, entrás con tu cuenta y aprobás los permisos que te pide. No hay token que generar ni que pegar en ningún lado, y no pasa por la revisión de ninguna app, que es lo que suele trabar este tipo de integración.

**Si no lo autorizás no se rompe nada.** Queda ahí sin conectar, figura como pendiente de autenticación en `/mcp`, y sus herramientas simplemente no aparecen. Las skills tratan esa ausencia igual que la del calendario: es información sobre tu cuenta, no un error, y nunca se te promete un informe que dependa de algo que no está.

Para agregarlo a mano, o para sacarlo:

```
claude mcp add --transport http meta-ads https://mcp.facebook.com/ads
claude mcp remove meta-ads
```

⚠️ **Esa URL es la única.** Buscando vas a encontrar varios servidores de terceros con nombres parecidos, que te piden generar un token de tu cuenta publicitaria y pegárselo. Ninguno es de Meta. Conectar la cuenta de anuncios al lugar equivocado es la peor cosa que puede salir mal acá, así que si la dirección no es exactamente la de arriba, no sigas.

### Si te preocupa que te toquen los anuncios

Es una preocupación razonable y la respuesta no depende de nosotros: el control lo pone Meta, del lado de tu cuenta. Si tenés control total de un portafolio comercial, en los ajustes de Meta Business Suite, abajo de Integraciones, hay una sección del servidor MCP donde permitís o bloqueás acciones por cuenta publicitaria, incluido poner un tope de presupuesto. El servidor de Meta hace cumplir esas reglas, así que un bloqueo ahí vale más que cualquier promesa de este lado. Y por si eso fuera poco, los anuncios que se crean desde un agente nacen pausados hasta que vos los prendas.

Para lo que hace este plugin alcanza con lectura: lo que nos falta es el gasto.

### Nota al pie: sólo si ya tenés una app de Meta propia

No es el camino recomendado y no hace falta para nada de lo anterior. Si tu empresa está obligada a pasar por su propia app de desarrollador, Meta documenta esta otra forma, que además pide configurar la redirect URL en los ajustes de Facebook Login for Business:

```
claude mcp add --transport http --client-id <META_APP_ID> meta-ads https://mcp.facebook.com/ads
```

Todavía no hay una skill que cruce el gasto con las ventas: falta una pieza del lado de Chatty para que la atribución no quede coja. Por ahora el conector queda disponible y explicado, que es lo honesto.
