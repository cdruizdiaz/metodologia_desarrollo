# Arquitectura — (nombre del proyecto)

> Etapa 3. Se define conversando: Desarrollo con Claude Code. En cada avance Claude Code
> escribe este archivo y publica el documento resumen en el artifact, que es lo que aprueba el OO.
> Al aprobarse, de acá sale el prompt de Setup.

## Documento resumen

(Lo que se publica en el artifact, para alguien no técnico: con qué se construye, dónde corre,
qué cuesta, qué riesgos se probaron y qué queda decidido. Va tomando forma con la conversación.)

## Stack

| Parte | Elección | Por qué |
|---|---|---|
| Interfaz | | |
| Servidor | | |
| Base de datos | | |
| Hosting | | |
| Pruebas automáticas (5 navegadores) | | |

## Estándares

(Estructura de carpetas, nombres, formato, cómo se escribe una prueba.)

## Ambientes

| Ambiente | Rama | Dirección | Cómo se despliega |
|---|---|---|---|
| develop | `develop` | | |
| revisión | `revision` | | |
| producción | `main` | | |

## Prueba de humo

(Los pocos pasos que confirman que producción quedó viva después de una salida, y cómo se corren.)

## Vuelta atrás

(Cómo se vuelve producción al tag anterior.)

## Lo riesgoso, ya probado

| Riesgo | Cómo se probó | Resultado |
|---|---|---|

## Seguridad y datos

(Quién puede ver qué, dónde están los secretos, copias de respaldo.)
