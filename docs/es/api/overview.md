# Visión general de la API de Kommo

> Estado: plantilla inicial. Amplía con detalles oficiales y enlaces a la documentación de Kommo.

## URL base

```
https://{subdominio}.kommo.com/api/v4/
```

`{subdominio}` corresponde al de tu cuenta (por ejemplo, `miempresa.kommo.com`).

## Versión

La versión actual estable es **v4**. Documenta aquí cualquier diferencia con v2 si tu integración aún la usa.

## Formato

- Solicitudes y respuestas en **JSON** (`Content-Type: application/json`).
- Códigos HTTP estándar (`200`, `201`, `204`, `400`, `401`, `403`, `404`, `429`, `5xx`).
- Respuestas con colecciones siguen el formato HAL (`_embedded`, `_links`, `_page`).

## Recursos principales

- `/leads` — oportunidades de venta.
- `/contacts` — contactos.
- `/companies` — empresas.
- `/tasks` — tareas.
- `/events` — eventos del feed.
- `/notes` — notas.
- `/pipelines` — embudos y etapas.
- `/users` — usuarios de la cuenta.
- `/webhooks` — suscripciones a eventos.

Cada recurso tiene su propia ficha en [`endpoints/`](endpoints/).

## Paginación

Las colecciones devuelven `_page`, `_links.next` y `_links.prev`. Usa el parámetro `page` y `limit` (por defecto 250 en algunos recursos).

## Filtros y búsqueda

Parámetros comunes: `with`, `query`, `filter[...]`, `order[...]`. Detallar por endpoint en su ficha.

## Errores

Documenta aquí los formatos de error típicos y los códigos más comunes. Más detalle en [`rate-limits.md`](rate-limits.md) y en cada endpoint.

## Referencias

- Documentación oficial: https://developers.kommo.com/
