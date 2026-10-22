# LLM-Based Simulations of Survey Responses: A Systematic Literature Review and Synthesis of Methods

Replication materials and literature databases for RESEARCH SYNTHESIS -- *LLM-Based Simulations of Survey Responses: A Systematic Literature Review and Synthesis of Methods*.

**Abstract**: Large language models (LLMs) are increasingly used in survey research, including to predict, simulate, or impute responses. Despite a rapidly growing interest in applications to create synthetic survey data, the empirical evidence remains fragmented, with substantial variation in research objectives, model architectures, "persona" construction strategies, and validation practices. This paper presents a systematic review of 55 studies published between 2022 and 2025 that use LLMs to generate, predict, or impute responses in survey and related attitudinal or behavioral data. The results suggest that LLM-based survey simulation is best understood as a conditional, use-case-specific tool rather than a general substitute for traditional imputation methods and human respondents. We outline concrete steps toward a more coherent evaluation framework, including use-case-specific metric selection, relational alignment as a standard validity check, and greater transparency in model and design choices.

---

## Overview

The review follows the PRISMA workflow (flow diagram: [docs/Prisma Flow Diagram.png](<docs/Prisma Flow Diagram.png>), editable source in [docs/Diagrams.pptx](docs/Diagrams.pptx)) and is implemented in three Jupyter notebooks (SLR1–SLR3), run in order:

| Notebook | Stage | Main inputs | Main outputs |
|---|---|---|---|
| SLR1: [1_Database_extraction](notebooks/1_Database_extraction.ipynb) | **Identification**: query Web of Science and Semantic Scholar, validate search-string recall against a reference set of key papers, merge and de-duplicate, pre-screen | Database APIs (keys required), `data/search/LLM - Survey Proxies (key papers).csv` | `data/screening/df_final_prescreen.csv`, `data/search/recall_table_final.csv` |
| SLR2: [2_Screening-Validation](notebooks/2_Screening-Validation.ipynb) | **Screening**: inter-rater validation of title/abstract screening (ASReview), coverage check, preprint handling, merge with records from other methods | `data/screening/`, `data/search/`, `data/extraction/DataExtraction_Final.csv` | Validation samples (`data/screening/df_asreview_sample_*.csv`, `df_asreview_screenerA.csv`); screening counts and agreement statistics (in notebook) |
| SLR3: [3_DataExtractionAnalysis](notebooks/3_DataExtractionAnalysis.ipynb) | **Data extraction & synthesis**: coding of the 55 included studies, descriptive analyses and cross-tabulations by validity verdict | `data/extraction/DataExtraction_Final*.csv` | `results/figures/*.pdf`, `data/extraction/Master_Coded_Table.csv`, `results/tables/Crosstab_Table_byVerdict.csv` |

## Repository structure

```
├── notebooks/            Python notebooks (SLR1 → SLR2 → SLR3)
├── data/
│   ├── search/           Search stage: retrieved records, recall table, reference set, records from other methods
│   ├── screening/        Screening stage: ASReview exports, validation samples, full-text eligibility
│   └── extraction/       Extraction stage: Elicit outputs and prompts, human–Elicit validation, hand edits to the Elicit extraction, final coded tables
├── results/
│   ├── figures/          Figures (PDF)
│   ├── tables/           Cross-tabulation by validity verdict
│   └── references.bib    BibTeX bibliography (Zotero export)
├── docs/                 PRISMA flow diagram (PNG; PowerPoint source in Diagrams.pptx), ASReview stopping-rule plot
├── requirements.txt      Python dependencies (pinned)
└── .env.example          Template for API keys (SLR1 only)
```

### Data files

