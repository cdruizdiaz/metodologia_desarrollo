---
name: urgencia
description: Arregla algo roto en producción sin tocar la entrega abierta - rama hotfix desde main, test primero, aprobación del OO, tag y prueba de humo. Usar solo cuando producción está rota.
---

# Urgencia en producción

Es el único camino a producción que no pasa por una entrega. Usalo solo si producción está
rota; todo lo demás va por el circuito normal.

1. **Entendé el problema** con el usuario: qué se ve, desde cuándo, a quién afecta.

2. **Registrá** una OBS nueva de tipo Error que Bloquea, con `entrega: "urgencia"`.

3. **Rama** `hotfix/<nombre-corto>` desde `main`.

4. **Test primero**: una prueba `@OBS-NNN` que falla mostrando el problema. Después el arreglo,
   lo más chico posible. Corré todas las pruebas.

5. **Mostrale al OO** qué estaba mal, qué cambiaste y qué pruebas pasan. Pedí aprobación
   explícita. Sin un sí, no sigas. Anotá en el `historial` de la OBS quién aprobó y cuándo.

6. **Merge a `main`**, tag `prod-AAAA-MM-DD` (o `-2`, `-3`…), despliegue y prueba de humo
   según `docs/arquitectura.md`. Si el humo falla, volvé al tag anterior y avisá.

7. **Llevá el arreglo a `develop`** (merge de `main`). No toques `revision` si hay una
   entrega abierta: el arreglo llega a revisión con la entrega siguiente.

8. **Registrá**: OBS → `Corregida` (la cierra QA en la próxima entrega, como paso del guion);
   `salidas/<tag>`; `proyecto/info`; diario y commit.

Si el arreglo cambia cómo funciona algo, es además una `DEC-NN` y va a `docs/funcional.md`
en el mismo commit.
