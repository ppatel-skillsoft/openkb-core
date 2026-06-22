# openkb-core

The `openkb` Python package — CLI, document compiler, knowledge base indexer, storage backends, and HTTP API.

This is a maintained fork of [VectifyAI/OpenKB](https://github.com/VectifyAI/OpenKB), extended with:

- `openkb/api/` — FastAPI HTTP wrapper (`openkb serve`)
- `openkb/storage/` — `StorageBackend` abstraction (local filesystem + Azure Blob)
- `openkb/services/` — service layer (init, add, query, status, list)
- `openkb/db/` — SQLAlchemy schema and Alembic migrations

## Install

```bash
pip install "openkb-core @ git+https://github.com/ppatel-skillsoft/openkb-core@v0.1.0"

# With API and database extras:
pip install "openkb-core[api,db] @ git+https://github.com/ppatel-skillsoft/openkb-core@v0.1.0"
```

## CLI Usage

```bash
openkb --help
openkb init <kb-name>
openkb add <kb-name> <file-or-url>
openkb compile <kb-name>
openkb query <kb-name> "<question>"
```

## API Server

```bash
openkb serve --host 0.0.0.0 --port 8000
```

Starts a FastAPI server exposing KB operations over HTTP. Used as a sidecar by [openkb-platform](https://github.com/ppatel-skillsoft/openkb-platform).

## Package Structure

```
openkb/
├── cli.py           Click commands
├── config.py        Configuration loading
├── converter.py     Document → raw text conversion
├── indexer.py       PageIndex tree indexing
├── api/             FastAPI app and route handlers
├── db/              SQLAlchemy metadata and Alembic migrations
├── services/        Service functions (init, add, query, status, list)
├── storage/         StorageBackend ABC + Local and Azure Blob implementations
└── agent/           LLM agent runners (compiler, query, chat)
```

## Upstream Sync

This repo tracks [VectifyAI/OpenKB](https://github.com/VectifyAI/OpenKB) as an upstream remote. See [docs/UPSTREAM_SYNC.md](docs/UPSTREAM_SYNC.md) for the sync runbook.

## Related

- [openkb-platform](https://github.com/ppatel-skillsoft/openkb-platform) — the full service stack built on this package
