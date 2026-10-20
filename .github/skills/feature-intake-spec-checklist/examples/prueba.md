## Business context (brief)
Ventana 3 . ejecucion de bin 14 en nuevo switch

## Feature request / user story
Como usuario quiero que las transacciones de tarjeta de debido bin 14 se ejecuten bajo el nuevo switch transaccional de tarjetas base 24

## Expected impact
reducir las transacciones truncas de switch actual

## Scope (in/out)
In: operaciones de tarjeta de debido, yape a plin, plin a yape
Out: operaciones exitosas, disminucion de operaciones truncas

## Known technical constraints
los desplieges a produccion solo se pueden hacer en fechas cordinadas con visa
el switch debe responder operaciones en menos de 10 segundos a visa o visa dara error y generara una operacion trunca

## Known risks
Sensitive data exposure in logs; amount-validation errors.

## Dependencies
Accounts service and transfers service.

## Team Definition of Done (DoD)
Unit tests, brief documentation, and reviewed PR.