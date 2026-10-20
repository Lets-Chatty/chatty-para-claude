# Ejemplo: de la entrevista al flujo

Una veterinaria de barrio, inventada. Sirve para ver la forma de la entrevista y del plan, no para copiar sus textos.

## Lo que dijo `flujo_ver`

En borrador, sin bienvenida, sin horario, Consulta general sin armar, ningún motivo. Se lo contaste así:

> Hoy el flujo está en borrador y vacío: tus clientes se atienden como siempre. Para lanzarlo hace falta, como mínimo, decidir qué recibe alguien que escribe. Te hago unas preguntas y lo armamos.

## La entrevista, en tres tandas

**Primera tanda.** ¿Qué días y horarios atienden? ¿Qué querés que reciba alguien que escribe con el local cerrado? ¿Por qué te escriben más seguido?

> De lunes a viernes de 9 a 13 y de 16 a 20, sábados de 9 a 13. Cerrados que les digan que mañana les contestamos y que si es una urgencia llamen a la guardia. Escriben por turnos, por precios de vacunas y por pedidos de alimento.

**Segunda tanda.** Para cada uno, qué contestás hoy. ¿Tenés un agente de IA en Chatty?

> Los precios de vacunas los mando siempre igual, tengo la lista. Los turnos los da Sofía (la IA) que ve la agenda. Los pedidos de alimento los ve Marcos.

**Tercera tanda, con una propuesta para corregir.** «Si a los de precios les mandamos la lista y en 3 horas no contestan, ¿les escribimos "¿Pudiste verla? Si querés te reservo un turno"? ¿Y a la hora 24 lo damos por sin respuesta?»

> Sí, pero el segundo mensaje al día siguiente, no a las 3 horas.

## El plan que le mostraste antes de cargar

| | Lo que les enviamos | Si no responde | Al entrar |
|---|---|---|---|
| Bienvenida | «¡Hola! Gracias por escribir a la veterinaria…» | | |
| Fuera de horario | «Estamos cerrados. Mañana te contestamos. Si es una urgencia, llamá a la guardia…» | | |
| Consulta general | Contesta Sofía | Sofía retoma a las 3 h; sin respuesta a las 24 h | |
| Precios de vacunas | La lista (texto) | «¿Pudiste verla?…» al día siguiente; sin respuesta a las 48 h | |
| Turnos | Contesta Sofía: «ofrecé los horarios libres» | Como Consulta general | |
| Pedidos de alimento | «¡Hola! Ya le paso tu pedido a Marcos…» | Ninguno | Queda para Marcos |

El dueño cambió una palabra de la bienvenida. Recién ahí se cargó, en el orden de la skill: bienvenida, horario con aviso, Consulta general, los tres motivos (crear y cargarles lo que les enviamos), seguimientos, y asignar «Pedidos de alimento» a Marcos.

## Las pruebas

Cuatro clientes de prueba: «¿cuánto sale la vacuna antirrábica?», «¿tienen turno el sábado?», «quiero pedir dos bolsas de alimento», y «¿hacen baño y corte?» (no encaja en ninguno: tiene que caer en Consulta general). El de alimento cayó en Consulta general: se le sumaron las frases «necesito alimento» y «¿me traen balanceado?» al motivo y se volvió a probar.

## La activación

> Si lo activo, desde el próximo mensaje todos los chats de tus clientes entran por este flujo: la bienvenida, el horario, Consulta general con Sofía, y los motivos Precios de vacunas, Turnos y Pedidos de alimento con sus seguimientos. ¿Lo activo?

Sólo después de «sí, activalo», `flujo_activar` con `confirmo: true`.
