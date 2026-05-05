# Autenticación

> Estado: plantilla inicial. Completa con capturas y ejemplos reales.

Kommo soporta dos métodos:

## 1. OAuth 2.0 (recomendado para integraciones públicas)

Flujo de autorización por código:

1. El usuario instala la integración desde el marketplace o un enlace de instalación.
2. Kommo redirige a tu `redirect_uri` con un `code` de un solo uso.
3. Intercambias el `code` por un `access_token` (vida corta) y un `refresh_token`.
4. Refrescas el token cuando expira usando el `refresh_token`.

### Intercambio de código por token

```http
POST /oauth2/access_token
Host: {subdominio}.kommo.com
Content-Type: application/json

{
  "client_id": "...",
  "client_secret": "...",
  "grant_type": "authorization_code",
  "code": "...",
  "redirect_uri": "https://tu-app/callback"
}
```

### Refresco

```http
POST /oauth2/access_token
Host: {subdominio}.kommo.com
Content-Type: application/json

{
  "client_id": "...",
  "client_secret": "...",
  "grant_type": "refresh_token",
  "refresh_token": "...",
  "redirect_uri": "https://tu-app/callback"
}
```

## 2. Token de larga duración

Útil para integraciones internas. Se genera desde el panel de la integración en la cuenta de Kommo y se envía como:

```
Authorization: Bearer <token>
```

## Encabezado en cada solicitud

```
Authorization: Bearer <access_token>
Content-Type: application/json
```

## Buenas prácticas

- Almacena `client_secret` y `refresh_token` cifrados.
- Refresca proactivamente unos minutos antes de la expiración.
- Maneja `401` reintentando con un token nuevo una sola vez.
