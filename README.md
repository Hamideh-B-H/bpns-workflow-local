
# bpns-workflow-local

This is exercise 2 from onboarding: running the EMO-BON data pipeline **locally**, step by step, instead of letting GitHub do it automatically.

The pipeline has 4 phases overall:
```
Google Sheets --> local CSV --> validated CSV --> RDF graph (RO-Crate) --> published web page
```
This repo covers **Phase 1**: getting data out of Google Sheets and into local CSV files.

---

## Step 1: Set up the local environment

- Installed Git (`2.55.0`) and Poetry (`2.4.3`) on Windows
- Created this repo, linked to GitHub (`main` branch)
- Added a `.gitignore` so personal notes and Python cache files never get committed

## Step 2: Find the code that does Phase 1

Phase 1 is handled by a GitHub Action called `populate-action`:
- Repo: https://github.com/emo-bon/populate-action

Cloned it locally and read its 3 files:
- `action.py` – the actual logic (downloads Google Sheets, saves as CSV)
- `Dockerfile` – shows what needs to be installed: `pandas`, `openpyxl`, and VLIZ's own [emobon-dm-tools](https://github.com/emo-bon/emobon-dm-tools) package
- `entrypoint.sh` – just runs `python /opt/action.py`, nothing else

## Step 3: Find the required URLs

`action.py` needs 3 Google Sheets URLs (water, sediment, hard substrates) passed in as environment variables. These live in a master registry:
- Repo: https://github.com/emo-bon/governance-crate
- File: `logsheets.csv`

**BPNS (Belgium / VLIZ) row:**

| Variable | URL |
|---|---|
| `WATER_LOGSHEET_URL` | https://docs.google.com/spreadsheets/d/1mEi4Bd2YR63WD0j54FQ6QkzcUw_As9Wilue9kaXO2DE/edit?usp=sharing |
| `SEDIMENT_LOGSHEET_URL` | https://docs.google.com/spreadsheets/d/1zc0bZdpl-Eoi35lI_5BGkElbscplyQRyNPLkSgeEyEQ/edit?usp=sharing |
| `HARD_LOGSHEET_URL` | https://docs.google.com/spreadsheets/d/1qhp2osFgZ9xuHg59AzwCAw2AoVjl3m8OEO5ou_oZOqg/edit?usp=sharing |

## Step 4: Install dependencies (next)

```
pip install pandas openpyxl
pip install git+https://github.com/emo-bon/emobon-dm-tools.git@release/v0.1.0
```

## Step 5: Set environment variables and run (next)

Two more variables are needed besides the three URLs above:
- `GITHUB_WORKSPACE` – local folder where output CSVs will be saved
- `GITHUB_REPOSITORY` – any placeholder name, only used for a README the script writes

Then run:
```
python action.py
```

## Step 6: Check the output (next)

Expect a `logsheets/raw/` folder to appear, containing CSV files like `water_observatory.csv`, `water_sampling.csv`, `water_measured.csv` (and the same for sediment/hard, where data exists).

---

## Progress

- [x] Step 1: local environment set up
- [x] Step 2: found and read `populate-action`
- [x] Step 3: found the 3 BPNS logsheet URLs
- [ ] Step 4: install dependencies
- [ ] Step 5: set env vars and run
- [ ] Step 6: verify output CSVs
- [ ] Phase 2: validate CSVs
- [ ] Phase 3: uplift to RDF/RO-Crate
- [ ] Phase 4: publish locally