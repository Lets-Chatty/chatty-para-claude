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
