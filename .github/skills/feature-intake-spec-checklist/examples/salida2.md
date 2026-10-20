# Summary
- Incorporar una confirmación previa al envío de transferencias, con un resumen y validación de datos, para reducir las quejas por transferencias duplicadas en 20% durante un mes.

# Scope
- **Dentro del alcance**
  - Confirmación antes de enviar una transferencia.
  - Mostrar un resumen previo al envío.
  - Validar datos de la transferencia.
- **Fuera del alcance**
  - Cambios de infraestructura.
  - Rediseño completo de la interfaz.

# Supuestos
- No se declaran supuestos adicionales. Los campos del resumen y las reglas de validación deben definirse antes de implementar esos comportamientos.

# Decisiones técnicas por tomar (pendientes o resueltas)
- **Resuelto:** usar la API REST y el frontend web existentes, y mantener el registro estandarizado.
- **Pendiente:** definir los datos que aparecerán en el resumen de confirmación.
- **Pendiente:** acordar reglas de validación, especialmente para el monto, y sus mensajes de error.
- **Pendiente:** confirmar el comportamiento de los servicios de cuentas y transferencias ante solicitudes inválidas o duplicadas.

# Riesgos y mitigaciones
- **Exposición de datos sensibles en logs:** revisar los logs del flujo y comprobar que no incluyan datos sensibles.
- **Errores al validar el monto:** acordar las reglas antes de implementar y cubrir casos válidos e inválidos con pruebas.
- **Dependencias de los servicios de cuentas y transferencias:** validar la integración funcional con ambos servicios y documentar los resultados.

# Criterios de aceptación (medibles)
- Antes de cada envío, el frontend muestra la confirmación y el resumen de la transferencia.
- No se envía una solicitud de transferencia hasta que la persona usuaria confirma.
- Para cada caso inválido definido en las reglas acordadas, el envío queda bloqueado y se informa el error.
- En el mes posterior al lanzamiento, las quejas por transferencias duplicadas disminuyen al menos 20% respecto de la línea base acordada.

# Plan de validación
- **Pruebas unitarias**
  - Verificar que la solicitud no se envíe antes de la confirmación explícita.
  - Cubrir los casos válidos e inválidos definidos para la validación de datos y monto.
  - Comprobar el comportamiento de presentación del resumen.
- **Pruebas funcionales**
  - Verificar el flujo desde la carga de datos hasta la confirmación y el envío.
  - Comprobar la integración con los servicios de cuentas y transferencias.
  - Confirmar que los casos inválidos bloquean el envío y presentan el error acordado.
- **Seguridad (comprobaciones mínimas)**
  - Revisar que los logs del flujo no contengan datos sensibles.
  - Comprobar que los datos inválidos no se envíen al servicio de transferencias.
- **Evidencia requerida (qué adjuntar)**
  - Salida de las pruebas unitarias y funcionales.
  - Documentación breve actualizada y evidencia de revisión del PR.
  - Línea base, fuente de medición y resultado de quejas por duplicados al completar un mes.
  - Referencias versionadas a `SKILL.md`, `examples/input.sample.md`, `examples/output.sample.md` y al tag de versión del Skill.

# Tareas sugeridas por rol
- **Chapter/PO**
  - Definir la fuente de datos y la línea base para medir quejas por duplicados.
  - Confirmar qué información necesita la persona usuaria en el resumen.
  - Revisar los criterios de aceptación y evaluar la métrica al completar un mes.
- **Tech Lead**
  - Acordar las reglas de validación con los equipos responsables.
  - Confirmar los contratos y comportamientos esperados de los servicios dependientes.
  - Revisar el diseño técnico y la propuesta de logging del flujo.
- **Backend**
  - Verificar los contratos existentes de los servicios de cuentas y transferencias.
  - Implementar o confirmar las validaciones acordadas en el límite correspondiente.
  - Revisar que los logs no expongan datos sensibles.
- **Frontend**
  - Implementar la confirmación previa al envío en la interfaz existente.
  - Mostrar el resumen con los datos que se acuerden.
  - Bloquear el envío y presentar errores para los casos inválidos acordados.
- **QA**
  - Preparar casos de prueba para confirmación, resumen y validación.
  - Ejecutar pruebas funcionales de integración con los servicios dependientes.
  - Adjuntar resultados de pruebas y comprobar la evidencia requerida.
- **Cyber**
  - Revisar los logs generados por el flujo para detectar exposición de datos sensibles.
  - Comprobar que los casos inválidos no se envíen al servicio de transferencias.
  - Documentar los resultados de las comprobaciones de seguridad.

# Preguntas abiertas
- ¿Qué datos debe contener el resumen previo al envío?
- ¿Cuáles son las reglas exactas para validar los datos y el monto?
- ¿Cuál será la fuente de medición y la línea base para verificar la reducción del 20%?
- ¿Qué comportamiento ofrecen actualmente los servicios de cuentas y transferencias para solicitudes inválidas o duplicadas?