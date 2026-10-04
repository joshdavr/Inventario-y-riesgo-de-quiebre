# ADR-001 — Monolito modular con el pronóstico como servicio separado

| | |
|---|---|
| **Estado** | Aceptada |
| **Tipo** | arquitectónica |
| **Fecha** | 2026-10-04 |
| **Ejercicio** | Ejercicio 1 — Inventario y riesgo de quiebre |

## Contexto

Un equipo pequeño, un dominio cohesivo y un componente (pronóstico) con ritmo de cambio y consumo de cómputo distintos que además puede fallar.

## Decisión

Un backend monolítico modular (Inventario, Recomendaciones, Aprobación, Auditoría, Adaptador de compras) y el pronóstico como contenedor independiente consumido por REST con timeout y circuit breaker.

## Alternativas consideradas

- Microservicios completos (rechazado: complejidad operativa y consistencia distribuida sin necesidad)
- Monolito con pronóstico embebido (rechazado: un fallo o pico de cómputo afectaría la consulta de existencias).

## Consecuencias

- (+) despliegue simple, transacciones locales, pronóstico reemplazable.
- (−) hay que hacer cumplir los límites de módulo con pruebas; el pronóstico exige manejo explícito de fallos.
