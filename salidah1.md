# Summary
- Ajustar la emisión y validación de tokens de `POST /auth/login` para incluir los claims mínimos, reflejar el `client` validado de la solicitud y hacer coherente `expiresIn` con la expiración del token.

# Scope
- **In**
  - Incluir `sub`, `client`, `iat` y `exp` en los tokens.
  - Obtener y validar `client` desde la solicitud.
  - Comprobar la coherencia entre `expiresIn` y `exp - now` con una tolerancia documentada.
  - Resolver el tratamiento del secreto por defecto fuera del laboratorio y evitar registrar tokens o secretos.
  - Documentar la decisión entre conservar HMAC o adoptar una librería JWT.
- **Out**
  - La HU no define explícitamente funcionalidades fuera del alcance. No se asumen exclusiones adicionales.

# Assumptions
- No se agregan supuestos funcionales. Los detalles no confirmados se registran en **Open questions**.

# Technical decisions to make
- Definir la tolerancia para comparar `expiresIn` con `exp - now` y documentarla.
- Decidir si se conserva HMAC o se adopta una librería JWT; documentar la decisión.
- Definir cómo se elimina el secreto por defecto fuera del laboratorio o cómo se documenta como exclusivo del laboratorio.
- Acordar las reglas para validar `client`; la HU no especifica cuáles son.

# Risks and mitigations
- **Exposición de tokens, secretos o valores sensibles en logs:** evitar registrarlos y adjuntar evidencia de logs redactados.
- **Inconsistencia entre `expiresIn` y `exp - now`:** documentar la tolerancia y probar la comparación.
- **Inclusión de un `client` inválido en el token:** validar el valor recibido antes de incluirlo.

# Acceptance criteria
- Todo token válido contiene los claims `sub`, `client`, `iat` y `exp`.
- El claim `client` corresponde al valor recibido y validado en la solicitud.
- `expiresIn` coincide con `exp - now` dentro de la tolerancia documentada.
- Hay pruebas para login válido, credenciales inválidas y token expirado.
- Los logs no contienen tokens, secretos ni valores sensibles.
- La decisión entre HMAC y una librería JWT queda documentada.

# Validation plan
- **Unit tests**
  - Verificar presencia de los cuatro claims y el valor de `client`.
  - Probar la coherencia de expiración dentro y fuera de la tolerancia documentada.
  - Cubrir login válido, credenciales inválidas y token expirado.
- **Functional tests**
  - Validar el flujo de `POST /auth/login` y la respuesta con `token`, `expiresIn` y `user`.
  - Comprobar el comportamiento para un `client` inválido según las reglas acordadas.
- **Security**
  - Revisar que los logs no incluyan tokens, secretos ni valores sensibles.
  - Verificar que la documentación de `AUTH_TOKEN_SECRET` no exponga su valor.
- **Required evidence**
  - Resultados de pruebas de claims, expiración y errores de autenticación.
  - Documentación de la configuración de `AUTH_TOKEN_SECRET`, sin incluir el secreto.
  - Evidencia de logs redactados y de la decisión técnica documentada.

# Suggested tasks by role
- **Chapter/PO**
  - Confirmar las reglas de validación de `client`.
  - Acordar la tolerancia de expiración junto con Tech Lead.
  - Revisar los criterios de aceptación y la evidencia esperada.
- **Tech Lead**
  - Resolver y documentar la decisión HMAC frente a librería JWT.
  - Acordar la tolerancia para `expiresIn` y `exp - now`.
  - Definir el tratamiento del secreto por defecto fuera del laboratorio.
- **Backend**
  - Implementar los claims mínimos y obtener `client` desde la solicitud.
  - Validar `client` y la coherencia del tiempo de expiración según las decisiones acordadas.
  - Evitar registrar tokens, secretos o valores sensibles y actualizar la documentación de configuración.
- **Frontend**
  - La HU no especifica cambios de frontend; confirmar si el cliente debe enviar `client` en la solicitud de login.
  - Si se confirma un cambio, actualizar el envío conforme al contrato acordado.
  - Verificar con QA la compatibilidad del flujo afectado.
- **QA**
  - Preparar pruebas para claims, expiración, login válido, credenciales inválidas y token expirado.
  - Verificar el comportamiento con valores válidos e inválidos de `client` según las reglas acordadas.
  - Adjuntar resultados de pruebas y evidencia de logs redactados.
- **Cyber**
  - Revisar logs para comprobar que no se expongan tokens, secretos ni valores sensibles.
  - Revisar la documentación de configuración para asegurar que no revele el valor del secreto.
  - Registrar los resultados de las comprobaciones de seguridad.

# Open questions
- ¿Qué formato, fuente y reglas de validación debe tener `client` en la solicitud?
- ¿Cómo se determina `sub` y qué valor debe representar?
- ¿Qué tolerancia se acepta para comparar `expiresIn` con `exp - now`?
- ¿Cómo se manejará el secreto por defecto fuera del laboratorio: eliminarlo o documentarlo como exclusivo del laboratorio?
- ¿El cliente actual ya envía `client` en la solicitud de login?
- ¿Qué roles del equipo serán responsables de cada decisión y aprobación?