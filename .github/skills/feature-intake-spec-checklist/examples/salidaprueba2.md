# Summary
- En el tercer despliegue parcial de BINes a producción (Ventana 3), habilitar las transacciones de tarjeta de débito BIN 14 en el nuevo switch transaccional Base24 para reducir operaciones truncas.

# Scope
- **Dentro del alcance**
  - Operaciones de tarjeta de débito asociadas a BIN 14 para el tercer despliegue parcial.
  - Los flujos Yape a Plin y Plin a Yape, indicados en la entrada; confirmar su relación con BIN 14 y el enrutamiento a Base24.
- **Fuera del alcance**
  - La entrada enumera “operaciones exitosas” y “disminución de operaciones truncas” como fuera del alcance. Se debe confirmar esta clasificación, ya que la reducción de truncas también se indica como impacto esperado.

# Supuestos
- No se agregan supuestos funcionales. La definición de Ventana 3 como tercer despliegue parcial de BINes fue confirmada en la solicitud.

# Decisiones técnicas por tomar (pendientes o resueltas)
- **Resuelto:** Ventana 3 corresponde al tercer despliegue parcial de BINes a producción.
- **Resuelto:** las transacciones de tarjeta de débito BIN 14 deben ejecutarse bajo el nuevo switch Base24.
- **Resuelto:** el switch debe responder a Visa en menos de 10 segundos.
- **Pendiente:** definir y validar la composición del tercer despliegue parcial y confirmar que BIN 14 corresponde a esta etapa.
- **Pendiente:** definir si Yape a Plin y Plin a Yape se ejecutan bajo Base24 y su relación con BIN 14.
- **Pendiente:** acordar la fecha de producción con Visa.
- **Pendiente:** definir línea base, período y meta cuantitativa para medir la reducción de operaciones truncas.

# Riesgos y mitigaciones
- **Respuesta del switch en 10 segundos o más:** Visa puede generar una operación trunca. Medir los tiempos de respuesta y verificar en pruebas el límite de menos de 10 segundos.
- **Exposición de datos sensibles en logs:** revisar los registros del flujo y comprobar que no incluyan datos sensibles.
- **Errores en la validación del monto:** acordar las reglas aplicables y probar casos válidos e inválidos antes del despliegue.
- **Despliegue parcial fuera de la fecha acordada con Visa o con BINes incorrectos:** confirmar calendario y lista de BINes antes del pase a producción, y adjuntar evidencia de aprobación.

# Criterios de aceptación (medibles)
- La Ventana 3 queda identificada en el plan de despliegue como el tercer despliegue parcial de BINes, y su lista aprobada incluye BIN 14.
- Las transacciones de débito de BIN 14 incluidas en la etapa se enrutan por Base24.
- El tiempo de respuesta del switch a Visa es menor de 10 segundos por operación probada.
- El pase a producción ocurre en una fecha coordinada con Visa y se conserva evidencia de la coordinación.
- Se ejecutan y registran pruebas para cada flujo confirmado dentro del alcance; la reducción de operaciones truncas se mide contra una línea base y período que el equipo debe acordar.

# Plan de validación
- **Pruebas unitarias**
  - Verificar el enrutamiento de las transacciones BIN 14 hacia Base24.
  - Cubrir los casos válidos e inválidos según las reglas de validación de monto acordadas.
  - Probar el manejo del tiempo de respuesta en torno al límite de menos de 10 segundos.
- **Pruebas funcionales**
  - Verificar la ejecución en Base24 para transacciones de débito BIN 14.
  - Probar Yape a Plin y Plin a Yape conforme al enrutamiento que se acuerde.
  - Medir y registrar los tiempos de respuesta a Visa y verificar que sean menores de 10 segundos.
- **Seguridad (comprobaciones mínimas)**
  - Inspeccionar los logs para comprobar que no se expongan datos sensibles.
  - Revisar el manejo de errores de validación y verificar que no registren datos sensibles.
- **Evidencia requerida (qué adjuntar)**
  - Resultados de pruebas unitarias y funcionales, incluidos tiempos de respuesta medidos.
  - Plan o registro del tercer despliegue parcial con la lista aprobada de BINes.
  - Evidencia de la fecha coordinada con Visa, documentación breve actualizada y PR revisado.
  - Línea base, período y resultado de la medición de operaciones truncas, una vez acordados.
  - Referencias versionadas a `SKILL.md`, `examples/input.sample.md`, `examples/output.sample.md` y al tag de versión del Skill.

# Tareas sugeridas por rol
- **Chapter/PO**
  - Confirmar que BIN 14 corresponde al tercer despliegue parcial y validar el alcance funcional.
  - Aclarar si “operaciones exitosas” y “disminución de operaciones truncas” son exclusiones o resultados esperados.
  - Acordar la métrica, línea base, período y meta de reducción de operaciones truncas.
- **Tech Lead**
  - Validar la secuencia de despliegues parciales y la composición de la Ventana 3.
  - Definir la matriz de enrutamiento para BIN 14 y los flujos Yape/Plin confirmados.
  - Acordar con los equipos responsables la validación del límite de respuesta y la coordinación con Visa.
- **Backend**
  - Implementar o verificar el enrutamiento de BIN 14 hacia Base24.
  - Implementar o verificar las reglas acordadas de validación de monto y manejo de errores.
  - Revisar el logging del flujo y comprobar que no exponga datos sensibles.
- **Frontend**
  - Confirmar con PO y Tech Lead si el tercer despliegue requiere cambios en el frontend; la entrada no los especifica.
  - Si se confirma trabajo de interfaz, documentar los flujos y comportamientos afectados.
  - Validar con QA cualquier cambio de interfaz acordado antes de incluirlo en la entrega.
- **QA**
  - Preparar pruebas de enrutamiento para BIN 14 y los flujos Yape/Plin cuyo alcance se confirme.
  - Medir y registrar el tiempo de respuesta por operación, verificando el límite de menos de 10 segundos.
  - Adjuntar resultados de pruebas y comprobar la evidencia del despliegue parcial y de la coordinación con Visa.
- **Cyber**
  - Revisar los logs del flujo para detectar exposición de datos sensibles.
  - Revisar el tratamiento de los errores de validación de monto desde la perspectiva de exposición de datos.
  - Documentar las comprobaciones de seguridad y sus resultados para la revisión del PR.

# Preguntas abiertas
- ¿La lista del tercer despliegue parcial incluye exclusivamente BIN 14 o también otros BINes?
- ¿Yape a Plin y Plin a Yape están asociados a BIN 14 y deben enrutarse por Base24?
- ¿“Operaciones exitosas” y “disminución de operaciones truncas” son exclusiones de alcance o resultados esperados?
- ¿Cuál es la línea base, el período y la meta cuantitativa para evaluar la reducción de operaciones truncas?
- ¿Cuáles son las reglas exactas de validación del monto?
- ¿Qué evidencia de coordinación con Visa se requiere para autorizar el pase a producción?