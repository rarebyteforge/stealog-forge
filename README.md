# SteaLog Forge
## Forensics Cyber Security Suite

A professional stealer malware forensics platform built for security awareness training, incident response, and threat intelligence.

### Architecture

```
stealog-forge/          ← This repo (public docs + architecture)
stealog-core/           ← FastAPI backend + database (private)
stealog-frontend/       ← React forensics dashboard (private)
stealog-parser/         ← Log parser engine (private)
stealog-scripts/        ← Dev tooling + deployment (private)
```

### Platform Capabilities

| Feature | Status |
|---------|--------|
| Stealer log ingestion (ZIP auto-extract) | ✅ |
| Family detection (RedLine, Lumma, Raccoon, Vidar) | ✅ |
| Credential extraction + domain browser | ✅ |
| Session cookie extraction + validation | ✅ |
| Crypto wallet + private key detection | ✅ |
| FTS5 full-text search across all cases | ✅ |
| Background async ingest with progress | ✅ |
| Batch processing (1000+ files) | ✅ |
| Role-based access (admin/analyst/readonly) | ✅ |
| MITRE ATT&CK mapping | ✅ |
| Chain of custody audit log | ✅ |
| JSON/CSV forensics export | ✅ |
| Offline / air-gapped deployment | ✅ |

### Stack

- **Backend:** FastAPI + Python 3.13 + SQLite + SQLAlchemy 2.0
- **Frontend:** React (Babel CDN) served by FastAPI — single port, no CORS
- **Auth:** PyJWT + bcrypt
- **Search:** SQLite FTS5 full-text index
- **Deployment:** Native Termux (ARM64) — no Docker, no proot

### Device

Deployed on T-Mobile Revvl Tab 2 (MediaTek Dimensity 6300, ARM64, Android 15)

### Legal

This platform is built exclusively for authorized security research,
incident response, and victim notification. Unauthorized access to
computer systems is illegal. Use responsibly.

### Repository Map

| Repo | Contents | Visibility |
|------|----------|------------|
| [stealog-forge](https://github.com/rarebyteforge/stealog-forge) | Docs, architecture, roadmap | Public |
| stealog-core | API, models, CRUD, auth | Private |
| stealog-frontend | React dashboard HTML | Private |
| stealog-parser | Parser engine, family detection | Private |
| stealog-scripts | Deploy, batch, dev scripts | Private |
