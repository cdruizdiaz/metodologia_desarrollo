# Datos del artifact del proyecto

`artifact/proyecto.html` es una página genérica: no guarda contenido. Todo lo que muestra
lo lee de la base de datos del artifact. Claude Code escribe el contenido con la herramienta
`ArtifactData`; las personas marcan casillas en la página.

El texto de las etapas Funcional, Arquitectura, Setup y Plan de fases no se edita en la página:
se trabaja conversando con Claude Code, que lo escribe acá en cada avance. El Markdown se
muestra como texto enriquecido (títulos, tablas, listas y diagramas `mermaid`).

## Quién escribe qué

| Colección | La escribe | Para qué |
|---|---|---|
| `proyecto`, `etapas`, `fases`, `entregas`, `observaciones`, `decisiones`, `salidas` | Claude Code | Contenido y estados |
| `pantallas` | Claude Code, y personas desde la página mientras la etapa no esté Aprobada | Pantallas y sus imágenes |
| `backlog` | Personas (pedido nuevo) y Claude Code (número, destino) | Pedidos |
| `marcas/<dueño>/items` | **Solo personas**, desde la página | Casillas, textos de observación, cierre |

**Regla dura:** Claude Code nunca crea, cambia ni borra documentos bajo `marcas/`. Solo los lee.
Un estado que solo da una persona (`Aprobada`, `Cerrada`, `Aceptada`) se escribe únicamente
cuando existe la marca que lo respalda.

Cada escritura sobre un documento que ya existe lleva `if_version` con la versión leída.
Para escribir varios documentos, usar `batch`.

## Documentos

### `proyecto/info`
```json
{ "nombre": "…", "etapa": 1, "proximoPaso": "…",
  "ambientes": { "develop": "…", "revision": "…", "produccion": "…" },
  "roles": { "oo": "…", "analista": "…", "qa": "…", "desarrollo": "…" } }
```
`etapa` va de 1 a 6.

### `proyecto/probar`
`{ "md": "…" }` — dirección de revisión, cuentas de prueba y trucos.

### `etapas/<id>`
`<id>` es `funcional`, `pantallas`, `arquitectura`, `setup`, `plan` o `modelo`.
```json
{ "titulo": "1. Funcional", "version": "0.1", "estado": "Borrador",
  "ronda": 1, "actualizado": "30 sep", "md": "…" }
```
- `estado`: `Borrador` → `En revisión` → `Aprobada`. `modelo` no lleva estado.
- `md`: lo que la sección muestra. Se reescribe en cada avance de la conversación, no solo al
  pedir la revisión, y también después de Aprobada cuando el contenido cambia (una decisión
  en el funcional, un cambio en el plan). Qué lleva en cada etapa:
  - `funcional`: el funcional completo, igual a `docs/funcional.md`.
  - `arquitectura`: el documento resumen de `docs/arquitectura.md`, en lenguaje llano.
  - `setup`: la lista de comprobación con su evidencia.
  - `plan`: el documento resumen de `docs/plan-de-fases.md`.
- `actualizado`: fecha corta de la última vez que se escribió `md`. La página la muestra como "al día al…".
- `version`: sube cada vez que la etapa se pone En revisión y, ya Aprobada, con cada cambio.

Campos propios de una etapa:

- `setup.aprueba: "Desarrollo"`: la etapa la aprueba Desarrollo y no el OO; la página lo avisa.
- `setup.prompt`: texto plano con el prompt para Claude Code que revisa e instala todo lo que
  el proyecto necesita. Claude Code lo genera solo a partir de la arquitectura aprobada. La
  página lo muestra arriba de la lista, con un botón para copiarlo.
- `plan.cambios`: Markdown con el registro de cambios al plan después de aprobado: fecha, qué
  cambió y qué lo respalda (el destino que el OO dio a un pedido, o una `DEC-NN`).
- `ronda`: número de vuelta de revisión. Las marcas de la etapa se guardan por ronda; si el OO
  devuelve con observaciones, al volver a poner En revisión se suma 1 y las casillas salen limpias.

