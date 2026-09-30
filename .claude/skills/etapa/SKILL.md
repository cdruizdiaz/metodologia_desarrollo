---
name: etapa
description: Avanza la etapa de definición en curso (1 Funcional, 2 Pantallas, 3 Arquitectura, 4 Setup, 5 Plan de fases) - la pone en revisión en el artifact o procesa su revisión. Usar durante las etapas 1 a 5.
---

# Etapa de definición

Leé "Etapa actual" en `CLAUDE.md` y el documento `etapas/<id>` del artifact
(`funcional`, `pantallas`, `arquitectura`, `setup`, `plan`). Según su estado:

## Si está en Borrador

Funcional, Arquitectura, Setup y Plan de fases se hacen **chateando con el usuario**. Nadie
escribe ese contenido en la página: ahí se lee cómo va quedando.

**En cada avance de la conversación**, sin esperar a que pidan la revisión:

1. Escribí el documento en `docs/`.
2. Escribí la sección del artifact: `etapas/<id>.md` con lo que corresponde a la etapa (ver
   abajo) y `actualizado` con la fecha de hoy.

Un avance es cualquier cosa que cambia el contenido: una regla nueva, una elección de stack,
una fase que se parte. Si algo no está definido, preguntá; no lo inventes para completar.

**Contenido por etapa**

- **1. Funcional.** Se escribe en `docs/funcional.md` y va **completo** a `etapas/funcional.md`,
  en Markdown: la página lo muestra como texto enriquecido (títulos, tablas, listas, diagramas
  `mermaid`). Tiene que decir qué hace el producto, sus reglas, la lista de pantallas con código
  y los mensajes.
- **2. Pantallas.** Es la única etapa cuyo contenido se carga en la página. Escribí un documento
  `pantallas/<código>` por cada pantalla que nombra el funcional, con `img` vacío: así se ve
  cuáles faltan. Las imágenes se cargan de dos maneras:
  - **Desde la página**: la persona toca el cuadro y pega con Ctrl+V, o elige un archivo.
    Queda en `img` un id de 32 caracteres. También puede agregar pantallas con "Agregar pantalla".
  - **Desde el repo**: PNG en `docs/pantallas/` con el código como nombre (`P01.png`).
    Republicá `artifact/proyecto.html` pasando en `files` cada imagen como
    `"pantallas/P01.png": "docs/pantallas/P01.png"` (sin pasar `capabilities`, para conservarlas)
    y escribí esa ruta en `img`.

  Antes de poner en revisión, leé `pantallas`: si alguien agregó desde la página una pantalla
  que el funcional no nombra, sumala a la lista de pantallas del funcional con el usuario.
- **3. Arquitectura.** Se escribe en `docs/arquitectura.md`: stack, estándares, cómo se
  despliega cada ambiente, prueba de humo y pruebas de lo riesgoso. Acá se elige la
  herramienta de pruebas en 5 navegadores. A `etapas/arquitectura.md` va el **documento
  resumen** (la sección "Documento resumen" del archivo), en lenguaje llano, para que el OO
  pueda aprobar sin ser técnico. Va tomando forma con la conversación: lo que todavía no se
  decidió se muestra como pendiente, no se rellena.
