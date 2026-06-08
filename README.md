# Draftout API Spec

Community-maintained OpenAPI documentation for the public Draftout API at `https://draftoutmc.com`.

This is not an official Draftout project. The spec is based on observed API responses.

## Docs

- OpenAPI spec: [openapi.yaml](./openapi.yaml)
- Generated Markdown: [docs/api.md](./docs/api.md)
- Generated Redoc HTML: [docs/index.html](./docs/index.html)

## GitHub Pages

Docs are deployed automatically from `main` with GitHub Actions. Configure the repository's Pages source to **GitHub Actions**, then pushes to `main` publish the generated `docs/` directory.

The published site is expected at `https://memerson12.github.io/draftout-api-spec/`.

Pull requests that change the OpenAPI spec or docs inputs run a generated-docs check. If `openapi.yaml` changes, run `pnpm docs:generate && pnpm docs:html` and commit the updated files in `docs/`.

## Development

```bash
pnpm install
pnpm docs:generate
pnpm docs:html
pnpm docs:serve
pnpm spec:lint
```

`pnpm docs:serve` builds the Redoc HTML and serves `docs/` at `http://localhost:8088`.
