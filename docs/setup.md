# Setup — (nombre del proyecto)

> Etapa 4. La hace Desarrollo con Claude Code, con el stack que quedó aprobado en la
> arquitectura. **La aprueba Desarrollo**, no el OO. Sin esto no se puede empezar a construir.

## Criterio de fin

Una página mínima recorre los tres ambientes: desarrollo → testing → producción.

## Lista de comprobación

Cada punto se marca cuando se comprobó, con la evidencia al lado (un comando, una dirección, un commit).

| Hecho | Qué | Evidencia |
|---|---|---|
| ☐ | El repo local está conectado al repo remoto, con las ramas `main`, `develop` y `revision` subidas | |
| ☐ | La rama `main` está protegida en el remoto | |
| ☐ | Las dependencias están instaladas y el proyecto arranca en local | |
| ☐ | Las pruebas automáticas corren en los 5 navegadores, con al menos una prueba | |
| ☐ | Desarrollo (`develop`) publica la página mínima | |
| ☐ | Testing (`revision`) publica la página mínima | |
| ☐ | Producción (`main`) publica la página mínima, con su primer tag | |
| ☐ | La prueba de humo de producción está escrita y pasa | |
| ☐ | La vuelta atrás al tag anterior se probó una vez | |

## Cómo se levanta el entorno desde cero

(Los pasos para que otra persona clone el repo y lo tenga andando: requisitos, comandos,
variables de entorno y dónde se piden los secretos. Los secretos no se escriben acá.)

## Lo que cambió respecto de la arquitectura

(Si al armar el entorno hubo que cambiar algo de lo aprobado en la etapa 3, se anota acá y
se corrige `docs/arquitectura.md`. Si el cambio es grande, la arquitectura vuelve a revisión.)
