Packaging Stock Rebalancing (AI/ML)

Python script that suggests packaging stock transfers between units, using business rules and outlier detection support (IsolationForest).

Key rules implemented

Outliers (IsolationForest):

Outlier receivers are processed last.

Outlier donors are avoided; used only as a “last resort”.

Minimum donor stock: keeps at least DONOR_KEEP_DAYS days of consumption (default: 60).

Receiver replenishment: computes the need to restore COVER_TARGET_DAYS days of coverage (default: 30).

Stockout risk: considers deficit when days_cover <= RUPTURE_THRESHOLD (default: 3).

Multiples of 5 and minimum quantities per package type (e.g., small envelope/box = 10; medium/large box = 5).

Daily idempotency: deletes existing rows for the day (date = TODAY) from the target table before inserting new ones.

Use environment variables:

Option A (recommended): DATABASE_URL

Example:

DATABASE_URL=postgresql+psycopg2://USER:PASSWORD@HOST:5432/DB

Option B: separate variables
PG_HOST=...
PG_PORT=5432
PG_DB=postgres
PG_USER=...
PG_PASS=...


Create a local .env file (not versioned) and run normally. A .env.example is included in the repository.

How to run
1) Create a venv and install dependencies

Windows (PowerShell):

python -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt

2) Configure .env

Copy:

copy .env.example .env


Edit .env with your settings.

3) Execute
python processa_remanejamentos.py


Dry-run (no DB writes):

python processa_remanejamentos.py --dry-run

Useful arguments

--lookback-days 60

--cover-target-days 30

--donor-keep-days 60

--rupture-threshold 3

--only-prefixes "AC ,CE "

--target-table public.fato_estoque_remanejamento

--today 2025-12-23

--log-level INFO

--dry-run

Repository structure

processa_remanejamentos.py — main script

requirements.txt — dependencies

.env.example — environment variables template

.gitignore — avoids versioning .env, venv, caches, etc.
