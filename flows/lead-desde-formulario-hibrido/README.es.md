# Lead desde formulario web (híbrido)

[English](README.en.md)

## Qué hace

Recibe un POST de un formulario web con `nombre`, `apellido`, `whatsapp`, `email`, crea un lead en Kommo y lo asocia al contacto correspondiente. Si el contacto no existe, lo crea; si ya existe, lo vincula y refresca sus datos.

Usa el patrón **híbrido**: una llamada a `POST /leads/complex` (que se encarga de no duplicar contactos) seguida de un `PATCH /contacts/:id` para mantener el contacto al día con los datos del formulario.

## Diagrama

```
Webhook → Normalizar → POST /leads/complex → PATCH /contacts/:id → Respond
```

## Nodos

1. **Webhook formulario** — Recibe el POST en `/webhook/kommo-form-lead`.
2. **Normalizar** (Code) — `email` a minúsculas/trim; `whatsapp` a E.164 (`+` + dígitos).
3. **POST /leads/complex** — Crea el lead. Si el email/teléfono ya existe en Kommo, vincula al contacto existente; si no, lo crea. Devuelve `[{ id, contact_id }]`.
4. **PATCH /contacts/:id** — Actualiza nombre, apellido, email y teléfono del contacto (existente o recién creado) con los datos del formulario.
5. **Respond to Webhook** — Devuelve `200` con `lead_id` y `contact_id`.

## Requisitos

### Credenciales en n8n

Crea una credencial **Header Auth** llamada `Kommo API`:

- Nombre del header: `Authorization`
- Valor: `Bearer <tu_token_de_larga_duracion>`

> Si prefieres OAuth2, sustituye los nodos HTTP a `OAuth2 API` y ajusta el campo `authentication`.

### Variables de entorno

- `KOMMO_SUBDOMAIN` — el subdominio de tu cuenta (sin `.kommo.com`). Ejemplo: `miempresa`.

### Permisos del token

- Lectura/escritura de **Contactos**.
- Lectura/escritura de **Leads**.

### Payload esperado del formulario

```json
{
  "nombre": "Ana",
  "apellido": "Pérez",
  "whatsapp": "+54 9 11 5555 1234",
  "email": "ana@correo.com"
}
```

## Cómo importarlo

1. **Workflows → Import from File** y selecciona [`workflow.json`](workflow.json).
2. En cada nodo HTTP Request, asigna la credencial `Kommo API` (al importar, el `id` queda como `REPLACE_WITH_CREDENTIAL_ID`).
3. Define la variable de entorno `KOMMO_SUBDOMAIN` en la instancia de n8n, o reemplaza la expresión por un valor literal en los nodos HTTP.
4. Activa el workflow y copia la URL pública del nodo Webhook.
5. Apunta el formulario web a esa URL.

## Comportamiento de `/leads/complex` con contacto existente

- ✅ No duplica: si email/teléfono coinciden, vincula el lead al contacto existente.
- ✅ Devuelve siempre el `contact_id` (existente o nuevo) para que el PATCH siguiente funcione igual en ambos casos.
- ❌ No actualiza datos del contacto existente. Por eso el PATCH posterior es necesario si quieres reflejar nombre/teléfono del formulario.
- ⚠️ El match es exacto sobre el valor guardado: la normalización previa es crítica.

## Limitaciones y mejoras

- **PATCH redundante en altas nuevas**: el PATCH siempre se ejecuta, también cuando el contacto se acaba de crear. Es inocuo pero gasta una petición extra. Si te importa el rate limit (~7 req/s), añade un nodo IF que compare `contact_id` con el conjunto de IDs preexistentes (requiere lógica adicional).
- **Sobrescritura**: el PATCH **reemplaza** los valores actuales por los del formulario. Si un contacto tenía un teléfono distinto y ahora viene otro, queda solo el nuevo. Si quieres conservar valores anteriores, lee el contacto antes y haz merge.
- **Race condition**: dos webhooks simultáneos del mismo email pueden generar dos `/leads/complex` que vean el contacto como inexistente y creen uno cada uno. Para evitarlo, serializa con cola (Redis, SQS) o añade un Wait + lock por email.
- **Manejo de errores**: el flujo asume `2xx`. Para producción, marca **Continue On Fail** en los HTTP y añade ramas de error que registren el fallo (Slack, Sheets) y devuelvan `5xx` al formulario.

## Variantes

- **Solo `/leads/complex`**: elimina el nodo PATCH si no necesitas refrescar datos. Más simple, menos cuota de API.
- **Flujo manual de 7 pasos**: si quieres lógica condicional distinta para contactos nuevos vs existentes (tags, pipeline, responsables), reemplaza estos dos nodos por: buscar → IF → crear contacto / usar existente → crear lead. Da más control a costa de más nodos.
