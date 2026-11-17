# Reproduction and Inspection Workflow 

# This guide describes **the files actually present** in `CraftAI\\\_.zip`. It distinguishes inspection steps from execution steps that have not been validated end-to-end.

## 1\. Repository orientation

* `code/CraftAI code/`: an implementation using text preprocessing, spaCy-based phrase extraction, Doc2Vec embeddings, BIRCH clustering, and combinational requirement construction.
* `code/CraftAI code/features/` and top-level `\\\*-features.txt`: raw feature descriptions for antivirus, browser, file manager, and file sharing domains.
* `code/CraftAI code/clean/` and `clean-\\\*-features.txt`: preprocessed feature descriptions.
* `code/CraftAI code/boilerplate/`: extracted action/object/detail records.
* `code/CraftAI code/model/`: saved Doc2Vec models.
* `code/CraftAI code/requirement/`: generated requirements and source requirement CSVs.
* Two top-level Excel workbooks: evaluation data; `Survey form.docx`: survey documentation.
* `results/`: five exported CSV summaries/comparisons, not executable analysis scripts.

## 2\. Setup (suggested; not yet environment-tested)

Use a fresh environment with a compatible Python version. From repository root:

```bash
cd 'code/CraftAI code'
python -m venv .venv
# Windows PowerShell: .venv\\\\Scripts\\\\Activate.ps1
# macOS/Linux: source .venv/bin/activate
python -m pip install -r requirements.txt
python -m spacy download en\\\_core\\\_web\\\_sm
python -m nltk.downloader wordnet words
```

**Important:** the supplied `requirements.txt` is UTF-16 encoded and contains historical pinned packages, including `gensim==4.3.2` and spaCy 3.7-era dependencies. Some combinations may not install on modern Python releases. Convert it to UTF-8 and validate versions before promising a one-command installation. `requirements.txt` is not a verified lockfile for the present environment.

## 3\. Processing and generation stages

The scripts use `os.getcwd()` or relative paths; **run them from `code/CraftAI code/`**, not from repository root.

1. **Input inspection**: review `\\\*-features.txt` and the copies under `features/`. Do not assume duplicate copies are synchronized.
2. **Preprocess (optional)**: `python clean\\\_text.py` produces top-level `clean-\\\*-features.txt` from top-level feature files. Existing files are provided; this step overwrites them. It requires NLTK corpora.
3. **Extract phrases (optional)**: `python extract\\\_phrases.py` reads `clean/clean-\\\*-features.txt`, not the top-level cleaned files. It uses spaCy `en\\\_core\\\_web\\\_sm` and writes boilerplate files. Inspect its output paths before running because it may overwrite supplied CSVs. Its function `extract\\\_verbs\\\_nps\\\_pair` refers to the loop variable `line` outside its argument, which is a fragile global dependency.
4. **Doc2Vec training (optional)**: `python train\\\_doc2vec.py` reads top-level cleaned files and saves models in `model/`. Models are already included. Training has no fixed seed and regenerated models may differ. `train\\\_doc2vec2.py` is an alternative, not a required next stage; it uses deprecated `model.init\\\_sims()` and contains `os.path.getcwd()` (invalid), so it should not be used unchanged.
5. **Clustering**: `cluster.py` defines `cluster\\\_requirements(model\\\_path, feature\\\_path, n\\\_clusters)`; it loads Doc2Vec models, infers vectors, applies BIRCH, and returns cluster assignments and silhouette score. It is a library module, not a standalone command.
6. **Requirement generation**: `python requirement\\\_generator.py` attempts to find cluster counts with silhouette scores > 0.3, then asks interactively for a cluster count and selection method. **Do not run unchanged on the supplied artifacts**: it can fail if no cluster passes the threshold (`min(cluster\\\_fit)`), and it constructs `bl` as an already-qualified path but then prefixes `boilerplate/` again in `pd.read\\\_csv`, causing a path error. It also overwrites `requirement/\\\*-requirements.txt`, and random sampling has no fixed seed. Fix these problems and run on a copy before using for reproducibility claims.
7. **Similarity analysis**: `python text\\\_similarity.py` asks for a cosine-similarity cutoff and reads `requirement/browser-requirements.txt` and `clean/clean-browser-features.txt`. Record the cutoff and output location; inspect the remainder of the script before treating its output as a published result.
8. **Exploratory notebooks**: `requirement\\\_analysis.ipynb` and `requirement\\\_generator.ipynb` read seven CSV files under `requirement/`. `requirement\\\_analysis.ipynb` uses `nlp` before loading it in a later cell, so it will not run cleanly top-to-bottom without reordering initialization. Notebook outputs should not be treated as a reproducible pipeline without verification.

## 4\. Inspecting evaluation materials

* `CRAFT-AI\\\_Requirements\\\_Evaluation\\\_Data\\\_ForAnalysis.xlsx`: one worksheet (`Sheet1`, 73 rows × 44 columns).
* `Creativity\\\_Framework\\\_Evaluation\\\_SecondStudyDataset\\\_ForAnalysis.xlsx`: one worksheet (`Sheet 1`, 47 rows × 5 columns).
* `Survey form.docx`: survey documentation.

Inspect these files using Excel, LibreOffice, Python/openpyxl, or R. **No executable script in the archive has been established as the generator of the five CSV result files.** Do not claim that running the code regenerates them.

## 5\. Inspecting reported results

* `results/A1\\\_Individual\\\_Summaries.csv`: per-requirement descriptive summaries.
* `results/A2\\\_Individual\\\_Percentages.csv`: rating category percentages.
* `results/A3\\\_CRAFT\\\_AI\\\_Summaries.csv`: CRAFT-AI summaries with median, quartiles, and confidence interval columns.
* `results/B1\\\_All\\\_27\\\_Paired\\\_Comparisons.csv`: 27 paired comparisons with raw and Holm-adjusted p-value columns.
* `results/B2\\\_Comparison\\\_Categories.csv`: comparison-category counts.

To independently reproduce these tables, the original analysis scripts, input-to-output mapping, statistical test specifications, and any resampling settings must be supplied or documented. Their provenance is **not established by the archive alone**.

## 6\. Reproducibility status

**Verified by static inspection:** file inventory, Python syntax parsing, CSV schemas, major code dependencies and input/output references.

**Not verified:** successful dependency installation, complete execution, deterministic output, correspondence to the manuscript, or regeneration of `results/` CSVs. No external scraping, proxy checking, or API calls were executed.

