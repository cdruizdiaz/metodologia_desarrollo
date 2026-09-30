---
name: etapa
description: Avanza la etapa de definición en curso (1 Funcional, 2 Pantallas, 3 Arquitectura, 4 Setup, 5 Plan de fases) - la pone en revisión en el artifact o procesa su revisión. Usar durante las etapas 1 a 5.
---

# Etapa de definición

Leé "Etapa actual" en `CLAUDE.md` y el documento `etapas/<id>` del artifact
(`funcional`, `pantallas`, `arquitectura`, `setup`, `plan`). Según su estado:

## Si está en Borrador

Trabajá el contenido con el usuario y, cuando diga que está listo, ponela en revisión.

**Contenido por etapa**

- **1. Funcional.** El texto vive en el artifact (`etapas/funcional.md`). Leelo siempre de ahí
  antes de tocarlo: alguien pudo editarlo en la página. Tiene que decir qué hace el producto,
  sus reglas, la lista de pantallas con código y los mensajes. Si algo no está definido, preguntá.
- **2. Pantallas.** Escribí un documento `pantallas/<código>` por cada pantalla que nombra el
  funcional, con `img` vacío: así se ve cuáles faltan. Las imágenes se cargan de dos maneras:
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
  herramienta de pruebas en 5 navegadores. En `etapas/arquitectura.md` va un resumen en
  lenguaje llano, para que el OO pueda aprobar sin ser técnico.
- **4. Setup.** Se hace con el stack aprobado en la arquitectura y se registra en `docs/setup.md`.
  Recorré la lista de comprobación con el usuario: conectar el repo remoto del proyecto (nunca el de la plantilla) y subir las tres
  ramas, proteger `main`, instalar dependencias, dejar las pruebas corriendo en 5 navegadores
  y publicar una página mínima en desarrollo, testing y producción, con prueba de humo y vuelta
  atrás probadas. Marcá un punto solo cuando lo comprobaste, con la evidencia al lado; lo que
  necesita una cuenta o un permiso del usuario (crear el repo remoto, el hosting, los secretos)
  pedíselo, no lo inventes. Crear el primer tag en `main` necesita su confirmación en el momento.
  En `etapas/setup.md` va la misma lista, y el documento lleva `aprueba: "Desarrollo"`:
  esta etapa la aprueba la persona de Desarrollo, no el OO.
- **5. Plan de fases.** Se escribe en `docs/plan-de-fases.md`. Cada fase es comprobable y
  tiene criterio de fin. En `etapas/plan.md` va el resumen.

**Poner en revisión**

1. Actualizá el documento de la etapa: `estado: "En revisión"` y la versión.
2. Actualizá `proyecto/info.proximoPaso`: "El OO revisa <etapa> en Definición." (en el Setup, "Desarrollo revisa…").
3. Avisale al usuario que ya puede revisarla en el artifact.

## Si está En revisión

Leé las marcas de `marcas/etapa-<id>-r<ronda>/items`.

- **Sin `cierre`**: todavía no se revisó. Decilo y terminá.
- **`cierre.decision = "observaciones"`**: tomá cada marca `otra-*`, resolvela con el usuario
  y corregí el contenido. Después volvé a poner la etapa en revisión con `ronda` + 1 y la
  versión subida. No toques las marcas.
- **`cierre.decision = "apruebo"`**:
  1. Escribí `estado: "Aprobada"` y decí qué marca leíste (quién y cuándo).
  2. Llevá el contenido al repo:
     - Funcional → copiá `etapas/funcional.md` a `docs/funcional.md`.
     - Pantallas → bajá a `docs/pantallas/` las imágenes cargadas desde la página (herramienta
       `Artifact`, acción `read`, con `path` igual al id que hay en `img` y `out_dir`
       `docs/pantallas`), y renombralas con el código (`P01.png`). Verificá que cada pantalla
       del funcional tenga su imagen; anotá las que falten.
     - Arquitectura, Setup y Plan → ya están en el repo.
  3. Abrí la etapa siguiente: creá su documento en `Borrador` con `ronda: 1`, subí
     `proyecto/info.etapa` y actualizá "Etapa actual" en `CLAUDE.md`.
  4. **Al aprobarse el Plan (etapa 5)**: creá un documento `fases/<F01>` por fase, en
     `Planificada`; creá `etapas/modelo` si ya hay modelo de datos; creá `proyecto/probar`;
     poné `etapa: 6`. Próximo paso: construir la primera fase en `develop`.
  5. Copiá el espejo: leé `etapas` y `proyecto` con `out_dir: "docs/estado"`.
  6. Diario y commit en `develop`.

Después de aprobado, el funcional deja de ser editable en la página (`editable: false`):
desde ahí cambia solo por decisiones.
