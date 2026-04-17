# elk-apm-server-app

Zerops recipe that installs upstream Elastic APM Server (`apm-server=8.16.6`) via apt and forwards APM traces from instrumented apps to a sibling Elasticsearch service.

## Zerops service facts

- HTTP ports: `8200` (`httpSupport: true`)
- Siblings: `elkstorage` (Elasticsearch) — env: `ELASTICSEARCH_URL`, `ELASTICSEARCH_PASSWORD`
- Runtime base: `ubuntu@24.04`

## Zerops

No dev iteration loop — the app is an upstream Elastic binary installed via apt in `prepareCommands`. Changes in this repo only affect the `apm-server/` config directory and `zerops.yaml` install pins. Each change requires a full build+deploy through the **Zerops development workflow via `zcp` MCP tools**.

## Notes

- APM version pinned to `apm-server=8.16.6` in `prepareCommands`; bump the pin to upgrade.
- `apm-server/apm-server.yml` uses `%%VAR%%` placeholders (`%%ELASTICSEARCH_URL%%`, `%%ELASTICSEARCH_PASSWORD%%`, `%%SECRET_TOKEN%%`) replaced by `envReplace` at deploy time.
- `SECRET_TOKEN` must be set as an env secret on the service; APM agents authenticate via `Authorization: Bearer <SECRET_TOKEN>`.
