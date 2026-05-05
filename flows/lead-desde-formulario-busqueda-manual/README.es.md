# Lead desde formulario web (búsqueda manual)

[English](README.en.md)

## Qué hace

Recibe un POST de un formulario web con `nombre`, `apellido`, `whatsapp`, `email`, busca explícitamente el contacto en Kommo, y según el resultado:

- Si **existe** → crea un lead y lo asocia al contacto encontrado.
- Si **no existe** → crea el contacto y luego el lead asociado.

Es la variante con **más control**: tú decides cómo se hace el match, qué pasa cuando hay duplicados parciales y puedes ramificar la lógica (tags, pipelines, responsables) según sea cliente nuevo o existente.

## Diagrama

```
Webhook → Normalizar → GET /contacts?query=email → Validar match exacto → IF existe?
                                                                            ├─ Sí ────────────────┐
                                                                            └─ No → POST /contacts → Extraer id
                                                                                                  │
                                                                                                  ↓
                                                                                                Merge → POST /leads → Respond
```

## Nodos

1. **Webhook formulario** — `POST /webhook/kommo-form-lead-manual`.
2. **Normalizar** (Code) — `email` a minúsculas/trim; `whatsapp` a E.164.
3. **GET /contacts?query=email** — Busca por email. Configurado con `neverError: true` para tratar `204` (sin resultados) como respuesta normal.
4. **Validar match exacto** (Code) — `query` hace match parcial; este nodo recorre `_embedded.contacts[]` y solo acepta el contacto si **algún** `custom_fields_values` con `field_code: EMAIL` o `PHONE` coincide exactamente con los valores normalizados. Devuelve `{ ...norm, contact_id, found }`.
5. **IF existe?** — `found === true`.
6a. Rama "no existe" → **POST /contacts** → **Extraer contact_id (nuevo)** (toma `_embedded.contacts[0].id`).
6b. Rama "sí existe" → pasa directo al Merge.
7. **Merge** (modo `chooseBranch`, salida `input1`) — Une ambas ramas. Solo una se ejecuta, así que el merge entrega el item de la que haya corrido.
8. **POST /leads** — Crea el lead con `_embedded.contacts: [{ id: contact_id }]`.
9. **Respond to Webhook** — `200` con `lead_id` y `contact_id`.

> "7 pasos" se refiere a los 7 hitos lógicos: webhook, normalizar, buscar, validar, decidir, crear/usar contacto, crear lead. En n8n se materializan en ~9 nodos por las ramas.

## Cuándo elegirlo

- ✅ Quieres **lógica condicional** distinta según sea contacto nuevo vs existente (ej. tags, pipeline, responsable, mensaje de bienvenida).
- ✅ Necesitas controlar el criterio de match (p. ej. solo email, no teléfono).
- ✅ Quieres registrar/auditar explícitamente si el contacto ya estaba.
- ❌ Más nodos = más superficie para errores, retries y mantenimiento.
- ❌ Si dos webhooks del mismo email llegan simultáneos, ambos pueden ver el contacto como inexistente y crear duplicados (race condition). Para alto volumen, prefiere [`lead-desde-formulario-simple/`](../lead-desde-formulario-simple/) o [`lead-desde-formulario-hibrido/`](../lead-desde-formulario-hibrido/).

## Setup

Mismos requisitos que las otras variantes. Ver [`../lead-desde-formulario-hibrido/README.es.md#requisitos`](../lead-desde-formulario-hibrido/README.es.md#requisitos).

## Payload del formulario

```json
{
  "nombre": "Ana",
  "apellido": "Pérez",
  "whatsapp": "+54 9 11 5555 1234",
  "email": "ana@correo.com"
}
```

## Notas y extensiones

- **Búsqueda solo por email**: si el formulario puede traer un email distinto al guardado pero el mismo teléfono, no encontrará el contacto. Para fallback por teléfono, duplica el bloque `GET → Validar` después del primer "no encontrado" y vuelve a evaluar.
- **Actualizar datos**: este flujo no actualiza el contacto existente. Si lo necesitas, añade un `PATCH /contacts/{id}` antes del `POST /leads` en la rama "sí existe".
- **Tags por rama**: en la rama "sí existe" añade un nodo Set para incluir un tag `cliente_recurrente` en el body del lead; en la rama "no existe", `cliente_nuevo`.
- **Manejo de errores**: marca **Continue On Fail** en los HTTP y enruta los errores a un nodo de logging (Slack, Sheets) antes de devolver `5xx`.
