# Neo4j Connector

Adds Neo4j connectivity as an installable connector extension.

This connector is listed in the public Irodori extension marketplace.

## Connector

- Extension ID: `irodori.neo4j`
- Engine ID: `neo4j`
- Wire: `neo4j`
- Default port: `7687`
- Native ABI: `irodori.connector.native.v1`
- Driver linked: `true`

A desktop adapter source snapshot is staged in `native/source/` from `db/neo4j.rs`.

Connector metadata lives in `connector.config.json` and `irodori.extension.json`.
The Rust code keeps native ABI exports in `src/lib.rs`, shared buffer/JSON helpers in `src/abi.rs`, and Neo4j behavior in `src/driver.rs`.

## Connection Metadata

- Endpoint modes: `hostPort`, `connectionString`
- Transport modes: `direct`, `sshTunnel`, `socks5Proxy`, `httpConnectProxy`, `proxyChain`
- TLS supported: `true`
- Custom driver options: `true`

| Auth method | Label | Secret purposes |
|---|---|---|
| `none` | No authentication | none |
| `connectionString` | Connection string / DSN | none |
| `basic` | Basic authentication | `password` |
| `kerberos` | Kerberos / GSSAPI | `token` |
| `bearerToken` | Bearer token | `token` |
| `clientCertificate` | Client certificate / mTLS | `privateKey`, `privateKeyPassphrase` |
| `customDriverOptions` | Custom driver options | `password`, `token`, `privateKey`, `privateKeyPassphrase` |

## Experience Metadata

- Domains: `graph`
- Result views: `graph`, `path`, `table`
- Inspired by: `Neo4j Browser`, `Neo4j Bloom`, `Neo4j Graph Data Science`, `Cypher shortest path`

| Workflow | Result view | Templates |
|---|---|---|
| Schema overview | table | graph-cypher-label-counts |
| Explore neighborhood | graph | graph-cypher-neighborhood |
| Shortest path | path | graph-cypher-shortest-path |
| Algorithm starter | table | graph-cypher-degree-centrality |

| Template | Label | Language | Result view |
|---|---|---|---|
| `graph-cypher-label-counts` | Label counts | `cypher` | `table` |
| `graph-cypher-neighborhood` | Neighborhood graph | `cypher` | `graph` |
| `graph-cypher-shortest-path` | Shortest path | `cypher` | `path` |
| `graph-cypher-degree-centrality` | Degree centrality starter | `cypher` | `table` |

## ABI Calls

The driver handles these JSON requests today:

| Method | Response |
|---|---|
| `health` / `ping` | Connector health, engine id, ABI version, and driver link status. |
| `describe` / `capabilities` | Embedded manifest and connector config. |
| `manifest` | Raw `irodori.extension.json`. |
| `config` | Raw `connector.config.json`. |
| `connect` | Opens a Bolt connection through `neo4rs`. |
| `query` | Runs Cypher and returns rows. |
| `metadata` | Samples labels and relationship types. |
| `close` | Removes the cached native connection. |

## Development


Generated extension repositories share `../target` across sibling repositories so Rust dependencies are compiled once per checkout. DuckDB and MotherDuck are driver-linked by default; set `IRODORI_CONNECTOR_LINK_DUCKDB=0` only when you need metadata-only DuckDB-compatible scaffolds.


```sh
make check
make build
```

Release packages place platform-specific native artifacts under `dist/native`.
