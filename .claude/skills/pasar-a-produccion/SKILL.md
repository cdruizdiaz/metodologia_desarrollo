---
name: pasar-a-produccion
description: Lleva a producción las fases Aprobadas - verifica, pide confirmación, hace merge a main con tag y corre la prueba de humo. Solo cuando el usuario lo pide.
disable-model-invocation: true
---

# Pasar a producción

Nunca se corre por iniciativa propia. Solo cuando el usuario lo pide.

## Verificá, y detenete si algo no se cumple

- No hay entrega `En revisión`.
- Todas las fases que contiene `revision` están `Aprobada` o `En producción`. Si alguna sigue
  `En revisión`, no se puede salir: explicá cuál y por qué.
- Las pruebas automáticas pasan.

## Pasos

1. **Mostrá qué va a salir**: fases, decisiones aceptadas que incluye y el commit.
   Pedí confirmación explícita ("¿Paso esto a producción?"). Sin un sí, no sigas.

2. **Anotá el tag actual de producción**: es el punto de vuelta atrás.

3. **Merge de `revision` a `main`** y tag `prod-AAAA-MM-DD` (si ya existe, `-2`, `-3`…). Push con tags.

4. **Desplegá** y corré la **prueba de humo** definidas en `docs/arquitectura.md`.

5. **Si la prueba de humo falla**:
   - Volvé producción al tag anterior.
   - Abrí una OBS de tipo Error que Bloquea, con lo que falló.
   - Avisale al usuario. Las fases siguen `Aprobada`, no `En producción`.

6. **Si pasa**:
   - Fases → `En producción`.
   - `salidas/<tag>` con fecha y fases.
   - `proyecto/info.ambientes.produccion`.
   - Espejo (`fases`, `salidas`, `proyecto`, `observaciones` con `out_dir: "docs/estado"`), diario y commit en `develop`.
