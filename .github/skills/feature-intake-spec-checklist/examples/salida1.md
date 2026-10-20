# Summary
- Permitir que las personas usuarias revisen un resumen y confirmen una transferencia antes de enviarla.
- Resultado esperado: reducir en 20% las quejas por transferencias duplicadas durante un mes.

# Scope
- **Dentro del alcance**
  - Confirmación previa al envío.
  - Mostrar un resumen antes de confirmar.
  - Validar los datos de la transferencia.
- **Fuera del alcance**
  - Cambios de infraestructura.
  - Rediseño completo de la interfaz.

# Supuestos
- La confirmación ocurre en el frontend web antes de enviar la solicitud a la API REST.
- Las validaciones se basarán en reglas acordadas por el equipo; esas reglas aún no están especificadas.

# Decisiones técnicas por tomar (pendientes o resueltas)
- **Pendiente:** definir qué datos contiene el resumen.
- **Pendiente:** acordar las reglas de validación y los mensajes para datos inválidos.
- **Pendiente:** confirmar si el servicio de transferencias ofrece mecanismos para prevenir duplicados además de la confirmación en pantalla.
- **Resuelto:** se utilizarán la API REST existente y el registro estandarizado.

# Riesgos y mitigaciones
- **Exposición de datos sensibles en logs:** revisar los registros generados por el flujo y evitar incluir datos sensibles.
- **Errores de validación del monto:** acordar reglas con los equipos responsables y cubrir casos válidos e inválidos con pruebas.
- **Dependencia de los servicios de cuentas y transferencias:** verificar su integración en las pruebas funcionales y documentar fallos observados.

# Criterios de aceptación (medibles)
- Antes de enviar una transferencia, se muestra una confirmación con un resumen.
- La solicitud de transferencia no se envía antes de que la persona usuaria confirme.
- Los datos que incumplen las reglas acordadas de validación no se envían al servicio de transferencias.
- Al cabo de un mes, la métrica acordada muestra una reducción del 20% en las quejas por transferencias duplicadas respecto de la línea base acordada.

# Plan de validación
- **Pruebas unitarias**
  - Verificar que la confirmación controla si se envía la solicitud.
  - Probar datos válidos e inválidos según las reglas acordadas.
  - Verificar que los datos sensibles no se incluyan en los logs del flujo.
- **Pruebas funcionales**
  - Completar el flujo desde la revisión del resumen hasta la confirmación y el envío.
  - Comprobar que los servicios de cuentas y transferencias participan según el flujo existente.
  - Comprobar que los datos inválidos impiden completar el envío.
- **Seguridad (comprobaciones mínimas)**
  - Inspeccionar los logs del flujo para detectar exposición de datos sensibles.
  - Verificar que la confirmación no omita las validaciones acordadas.
- **Evidencia requerida (qué adjuntar)**
  - Salida de las pruebas unitarias y funcionales.
  - Documentación breve actualizada y PR revisado.
  - Línea base y resultado de la métrica de quejas al completar el período de un mes.
  - `SKILL.md`, `examples/input.sample.md`, `examples/output.sample.md` y tag `skill-feature-intake-v0.1.0`.

# Tareas sugeridas por rol
- **Chapter/PO**
  - Confirmar la línea base y el método de medición de quejas por duplicados.
  - Definir el resultado esperado del resumen junto con el equipo.
  - Revisar los criterios de aceptación y el resultado de la métrica al mes.
- **Tech Lead**
  - Acordar las reglas de validación con los equipos responsables.
  - Confirmar el contrato de integración con los servicios de cuentas y transferencias.
  - Revisar el diseño técnico y el PR.
- **Backend**
  - Verificar el contrato actual del servicio de transferencias y sus validaciones.
  - Acordar cómo se gestionan los datos inválidos en la integración.
  - Revisar que el logging del flujo no exponga datos sensibles.
- **Frontend**
  - Implementar la confirmación previa y el resumen.
  - Aplicar las reglas de validación acordadas antes del envío.
  - Añadir pruebas unitarias del flujo de confirmación y validación.
- **QA**
  - Preparar casos funcionales para confirmación, envío y datos inválidos.
  - Validar la integración con los servicios dependientes.
  - Adjuntar resultados de pruebas y verificar la evidencia requerida.
- **Cyber**
  - Revisar los logs del flujo en busca de datos sensibles.
  - Revisar que la confirmación y validación no puedan omitirse en el flujo normal.
  - Registrar hallazgos y comprobaciones de seguridad en la evidencia de entrega.

# Preguntas abiertas
- ¿Qué campos debe mostrar el resumen?
- ¿Cuáles son las reglas exactas para validar los datos y el monto?
- ¿Cuál es la línea base, la fuente de datos y la persona responsable de medir la reducción de quejas?
- ¿El servicio de transferencias ya cuenta con alguna protección contra duplicados?