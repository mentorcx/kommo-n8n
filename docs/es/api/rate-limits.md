# Límites y restricciones

> Estado: plantilla inicial. Verifica los valores con la documentación oficial vigente.

## Límite de velocidad (rate limit)

- Aproximadamente **7 solicitudes por segundo** por cuenta.
- Superarlo devuelve `HTTP 429 Too Many Requests`.

### Cómo manejar `429`

- Lee `Retry-After` si está presente.
- Aplica **backoff exponencial** con jitter (p. ej. 1s, 2s, 4s, 8s).
- Encola operaciones masivas en lugar de paralelizar agresivamente.

## Tamaño de lote en operaciones masivas

Muchos endpoints aceptan crear/actualizar múltiples entidades en una sola solicitud:

- Hasta **250 elementos por petición** en la mayoría de colecciones.
- Confirma el límite específico en la ficha de cada endpoint.

## Webhooks

- Kommo reintenta si tu endpoint no responde con `2xx`.
- Tu endpoint debe responder en menos de unos segundos; procesa de forma asíncrona si es necesario.

## Otras restricciones frecuentes

- Campos personalizados: tipos y valores válidos varían por entidad.
- Filtros: no todos los campos son filtrables; revisa la documentación de cada endpoint.
- Permisos del usuario/integración: una `403` suele indicar que el token no tiene acceso al recurso.

## Errores comunes

| Código | Causa típica                                  | Acción sugerida                                 |
|--------|-----------------------------------------------|------------------------------------------------|
| 400    | JSON inválido o parámetros mal formados        | Validar payload contra la ficha del endpoint    |
| 401    | Token expirado o inválido                      | Refrescar token y reintentar una vez            |
| 403    | Falta de permisos                              | Revisar scopes y rol del usuario                |
| 404    | Recurso inexistente                            | Verificar IDs y subdominio                      |
| 429    | Rate limit                                     | Backoff exponencial                             |
| 5xx    | Error del servidor                             | Reintentar con backoff; reportar si persiste    |