- **4. Setup.** Se hace con el stack aprobado en la arquitectura y se registra en `docs/setup.md`.
  La sección del artifact tiene dos partes:
  - **El prompt** (`etapas/setup.prompt`). Ya lo generaste al abrir la etapa (ver "Al aprobarse
    la Arquitectura"). Es lo que se corre para hacer el Setup: el usuario lo copia de la página
    y lo pega en Claude Code, o te pide que lo sigas en esta misma conversación.
  - **La lista de comprobación** (`etapas/setup.md`), con la evidencia de lo que quedó andando:
    repo remoto del proyecto conectado (nunca el de la plantilla) y las tres ramas subidas,
    `main` protegida, dependencias instaladas, pruebas corriendo en 5 navegadores, y una página
    mínima publicada en desarrollo, testing y producción, con prueba de humo y vuelta atrás probadas.

  Marcá un punto solo cuando lo comprobaste, con la evidencia al lado; lo que necesita una
  cuenta o un permiso del usuario (crear el repo remoto, el hosting, los secretos) pedíselo,
  no lo inventes. Crear el primer tag en `main` necesita su confirmación en el momento.
  El documento lleva `aprueba: "Desarrollo"`: esta etapa la aprueba la persona de Desarrollo,
  no el OO.
- **5. Plan de fases.** Se escribe en `docs/plan-de-fases.md`. Cada fase es comprobable y
  tiene criterio de fin. A `etapas/plan.md` va el **documento resumen**: cuántas fases son, qué
  trae cada una en una línea, en qué orden van y cuáles forman el MVP.

**Poner en revisión** (cuando el usuario dice que está listo)

1. Verificá que el documento de `docs/` y la sección del artifact digan lo mismo.
2. Actualizá el documento de la etapa: `estado: "En revisión"` y la versión.
3. Actualizá `proyecto/info.proximoPaso`: "El OO revisa <etapa> en Definición." (en el Setup, "Desarrollo revisa…").
4. Avisale al usuario que ya puede revisarla en el artifact.

## Si está En revisión

Leé las marcas de `marcas/etapa-<id>-r<ronda>/items`.

- **Sin `cierre`**: todavía no se revisó. Decilo y terminá.
- **`cierre.decision = "observaciones"`**: tomá cada marca `otra-*`, resolvela conversando con
  el usuario y corregí el contenido, en `docs/` y en el artifact. Después volvé a poner la etapa
  en revisión con `ronda` + 1 y la versión subida. No toques las marcas.
- **`cierre.decision = "apruebo"`**:
  1. Escribí `estado: "Aprobada"` y decí qué marca leíste (quién y cuándo).
  2. Llevá al repo lo que todavía no esté:
     - Pantallas → bajá a `docs/pantallas/` las imágenes cargadas desde la página (herramienta
       `Artifact`, acción `read`, con `path` igual al id que hay en `img` y `out_dir`
       `docs/pantallas`), y renombralas con el código (`P01.png`). Verificá que cada pantalla
       del funcional tenga su imagen; anotá las que falten.
     - Funcional, Arquitectura, Setup y Plan → ya están en `docs/`. Confirmá que coinciden
       con lo aprobado.
  3. Abrí la etapa siguiente: creá su documento en `Borrador` con `ronda: 1`, subí
     `proyecto/info.etapa` y actualizá "Etapa actual" en `CLAUDE.md`.
  4. **Al aprobarse la Arquitectura (etapa 3)**: generá el prompt de Setup, sin que te lo pidan.
     Sale de `docs/arquitectura.md` y es para pegar en Claude Code, así que tiene que entenderse
     solo, sin esta conversación. Tiene que decir:
     - Qué herramientas y qué versiones hacen falta, y cómo se comprueba cada una.
     - Qué instalar o configurar **solo si falta**: primero comprueba, después actúa.
     - Dependencias del proyecto, variables de entorno (los nombres, nunca los valores) y dónde
       se piden los secretos.
     - Cómo dejar las pruebas corriendo en 5 navegadores.
     - Cómo conectar el repo remoto y publicar la página mínima en los tres ambientes.
     - Que lo que necesita una cuenta o un permiso se le pide a la persona.
     - Que termine informando la lista de comprobación de `docs/setup.md`, punto por punto,
       con su evidencia.

     Guardalo en `docs/setup.md` ("Prompt para Claude Code") y en `etapas/setup.prompt`, junto
     con la lista de comprobación sin marcar en `etapas/setup.md`.
     Próximo paso: "Desarrollo corre el prompt de Setup que está en Definición → 4. Setup."
  5. **Al aprobarse el Plan (etapa 5)**: creá un documento `fases/<F01>` por fase, en
     `Planificada`; creá `etapas/modelo` si ya hay modelo de datos; creá `proyecto/probar`;
     poné `etapa: 6`. Próximo paso: construir la primera fase en `develop`.
  6. Copiá el espejo: leé `etapas` y `proyecto` con `out_dir: "docs/estado"`.
  7. Diario y commit en `develop`.

## Después de Aprobada: lo que se mantiene al día

Aprobar una etapa no congela su sección. Sin volver a ponerla en revisión:

- **Funcional.** Cambia solo por decisiones. Cada `DEC-NN` se escribe en `docs/funcional.md`
  en el mismo commit que el cambio, y en ese momento actualizás `etapas/funcional` (`md`,
  `actualizado`, versión).
- **Arquitectura y Setup.** Si una decisión cambia el stack o los ambientes, corregí
  `docs/arquitectura.md` y su resumen, y regenerá el prompt de Setup.
- **Plan de fases.** Se mantiene al día toda la etapa 6:
  - **Avance.** `/procesar-entrega` anota cada fase que queda Aprobada y `/pasar-a-produccion`
    cada fase que sale. La página arma con eso la tabla de fases cumplidas.
  - **Cambios.** Cuando el plan cambia (fase nueva, partida, quitada o cambiada de orden),
    actualizá `docs/plan-de-fases.md`, `fases` y `etapas/plan` (`md`, `actualizado`, versión), y
    sumá una línea a "Cambios al plan" en el archivo y en `etapas/plan.cambios`: fecha, qué
    cambió y qué lo respalda.
  - **Qué respalda un cambio.** El destino que el OO dio a un pedido del backlog al cerrar una
    entrega, o una `DEC-NN` en `A validar`, que se valida en la entrega siguiente. Si el cambio
    lo propone Desarrollo en la conversación, registrá la decisión antes de cambiar el plan.
    Una fase `Aprobada` o `En producción` no se cambia: lo nuevo va a otra fase.
