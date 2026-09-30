# Ciclo de desarrollo v3 — convenciones

La versión ilustrada está en `docs/metodologia.html`. Este archivo es la referencia completa.

## Roles

- **OO · Analista · QA**: decide, define, prueba y aprueba.
- **Desarrollo**: arquitectura, programación y pruebas automáticas.
- **Claude Code**: prepara, registra y responde. Nunca aprueba ni cierra.

Una misma persona puede tener varios roles; en ese caso hace una sola pasada.

## Las seis etapas

Cada etapa arranca cuando se aprueba la anterior. Las aprueba el OO, salvo el Setup, que lo aprueba Desarrollo.

| Etapa | Qué produce | Quién | Dónde se escribe |
|---|---|---|---|
| 1. Funcional | Qué hace: reglas, pantallas y mensajes | Analista + Claude Code | `docs/funcional.md`; en el artifact, el funcional completo |
| 2. Pantallas | Una imagen por pantalla, con código (`P01`, `C3`…) | Analista u OO | Se pegan o se eligen en el artifact; aprobada la etapa, se copian a `docs/pantallas/` |
| 3. Arquitectura | Stack, estándares y pruebas de lo riesgoso | Desarrollo + Claude Code | `docs/arquitectura.md`; en el artifact, el documento resumen en lenguaje llano |
| 4. Setup | El entorno andando: repo remoto, dependencias, pruebas y los tres ambientes | Desarrollo + Claude Code | `docs/setup.md`; en el artifact, el prompt para Claude Code y la lista de comprobación |
| 5. Plan de fases | Fases comprobables, cada una con criterio de fin | Desarrollo + Claude Code | `docs/plan-de-fases.md`; en el artifact, el documento resumen, el avance y los cambios |
| 6. Construcción | El producto, fase por fase, en entregas | Todos | El código, y una pestaña por entrega |

Las etapas 1 a 5 se hacen una vez. La 6 se repite entrega por entrega.

### Las etapas que se hacen conversando

Funcional, Arquitectura, Setup y Plan de fases se trabajan **chateando con Claude Code**. El
contenido no se escribe en el artifact: ahí se lee cómo va quedando, se observa y se aprueba.
(Las Pantallas son la excepción: las imágenes se pegan o se eligen en la página.)

- **El artifact va al día.** En cada avance de la conversación Claude Code escribe el documento
  en `docs/` y la sección del artifact, sin esperar a que se pida la revisión. La sección dice
  hasta qué fecha está al día.
- **Funcional**: en el artifact va completo, como texto enriquecido. Después de aprobado cambia
  solo por decisiones, y cada decisión lo actualiza en el repo y en el artifact.
- **Arquitectura**: en el artifact va el documento resumen, para que el OO apruebe sin ser técnico.
- **Setup**: Claude Code genera solo, a partir de la arquitectura aprobada, el **prompt** que
  revisa e instala todo lo necesario. Queda en la sección, listo para copiar, y sirve también
  para cada máquina nueva. Debajo va la lista de comprobación con la evidencia de lo que quedó
  andando; con eso aprueba Desarrollo. Si la arquitectura cambia, el prompt se regenera.
- **Plan de fases**: en el artifact va el documento resumen. No se congela al aprobarse (ver
  "El plan, después de aprobado").

El Setup termina cuando una página mínima recorre los tres ambientes: desarrollo (`develop`)
→ testing (`revision`) → producción (`main`). Sin eso no se puede empezar a construir.

### El plan, después de aprobado

- **Avance.** Cada fase cumplida queda anotada: en qué entrega se aprobó y en qué salida fue a
  producción. La sección del Plan lo muestra siempre al día.
- **Cambios.** Una fase nueva, partida, quitada o cambiada de orden se escribe en el plan en el
  momento, con una línea en "Cambios al plan": fecha, qué cambió y qué lo respalda.
- **Qué respalda un cambio.** El destino que el OO le dio a un pedido del backlog al cerrar una
  entrega, o una decisión (`DEC-NN`) que se valida en la entrega siguiente. El plan no vuelve a
  revisión: no se reaprueba la etapa entera.

## Fase y entrega

- Una **fase** es qué se construye. Sale del plan.
- Una **entrega** (`E-NN`) es lo que hay para probar ahora. Puede tocar una o varias fases.
- Hay **una sola entrega abierta a la vez**. Mientras está abierta, `revision` no se toca.
- Una entrega **cerrada no se reabre**. Lo pendiente pasa a la siguiente.

## El circuito de una entrega

1. En `develop` se construye la fase o se corrigen observaciones. Las pruebas automáticas
   corren en 5 navegadores. Para un Error: primero la prueba que falla (`@OBS-NNN`), después el arreglo.
2. `/pasar-a-testing` lleva `develop` a `revision` y arma la pestaña `E-NN`.
3. QA prueba solo lo que la máquina no puede, marcando **Anda bien** u **Observo**. El OO cierra.
4. `/procesar-entrega` lee las casillas. Desde ahí hay tres caminos:
   - Lo observado vuelve a `develop` con su número y reaparece en la entrega siguiente.
   - Lo aprobado libera la fase siguiente del plan.
   - Las fases Aprobadas pueden pasar a producción cuando el OO lo decide.

### Las siete partes de la pestaña de entrega

