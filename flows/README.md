# Flujos / Flows

Cada flujo vive en su propia carpeta `flows/<nombre-del-flujo>/` y contiene:

- `workflow.json` — exportación del workflow de n8n.
- `README.es.md` — explicación en español.
- `README.en.md` — explicación en inglés (puede ser un stub si aún no se traduce).

Plantilla disponible en [`_template/`](_template/).

## Índice

> Marca con `[x]` cuando el flujo esté disponible.

- [ ] `_template/` — plantilla de referencia (no es un flujo real)
- [x] [`lead-desde-formulario-simple/`](lead-desde-formulario-simple/) — Solo `/leads/complex` (mínimo)
- [x] [`lead-desde-formulario-hibrido/`](lead-desde-formulario-hibrido/) — `/leads/complex` + `PATCH /contacts/:id` (refresca datos)
- [x] [`lead-desde-formulario-busqueda-manual/`](lead-desde-formulario-busqueda-manual/) — Búsqueda explícita + ramas crear/usar (máximo control)

### Comparativa rápida

| Variante           | Llamadas API | Deduplica | Actualiza contacto | Lógica condicional | Race-safe |
|--------------------|--------------|-----------|--------------------|--------------------|-----------|
| `simple`           | 1            | sí (Kommo) | no                 | no                 | sí        |
| `hibrido`          | 2            | sí (Kommo) | sí                 | limitada           | sí        |
| `busqueda-manual`  | 2-3          | sí (n8n)   | opcional           | total              | no        |

---

Each flow lives in its own folder `flows/<flow-name>/` and contains:

- `workflow.json` — exported n8n workflow.
- `README.es.md` — Spanish explanation.
- `README.en.md` — English explanation (may be a stub until translated).

Template available at [`_template/`](_template/).
