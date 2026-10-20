# Summary
- En la Ventana 3, enrutar las transacciones de tarjeta de débito BIN 14 por el nuevo switch transaccional Base24, con el objetivo de reducir las transacciones truncas.

# Scope
- **Dentro del alcance**
  - Operaciones de tarjeta de débito indicadas para BIN 14.
  - Flujos Yape a Plin y Plin a Yape, sujetos a confirmar su relación con el nuevo switch.
- **Fuera del alcance**
  - Según la entrada: operaciones exitosas y disminución de operaciones truncas. Se debe confirmar si se refiere a exclusiones del alcance o a resultados esperados.

# Supuestos
- No se declaran supuestos adicionales. El significado de “Ventana 3” y el alcance de los flujos Yape/Plin están pendientes de confirmación.

# Decisiones técnicas por tomar (pendientes o resueltas)
- **Resuelto:** las transacciones de tarjeta de débito BIN 14 deben ejecutarse bajo el nuevo switch transaccional Base24.
- **Resuelto:** el switch debe responder a Visa en menos de 10 segundos.
- **Pendiente:** definir la fecha de despliegue coordinada con Visa.
- **Pendiente:** confirmar si los flujos Yape a Plin y Plin a Yape deben pasar por Base24.
- **Pendiente:** precisar qué significa “Ventana 3” y qué operaciones se consideran dentro de ella.
- **Pendiente:** acordar la métrica, línea base y período para evaluar la reducción de transacciones truncas.

# Riesgos y mitigaciones
- **Respuesta del switch en 10 segundos o más:** Visa puede generar una operación trunca. Medir el tiempo de respuesta y verificar el límite de menos de 10 segundos en las pruebas.
- **Exposición de datos sensibles en logs:** revisar los registros del flujo y comprobar que no incluyan datos sensibles.
- **Errores en la validación del monto:** definir las reglas aplicables y cubrir casos válidos e inválidos con pruebas.
- **Despliegue fuera de una fecha coordinada con Visa:** tratar la coordinación de la fecha como requisito de salida a producción y conservar evidencia de la coordinación.

# Criterios de aceptación (medibles)
- Las transacciones de tarjeta de débito identificadas como BIN 14 se enrutan por Base24.
- El tiempo de respuesta del switch a Visa es menor de 10 segundos por operación.
- El despliegue a producción se realiza únicamente en una fecha coordinada con Visa.
- Se ejecutan y documentan pruebas para cada flujo confirmado dentro del alcance: tarjeta de débito BIN 14, Yape a Plin y Plin a Yape.
- La reducción de transacciones truncas se compara con una línea base durante el período que acuerde el equipo; el objetivo cuantitativo queda pendiente.

# Plan de validación
- **Pruebas unitarias**
  - Verificar el criterio de enrutamiento para transacciones BIN 14.
  - Cubrir las reglas acordadas de validación del monto, incluidos casos válidos e inválidos.
  - Probar los límites de manejo del tiempo de respuesta definidos para el switch.
- **Pruebas funcionales**
  - Comprobar que una transacción de débito BIN 14 utiliza Base24.
  - Probar los flujos Yape a Plin y Plin a Yape una vez confirmado su enrutamiento esperado.
  - Medir y registrar el tiempo de respuesta a Visa para verificar que sea menor de 10 segundos.
- **Seguridad (comprobaciones mínimas)**
  - Inspeccionar los logs para verificar que no expongan datos sensibles.
  - Validar que los errores de monto se manejen conforme a las reglas acordadas y no generen registros con datos sensibles.
- **Evidencia requerida (qué adjuntar)**
  - Resultados de pruebas unitarias y funcionales, incluidos los tiempos de respuesta medidos.
  - Documentación breve actualizada y evidencia de revisión del PR.
  - Confirmación de la fecha coordinada con Visa.
  - Línea base, período y resultado de la medición de transacciones truncas, una vez acordados.
  - Referencias versionadas a `SKILL.md`, `examples/input.sample.md`, `examples/output.sample.md` y al tag de versión del Skill.

# Tareas sugeridas por rol
- **Chapter/PO**
  - Aclarar el objetivo de “Ventana 3” y qué significa el apartado “Out”.
  - Confirmar si los flujos Yape a Plin y Plin a Yape forman parte del enrutamiento a Base24.
  - Definir el indicador, la línea base, el período y la meta de reducción de operaciones truncas.
- **Tech Lead**
  - Definir la matriz de enrutamiento para BIN 14 y los flujos incluidos.
  - Acordar el plan de pruebas del límite de respuesta de menos de 10 segundos.
  - Confirmar la coordinación técnica y la fecha de despliegue con Visa.
- **Backend**
  - Implementar o verificar el enrutamiento de las transacciones BIN 14 hacia Base24.
  - Implementar o verificar las validaciones de monto acordadas.
  - Revisar el logging y comprobar que no registre datos sensibles.
- **Frontend**
  - Confirmar si hay cambios en el frontend dentro del alcance; la entrada no describe cambios de interfaz.
  - Si se confirma trabajo de interfaz, documentar los flujos afectados con PO y Tech Lead.
  - Validar con QA cualquier cambio de interfaz aprobado antes de incluirlo en la entrega.
- **QA**
  - Preparar pruebas de enrutamiento para BIN 14 y los flujos cuyo alcance se confirme.
  - Medir el tiempo de respuesta por operación y verificar el límite de menos de 10 segundos.
  - Adjuntar resultados de pruebas y evidencias de revisión según la DoD.
- **Cyber**
  - Revisar los logs del flujo para detectar exposición de datos sensibles.
  - Revisar el manejo de errores de monto desde la perspectiva de exposición de datos.
  - Documentar las comprobaciones de seguridad y sus resultados.

# Preguntas abiertas
- ¿Qué significa exactamente “Ventana 3” y cuál es la fecha coordinada con Visa?
- ¿Los flujos Yape a Plin y Plin a Yape también deben ejecutarse bajo Base24?
- En el apartado “Out”, ¿operaciones exitosas y disminución de operaciones truncas son exclusiones o resultados esperados?
- ¿Cuál es la línea base, el período y la meta cuantitativa para medir la reducción de transacciones truncas?
- ¿Qué reglas exactas se aplican a la validación del monto?
- ¿Qué evidencia de coordinación con Visa se requiere para autorizar el despliegue a producción?