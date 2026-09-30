# Plantilla — Ciclo de desarrollo v3

Repo plantilla para empezar un proyecto nuevo con el Ciclo de desarrollo v3:
seis etapas, entregas que se prueban con casillas, y un único artifact de seguimiento
por proyecto. Se usa con [Claude Code](https://claude.com/claude-code).

La metodología, ilustrada: `docs/metodologia.html`. Las reglas completas: `docs/ciclo-convenciones.md`.

## Arrancar un proyecto

1. **Traé la plantilla a una carpeta nueva.** Cualquiera de las dos formas sirve:
   - Clonarla directo:
     ```
     git clone https://github.com/cdruizdiaz/metodologia_desarrollo.git mi-proyecto
     ```
     `/iniciar-proyecto` la desconecta de la plantilla y arranca el historial de cero. El repo
     remoto del proyecto se crea y se conecta más adelante, en el Setup.
   - En GitHub, "Use this template" para crear el repo del proyecto, y clonar ese.
2. **Verificá lo mínimo** para que los comandos funcionen:
   - `git --version` responde. Si no, instalá git desde https://git-scm.com.
   - `claude --version` responde. Si no, instalá Claude Code desde https://claude.com/claude-code.
   - Claude Code tiene la sesión iniciada con la cuenta de claude.ai donde van a vivir los
     artifacts del proyecto.

   `/iniciar-proyecto` vuelve a comprobarlo y se detiene si algo falta.
3. **Abrí Claude Code** en la carpeta y corré:
   ```
   /iniciar-proyecto
   ```
   Te pregunta el nombre y quién cumple cada rol, crea el artifact del proyecto y guarda su
   enlace en `CLAUDE.md`.
4. **Escribí el funcional** en el artifact (Definición → 1. Funcional), solo o conversando con
   Claude Code. Cuando esté listo para revisar: `/etapa`.

## Las seis etapas

1. Funcional · 2. Pantallas · 3. Arquitectura · 4. Setup · 5. Plan de fases · 6. Construcción

En el Setup (etapa 4) se conecta el repo remoto, se instalan las dependencias y se dejan
andando los tres ambientes: desarrollo (`develop`), testing (`revision`) y producción (`main`).
Ahí también se protege la rama `main` (en GitHub: Settings → Branches → Add rule), que es el
refuerzo de la regla "solo fases Aprobadas llegan a producción".

## Comandos

| Comando | Qué hace | Cuándo |
|---|---|---|
| `/iniciar-proyecto` | Crea el artifact y deja el repo listo para la etapa 1 | Una vez, después de clonar |
| `/estado` | Resume etapa, entrega abierta, observaciones y próximo paso | Al empezar cada sesión |
| `/etapa` | Pone en revisión la etapa en curso (1 a 5) o procesa su revisión | Durante la definición |
| `/pasar-a-testing` | Publica en revisión y arma la pestaña `E-NN` | Cuando `develop` está listo para probar |
| `/procesar-entrega` | Lee las casillas, numera las observaciones y actualiza estados | Después de que el OO cierra |
| `/pasar-a-produccion` | Merge a `main`, tag y prueba de humo, con tu confirmación | Con fases Aprobadas |
| `/urgencia` | Arreglo desde `main` sin tocar la entrega abierta | Producción rota |

## Qué hay en el repo

```
CLAUDE.md                    Lo que Claude Code lee al empezar
.claude/skills/              Los comandos del ciclo
.claude/settings.json        Pide confirmación antes de push, merge, tag o tocar main
artifact/proyecto.html       La página del artifact de seguimiento
artifact/datos.md            Cómo se guardan los datos del artifact
docs/ciclo-convenciones.md   Las reglas completas
docs/metodologia.html        La metodología ilustrada
docs/guia-comandos.html      Cuándo y cómo se usa cada comando
docs/funcional.md            Etapa 1 (plantilla)
docs/pantallas/              Etapa 2: un PNG por pantalla
docs/arquitectura.md         Etapa 3 (plantilla)
docs/setup.md                Etapa 4 (lista de comprobación)
docs/plan-de-fases.md        Etapa 5 (plantilla)
docs/modelo-de-datos.md      Diagrama entidad-relación
docs/estado/                 Espejo del artifact, se actualiza al cerrar cada entrega
docs/avances/                Diario: un archivo por día
tests/                       Pruebas automáticas (la herramienta se elige en la etapa 3)
```

## El artifact del proyecto

- Es uno solo por proyecto y acompaña todo el ciclo. Cada entrega es una pestaña nueva.
- Nace privado. Para que otra persona marque casillas, compartilo desde el menú Share como
  **Contributor**. Quien entra como Viewer solo lee.
- Las capturas adjuntas hacen que el artifact no se pueda compartir por enlace público.
  Si necesitás enlace público, quitá `assets` de las `capabilities` en `/iniciar-proyecto`.
- Las casillas son de las personas. Claude Code las lee, pero nunca las marca.
