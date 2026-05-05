# Cómo contribuir / How to contribute

> Español primero, English below.

---

## Español

¡Gracias por aportar! Esta wiki es colaborativa: cualquier persona puede mejorar la documentación o subir un flujo.

### Tipos de aportes

- **Documentación de endpoint**: añadir o mejorar un archivo en `docs/es/api/endpoints/`.
- **Guía de n8n**: añadir patrones, trucos o configuraciones en `docs/es/n8n/`.
- **Flujo de n8n**: subir un workflow exportado en `flows/<nombre-del-flujo>/`.
- **Traducción**: traducir contenido existente a otro idioma bajo `docs/<idioma>/`.
- **Corrección**: tipos, enlaces rotos, ejemplos obsoletos.

### Proceso

1. Abre un issue si tu cambio es grande, para evitar trabajo duplicado.
2. Crea una rama: `feat/endpoint-leads`, `flow/sync-contactos`, `i18n/en-overview`.
3. Sigue las plantillas:
   - Endpoint: [`docs/es/api/endpoints/_template.md`](docs/es/api/endpoints/_template.md)
   - Flujo: [`flows/_template/`](flows/_template/)
4. Si tocas contenido que ya existe en otros idiomas, marca el estado en [`TRANSLATIONS.md`](TRANSLATIONS.md) (`OK` / `Outdated` / `Missing`).
5. Abre un Pull Request describiendo qué cambió y por qué.

### Estilo

- Idioma fuente: español. Si añades contenido nuevo solo en otro idioma, indícalo en el PR.
- Markdown plano, sin HTML salvo cuando sea imprescindible.
- Bloques de código con lenguaje (` ```json `, ` ```bash `, ` ```http `).
- Enlaces relativos para navegar entre archivos del repo.
- No incluyas credenciales reales ni IDs de cuentas privadas en ejemplos.

### Flujos de n8n

- Exporta desde n8n con **Download** y guarda como `workflow.json`.
- Limpia credenciales antes de subir (n8n las separa, pero verifica IDs/URLs sensibles).
- Acompaña el JSON con un `README.es.md` que explique: qué hace, qué nodos usa, variables/credenciales requeridas, cómo importarlo.

---

## English

Thanks for contributing! This wiki is collaborative: anyone can improve the docs or upload a flow.

### Types of contributions

- **Endpoint documentation**: add or improve a file in `docs/en/api/endpoints/`.
- **n8n guide**: patterns, tips, or configurations in `docs/en/n8n/`.
- **n8n flow**: upload an exported workflow under `flows/<flow-name>/`.
- **Translation**: translate existing content under `docs/<lang>/`.
- **Fix**: typos, broken links, outdated examples.

### Process

1. Open an issue if your change is large, to avoid duplicated work.
2. Create a branch: `feat/endpoint-leads`, `flow/sync-contacts`, `i18n/en-overview`.
3. Follow the templates:
   - Endpoint: [`docs/en/api/endpoints/_template.md`](docs/en/api/endpoints/_template.md)
   - Flow: [`flows/_template/`](flows/_template/)
4. If you touch content that exists in other languages, mark its state in [`TRANSLATIONS.md`](TRANSLATIONS.md) (`OK` / `Outdated` / `Missing`).
5. Open a Pull Request describing what changed and why.

### Style

- Source language: Spanish. If you add brand-new content only in another language, note it in the PR.
- Plain markdown, no HTML unless strictly necessary.
- Fenced code blocks with language (` ```json `, ` ```bash `, ` ```http `).
- Relative links to navigate between repo files.
- Never include real credentials or private account IDs in examples.

### n8n flows

- Export from n8n via **Download** and save as `workflow.json`.
- Strip credentials before uploading (n8n separates them, but verify sensitive IDs/URLs).
- Pair the JSON with a `README.en.md` explaining: what it does, which nodes it uses, required variables/credentials, how to import it.