1. Cómo usar esta pestaña.
2. Qué trae: fases, observaciones corregidas, decisiones nuevas y commit.
3. Ya verificado automáticamente, y qué no se puede verificar desde acá.
4. Guion para probar.
5. Decisiones de desarrollo a validar.
6. Otras observaciones.
7. Cierre de la entrega, con el destino de los pedidos nuevos del backlog.

### Cómo se escribe un paso del guion

Cada paso se entiende solo, sin ir a otra pestaña:

- **Contexto**: qué se vio antes y qué cambió. Si viene de una observación, su número y su historia en una línea.
- **Qué hacer**: dónde arrancar y qué tocar, con los nombres que la persona ve en pantalla.
- **Qué esperar**: el resultado, descrito para que no haya dudas.

El guion trae solo lo que la máquina no puede probar: el teléfono real, la cuenta real, lo que hay que mirar con ojos.

### Al cerrar

- **Apruebo esta entrega** o **Cierro con observaciones**. Una sola casilla vale como resultado de QA y decisión del OO.
- Un paso sin marcar queda **No probado** y vuelve en la entrega siguiente.
- Una fase pasa a Aprobada cuando no le quedan observaciones abiertas ni pasos No probados.
- Si una observación que Bloquea impide seguir probando, se cierra con observaciones en ese momento.
- En la misma pasada, el OO da destino a los pedidos nuevos del backlog.

## Observación, comentario y decisión

| | Observación | Comentario | Decisión |
|---|---|---|---|
| Qué es | Algo que hay que arreglar o responder | Una aclaración | Un cambio en cómo se ve o funciona |
| Cómo nace | Marcando Observo | Comentando sobre el artifact | De una observación de tipo Cambio, de un pedido o de Desarrollo |
| Número | `OBS-NNN`, lo pone Claude Code | No lleva | `DEC-NN` |
| Cómo termina | QA marca Anda bien al verificarla | — | Aceptada al cerrar la entrega, o pasa a observación |

- **Tipo**: Error (test primero), Cambio (se registra una DEC), Duda (se pregunta, no se inventa).
- **Gravedad**: Bloquea, Normal, Menor.
- Una observación que sigue fallando vuelve a Abierta **con el mismo número**.

### Decisiones

- Toda decisión va a `docs/funcional.md` en el mismo commit que el cambio.
- Las de Desarrollo se validan en la entrega siguiente, con la casilla "No estoy de acuerdo".
  Sin marcar al cierre, quedan Aceptadas.
- **Aprobación explícita**: las que cuestan caro de revertir (modelo de datos, plata, seguridad
  y permisos, borrado de datos) necesitan la casilla "Apruebo". Sin marcar, siguen A validar
  y vuelven en la entrega siguiente. La marca la pone Desarrollo al registrar la decisión.
- Un desacuerdo convierte la decisión en "Pasa a observación" y abre una OBS de tipo Cambio.

## Estados

En **negrita**, los que solo da una persona.

- **Etapa 1 a 5**: Borrador → En revisión → **Aprobada**
- **Fase**: Planificada → En construcción → En revisión → **Aprobada** → En producción
- **Entrega**: En preparación → En revisión → **Cerrada**
- **Observación**: Abierta → Corregida → **Cerrada** · o Descartada, Pospuesta
- **Decisión**: A validar → **Aceptada** · o Pasa a observación
- **Pedido del backlog**: Nuevo → Asignado · o Descartado

## Producción

- Solo salen fases Aprobadas, y solo cuando el usuario lo confirma en el momento.
- Cada salida lleva un tag `prod-AAAA-MM-DD` (si hay dos el mismo día, `-2`, `-3`…).
- Después del merge corre la prueba de humo definida en `docs/arquitectura.md`.
- Si la prueba de humo falla: se vuelve al tag anterior y se abre una OBS que Bloquea.
- En el MVP se juntan varias fases antes de salir. En mantenimiento, una entrega aprobada puede salir el mismo día.

## Urgencia

El único camino a producción que no pasa por una entrega.

1. Rama `hotfix/<nombre>` desde `main`.
2. Test primero, después el arreglo.
3. Se registra como OBS de tipo Error que Bloquea.
4. El OO aprueba; recién entonces se hace merge a `main`, con tag y prueba de humo.
5. El arreglo se lleva a `develop`. La entrega abierta no se toca.

## Backlog

Un pedido nuevo no entra en la fase en curso. Va al backlog, y al cerrar cada entrega el OO
le da destino: una fase existente, una fase nueva o descartado.

## El artifact y el repo

- El artifact es uno solo por proyecto, desde el primer día. Es la cara para las personas.
- El repo guarda lo técnico y un espejo del artifact en `docs/estado/`, que se actualiza al
  cerrar cada entrega y cada etapa.
- Los documentos de `docs/` y las secciones de Definición dicen lo mismo: se escriben juntos.
- Pestañas: Inicio · entrega abierta · Definición · Observaciones · Decisiones · Backlog · Cómo probar · Historial.

## Reglas que no se negocian

1. **Solo personas aprueban.**
2. **Una entrega abierta a la vez.**
3. **Una entrega cerrada no se reabre.**
4. **Cada paso se entiende solo.**
5. **El guion trae solo lo que la máquina no puede probar.**
6. **Toda decisión va al funcional en el mismo commit.**
7. **Error significa test primero.**
8. **Toda salida a producción lleva tag.**
9. **Sin burocracia.** Las excepciones se anotan en el diario en vez de prohibirse.
