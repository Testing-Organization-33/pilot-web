# pilot-web

Throwaway imitation repo for the DASH release-promotion pilot. Stands in for
`dash-frontend-local`: one required CI check, no migrations, and a declared dependency on
`pilot-api`'s contract so a cross-repo break can be staged deliberately.

Not a real app. Nothing here is meant to run.

## Branches

| Branch | Role |
|---|---|
| `development` | Integration. CI runs here. Default branch. |
| `main` | Production. Push triggers Deploy to Prod. Only ever advanced by `pilot-infra`'s Promote workflow, fast-forward only. |
| `deploy-state` | Machine-written. Holds `state.json`. Never edit by hand. |

## Arming failures

`pilot-config.json`: `fail_ci` ∈ {`null`, `"build"`}, `fail_deploy` ∈ {`null`, `"build"`, `"health"`}.

## Staging a cross-repo break

`contract.json.requires_api_contract` mirrors the real FE consuming a pulled snapshot of BE's GraphQL
schema. Bump it to `2` on `development` and promote **pilot-web only** (`repos: pilot-web`) while
`pilot-api` is still on v1 — production is then broken by promotion *ordering*, not by either repo's
own code. That is the failure the ship order and abort-on-first-failure rule exist to prevent, and it
is worth seeing once.
