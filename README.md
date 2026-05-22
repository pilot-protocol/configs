# configs

Pilot Protocol shipped configurations. The canonical operational
JSON the daemon and rendezvous ship to operators.

## Layout

| Path | What it is |
|---|---|
| `daemon.json` | Reference `pilot-daemon` config — registry, beacon, listen, identity paths. |
| `gateway.json` | Reference `pilot-gateway` config — alias mappings, port allocations. |
| `rendezvous.json` | Reference `pilot-rendezvous` config — beacon coords, listen, store. |
| `networks/` | The shipped network policy catalog — one `.json` per blueprint. |
| `networks/SHIPPED.md` | Index of which blueprints are live in the production rendezvous. |
| `sim/` | Traffic simulator scenarios (default, stress-smoke). |

## Network blueprints

`networks/*.json` are `LoadBlueprint`-shaped JSON files. Each defines:

- `name` + `description`
- `rules` (or `expr_policy`) — admission / event policy
- Optional defaults: `max_nodes`, `trust_decay_rate`, `discovery_mode`, etc.

The protocol repo's `pkg/registry/wire` package and the
`pilot-protocol/policy` plugin parse these. Integration tests in
`pilot-protocol/web4` (`tests/zz_provision_shipped_test.go`, etc.)
verify that every blueprint in this catalog round-trips through
`LoadBlueprint` cleanly.

## Live vs. shipped

Only **3 of the 51 blueprints** are currently active in the
production rendezvous: `trust-decay`, `high-trust-society`,
`data-exchange-policy`. The rest are catalog entries that can be
provisioned on demand. See `networks/SHIPPED.md` for the canonical
list.
