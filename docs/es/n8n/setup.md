# Configuración inicial de n8n con Kommo

> Estado: plantilla inicial. Añade capturas y casos reales conforme la wiki crezca.

## Requisitos

- Una instancia de n8n (cloud o self-hosted).
- Una cuenta de Kommo con permisos para crear integraciones.
- Tokens generados (OAuth o larga duración). Ver [`../api/authentication.md`](../api/authentication.md).

## Opción A — Nodo HTTP Request genérico

Como no existe un nodo nativo oficial de Kommo en n8n, lo más habitual es usar el **HTTP Request**.

### Credencial recomendada: Header Auth

1. En n8n: **Credentials → New → Header Auth**.
2. Nombre del header: `Authorization`
3. Valor: `Bearer <tu_token>`

### Ejemplo: listar leads

| Campo               | Valor                                            |
|---------------------|--------------------------------------------------|
| Method              | GET                                              |
| URL                 | `https://{subdominio}.kommo.com/api/v4/leads`    |
| Authentication      | Header Auth (la credencial creada arriba)        |
| Send Query Params   | sí, `limit=50`, `with=contacts`                  |

## Opción B — OAuth2 en n8n

1. **Credentials → New → OAuth2 API**.
2. Grant Type: `Authorization Code`.
3. Authorization URL: `https://www.kommo.com/oauth?client_id=<id>&mode=post_message`
4. Access Token URL: `https://{subdominio}.kommo.com/oauth2/access_token`
5. Client ID / Secret: los de la integración.
6. Scope: según los permisos solicitados.

> n8n se encarga de refrescar el token. Verifica que la URL de redirección de la integración en Kommo coincida con la que muestra n8n.

## Webhooks entrantes

1. Crea un nodo **Webhook** en n8n y copia la URL pública.
2. En Kommo: **Configuración → Integraciones → Webhooks** y registra esa URL para los eventos deseados.
3. En n8n, procesa el payload (suele venir como `application/x-www-form-urlencoded` con estructura anidada).

## Buenas prácticas

- Usa variables/credentials para `subdominio` y tokens; no los hardcodees en nodos.
- Activa **Retry On Fail** con backoff para mitigar `429`.
- Para operaciones masivas, agrupa en lotes con el nodo **Split In Batches**.
