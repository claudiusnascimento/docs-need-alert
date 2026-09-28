# Documentation project instructions

## About this project

- Public documentation for **Need Alert**, built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter; configuration lives in `docs.json`
- Two languages: **pt-BR** (default, pages at the root) and **en** (pages under `en/`). Every page exists in both; a page path belongs to exactly one language in `docs.json`
- Source of truth for the product is the app repository (`../need-alert`): `docs/general.md`, the specs under `docs/specs/`, and the SPA translation files under `resources/js/i18n/{pt-BR,en}/`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

## Terminology

UI element names must match the app's translation files exactly (bold them). Preferred terms:

| pt-BR | en | Notes |
|---|---|---|
| estabelecimento / unidade | establishment / unit | A physical unit |
| rede | chain | The paying account that owns units, credits and catalog (`Tenant` in code) |
| item | item | What people wait for |
| fila (única / recorrente) | queue (single / recurring) | Not "lista de espera" / "waitlist" in UI references |
| inscrição | subscription | A person's entry in a queue |
| disparo / alerta | dispatch / alert | Button: **Disparar alerta** / **Send alert**; section: **Disparos** / **Dispatches** |
| créditos | credits | 1 credit = 1 person notified |
| catálogo da rede | chain catalog | |
| papel / permissão | role / permission | |
| dono | owner | |

## Style preferences

- Use active voice and second person ("você" / "you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Configurações** / **Settings**
- Code formatting for file names, commands, paths, headers, and code references
- Don't hardcode values that are configurable in the app (package prices, bonus amount) unless the product decided them

## Content boundaries

- Public audience: establishments, people waiting in queues, and API integrators
- Don't document internal architecture, the admin panel, or features that are not released; planned features are marked as such (see `api/visao-geral.mdx`)
- The API reference is generated from OpenAPI once the public API ships (Phase 15 in the app roadmap)
