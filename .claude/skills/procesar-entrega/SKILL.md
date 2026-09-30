---
name: procesar-entrega
description: Procesa una entrega que el OO ya cerró en el artifact - lee las casillas, numera las observaciones, actualiza estados de fases, decisiones y backlog, y copia el espejo al repo. Usar después de que el OO marca el cierre.
---

# Procesar una entrega

Leé la entrega `En revisión` y sus marcas en `marcas/<E-NN>/items` (ver `artifact/datos.md`).

**Si no hay marca `cierre` con decisión, detenete.** Cerrar es del OO: decile al usuario qué
falta y no avances.

Las marcas no se tocan: se leen.

## 1. Pasos del guion

| Marca | Qué hacer |
|---|---|
| `resultado: "ok"` y el paso viene de una OBS | Esa OBS pasa a `Cerrada` (QA la verificó) |
| `resultado: "ok"` en un paso nuevo | Nada: el paso está bien |
| `resultado: "obs"` y el paso viene de una OBS | Esa OBS vuelve a `Abierta`, mismo número; sumá lo nuevo a su `historial` |
| `resultado: "obs"` en un paso nuevo | OBS nueva |
| Sin marca | "No probado": anotalo para la entrega siguiente |

## 2. Otras observaciones

Cada marca `otra-*` con texto es una OBS nueva.

## 3. Observaciones nuevas

Para cada una: siguiente `OBS-NNN`, título en una línea, el texto de la persona tal cual en
`detalle`, las `capturas`, la entrega y la fase. Clasificá:

- **Tipo**: Error (algo no anda como dice el funcional), Cambio (anda como dice, pero se pide
  otra cosa), Duda (no se entiende qué se pide).
- **Gravedad**: Bloquea, Normal o Menor.

Si el tipo no es claro, o es una Duda, **preguntale al usuario** antes de seguir. No inventes.
Un Cambio genera además una `DEC-NN` en `A validar`.

## 4. Decisiones

| Marca `dec-<id>` | Resultado |
|---|---|
| `voto: "no"` | `Pasa a observación` + OBS nueva de tipo Cambio con el texto |
| `voto: "apruebo"` | `Aceptada` |
| Sin voto, decisión común | `Aceptada` (por silencio) |
| Sin voto, `explicita: true` | Sigue `A validar`; vuelve en la entrega siguiente |

## 5. Backlog

Cada marca `backlog-<id>` con destino: escribí el destino en el pedido y pasalo a `Asignado`
(o `Descartado`). Si el destino es "Fase nueva", agregala a `docs/plan-de-fases.md` y a `fases`.
A los pedidos que llegaron con id automático, dales número `B-NN`.

## 6. Fases

Una fase de la entrega pasa a `Aprobada` si no le quedan observaciones `Abierta` ni pasos
"No probado". Si no, sigue `En revisión`.

## 7. Cierre

1. `entregas/<E-NN>.estado = "Cerrada"`. Decí qué marca leíste: quién cerró, cuándo y cómo.
2. `proyecto/info`: ambientes y próximo paso.
3. **Espejo**: leé `proyecto`, `etapas`, `fases`, `entregas`, `observaciones`, `decisiones`,
   `backlog`, `salidas` y `marcas/<E-NN>/items` con `out_dir: "docs/estado"`.
4. Diario y commit en `develop`.

## 8. Contale al usuario

Corto y en este orden:

- Cómo cerró la entrega.
- Observaciones nuevas y reabiertas, con número y una línea cada una.
- Fases que quedaron Aprobadas.
- Qué sigue: corregir (primero lo que Bloquea), construir la fase siguiente del plan, o
  —si hay fases Aprobadas— que puede pedir `/pasar-a-produccion` cuando quiera.
