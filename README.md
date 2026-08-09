# oceanic-bin
> A database management system with SQL-style syntax on a native BSON storage engine — built for developers who already think in relational terms but want document-model flexibility.

[![Status](https://img.shields.io/badge/status-early%20development-orange)]()
[![License](https://img.shields.io/badge/license-APACHE-blue)]()

---

## The Problem

Developers who know SQL/ORDBMS (Postgres, MySQL) have to relearn an entirely different mental model and query syntax when they need document flexibility (MongoDB). Switching between the two costs real time — different query languages, different schema philosophies, different tooling.

**NexusDB** aims to remove that switching cost: one syntax, backed by a storage engine built for flexible, schema-optional data.

---

## Core Idea

- **SQL-style syntax, BSON-native storage.** Write `SELECT`, `INSERT`, `WHERE` — the engine stores and executes against BSON documents underneath, not relational tables.
- **Schema-on-demand.** Start schema-less by dumping JSON in. Define a schema later, and existing data maps onto it — no forced upfront modeling.
- **Visual schema pipeline.** A live, on-screen view of how collections/tables relate to each other — see your database structure as a pipeline diagram, not just a list of table names.
- **Multi-language SDKs.** First-class clients for Node.js, TypeScript, and Python.
- **Runtime portability.** Designed to run cleanly in Docker and Kubernetes environments from day one.

---

## Why Not Just Use X?

Being upfront about prior art matters more than pretending this is unprecedented:

| Tool | What it does well | Where NexusDB differs |
|---|---|---|
| **SurrealDB** | Multi-model, SQL-like syntax over flexible storage | Closest existing analog — worth studying closely |
| **MongoDB + Atlas SQL** | SQL querying over BSON via BI connector | Bolted on for BI tools, not a native query path |
| **Prisma** | Unified syntax across multiple DB backends | Client/ORM layer, not its own storage engine |

NexusDB's differentiation, if it holds up, is the **visual schema pipeline** combined with a **purpose-built BSON storage engine** rather than sitting on top of an existing database.

---

## Architecture (Planned)

```
┌─────────────────────────────┐
│   SDKs (Node / TS / Python) │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│   SQL Parser → Query AST      │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│   Query Planner / Executor    │
└──────────────┬───────────────┘
               │
┌──────────────▼───────────────┐
│   BSON Storage Engine         │
│   (pages, WAL, indexing)      │
└───────────────────────────────┘
```

### Build order
1. On-disk BSON storage format (page/document layout)
2. Write-ahead log (WAL) for crash durability
3. Basic CRUD, single collection, no indexing
4. SQL parser (`SELECT` / `INSERT` / `WHERE` first)
5. Indexing (B-tree or LSM)
6. JOIN support across collections
7. Transactions / concurrency control
8. Visual schema pipeline UI
9. Docker / Kubernetes packaging, replication

---

## Status

Early development. Currently scoping the MVP: minimal BSON storage + basic SQL parsing (`SELECT`/`INSERT`/`WHERE`) + a Node/TS SDK, single collection, no indexing yet.

This is a solo, curiosity-driven build — expect the roadmap to evolve as design decisions (schema enforcement model, JOIN strategy, consistency guarantees) get made in public.

---

## Open Design Questions

- **Schema enforcement:** does defining a schema *enforce* types going forward, or just provide a friendly SQL-shaped view over schema-less BSON?
- **JOINs vs. embedding:** support relational-style `JOIN` across collections, nested-document access (`user.address.city`), or both?
- **Consistency model:** what are the transaction/concurrency guarantees once multiple writers are involved?

---

## Contributing

Not yet open for contributions — building the initial proof of concept solo first. Star/watch the repo if you want to follow progress.

## License

APACHE 2.0.
