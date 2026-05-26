# Draftout API Spec

Community-maintained OpenAPI documentation for the public Draftout API at `https://draftoutmc.com`.

This is not an official Draftout project. The spec is based on observed API responses.

## Docs

- OpenAPI spec: [openapi.yaml](./openapi.yaml)
- Generated Markdown: [docs/api.md](./docs/api.md)
- Generated Redoc HTML: [docs/api.html](./docs/api.html)

## Development

```bash
pnpm install
pnpm docs:generate
pnpm docs:html
pnpm docs:serve
pnpm spec:lint
```

`pnpm docs:serve` builds the Redoc HTML and serves `docs/` at `http://localhost:8088`.
