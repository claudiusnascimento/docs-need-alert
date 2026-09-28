# Documentation project instructions

## About this project

- Public documentation for **Need Alert**, built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter; configuration lives in `docs.json`
- Two languages: **pt-BR** (default, pages at the root) and **en** (pages under `en/`). Every page exists in both; a page path belongs to exactly one language in `docs.json`
- Source of truth for the product is the app repository (`../need-alert`): `docs/general.md`, the specs under `docs/specs/`, and the SPA translation files under `resources/js/i18n/{pt-BR,en}/`
- Use the Mintlify MCP server, `https://mcp.mintlify.com`, to edit content and settings via MCP
- Use the Mintlify docs MCP server, `https://www.mintlify.com/docs/mcp`, to query information about using Mintlify via MCP

## Terminology

**Need Alert is not a queue.** There is no order, position, or waiting time: everyone subscribed to an item is notified at the same time. Never describe the product with "fila", "lista de espera", "queue", or "waitlist" (the only allowed use is contrasting Need Alert with queue tools, as in `como-funciona.mdx`).

UI element names must match the app's translation files exactly (bold them). The app's UI still says "fila" in some labels; the docs already use the new labels below, and the app will be renamed to match. Preferred terms:

| pt-BR | en | Notes |
|---|---|---|
| estabelecimento / unidade | establishment / unit | A physical unit |
| rede | chain | The paying account that owns units, credits and catalog (`Tenant` in code) |
| item | item | What people wait for |
| inscrição / inscritos | subscription / subscribers | A person's request to be notified about an item; everyone subscribed is notified at the same time |
| inscrever-se / cancelar inscrição | subscribe / unsubscribe | Buttons: **Quero ser avisado** / **Notify me**, **Cancelar inscrição** / **Unsubscribe** |
| Minhas inscrições | My subscriptions | The person's list of subscriptions |
| aviso único / aviso recorrente | one-time alert / recurring alert | The item's alert type (`queue_type` in code) |
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

- Public audience: establishments, people subscribed to items, and API integrators
- Don't document internal architecture, the admin panel, or features that are not released; planned features are marked as such (see `api/visao-geral.mdx`)
- The API reference is generated from OpenAPI once the public API ships (Phase 15 in the app roadmap). Follow `GUIA-DOCUMENTACAO-API.md` (internal, not published) for what to do then
