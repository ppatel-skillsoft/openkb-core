# openkb-core

OpenKB Python package — CLI, indexer, compiler, storage backends, and HTTP API.

This is a fork of [VectifyAI/OpenKB](https://github.com/VectifyAI/OpenKB) extended with:
- `openkb/api/` — FastAPI HTTP wrapper (`openkb serve`)
- `openkb/storage/` — StorageBackend abstraction (local + Azure Blob)
- `openkb/services/` — Service layer (init, add, query, status, list)

## Install

```bash
pip install openkb-core @ git+https://github.com/ppatel-skillsoft/openkb-core@v0.1.0
# or with extras:
pip install "openkb-core[api,db] @ git+https://github.com/ppatel-skillsoft/openkb-core@v0.1.0"
```

## Usage

```bash
openkb --help
openkb serve --host 0.0.0.0 --port 8000
```

## Upstream Sync

See [docs/UPSTREAM_SYNC.md](docs/UPSTREAM_SYNC.md) for how to pull changes from VectifyAI/OpenKB.

## Platform

For the full platform stack (generator-api, compiler-worker, Docker Compose) see:
→ [ppatel-skillsoft/openkb-platform](https://github.com/ppatel-skillsoft/openkb-platform)
