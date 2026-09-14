# La familia de prospectos de alto potencial, trabajada

Esta es la familia con la que se arranca. Abajo está primero cómo se entrevista, después la plantilla lista para completar, y al final por qué cada pieza está donde está.

La plantilla sale de una rutina real que funciona hace meses en un negocio de verdad, generalizada y con tres agujeros tapados que se comen casi todas las rutinas de este tipo. **Lo que está entre `<>` lo contesta el dueño en la entrevista y no lo inventás vos**, empezando por el menú de ofertas, que es el corazón.

## Qué hay que sacar de la entrevista

1. Qué vende y cuál es el próximo paso que quiere que dé la gente.
2. Las dos o tres patologías de su pipeline, dichas como criterio de selección y no como deseo. "Prospectos que mostraron interés fuerte y no se comprometieron" y "prospectos que tuvieron reunión y se frenaron" son criterios; "los buenos" no lo es.
3. El menú de ofertas: cada freno posible con su oferta distinta, y qué resultado da cada rama.
4. Qué dato suyo vive en texto libre de los chats (volumen, presupuesto, tamaño del equipo) y por lo tanto puede faltar.
5. Cadencia, tope y quién lo lee.
6. Si el informe tiene que aterrizar en algún lado además de contestarle a él.

## Plantilla

```markdown
# <nombre de la rutina>
Contesta: <qué pregunta del negocio contesta>.
Se corre: <cadencia>.
Escrita sobre el mapa del <fecha del mapa>.

Revisá la actividad comercial de los últimos <N> días en Chatty y marcá los
prospectos que ameritan que se meta <quién: el dueño, el socio, quien sea>.

Escribí el informe entero en <idioma>.

## Antes de empezar
Leé `<nombre>.estado.md`, al lado de este archivo. Ahí está lo que marcaste la última vez
y cuándo. El informe abre con qué cambió desde entonces: quién se movió, quién
sigue igual, quién entró nuevo, quién salió.

## A quién marcar
1. <patología 1 del dueño, dicha como criterio verificable>
2. <patología 2>
3. <patología 3, si la hay>

## La tabla
Una fila por prospecto marcado, con estas columnas:
- Prospecto (nombre o empresa)
- Chat: el link, nunca el chat_id crudo, con este formato exacto
  https://app.letschatty.com/inbox?area=waiting-agent&company_id=<company_id>&chatId=<chat_id>
- Primer contacto: hace cuánto, comparando la fecha de creación del chat contra hoy
- Último mensaje: hace cuánto, quién lo mandó, y si quedó sin respuesta
- <dato del negocio que vive en texto libre>: lo que el prospecto haya dicho,
  o "no especificado" si no lo dijo. Prohibido estimarlo.
- <la perilla del menú de ofertas, ej. "¿lleva <oferta A>?">: <valores posibles>
- Por qué es de alto potencial
- Próximo paso recomendado

Si un dato no está en el chat, va "no especificado". No lo deduzcas del rubro,
del tamaño aparente ni de otros clientes parecidos.

## Cómo se decide el próximo paso
Razoná por el freno REAL del prospecto, no por cuánto interés mostró:
(a) <freno A> -> <oferta A> -> <resultado en la columna de la perilla>
(b) <freno B> -> <oferta B> -> <resultado>, aclarando que <por qué ese resultado
    no es un no de menor calidad>
(c) <freno C> -> <oferta C> -> <resultado>

## Orden
Ordená la tabla entera por <criterio con opinión: tamaño del negocio y qué tan
vivo está>.

## Cierre
Después de la tabla, <N> líneas de resumen: qué cambió, dónde está la plata, qué
harías primero.
Máximo <tope> filas. Si hay más, quedate con las <tope> más urgentes y decí
cuántas quedaron afuera.
Si no hay ninguno que cumpla los criterios, decilo en una línea y terminá ahí.
No completes la tabla con casos flojos para que no quede vacía.

## Al terminar
Actualizá `<nombre>.estado.md` con los marcados de hoy y la fecha.
<Si el dueño pidió que aterrice en algún lado: mostrale el texto y pedile
confirmación antes de mandarlo. Nombrá acá la herramienta exacta que lo manda.>
```

## Con qué tools se llena

Con las de lectura que lista `mapa-de-chatty`, y en este orden:

1. `pendientes_list(only_new=False, reasons=[...])` filtrando por los motivos que se parecen a las patologías que dijo el dueño. `senal_compra_sin_seguimiento` y `silencio_post_info` suelen ser el punto de partida de esta familia. Pedí un `limit` acorde al tope de la rutina, no doscientas filas para mostrar cinco.
2. `pendientes_detail(pendiente_id)` sólo sobre los candidatos que van a entrar a la tabla. Ahí está el hilo (para el porqué), el estado CRM y `free_text_window`.
3. `chat_ver` sólo si hace falta ir más atrás en la historia de un chat puntual.

Antes de recomendar cualquier próximo paso que implique escribirle a alguien, mirá `automations_in_flight` de la fila y, si la cosa es fina, `programados_ver(chat_id)`: recomendarle al dueño que salude a alguien que ya tiene un mensaje nuestro en camino lo deja pagando.

Y mirá `free_text_window` antes de prometer un contacto. Con la ventana cerrada, escribirle a esa persona cuesta una plantilla paga que además tiene que aceptar, así que el próximo paso honesto es otro. Si el mapa dice que la empresa no tiene plantillas aprobadas, esos prospectos directamente no son alcanzables por WhatsApp y hay que decirlo en el informe en vez de recomendar algo imposible.

## Por qué cada pieza está donde está

- **Los criterios de selección van antes que el formato.** Casi todo el mundo escribe el formato primero y deja la selección en "los buenos prospectos". El formato es lo fácil: lo que hace que el informe sirva es la definición de a quién marcar.
- **Clasificar por el freno y no por el interés** es lo que hace que la tabla decida en vez de listar. Tres frenos distintos llevan a tres ofertas distintas; sin eso escrito, las tres ramas colapsan en "hacele seguimiento", que es lo que el dueño ya iba a hacer solo.
- **Que una rama dé un resultado distinto sin ser peor** hay que decirlo con todas las letras. Si no, el modelo interpreta el valor negativo como fracaso y empuja a todos hacia la rama cara.
- **El link con el `company_id` adentro** convierte un informe en una herramienta. Es la diferencia entre leer y hacer.
- **Tiempo relativo** porque la urgencia comercial es relativa, y porque un timestamp obliga a una cuenta mental que nadie hace.
- **El permiso de volver vacío** existe porque una tabla de ocho columnas es una invitación a llenarla. Sin esa línea, un modelo te va a encontrar prospectos aunque no los haya.
- **El "no especificado"** marca qué dato no se puede inventar. Los datos que salen de texto libre de un chat son exactamente los que un modelo completa con una estimación razonable y falsa.
- **El orden con opinión** es lo que hace que el dueño lea de arriba para abajo y pare cuando se le acaba el tiempo, sabiendo que lo que dejó abajo importaba menos.
- **La ventana temporal, la memoria entre corridas y la confirmación antes de publicar** son los tres agujeros que casi nunca están tapados. "Reciente" hace que cada corrida mire algo distinto sin que nadie se entere; sin memoria el mismo prospecto aparece igual cinco lunes seguidos y el informe se vuelve ruido; y una acción que sale hacia afuera se confirma en el momento, con el texto a la vista, por más que la rutina la haya escrito el propio dueño.
