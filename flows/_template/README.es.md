# {Nombre del flujo}

[English](README.en.md)

> Plantilla. Copia esta carpeta para añadir un flujo nuevo.

## Qué hace

Una descripción breve del propósito del flujo (1–3 frases).

## Diagrama / Trigger

- **Trigger**: Webhook / Cron / Manual / etc.
- **Frecuencia**: si aplica.

## Nodos principales

- Lista de nodos clave y para qué sirven.

## Requisitos

- Cuenta de Kommo con permisos `...`.
- Credenciales de n8n configuradas:
  - `Kommo API` (Header Auth o OAuth2).
  - Otras (Slack, Sheets, etc.) si las usa.
- Variables / parámetros del workflow:
  - `KOMMO_SUBDOMAIN`
  - `KOMMO_PIPELINE_ID`
  - ...

## Cómo importarlo

1. En n8n: **Workflows → Import from File**.
2. Selecciona [`workflow.json`](workflow.json).
3. Asigna las credenciales en cada nodo HTTP Request.
4. Define las variables requeridas.
5. Activa el workflow.

## Notas

Limitaciones, gotchas o ideas de extensión.
