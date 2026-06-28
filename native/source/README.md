# Native Source

The initial source snapshot was copied from `db/neo4j.rs` in the desktop app.

Source SHA-256: `ec1edf1dc6ae2cdb2b6b7c01054cf72ff50fe76dbca7a4c382fc4b3e9ee8fe2a`.


This directory is a migration staging area for `irodori.neo4j`. The active native
ABI shim lives in `src/lib.rs`; engine-specific connect/query/metadata behavior
should move here as the connector runtime contract is wired into the desktop app.

Engine status from `knowledge/engines.json`: `wired`.
