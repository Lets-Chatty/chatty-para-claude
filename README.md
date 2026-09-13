# Chatty para Claude

El conector de Chatty le da a tu Claude la capacidad de operar el WhatsApp del negocio: 45 herramientas sobre la cola de pendientes, los embudos, las plantillas, las campañas y la agenda. Este plugin le da el criterio, que es la otra mitad: qué preguntarle a esos datos, qué campos mienten si los leés rápido, y cómo escribir rutinas que sigan sirviendo al mes siguiente.

## Antes de instalar

Necesitás el conector de Chatty ya conectado en tu Claude. El plugin no trae herramientas ni credenciales: es criterio sobre las herramientas que ya tenés. Sin el conector se instala igual y no tiene nada sobre qué trabajar.

## Instalación

```
/plugin marketplace add Lets-Chatty/chatty-para-claude
/plugin install chatty@chatty-para-claude
```

## Qué hacer primero

**1. Corré el mapa.** Pedile a Claude que arme el mapa de tu empresa en Chatty. Es una pasada de sólo lectura (no manda ni un mensaje) que deja escrito en `chatty/mapa.md` el tamaño real de la cola, los motivos por los que cada chat está ahí, tus embudos, y el inventario de lo que está realmente cableado: plantillas aprobadas, listas, audiencias, campañas, archivos, calendario.

Leelo y corregilo a mano donde algo no sea así. Ese archivo le gana a lo que Claude deduzca, y la mitad de su valor es que vos lo edites.

**2. Después armá la primera rutina.** Pedile una rutina sobre el conector (un informe diario de la cola, un repaso semanal de prospectos, lo que se te esté escapando). Claude te va a entrevistar para que la rutina quede escrita con TUS criterios, no con los de otro negocio, y la va a correr contra tus datos reales antes de dártela por buena. Queda en `chatty/rutinas/`, en castellano, y se toca cuando quieras.

El orden importa: sin mapa, la entrevista de la rutina es un formulario en blanco.

## Qué trae

- **`mapa-de-chatty`** arma la foto de la empresa y el inventario de lo que se puede usar. Sólo lectura.
- **`rutinas-de-chatty`** convierte esa foto en rutinas que trabajan: entrevista, andamiaje que vos no tenés por qué saber que hace falta, y una corrida de prueba contra tus datos.
