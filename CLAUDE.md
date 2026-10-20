# chatty-para-claude — notas para el que venga a tocar el plugin

Este repo es **sólo el plugin**: el criterio que un Claude necesita para usar
bien el conector de Chatty. No hay servidor acá, no hay credenciales, no hay
tools. El servidor del conector se desarrolla aparte y no es público, y este
repo existe justamente para que el plugin sí lo sea: un cliente no puede
instalar desde donde no tiene acceso.

⚠️ **ESTE REPO ES PÚBLICO.** Lo clona cualquiera que instale el plugin, y lo
lee un cliente. No entra acá: rutas a código privado, nombres de repos
privados, nombres de servicios internos o de despliegue, fechas de decisiones
internas, ids de base de datos, ni nada de lo que se hable puertas adentro. La
limpieza ya se hizo una vez y se deshace sola en cuanto alguien pega un párrafo
de un handoff: si estás por copiar texto desde otro lado, reescribilo.

## Estructura

```
.claude-plugin/marketplace.json     el marketplace (la raíz del repo)
plugins/chatty/
  .claude-plugin/plugin.json        el manifiesto del plugin
  skills/mapa-de-chatty/            la foto de la empresa, sólo lectura
  skills/rutinas-de-chatty/         la entrevista que escribe las rutinas
  skills/anuncios-y-ventas/         anuncios → leads → ventas, cruzado con el gasto de Meta
  skills/whatsapp-del-negocio/      que las preguntas del día a día vayan al conector
  skills/probar-mi-chatty/          simular un cliente que escribe y ver cómo responde (chats de prueba)
  skills/optimizar-por-conversiones/ que Meta optimice por las ventas y leads buenos reales (API de Conversiones)
  skills/flujo-de-atencion/         armar con el dueño el flujo de atención (bienvenida, horario, motivos, seguimientos), probarlo y activarlo
```

⚠️ **El plugin va en `plugins/chatty/` y no en la raíz, y no es prolijidad.**
Con el marketplace y el plugin compartiendo raíz, `claude plugin validate`
resuelve el marketplace y **nunca llega a validar el `plugin.json`**: queda sin
chequear para siempre, sin que nada avise. Separarlos es lo único que hace que
el manifiesto se valide alguna vez.

## Validar

Los dos niveles, siempre, porque cada uno chequea una cosa distinta:

```
claude plugin validate .
claude plugin validate plugins/chatty
```

## Instalar (lo mismo que hace un cliente)

```
/plugin marketplace add Lets-Chatty/chatty-para-claude
/plugin install chatty@chatty-para-claude
```

Después de tocar algo, probalo entrando por acá y no desde el checkout local:
es el único camino que reproduce lo que le pasa a alguien de afuera.

## Al escribir las skills

- Castellano rioplatense, dirigido al Claude que va a hacer el trabajo.
- **Nada de conteos de tools que envejecen.** El servidor esconde del listado
  lo que la empresa no puede usar, así que un número fijo queda mal el día que
  entra o sale una tool, y queda mal igual para cualquier cliente cuyo listado
  no coincida. La regla que sí vale: contar las que tenés y escribir eso.
- La tabla de tools de sólo lectura de `mapa-de-chatty` **sí** es un contrato
  explícito y se mantiene a mano: es lo que impide que un diagnóstico le mande
  un WhatsApp a la cartera del dueño.

## Cada skill también se baja suelta, como `.zip`

En Claude (web o escritorio) no hay plugins: cada skill se sube por separado como un `.zip`. El sitio del Conector arma esos `.zip` a partir de ESTE repo (una ref configurable, `main` por defecto) cada vez que se publica, y muestra una tarjeta por skill. Tres consecuencias para el que escribe acá:

