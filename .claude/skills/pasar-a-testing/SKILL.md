---
name: pasar-a-testing
description: Pasa de desarrollo a testing (develop a revision) - publica en revisión lo que hay en develop y arma la pestaña E-NN del artifact con sus siete partes. Usar en la etapa 6, cuando develop está listo para que QA pruebe.
---

# Pasar a testing

## Antes de empezar, verificá

- No hay otra entrega `En revisión`. Si la hay, detenete: una sola abierta a la vez.
- Las pruebas automáticas pasan en `develop`, en los 5 navegadores. Si alguna falla, detenete y mostrala.
- No hay cambios sin commitear.

## Pasos

1. **Número.** El siguiente `E-NN` según `entregas`.

2. **Qué entra.** Juntá:
   - Fases construidas desde la entrega anterior.
   - Observaciones corregidas (cada una tiene su prueba `@OBS-NNN` pasando, si era un Error).
   - Pasos que quedaron "No probado" en la entrega anterior.
   - Decisiones en `A validar`.

3. **Llevá `develop` a `revision`** (merge) y desplegá el ambiente de revisión como indica
   `docs/arquitectura.md`. Anotá el commit.

4. **Escribí `entregas/<E-NN>`** con `estado: "En revisión"` (forma en `artifact/datos.md`):
   - `queTrae`: fases, observaciones corregidas y decisiones nuevas, en lenguaje llano.
   - `verificado`: cuántas pruebas pasaron, y qué no se puede verificar desde acá.
   - `pasos`: solo lo que la máquina no puede probar. Cada paso se entiende solo:
     - `ref` y `refNota`: de dónde viene ("OBS-006", "reabierta en E-11").
     - `contexto`: **Lo que viste** y **Qué cambió**, o que es la primera vez que se prueba.
     - `hacer`: dónde arrancar y qué tocar, con los nombres que se ven en pantalla.
     - `esperar`: el resultado, sin ambigüedad.
   - `decisiones`: los ids a validar.

5. **Actualizá estados**: fases incluidas → `En revisión`; observaciones incluidas → `Corregida`;
   `proyecto/info` (ambientes y `proximoPaso: "QA prueba E-NN y el OO la cierra."`).

6. **Releé el guion como si fueras QA.** Si un paso obliga a ir a otra pestaña para entenderlo, reescribilo.

7. Diario y commit en `develop`. Avisale al usuario que la entrega está lista para probar.

Desde ahora y hasta `/procesar-entrega`, `revision` no se toca. El trabajo sigue en `develop`.
