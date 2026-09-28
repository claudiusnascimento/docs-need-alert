# Guia — documentação da API pública

> Arquivo interno do repositório de documentação. **Não é publicado** (está no `.mintignore`). Serve para quem for continuar a documentação, pessoa ou agente.

## Situação atual

A aba **API** do site tem só duas páginas em cada idioma:

| pt-BR | en | Conteúdo |
|---|---|---|
| `api/visao-geral.mdx` | `en/api/overview.mdx` | O que a API vai permitir, com um aviso de "em desenvolvimento" |
| `api/convencoes.mdx` | `en/api/conventions.mdx` | Regras que já valem para a `v1`: versionamento, formato das respostas, erros, paginação, idioma, `X-Request-Id`, rate limit |

A **referência de endpoints, a autenticação e os guias de integração ainda não existem, de propósito.** Hoje a API do app só aceita a sessão do próprio painel (Sanctum em modo SPA, cookie + CSRF). Nenhum cliente externo consegue chamá-la, então documentar endpoints agora seria documentar algo que ninguém pode usar.

## Quando fazer

**Depois que a Fase 15 do app (API pública) estiver concluída e em produção.**

- Pré-spec: `need-alert/docs/pre-specs/pre-spec-15-api-publica.md` (PR #51 do repositório do app)
- Roadmap: `need-alert/docs/roadmap.md`, seção "Fase 15 — API pública (integrações)"

Não comece antes da spec e do plan da Fase 15 estarem fechados: nomes de campos, abilities, códigos de erro e o recorte de endpoints podem mudar até lá.

## O que fazer

### 1. Referência gerada do OpenAPI

A Fase 15 prevê o **Scramble** gerando o OpenAPI a partir do código, restrito às rotas liberadas para chaves de API.

1. No app: `php artisan scramble:export` gera o `openapi.json`.
2. Copie para este repositório como `api-reference/openapi.json`, de preferência por CI no repositório do app, para não desatualizar.
3. No `docs.json`, aponte a aba **API** de cada idioma para o arquivo (`"openapi": "api-reference/openapi.json"` no grupo ou na aba), para o Mintlify gerar as páginas de endpoint com o playground.
4. As descrições do OpenAPI ficam em **inglês**. O Mintlify gera as páginas uma vez por arquivo, e inglês é o padrão para referência de API. Os guias em volta ficam em pt-BR e en.
5. Confira no Mintlify como usar o mesmo OpenAPI nas duas línguas sem repetir o caminho de página (regra: um caminho pertence a um só idioma).

### 2. Guias novos, em pt-BR e en

| Página | Conteúdo |
|---|---|
| Autenticação | Criar a chave no painel (tela "Integrações"), header `Authorization: Bearer`, a chave aparece uma só vez, trocar e revogar, escopo de unidades e permissões |
| Primeira requisição | Do zero até disparar um alerta: listar unidades, achar o item pela `external_reference`, estimar o custo, disparar, acompanhar o status |
| Idempotência | `Idempotency-Key`: quando usar, janela de 24h, os erros `idempotency_key_reused` e `idempotency_request_in_progress` |
| Referência externa | Usar o SKU ou o código do seu sistema em `external_reference`, conflito `409` |
| Casos de uso | "Estoque voltou" (ERP), "horário cancelado" (agenda de clínica), sincronizar catálogo |

### 3. Ajustes nas páginas existentes

- **Visão geral:** tirar o `<Warning>` de "em desenvolvimento", trocar "o que estará disponível" pelo que existe, e linkar os guias novos.
- **Convenções:**
  - acrescentar a autenticação por chave e os códigos `401`/`403` específicos de chave (revogada, expirada, sem permissão, unidade fora do escopo → `404`);
  - rate limit por chave, com os valores reais;
  - idioma: sem `Accept-Language`, a chave usa o idioma padrão da rede (hoje a página diz pt-BR, que vale para a sessão do painel);
  - novos códigos de erro da Fase 15.
- **Casos de uso** (`casos-de-uso.mdx` / `en/use-cases.mdx`): linkar os guias de integração na seção "Integrações automáticas".
- **Changelog da API:** uma página nova, alimentada pelas diferenças do `openapi.json` a cada versão.

### 4. Vocabulário: nada de "fila"

O Need Alert **não é um app de fila**. Não há ordem nem posição: os inscritos num item são avisados todos ao mesmo tempo. A documentação usa **inscrição / inscritos** (en: **subscription / subscribers**) e **aviso único / aviso recorrente** (en: **one-time / recurring alert**). Veja a tabela de termos no `AGENTS.md`.

**Atenção antes de a Fase 15 congelar o contrato:** o código ainda usa `queue_type` (`single` | `recurring`) no item, e o `ItemResource` expõe esse nome. Quando a API ficar pública, o nome do campo vira contrato e só muda numa `v2`. Vale decidir **na spec da Fase 15** se o campo público continua `queue_type` ou vira algo como `alert_type` (`one_time` | `recurring`). Enquanto isso, na documentação, explique o campo pelo conceito de aviso único/recorrente, sem chamar de fila.

## Checklist

- [ ] Fase 15 concluída e em produção
- [ ] `openapi.json` exportado e copiado para `api-reference/` (idealmente por CI)
- [ ] `docs.json` com a referência na aba API, nos dois idiomas
- [ ] Guias de autenticação, primeira requisição, idempotência e referência externa, em pt-BR e en
- [ ] Visão geral sem o aviso de "em desenvolvimento"
- [ ] Convenções atualizadas (chave, rate limit, idioma, novos códigos)
- [ ] Nenhuma menção a "fila"/"queue" como conceito do produto
- [ ] `mint validate` e `mint broken-links` passando
- [ ] Este guia atualizado ou removido
