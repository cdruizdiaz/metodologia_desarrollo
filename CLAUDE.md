# Proyecto

<!-- /iniciar-proyecto completa este bloque. No editarlo a mano. -->
- Nombre: (sin iniciar)
- Artifact: (sin crear; correr `/iniciar-proyecto`)
- Etapa actual: 0
- Roles: OO — · Analista — · QA — · Desarrollo —

# Cómo se trabaja acá

Este repo sigue el **Ciclo de desarrollo v3**. Las reglas completas están en
`docs/ciclo-convenciones.md`; leelas antes de cualquier tarea que no sea trivial.
La forma de los datos del artifact está en `artifact/datos.md`.

Al empezar una sesión, corré `/estado` para saber dónde está el proyecto.

## Tu papel

Preparás, registrás y respondés. **Nunca aprobás ni cerrás.**

1. Nunca escribas bajo `marcas/` en la base del artifact. Esas casillas son de las personas.
2. Los estados `Aprobada`, `Cerrada` y `Aceptada` solo se escriben cuando existe la marca
   de una persona que los respalda. Al escribirlos, decí qué marca leíste.
3. Nunca toques `main` fuera de `/pasar-a-produccion` o `/urgencia`, y siempre con
   confirmación explícita del usuario en el momento.
4. Una entrega `Cerrada` no se modifica. Lo pendiente va a la siguiente.
5. Mientras hay una entrega `En revisión`, la rama `revision` no se toca.
6. Ante una Duda, no inventes: preguntá.

## Dónde está cada cosa

| Qué | Dónde |
|---|---|
| Seguimiento para las personas | El artifact del proyecto (enlace arriba) |
| Funcional, arquitectura, setup, plan, modelo de datos | `docs/` |
| Pantallas (un PNG por pantalla: `P01.png`) | `docs/pantallas/` |
| Espejo del artifact | `docs/estado/` |
| Diario, un archivo por día | `docs/avances/avances_AAAA-MM-DD.txt` |
| Pruebas automáticas | `tests/` |
| Página del artifact y forma de sus datos | `artifact/` |

## Ramas

- `develop`: trabajo diario.
- `revision`: lo que se está probando. Congelada mientras hay una entrega abierta.
- `main`: producción. Solo fases Aprobadas, siempre con tag.
- `hotfix/<nombre>`: urgencias, desde `main`.

## Al trabajar

- Error significa test primero: una prueba que falla, etiquetada `@OBS-NNN`, y después el arreglo.
- Toda decisión (`DEC-NN`) se escribe en `docs/funcional.md` en el mismo commit que el cambio.
- Un pedido nuevo no entra en la fase en curso: va al backlog.
- Al terminar cada sesión, agregá unas líneas al diario del día: qué se hizo y qué quedó pendiente.
- Escribí para las personas en lenguaje llano. Los números (`OBS`, `DEC`) son referencia, no explicación.

## Comandos

`/iniciar-proyecto` · `/estado` · `/etapa` · `/pasar-a-testing` · `/procesar-entrega` ·
`/pasar-a-produccion` · `/urgencia`
