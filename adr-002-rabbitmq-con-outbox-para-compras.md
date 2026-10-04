# ADR-002 — RabbitMQ con patrón outbox para integrar con compras

| | |
|---|---|
| **Estado** | Aceptada |
| **Tipo** | tecnológica |
| **Fecha** | 2026-10-04 |
| **Ejercicio** | Ejercicio 1 — Inventario y riesgo de quiebre |

## Contexto

La ejecución en compras no debe bloquear la aprobación ni perderse o duplicarse si compras falla.

## Decisión

Al aprobar, se guarda el evento en una tabla *outbox* en la misma transacción; un worker lo publica en RabbitMQ y otro consumidor llama al sistema de compras con una clave de idempotencia.

## Alternativas consideradas

- Llamada síncrona a compras al aprobar (rechazada: acopla disponibilidad)
- Kafka (rechazada: sobredimensionada para este volumen, sin necesidad de reproducción de historial)
- Redis (no se adopta, no hay necesidad demostrada).

## Consecuencias

- (+) entrega al menos una vez sin duplicar efectos, desacople temporal.
- (−) un componente más que operar y consistencia eventual en el estado `EJECUTADA`.
