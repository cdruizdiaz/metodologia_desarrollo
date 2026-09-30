---
name: estado
description: Resume dónde está el proyecto en el Ciclo de desarrollo v3 - etapa, entrega abierta, observaciones, decisiones pendientes y próximo paso. Usar al empezar cada sesión o cuando el usuario pregunta en qué estamos.
---

# Estado del proyecto

Solo lee. No cambia nada.

1. Leé el bloque "Proyecto" de `CLAUDE.md`. Si no hay artifact, decí que falta `/iniciar-proyecto` y terminá.
2. Con `ArtifactData` leé: `proyecto/info`, `etapas`, `fases`, `entregas`, `observaciones`,
   `decisiones` y `backlog`.
3. Si hay una etapa o una entrega `En revisión`, leé sus marcas (`marcas/<dueño>/items`,
   ver `artifact/datos.md`) para saber si ya hay cierre.
4. Mirá la rama actual y si hay cambios sin commitear.
5. Respondé corto, en este orden:
   - **Dónde estamos**: etapa, y fase en curso si es la etapa 6.
   - **Qué espera a una persona**: etapa o entrega en revisión sin cierre, decisiones a validar, pedidos sin destino.
   - **Qué espera a Desarrollo**: observaciones abiertas (primero las que Bloquean), cierre ya marcado sin procesar.
   - **Próximo paso**: uno solo, con el comando que corresponde.

Si el repo y el artifact no coinciden (por ejemplo, `CLAUDE.md` dice etapa 2 y el artifact 3,
o un documento de `docs/` dice algo distinto de su sección en Definición), decilo como primera línea.
