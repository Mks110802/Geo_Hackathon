# Geo_Hackathon — 2025 SPE Europe Energy GeoHackathon

**Event:** 2025 SPE Europe Energy GeoHackathon

**Team members:**

* Mohit Singh
* Rudradip Khanra
* Shreyansh Gupta
* Md Arhaan Ahmad
* Thai Phi
* Muhammad Sidqi

---

## Project overview

This repository implements an **agentic AI workflow** for automatically extracting structured data and key parameters from well reports and technical documents, validating the extracted data, and connecting that information to a wellbore flow model to perform nodal analysis and estimate well production rates. The project focuses on retrieval-augmented generation (RAG), agent orchestration, sanity checking of inputs, and optional vision-based extraction from images.

This work was prepared for the **2025 SPE Europe Energy GeoHackathon** under the challenge: *"Build an agentic AI workflow that retrieves relevant information from well reports and documents, connects to a wellbore flow model to perform nodal analysis calculations, and returns potential production estimates — including validation and user interaction for missing or ambiguous inputs."*

---

## Problem statement (official)

> Well reports and technical documents contain valuable information, such as well trajectory, well completion diagram, together with PVT report (fluid properties) and equipment specs that can be used for critical tasks like well integrity assessment or modelling wellbore performance for production and injection assessment.
>
> The challenge is to build and develop an agentic AI workflow that can automatically retrieve relevant information from well reports and documents, connecting to a wellbore flow model to perform nodal analysis calculations and estimate the potential production rates from the wells. The agent should have the capability to validate and perform sanity-checking of the extracted data (e.g. check measure depth > true vertical depth, realistic tubing and casing diameters) and interact with the user in terms of missing or ambiguous inputs.
>
> The challenge is divided into a number of sub-challenges of increasing difficulty. Each sub-challenge is scored separately from the others such you can still score points on one sub-challenge even if the results from the previous sub-challenge were not satisfactory. In addition, extra points can be earned by completing an optional bonus sub-challenges at the end.

---

## Sub-challenges (scoped for this repository)

1. **RAG summarisation** — Build a retrieval-augmented generation (RAG) workflow that can create an accurate and complete summary of a completion report within a given word limit based on a user prompt. Outputs should be concise and cite or reference the source locations in the source documents.

2. **Data extraction for nodal inputs** — Using the same RAG workflow, accurately retrieve the parameters and data required to estimate wellbore production capacity by performing nodal analysis calculations. Inputs include values found in tables and free text (e.g., tubing/casing sizes, depths, choke size, reservoir pressure, PVT properties, relative permeability or viscosity, porosity/permeability where available).

3. **Agentic orchestration + calculation** — Connect the RAG system to an agentic workflow that can automatically extract the parameters (sub-challenge 2), run nodal analysis (call the wellbore model), validate results with sanity checks, and return an accurate, explainable, and user-friendly response including the calculated production rates and any assumptions made.

**Bonus challenge:** Use a vision model (OCR + layout/vision transformer) to extract nodal analysis inputs directly from images instead of structured tables or text.

---

## What this repo contains (summary)

* **Agent components** — scripts to orchestrate or drive agents that query documents, ask clarifying questions, and manage the extraction pipeline.
* **RAG index build** — utilities to build document indices (embeddings, vector store) and retrieval components used by the RAG summariser and extractor.
* **Nodal analysis module** — code that implements or wraps a wellbore flow model for nodal calculations (inference or numeric solver). The module accepts the extracted parameters, performs calculations, and returns production rate estimates.
* **Sanity checks & validation** — helper functions that validate extracted values, enforce physical plausibility (e.g., MD ≥ TVD, tubing inner diameters within expected ranges), and create user prompts for missing or ambiguous inputs.
* **Multimodal app / demo** — a lightweight app or notebook demonstrating how to upload well reports (PDFs / images), run the RAG extraction, and show nodal results (PDF/plots).
* **Sample outputs** — example `nodal_results_*.pdf` files and sample plots to demonstrate expected outputs.

> If any of the above descriptions do not match the exact file contents, we can refine the README after you paste key files or grant file contents access.

---

## Quickstart — run locally (recommended)

1. Clone the repository

```bash
git clone https://github.com/Mks110802/Geo_Hackathon.git
cd Geo_Hackathon
```

2. Create & activate a virtual environment (Python 3.9+ recommended)

```bash
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows (PowerShell)
# Run the activation script in PowerShell: .venv/Scripts/Activate.ps1
```

3. Install dependencies

```bash
pip install --upgrade pip
pip install -r requirements.txt
```

4. Build the RAG index (if provided)

```bash
python prebuild_index.py
# or run the index builder script present in the repo; this will create the vector store used for retrieval
```

5. Run the main agent/workflow

```bash
python run.py
# or
python app_multimodal.py
# For Streamlit-based demos: streamlit run app_multimodal.py
```

Notes: the app or scripts may require environment variables or API keys (for LLM or embedding services). Check the repository for a `.env.example` or configuration instructions.

---

## How the pipeline is expected to work (high level)

1. **Document ingestion** — PDF, DOCX, or images are ingested and passed through OCR (for images) and simple text extraction for PDFs.
2. **Indexing & retrieval** — extracted texts are chunked and embedded into a vector store for retrieval during RAG summarisation and parameter extraction.
3. **RAG summariser** — the retrieval-augmented generator constructs short, accurate summaries on demand and extracts candidate parameter values.
4. **Agentic extraction** — an agent inspects retrieved passages, applies pattern matching (tables, regex) and heuristics to extract numeric inputs needed for nodal analysis.
5. **Sanity-checking & user interaction** — a validation layer flags suspicious values and engages the user via prompts to confirm or provide missing values.
6. **Nodal calculation** — the wellbore flow model is executed with the final validated parameters and returns production rate estimates, pressure drop profiles, and sensitivity notes.
7. **Report generation** — results and diagnostics are packaged into a readable report (PDF) and plots.

---

## Evaluation & scoring (how to present results for the hackathon)

* Provide a sample test set: 4–6 representative well reports (mixture of tables, scanned PDFs, and images).
* For each sub-challenge, produce a short evaluation script that:

  * Compares the extracted parameters with a ground-truth spreadsheet and reports precision/recall for fields.
  * Runs the nodal solver and compares the reported production estimate to a reference.
  * Logs sanitize/validation flags and the agent’s clarifying question transcript.
* For the bonus vision challenge, include OCR accuracy and table-extraction performance metrics.

---

## Suggested additions (priority list)

1. **Add `examples/`** with: sample input reports, expected ground-truth CSV, and a demo script that runs end-to-end. This makes judging easier.
2. **Add a config / .env.example** for API keys (LLM provider, embeddings, OCR provider).
3. **Add unit tests** for the extraction rules and nodal solver modules.
4. **Add clear CONTRIBUTING.md** and LICENSE (MIT recommended if you want an open permissive license).
5. **Add a short video or GIF demo** showing the app extracting inputs from a PDF and producing a nodal results PDF.

---

## Contributing

Contributions are welcome. Typical workflow:

1. Fork the repository.
2. Create a feature branch (`feature/readme-improvements`).
3. Add code or documentation and tests.
4. Open a PR describing the changes and how to run the demo.
