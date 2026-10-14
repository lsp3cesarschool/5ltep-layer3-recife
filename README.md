# 5LTEP-L3 · Recife instance (application in another context)

[![Tests](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/tests.yml/badge.svg)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/tests.yml) [![Layer 3 · traffic](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer3-recife%2Fmain%2Fdocs%2Fdata%2Fstatus-recife-infracoes-transito.json)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/layer3.yml) [![Layer 3 · environment](https://img.shields.io/endpoint?url=https%3A%2F%2Fraw.githubusercontent.com%2Flsp3cesarschool%2F5ltep-layer3-recife%2Fmain%2Fdocs%2Fdata%2Fstatus-recife-autos-infracao-ambiental.json)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/actions/workflows/layer3.yml) [![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

**English** · [Português](LEIAME.md)

**5L-TEP Layer 3 anomaly detection applied to two datasets of the open data portal of the city of
Recife: a third instance of [5ltep-layer3](https://github.com/lsp3cesarschool/5ltep-layer3), set up by
the author as an application of the same system in another context, alongside the
[ANEEL instance](https://github.com/lsp3cesarschool/5ltep-layer3-aneel).**

| Resource | What you find there |
|---|---|
| 📊 **Dashboard** | [lsp3cesarschool.github.io/5ltep-layer3-recife](https://lsp3cesarschool.github.io/5ltep-layer3-recife/?lang=en): anomalies, LLM labels, steward decisions, source checks and the provenance of every result (a *Dataset* selector switches between the two profiles) |
| 🧑‍⚖️ **Review queue** | [![open reviews](https://img.shields.io/github/issues/lsp3cesarschool/5ltep-layer3-recife/layer3?label=open%20reviews&color=0366d6)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/issues?q=is%3Aissue+is%3Aopen+label%3Alayer3) [![pending](https://img.shields.io/github/issues/lsp3cesarschool/5ltep-layer3-recife/review%3Apending?label=pending&color=d73a4a)](https://github.com/lsp3cesarschool/5ltep-layer3-recife/issues?q=is%3Aissue+is%3Aopen+label%3Areview%3Apending)<br>live counts; each badge opens its list of issues |
| 🏛️ **Main instance** | [5ltep-layer3](https://github.com/lsp3cesarschool/5ltep-layer3): IBAMA, and the full documentation |
| 🧪 **Model choice** | [5ltep-layer3-modeltest](https://github.com/lsp3cesarschool/5ltep-layer3-modeltest): the monthly benchmark that picks the LLM judge |
| 🔒 **Security** | [SECURITY.md](SECURITY.md): what is not trusted (the model, the data portal, web sources), how the toolkit contains it, and how to report a vulnerability |

> **Status: research demonstration.** This repository is not operated by, affiliated with or endorsed
> by the City of Recife (Prefeitura do Recife), its traffic authority (CTTU) or its environment
> secretariat; it only reads the city's open data. It shows that the toolkit can be reused on another
> portal. It does not assume that the city will review its results or adopt it. The open review issues
> demonstrate the flow: no steward is assigned.

## Use case in one paragraph

The city of Recife publishes, on its CKAN portal [dados.recife.pe.gov.br](https://dados.recife.pe.gov.br),
every traffic infraction recorded by its traffic authority (CTTU) and the environmental infraction
notices issued by its environment secretariat. Suppose the city, or anyone who reuses these data,
wants to know whether the published record is trustworthy and where to look first if something is
wrong. This layer builds the monthly number of records of each dataset, finds the months that depart
from the usual pattern, and uses a local AI model to check which departures have a known explanation
(a new traffic law, Carnival, a change of administration) and which look like a **data problem** (a
month with no records, a batch entered at once). The unexplained ones go to a person first; the AI only
proposes, and a data steward confirms or corrects every decision that leads to action.

## Why apply the system in another context

This instance demonstrates reuse of the same system in another context and helps assess the limits of that transfer.

A method that works on one dataset may only have learned that dataset's quirks. IBAMA (federal,
environmental, one zip) and ANEEL (federal, electricity, one CSV) are both federal agencies; Recife is
a **municipal** portal, and its two datasets differ from both in how they are published. This
repository runs **the same code**, following the steps another institution would take to adopt it,
and asks the same two questions as the ANEEL instance:

1. **Reuse (the *R* of FAIR):** can the toolkit be adopted by writing a profile, with no code changes?
2. **Transfer:** do detection, the LLM-as-a-Judge and the review protocol behave the same way on a
   very large municipal series (traffic) and on a very short one (environment)?

What differs from the main repository is only:

| File | Why |
|---|---|
| [`profiles/recife-infracoes-transito.json`](profiles/recife-infracoes-transito.json) | traffic infractions: portal, yearly resources, columns, date formats, series, domain text for the LLM |
| [`profiles/recife-autos-infracao-ambiental.json`](profiles/recife-autos-infracao-ambiental.json) | environmental infraction notices, the same way |
| [`profiles/events/`](profiles/events/) | one event calendar per profile, started small and meant to be filled with *Suggest events* and checked by a steward |
| [`profiles/i18n/`](profiles/i18n/) | Portuguese texts of the two profiles for the dashboard |
| this README and its Portuguese version, [`LEIAME.md`](LEIAME.md) | |

The IBAMA profiles and results are not included. Everything else (detectors, LLM-as-a-Judge,
human-in-the-loop review, source checks, dashboard, workflows, tests) is identical to
[`5ltep-layer3@6a6c9ce`](https://github.com/lsp3cesarschool/5ltep-layer3/tree/6a6c9ce354722c1c3acd27a3f6d9b089e2f70a5c),
and the built instance passes the same test suite before every update.

## Key terms

| Term | Meaning here |
|---|---|
| **Anomaly** | a month of a monthly series (number of records) that departs from its usual pattern, flagged by at least 2 of 4 statistical detectors |
| **Level shift** | a lasting change of level (not a one-month spike), detected by a Page-Hinkley test |
| **LLM-as-a-Judge** | a small language model, run locally and free of charge, that reads each anomaly with its context and says which of the four causes below explains it best |
| **Source check** | a problem of the published files themselves (date formats, invalid bytes, publication lag), shown on the dashboard as handled or as a warning |
| **Data steward** | the person who confirms or corrects the judge's label (through a GitHub Issue) before any action |

The four causes (categories) the judge chooses from:

| Code | Category | Example (Recife) | What it means |
|---|---|---|---|
| **PDC** | Policy-Driven Change | infractions change after a Traffic Code amendment or new speed cameras | explained by a known event: document it |
| **SP** | Seasonal Pattern | fewer records every Carnival or year-end holiday | expected behaviour: no action |
| **DQE** | Data-Quality Event | a month with no records in an active series; months entered in a batch | a **data problem**: always reviewed by a steward |
| **GES** | Genuine Enforcement Shift | a lasting change in enforcement intensity with no event and no data signs | a real change nobody has explained yet |

## How Recife's data differ from IBAMA's

| | IBAMA (main study) | Recife: traffic (CTTU) | Recife: environment |
|---|---|---|---|
| Records | ~711,000 notices since 1977 | ~8.5 million infractions, 2006-2025 | ~3,200 notices, 2021-2023 |
| Resource | one zip with a CSV per year | **one resource per year** (20 CSVs, ~2.4 GB) | one resource per year (3 CSVs) |
| Date column | `DAT_HORA_AUTO_INFRACAO` | `datainfracao`, in **three date formats** across the years, one of them day-first (`02/01/2024`) | `DATA DA VISTORIA` |
| Key / value columns | `SEQ_AUTO_INFRACAO` / `VAL_AUTO_INFRACAO` | none / none (count only) | `Nº AUTO` / none (count only) |
| Last published month | the current one | **months behind** (yearly files) | **2023-12** |
| Monthly volume | hundreds to thousands | tens of thousands | about 100 |
| Event calendar | curated, ~14 verified events | 9 verified events, to be extended | 2 verified events, to be extended |
| Licence of the data | IBAMA open data | ODbL | ODbL |

## How it works

The pipeline is the one documented in the [main README](https://github.com/lsp3cesarschool/5ltep-layer3#readme);
in short:

```
┌────────────────────────────────────────────────────────────┐
│  GitHub Actions: monthly cron + "Run workflow" button      │
│  (public repo runner: 4 vCPU / 16 GB, free of charge)      │
└─────────────────────────────┬──────────────────────────────┘
                              ▼
 ① Source     CKAN API of dados.recife.pe.gov.br → every yearly CSV (SHA-256 of each recorded)
 ② Check      source checks: date formats, invalid bytes, publication lag (shown on the dashboard)
 ③ Aggregate  only the date and identifier columns (no names, addresses or CPF/CNPJ kept)
 ④ Detect     Z-score · MAD · Isolation Forest · LSTM-ED (vote ≥ 2) + Page-Hinkley
 ⑤ Judge      Ollama + the model approved by the benchmark, 3 seeded runs, JSON answer
 ⑥ Review     GitHub Issues: steward:<CATEGORY> label + close (review:pending for DQE)
 ⑦ Report     layer3_summary.json (l3_rate, l3_pass, anomaly_flags) + dashboard
                              ▼
       Git history of every series, detection, judgment and review
```

- **One file per year:** the profiles name the resources by a pattern (`resource_pattern`), so the file
  of a new year is picked up by itself; each file is read and deleted before the next is downloaded.
- **The series ends at the last published month** (`period.end: "last_record"`): the months after it
  are not published yet, and counting them as zero would look like a collapse.
- **Batches:** each run judges up to 25 anomalies, commits, and starts the next batch until nothing is
  pending.
- **No re-judging by default:** each judgment stores a fingerprint of its own data and the model and
  prompt version that produced it. It is judged again only if its data change, or if a steward asks.
- **Model:** `LLM_MODEL=auto` (default) uses the model the
  [benchmark](https://github.com/lsp3cesarschool/5ltep-layer3-modeltest) currently approves.
- **Event calendar:** *Suggest events* asks the LLM for events grounded on the Wikipedia pages
  "*year* no Brasil" and opens a pull request; suggestions are marked `suggested` until a steward
  verifies them.

## What applying the system in another context showed

*First run on GitHub Actions, 09/10/2026; a snapshot for the record, the dashboard always shows the
current state.*

**Traffic.** 229 months analysed (2006-05 to 2025-05); 12 anomalous months and 7 Page-Hinkley alarms
(sustained level shifts). The judge (`qwen3:4b`, the benchmark's choice) labelled them GES 5, DQE 4,
SP 2, PDC 1. Three of the four DQE are at the very start of the series (2006-06, 2006-10, 2006-11),
where the 2006 file is small (2 MB against 40 to 200 MB in the other years). The first month of the
pandemic, 2020-04, was labelled seasonal with the three runs disagreeing (consistency 0.33), and the
next month, 2020-05, policy-driven: the kind of decision the design leaves to a steward.

**Environment.** 36 months (2021-01 to 2023-12); one anomalous month, 2022-12, labelled DQE.

**Review.** 11 review issues are open (5 of them `review:pending`, for the DQE labels), with no
steward assigned, by design. The labels above are the model's proposals, not findings.

**Source checks.** Only *handled* items on both datasets: the date formats of the traffic files and
the publication lag of both (no record after 2025-05 and 2023-12).

What setting the instance up showed:

- **Reuse needed three new profile fields, in the shared code.** The Recife portal publishes both
  datasets as one resource per year, with names that are not even consistent ("Registro **das**
  infrações", "Registro **de** Infrações", "Registro **da** infrações"). That led to `resource_pattern`
  (a regular expression for the yearly resources), `date_formats` (the traffic files use
  `YYYY-MM-DD`, `DD/MM/YYYY hh:mm` and `YYYY/MM/DD`, with and without the time) and
  `period.end: "last_record"`. A fourth, `encoding_errors`, was added when a sample of the 2014 file
  looked invalid in UTF-8; the encoding check later showed the whole file is valid (the sample had cut
  a character in half), so this instance does not use it. All of them went into the main repository,
  so IBAMA and ANEEL run the same code.
- **Problems in the files are now reported, not only worked around.** The
  [source checks](https://github.com/lsp3cesarschool/5ltep-layer3#source-checks) were written for this
  instance, and on their first run they caught a fourth date shape in one yearly file (2018) that
  the profile had missed. Without them, a day-first file read month-first would have moved records
  between months, and the missing months after the last yearly file would have looked like a
  collapse, both silently.
- **The environmental series sits at the minimum:** 36 months, the least the detectors accept. It
  tests how far the toolkit can be pushed, not how well it does on a comfortable series.

## Running it, and adapting it again

| Workflow | When | What |
|---|---|---|
| `layer3.yml` | 5th of every month, and manual (dashboard *Run Layer 3 now*, *Re-judge*) | detect → judge in batches → review issues → report, for both profiles |
| `reviews.yml` | when a `layer3` issue is labelled or closed | records steward decisions, refreshes the dashboard |
| `events.yml` | manual (dashboard *Suggest events*) | LLM event suggestions → pull request |
| `model-check.yml` | 22nd of every month | only if the model is pinned: proposes the benchmark's choice |
| `tests.yml` | push / pull request | test suite |

This instance is itself a recipe for another: fork the main repository, replace the profiles with your
own (`python main.py check-profile --profile profiles/<yours>.json` validates one against the live
portal and runs the source checks), mark it `"scheduled": true`, delete `data/`, `results/` and
`docs/data/`, enable Actions and Pages, and run the workflow once. The main README details each step
and every profile field.

## Reproducibility

Every run records the SHA-256 of each file analysed (and one over all of them) and of the profile,
the source checks, every method parameter, the Python and package versions, and for each judgment the
full prompt, the three answers with their seeds, the model digest and the prompt version. The monthly
series are versioned, so detection can be re-run on exactly the data of any past commit with
`python main.py detect --from-series`, even though the portal's files change over time.

## Limitations

- The traffic series counts infractions, not notices or fines: the files have no notice number and no
  value, so duplicates cannot be detected and no value series can be built.
- The environmental series is short (36 months) and stops in 2023: seasonality and the LSTM-ED have
  little to learn from.
- Both datasets are published months to years late, so Layer 3 cannot see recent months; the lag
  itself is reported as a source check.
- The event calendars are small: most anomalies are judged without the context a local expert would
  bring.
- No steward decisions, by design: the review flow is working and ready to be adopted, and the review
  metrics fill in automatically if and when a steward uses it. The author does not act as a steward,
  which would be self-evaluation.
- The [size and time limits](https://github.com/lsp3cesarschool/5ltep-layer3#size-and-time-limits) of
  the main instance apply.

## Documentation and references

Method, parameters, human-in-the-loop protocol, FAIR and replicability notes, and how to adapt the
toolkit: **<https://github.com/lsp3cesarschool/5ltep-layer3#readme>**. Model choice:
**<https://github.com/lsp3cesarschool/5ltep-layer3-modeltest>**.

- Pinheiro, L. S., et al. (2026). *Towards Trust Engineering in Open Data Systems: A Layered Conceptual Framework Integrating Quality Assurance and Governance Perspectives*. SOFTENG 2026, IARIA, pp. 21–28.
- Prefeitura do Recife. *Registro das Infrações de Trânsito*. Dados Recife. <https://dados.recife.pe.gov.br/dataset/registro-das-infracoes-de-transito>
- Prefeitura do Recife. *Autos de infração ambiental*. Dados Recife. <https://dados.recife.pe.gov.br/dataset/autos-de-infracao-ambiental>

## License

Code: MIT, see [LICENSE](LICENSE). The monthly series in `data/` are aggregates derived from the open
data of the city of Recife (ODbL); when reusing them, cite the Dados Recife portal as the original
source.