- **El frontmatter tiene que ser YAML estricto.** Claude Code perdona un `description` sin comillas que tenga `: ` adentro; el validador de la subida de skills, no. Ante la duda, entre comillas dobles. `name` en minúsculas con guiones, hasta 64, igual al nombre de la carpeta; `description` hasta 1024 caracteres y sin `<` ni `>`. El armado del sitio falla si alguna no cumple, a propósito.
- **`metadata` es lo que ve el dueño en la tarjeta**: `titulo`, `frase` (con qué se la pide a Claude) y `resumen` (qué hace, dicho para el dueño y no para Claude, que para eso está el `description`). Una skill sin `metadata` se publica igual, con su nombre pelado.
- **Lo que esté en la carpeta de la skill va en el `.zip`**, nada más: una skill que dependa de un archivo de otra carpeta queda rota al subirla sola.

## Sin bump de versión, el cambio no le llega a nadie

⚠️ **Un cambio en una skill que no venga con un `version` nuevo en
`plugins/chatty/.claude-plugin/plugin.json` NO llega a quien ya tiene el plugin
instalado.** `claude plugin marketplace update` refresca el clon del
marketplace, pero el instalador compara la versión y, si es la misma, deja el
cache como está. El resultado es el peor posible para depurar: el repo tiene el
contenido nuevo, el marketplace local también, y la skill que se carga sigue
siendo la vieja.

Pasó el 2026-09-14: se reescribió entera la skill del mapa, se pusheó, el dueño
la corrió, y obtuvo la versión anterior. Se descubrió comparando una frase
distintiva del contenido nuevo contra el archivo que vive en
`~/.claude/plugins/cache/`.

**Cómo verificar de verdad qué se va a cargar**, que no es mirar el repo:

```bash
grep -c "<una frase distintiva del contenido nuevo>" \
  ~/.claude/plugins/cache/chatty-para-claude/chatty/*/skills/<skill>/SKILL.md
```

El número de versión está en la ruta del cache, así que un bump crea una carpeta
nueva y el problema desaparece solo.

⚠️ `claude plugin install` **no acepta `--force`** (al menos hasta la 2.1.72).
Reinstalar sobre la misma versión no sirve: o se bumpea, o se desinstala y se
vuelve a instalar.

## El conector de Meta va declarado en el `plugin.json`

El plugin declara un servidor MCP remoto, el de Meta Ads, en el campo `mcpServers`
del manifiesto. Es lo único que trae el plugin además de skills, y existe porque
el gasto de publicidad es lo único grande que Chatty no sabe.

```json
"mcpServers": { "meta-ads": { "type": "http", "url": "https://mcp.facebook.com/ads" } }
```

⚠️ **Esa URL no se toca, no se deduce y no se reemplaza por ninguna alternativa.**
Hay varios servidores de terceros con nombres parecidos que piden un token de la
cuenta publicitaria; ninguno es de Meta, y un cliente conectando su cuenta de
anuncios al lugar equivocado es el peor resultado posible de este repo. La única
fuente válida es la documentación de Meta, que dice textualmente que el servidor
está hospedado en esa dirección.

**Por qué no lleva `client-id`, que es lo primero que alguien le va a querer
agregar:** el camino sin app propia es el camino principal y el que sirve para
un cliente. El Help Center de Meta lista a Claude Code entre los agentes
soportados y, para explicar cómo se conecta, linkea derecho a la instrucción de
servidor HTTP remoto de la documentación de Claude Code, la que no lleva App ID.
El `<META_APP_ID>` que aparece en el ejemplo de la documentación para
desarrolladores es para quien YA tiene app propia, no es el requisito general, y
encima ese camino pasa por la revisión de la app, que es justo donde se traba el
permiso de lectura de anuncios. Del lado técnico el servidor anuncia registro
dinámico de cliente (`registration_endpoint` en
`/.well-known/oauth-authorization-server/ads` de Facebook), así que el cliente
se registra solo. Ponerle un App ID inventado o el de otro lo rompería para
todos.

Si no lo autoriza, el servidor queda pendiente de autenticación y sus tools no
aparecen: no rompe la sesión ni el resto del plugin. Las skills tratan esa
ausencia igual que la de las tools de calendario, y ese paralelo es deliberado.
