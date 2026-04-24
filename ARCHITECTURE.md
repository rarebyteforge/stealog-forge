# SteaLog Forge
## Forensics Cyber Security Suite
### Complete Project Architecture

---

## Repository Map

```
stealog-forge/          PUBLIC  — Meta repo, docs, architecture, roadmap
stealog-core/           PRIVATE — FastAPI backend, database, API
stealog-frontend/       PRIVATE — React forensics dashboard
stealog-parser/         PRIVATE — Stealer log parser engine
stealog-scripts/        PRIVATE — Deployment, batch, dev tooling
```

---

## stealog-forge (Public)
**Purpose:** Public-facing documentation, architecture overview, and project roadmap.
No sensitive code. Entry point for anyone who discovers the project.

**Contents:**
- README.md — Full feature list, stack, legal notice
- ARCHITECTURE.md — System design diagrams
- ROADMAP.md — Modularization and optimization phases
- CONTRIBUTING.md — How to contribute (if open sourced)

---

## stealog-core (Private)
**Purpose:** The FastAPI backend — all business logic, database operations, API endpoints.

**Files:**
```
main.py           — FastAPI app + all route registrations
database.py       — SQLAlchemy engine + session management
models.py         — ORM: User, Case, Credential, Wallet, CookieValidation
schemas.py        — Pydantic request/response schemas
auth.py           — JWT + bcrypt + role-based access
crud.py           — All database read/write operations
seed.py           — Default user creation (admin/trainer)
new_routes.py     — Cookie validation + search routes
job_manager.py    — Background ingest job queue with progress tracking
ingest_routes.py  — Async ingest endpoints with folder selection
search_fts5.py    — FTS5-powered search endpoint
requirements.txt  — Python dependencies
.env.example      — Environment variable template
```

**Key API Endpoints:**
```
POST /auth/login              JWT authentication
GET  /cases                   List all cases
GET  /cases/{id}              Full case detail
POST /ingest                  Upload + parse (async for ZIPs)
GET  /jobs/{id}               Background job progress
GET  /search                  FTS5 credential search
POST /validate/cookie         Single cookie validation
POST /validate/case/{id}      Batch case validation
GET  /validate/audit/{id}     Chain of custody log
POST /search/rebuild          Rebuild FTS5 index
GET  /analytics/summary       Dashboard stats
GET  /analytics/reuse         Cross-case credential reuse
GET  /export/{id}/json        Forensics handoff package
GET  /export/{id}/csv         Redacted victim CSV
GET  /cases/{id}/cookies      Paginated cookie access
GET  /                        Serves frontend HTML
GET  /health                  Health check
```

---

## stealog-frontend (Private)
**Purpose:** Single-file React SPA served by FastAPI at GET /.

**Files:**
```
index.html (→ stealog-frontend.html)  — Complete React app
```

**Components:**
```
LoginScreen         JWT auth form
App                 Main state container + tab router
CredentialScreen    Domain browser → drill to credentials
CookieScreen        Domain grid → cookies + live validation
WalletScreen        Wallet cards + admin key reveal
FileScreen          Case metadata + system info
SearchScreen        FTS5 domain search + cookie correlation
```

**Design Decisions:**
- Single HTML file — no build step, no npm, no webpack
- Babel CDN — JSX compiled in-browser
- All CSS inline — zero external stylesheets
- Served same port as API — no CORS issues
- Offline capable — works with no internet after first load

---

## stealog-parser (Private)
**Purpose:** Stealer malware log parsing engine.

**Files:**
```
parser_engine.py  — Family detection + all extraction logic
```

**Family Detection:**
```python
FAMILIES = {
    "redline":  UserInformation.txt + Passwords.txt + Cookies.txt
    "lumma":    Passwords_*.txt or Autofills_*.txt
    "raccoon":  system.txt + passwords.txt
    "vidar":    information.txt + screenshot file
    "generic":  Fallback keyword scan
}
```

**Extraction Functions:**
```python
extract_credentials(text)         # URL:user:pass (both formats)
extract_system_info(text)         # IP, OS, hostname, HWID, CPU, RAM
parse_netscape_cookies(text)      # Tab-separated browser cookies
extract_cookies_from_files(files) # Multi-browser cookie extraction
extract_wallets(files)            # BTC/ETH/XMR/LTC + key detection
ingest_files(files)               # Main dispatcher → family detect + parse
```

**Credential Formats Handled:**
```
Format 1 (multi-line):
  URL: https://accounts.google.com
  Username: victim@gmail.com
  Password: hunter2

Format 2 (single-line):
  https://accounts.google.com:victim@gmail.com:hunter2
```

---

## stealog-scripts (Private)
**Purpose:** All deployment, development, and operational scripts.

**Files:**
```
stealog_setup.sh    — Complete fresh Termux install (one command)
dev.sh              — Server launcher with file watching + auto-restart
update.sh           — Copy from Downloads + restart (one command update)
batch_ingest.sh     — Overnight batch processing of 1000+ log files
fts5_migrate.sh     — Build FTS5 search index on existing database
patch_v2.sh         — Cookie validation + search patch
patch_ingest.sh     — Background ingest patch
github_push.sh      — This script — GitHub setup and push
bashrc.sh           — Optimized Termux environment
```

**Workflow Commands (from bashrc):**
```bash
# Server
slstart             # Start with file watching
slstop              # Stop server
slrestart           # Force restart
slstatus            # Check running status
sllog               # Live log stream
slupdate            # Copy from Downloads + restart

# Search
sl-search gmail     # FTS5 search across all cases
sl-validate CASE-X  # Validate session cookies

# Database
sldb                # SQLite shell
sldb-cases          # Pretty-print all cases
sldb-stats          # One-line summary

# Batch
sl-batch            # Process all new log files
sl-batch --dry-run  # Preview only

# GitHub
sl-push "message"       # Push core + frontend + parser
sl-push-scripts         # Push deployment scripts
sl-push-all "message"   # Push everything
```

---

## Database Schema

```sql
users                    — Auth: username, hashed_password, role
cases                    — Case metadata + parsed_result JSON blob
credentials              — Indexed: case_id, url, domain, username
wallets                  — Crypto: address, type, has_key
cookie_validations       — Audit: analyst, domain, cookie_hash, result
ingested_files           — Batch tracking: filename, case_id
search_index             — FTS5: domain, url, username, cookie correlation
credentials_fts          — SQLite FTS5 virtual table
```

---

## Deployment Target

```
Device:   T-Mobile Revvl Tab 2
CPU:      MediaTek Dimensity 6300 (ARM64, 6nm, octa-core)
RAM:      4GB physical + 4GB virtual
Storage:  64GB (expandable)
Android:  15
Runtime:  Native Termux — no proot, no Docker
Port:     8000 (frontend + API on same port)
```

---

## Modularization Roadmap

```
Phase 1 — Split main.py into routes/
Phase 2 — Split parser_engine.py into parsers/
Phase 3 — Dedicated cookies table (not in parsed_result blob)
Phase 4 — Encryption at rest for parsed_result
Phase 5 — Async background processing (currently threaded)
Phase 6 — API versioning at /api/v1/
Phase 7 — GCP deployment option (already scaffolded)
```

---

## Legal Notice

SteaLog Forge is built exclusively for:
- Authorized security incident response
- Security awareness training
- Academic research with proper authorization
- Victim notification programs

Unauthorized access to computer systems is illegal in all jurisdictions.
All cookie validation is performed for evidentiary purposes only.
Private keys and credentials are never transmitted externally.