**`data/search/`**
- `raw_wos/` – Web of Science records: API results of the nine candidate queries (`df_WoS_*.csv`) and the final web-interface export of the selected query (`df_WoS_LLM_and_Survey_and_SimulationA_FULL.xls`).
- `raw_semanticscholar/` – Semantic Scholar records: results of the nine candidate queries (`df_SS_*.csv`, `*_filtered.csv` after the field and publication-type filters) and the final retrieval of the selected query (`df_SS_final_FULL.csv`).
- `recall_table_final.csv` / `.xlsx` – Recall of each candidate query against the reference set.
- `LLM - Survey Proxies (key papers).csv` – Reference set of key papers used to assess search-string recall.
- `Identified papers other methods (Zotero).csv` – Records identified through other methods (outside the database searches; Zotero export).

**`data/screening/`**
- `df_final_prescreen.csv` – De-duplicated records entering title/abstract screening (input to ASReview).
- `asreview_results*.csv` – ASReview exports of title/abstract screening decisions (`asreview_results_FINAL.csv` is the final version).
- `asreview_Validation_Screening_*.csv`, `df_asreview_sample_*.csv`, `df_final_prescreen_sample.csv`, `asreview_sample.csv`, `asreview_results_initialscreening.csv` – Calibration and inter-rater validation samples.
- `asreview_results_Validating_ScreenerA_Screening.csv`, `df_asreview_screenerA.csv` – Re-validation of one screener's decisions.
- `asreview_results_FINAL_ZoteroSupplements.csv`, `recordstofix_preprintstatus.csv` – Metadata supplements and publication-status corrections for preprints.
- `Fulltext_Screening - Eligibility.csv` – Full-text eligibility decisions.

**`data/extraction/`**
- `Elicit Prompts Instructions.csv` – Prompts used for AI-assisted data extraction with Elicit.
- `Elicit - original full data extraction.csv` – Original Elicit extraction output, before hand re-coding.
- `Data Extraction - Human and Elicit Validation.xlsx` – Coding instructions, human vs. Elicit extraction tables, and inter-rater agreement (sheet `IRA`).
- `Elicit_vs_Final_hand_edits.xlsx` – Comparison of the original Elicit extraction with the final coded table, marking the cells re-coded by hand.
- `DataExtraction_Final.csv`, `DataExtraction_Final_zotero.csv` – Final extraction table of included studies, and matching bibliographic metadata.
- `Master_Coded_Table.csv` – Final coded table used for the synthesis (generated by SLR3).

### Screeners

Screening was done by the lead reviewer and two student screeners. In all files, screeners appear as **Lead Reviewer**, **Student Screener A**, and **Student Screener B**.

## How to reproduce

### 1. Set up the environment

Python 3.13 was used.

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate    macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
```

### 2. Run the notebooks

Start Jupyter from the repository root and run the notebooks **from within the `notebooks/` folder** (paths are resolved relative to the repository root as `Path.cwd().parent`):

```bash
jupyter lab
```

- **SLR2** and **SLR3** run entirely on the data included in this repository.
- **SLR3** regenerates all figures in `results/figures/` and the coded tables.

### 3. Database searches (SLR1, optional)

SLR1 queries live APIs and **cannot be re-run as-is from this package**:

- It requires API keys: copy `.env.example` to `.env` and add your own Web of Science Starter API and Semantic Scholar API keys.
- The retrieved records are included in `data/search/raw_wos/` and `data/search/raw_semanticscholar/`, so section 2 (recall validation) and section 4 (merging and screening) can be re-run from these files without API access.
- Re-running the queries today will return different results because the databases are continuously updated. See the manuscript for the exact search dates and strings.
The notebook outputs saved in SLR1 document the original run.

## Steps that are not computationally reproducible

- **Title/abstract screening** used ASReview (active learning with human labelling). The screening decisions are provided as ASReview exports in `data/screening/`; the ASReview project files are not shared because they embed screener identities.
- **Data extraction** was assisted by Elicit (an LLM-based tool) and validated against human coding. The prompts, raw Elicit outputs, and human–Elicit comparison are provided in `data/extraction/`.

## License

A license will be added upon publication. Until then, these materials are shared for the purpose of peer review.