### `pantallas/<código>`
`{ "nombre": "Agenda del día", "img": "pantallas/P01.png" }` — `img` vacío muestra "sin imagen".
`img` admite dos formas:
- Una ruta (`pantallas/P01.png`): la imagen se publica junto a la página con el parámetro
  `files` de la herramienta `Artifact`.
- Un id de 32 caracteres: la imagen la pegó o la eligió una persona en la página, y está
  guardada en el artifact. Se baja al repo al aprobarse la etapa.

### `fases/<F01>`
`{ "nombre": "…", "criterio": "…", "estado": "Planificada", "aprobadaEn": "E-03", "salida": "prod-2026-10-15" }`
Estados: `Planificada` → `En construcción` → `En revisión` → `Aprobada` → `En producción`.
`aprobadaEn` se escribe al pasar a `Aprobada` (la entrega cuyo cierre la respalda) y `salida`
al pasar a `En producción`. Con esto la sección del Plan muestra las fases cumplidas.

### `entregas/<E-01>`
```json
{ "fecha": "30 sep", "estado": "En revisión", "commit": "a1b2c3d",
  "queTrae": "markdown", "verificado": "markdown",
  "pasos": [ { "id": "1", "ref": "OBS-006", "refNota": "reabierta en E-11",
               "contexto": "markdown", "hacer": "markdown", "esperar": "markdown" } ],
  "decisiones": ["DEC-07"] }
```
Estados: `En preparación` → `En revisión` → `Cerrada`. La página muestra como pestaña
la que está `En revisión`; las `Cerrada` aparecen en Historial. El `id` de un paso no cambia
una vez publicada la entrega.

### `observaciones/<OBS-001>`
```json
{ "titulo": "…", "detalle": "markdown", "tipo": "Error", "gravedad": "Normal",
  "estado": "Abierta", "entrega": "E-01", "fase": "F01",
  "capturas": ["<id de asset>"], "historial": "markdown" }
```
Tipo: `Error`, `Cambio`, `Duda`. Gravedad: `Bloquea`, `Normal`, `Menor`.
Estado: `Abierta` → `Corregida` → `Cerrada`, o `Descartada`, `Pospuesta`.

### `decisiones/<DEC-01>`
```json
{ "titulo": "…", "texto": "markdown", "quien": "Desarrollo", "estado": "A validar",
  "explicita": false, "seccion": "Reglas, punto 3", "entrega": "E-02" }
```
Estado: `A validar` → `Aceptada`, o `Pasa a observación`.
`explicita: true` para modelo de datos, plata, seguridad y permisos, y borrado de datos.

### `backlog/<id>`
`{ "pedido": "…", "prioridad": "Media", "estado": "Nuevo", "destino": "" }`
Estado: `Nuevo` → `Asignado` o `Descartado`. Los que agrega una persona desde la página
llegan con un id automático.

### `salidas/<tag>`
`{ "tag": "prod-2026-10-15", "fecha": "15 oct", "fases": "F01, F02" }`

## Marcas (solo lectura para Claude Code)

Colección `marcas/<dueño>/items`. El dueño es el id de la entrega (`E-02`) o
`etapa-<id>-r<ronda>` (`etapa-funcional-r1`).

| Documento | Contenido | Significado |
|---|---|---|
| `paso-<id>` | `{ resultado: "ok" \| "obs" \| null, texto, capturas }` | Resultado de un paso del guion |
| `dec-<DEC-NN>` | `{ voto: "apruebo" \| "no" \| null, texto }` | Validación de una decisión |
| `otra-<algo>` | `{ resultado: "obs", texto, capturas }` | Observación fuera del guion |
| `backlog-<id>` | `{ destino }` | Destino de un pedido: id de fase, `Fase nueva` o `Descartado` |
| `cierre` | `{ decision: "apruebo" \| "observaciones" \| null }` | Cierre de la entrega o de la etapa |

Todas llevan además `por` (id de quien marcó) y `cuando` (fecha ISO).
Un paso sin marca, o con `resultado: null`, es "No probado".
`texto` y `capturas` solo cuentan si `resultado` es `"obs"` (o `voto` es `"no"`): si la persona
cambia de idea, la página los conserva por si vuelve atrás, pero no son una observación.
