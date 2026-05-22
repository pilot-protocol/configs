# configs

Pilot Protocol shipped configurations — the canonical operational JSON
the daemon, gateway, and rendezvous ship to operators, plus the
network blueprint catalog deployed to the production rendezvous.

## Layout

| Path | What it is |
|---|---|
| `daemon.json` | Reference `pilot-daemon` config — registry, beacon, listen, identity paths. |
| `gateway.json` | Reference `pilot-gateway` config — alias mappings, port allocations. |
| `rendezvous.json` | Reference `pilot-rendezvous` config — beacon coords, listen, store. |
| `networks/` | The shipped network policy catalog — one `.json` per blueprint. |
| `networks/SHIPPED.md` | Index of every blueprint deployed to the production registry. |
| `sim/` | Traffic simulator scenarios (`default.json`, `stress-smoke.json`). |

## Network blueprints

`networks/*.json` are `LoadBlueprint`-shaped JSON files. Each defines:

- `name` + `description`
- `rules` (or `expr_policy`) — admission / event policy
- Optional defaults: `max_nodes`, `trust_decay_rate`, `discovery_mode`, etc.

The catalog ships 50 blueprints across three families:

- **Open-data networks** (`science`, `geo`, `weather`, `news`, `finance`,
  `dev`, `transit`, `sports`, …) — default-allow, open-join service nets.
- **Reputation / membership policies** (`trust-decay`,
  `high-trust-society`, `dunbar-150`, `aristocracy`, `meritocracy`,
  `ostracism`, `cold-shoulder`, …) — programmable `expr_policy`
  blueprints exercising different trust and eviction dynamics.
- **Reference networks** (`data-exchange-policy`) — canonical
  service-tag pattern used as inspiration for the open-data set.

See [`networks/SHIPPED.md`](networks/SHIPPED.md) for the full index
and current deployment state. Every blueprint here is round-tripped
through `LoadBlueprint` at test time, and `apply-networks.yml`
auto-provisions changes to the production rendezvous on push.
