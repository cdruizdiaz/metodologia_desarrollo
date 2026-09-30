---
name: iniciar-proyecto
description: Arranca un proyecto nuevo con el Ciclo de desarrollo v3 - crea el artifact de seguimiento, carga la etapa 1 (Funcional) y deja el repo listo. Se usa una sola vez, después de clonar la plantilla.
---

# Iniciar proyecto

Se corre una sola vez. Si `CLAUDE.md` ya tiene un enlace en "Artifact", detenete y avisá
que el proyecto ya está iniciado.

## Pasos

1. **Preguntá** (todo junto, en un solo mensaje):
   - Nombre del proyecto (dos a cuatro palabras).
   - Una o dos frases sobre qué es.
   - Quién es OO, Analista, QA y Desarrollo. Puede ser la misma persona.

2. **Repo.** El proyecto nuevo no puede quedar atado al repo plantilla.
   - Mirá `git remote get-url origin`. Si apunta a la plantilla
     (`cdruizdiaz/metodologia_desarrollo`, o el repo del que se clonó y que no es el del
     proyecto), la carpeta vino de un `git clone` directo: trae el historial de la plantilla
     y un `git push` iría contra ella. Avisale al usuario y, con su confirmación, borrá la
     carpeta `.git` y empezá de cero con `git init -b main`. No queda ningún remoto: el del
     proyecto se conecta en la etapa 4 (Setup).
   - Si `origin` ya es el repo del proyecto (creado con "Use this template"), dejalo como está.
   - Si no es un repositorio git, `git init -b main`.
   - Si no sabés si el remoto es la plantilla o el proyecto, preguntá.

   Después creá las ramas `develop` y `revision` si no existen y quedate en `develop`.

3. **Página.** En `artifact/proyecto.html` cambiá solo el `<title>` por el nombre del proyecto.

4. **Publicá** con la herramienta `Artifact`:
   - `file_path`: `artifact/proyecto.html`
   - `icon`: `kanban`
   - `description`: una frase con el nombre y qué es
   - `capabilities`: `{"db": {}, "user": {}, "assets": {}}`

   Guardá la URL que devuelve.

5. **Cargá los datos iniciales** con `ArtifactData`, acción `batch` (formas en `artifact/datos.md`):
   - `proyecto/info`: nombre, `etapa: 1`, roles, ambientes vacíos y
     `proximoPaso: "El Analista escribe el funcional en Definición → 1. Funcional."`
   - `etapas/funcional`: `titulo: "1. Funcional"`, `version: "0.1"`, `estado: "Borrador"`,
     `ronda: 1`, `editable: true`, y en `md` el contenido de `docs/funcional.md`
     con el nombre y la descripción ya puestos.

6. **Completá el bloque "Proyecto"** de `CLAUDE.md`: nombre, URL del artifact, `Etapa actual: 1`, roles.

7. **Diario.** Creá `docs/avances/avances_AAAA-MM-DD.txt` con la fecha de hoy y una línea: proyecto iniciado.

8. **Commit** en `develop`: "Inicia el proyecto <nombre>".

9. **Contale al usuario**:
   - El enlace del artifact. Es privado: para que otros lo usen hay que compartirlo desde
     el menú Share, como Contributor para que puedan marcar casillas.
   - Próximo paso: escribir el funcional, en el artifact con "Editar el texto" o conversando
     con vos. Cuando esté listo para revisar, `/etapa`.
   - El repo remoto, la protección de `main` y las dependencias se resuelven en la etapa 4 (Setup),
     cuando ya esté elegido el stack.
