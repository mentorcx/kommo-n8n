# Patrones de integración

Recetas reutilizables al integrar Kommo con n8n.

## 1. Polling con cursor

Cuando no puedes (o no quieres) usar webhooks. Programa un Cron y guarda la última fecha consultada.

- Nodo Cron → HTTP Request con `filter[updated_at][from]={ultima_fecha}` → procesar → guardar nueva marca.

## 2. Webhook + cola interna

Para protegerte de picos:

- Webhook recibe → responde `200` rápido → encola en una base/Redis → otro workflow consume y llama a la API.

## 3. Lote con Split In Batches

Para crear/actualizar muchas entidades sin saturar:

- Lista de items → **Split In Batches** (tamaño 100–250) → HTTP Request POST/PATCH → **Wait** breve si es necesario.

## 4. Refresco de token (OAuth manual)

Si gestionas tokens manualmente:

- Antes de cada llamada, comprobar `expires_at` en almacenamiento.
- Si quedan menos de N segundos, llamar a `/oauth2/access_token` con `refresh_token` y guardar los nuevos.

## 5. Mapeo de campos personalizados

Los `field_id` cambian entre cuentas. Centraliza un nodo **Set** con un mapa `nombre → field_id` cargado al inicio del flujo (idealmente desde un endpoint que los liste y se cachee).

## 6. Manejo de errores

- En cada HTTP Request, marca **Continue On Fail**.
- Tras el nodo, divide con **IF**: éxito vs. error.
- Si `429`, ramifica a un **Wait** + reintento; en otros errores, registra en una hoja/Slack.

## 7. Idempotencia

Para evitar duplicados en reintentos, busca por un campo único (email, teléfono, custom field externo) antes de crear:

- Buscar → si existe, PATCH; si no, POST.
