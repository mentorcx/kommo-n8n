# Lead desde formulario web (simple)

[English](README.en.md)

## Qué hace

Recibe un POST de un formulario web con `nombre`, `apellido`, `whatsapp`, `email`, y crea un lead en Kommo. Usa `POST /leads/complex` que internamente busca el contacto por email/teléfono: si existe, vincula el lead; si no, crea el contacto y luego el lead.

Es la variante **mínima**: una sola llamada a la API.

## Diagrama

```
Webhook → Normalizar → POST /leads/complex → Respond
```

## Nodos

1. **Webhook formulario** — `POST /webhook/kommo-form-lead-simple`.
2. **Normalizar** (Code) — `email` a minúsculas/trim; `whatsapp` a E.164.
3. **POST /leads/complex** — Crea lead, deduplica contacto.
4. **Respond to Webhook** — `200` con `lead_id` y `contact_id`.

## Cuándo elegirlo

- ✅ Solo te importa **no duplicar contactos**.
- ✅ Volumen alto / posibles duplicados simultáneos (la deduplicación la hace Kommo).
- ✅ Mínima superficie de mantenimiento.
- ❌ No esperes que actualice el contacto si ya existe (nombre, teléfono adicional, etc.). Para eso, ver la variante [`lead-desde-formulario-hibrido/`](../lead-desde-formulario-hibrido/).
- ❌ No te da control sobre la lógica de match (Kommo decide).

## Setup

Mismos requisitos que el resto de flujos (credencial Header Auth `Kommo API`, variable `KOMMO_SUBDOMAIN`). Ver detalles en [`../lead-desde-formulario-hibrido/README.es.md`](../lead-desde-formulario-hibrido/README.es.md#requisitos).

## Payload del formulario

```json
{
  "nombre": "Ana",
  "apellido": "Pérez",
  "whatsapp": "+54 9 11 5555 1234",
  "email": "ana@correo.com"
}
```
