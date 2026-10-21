# Tasks

## 1. Caracterizar el contrato actual

- [ ] 1.1 Confirmar con `AuthResourceTest` la respuesta exitosa actual (`token`, `expiresIn`, `user`, `debug`), sus campos de usuario y la precedencia de validaciones; ejecutar `mvn -B -ntp -Dtest=AuthResourceTest test` antes de cambiar producción.
- [ ] 1.2 Confirmar con `TokenServiceTest` RS256, claims existentes, vida exacta de 3600 segundos y rechazo en `exp`; ejecutar `mvn -B -ntp -Dtest=TokenServiceTest test` antes de cambiar producción.
- [ ] 1.3 Confirmar con `EndpointContractTest`, `EndpointFailureTest` y `AuthResourceTest` las respuestas 400/401/500 y la redacción de logs ya cubiertas; ejecutar las pruebas focalizadas y registrar cualquier hueco antes del cambio.

## 2. Añadir pruebas para el comportamiento acordado

- [ ] 2.1 Añadir a `TokenServiceTest` casos con reloj fijo para `client`, claims requeridos, claims existentes, RS256 y límites de expiración; verificar que las nuevas aserciones describen la spec y fallan solo donde falta el comportamiento nuevo.
- [ ] 2.2 Añadir a `EndpointContractTest` casos de `client` omitido, nulo, vacío y solo espacios con HTTP 400 y sin token; reemplazar la expectativa permisiva existente y verificar con `mvn -B -ntp -Dtest=EndpointContractTest test`.
- [ ] 2.3 Ampliar la prueba de login exitoso para comprobar que el token firmado contiene el `client` enviado y que el cuerpo conserva exactamente su forma pública; verificar con `mvn -B -ntp -Dtest=AuthResourceTest test`.
- [ ] 2.4 Añadir o completar una prueba de logs para login exitoso y fallido con valores centinela de password, token y secreto; verificar que no aparecen en registros y que el error HTTP 500 sigue siendo genérico.

## 3. Implementar el cambio incremental

- [ ] 3.1 Validar `client` en `AuthResource` después de las validaciones existentes de request, username y password; comprobar 400 para cada valor ausente/vacío y que las validaciones previas mantienen precedencia.
- [ ] 3.2 Propagar el `client` validado por `AuthService` hasta `TokenService` e incluirlo como claim sin alterar la respuesta; verificar con `TokenServiceTest` y `AuthResourceTest` que el claim coincide y la firma sigue siendo RS256.
- [ ] 3.3 Mantener una sola referencia temporal para `iat` y `exp`, vida de 3600 segundos y `expiresIn` coherente con tolerancia maxima de un segundo; verificar los límites con el reloj fijo de `TokenServiceTest`.
- [ ] 3.4 Preservar el manejo genérico de errores y evitar registrar password, token, secreto o clave privada; verificar con `EndpointFailureTest` y la prueba de logs centinela.

## 4. Verificación de integración

- [ ] 4.1 Ejecutar `mvn -B -ntp test` y confirmar que pasan la suite completa y los escenarios de contrato de `auth-token`.
- [ ] 4.2 Ejecutar `openspec validate "hu-01-token-claims-expiration" --type change --json` y confirmar que el cambio OpenSpec queda válido.