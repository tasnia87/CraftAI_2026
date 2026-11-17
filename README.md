# CRAFT-AI: Research Artifacts for Creative Software Requirements Engineering

## Overview

This repository provides supplementary artifacts for an empirical study of creativity in software requirements engineering. It contains implementation resources for automated requirements generation, datasets used for requirements evaluation, the survey instrument, and exported statistical results. The materials are intended to facilitate inspection of the study's data, methods, and reported findings during anonymous peer review.

The accompanying manuscript describes the research questions, evaluation protocol, and interpretation of results.

## Repository structure

```text
CraftAI_/
├── code/
│   └── CraftAI code/
│       ├── features/                # Input feature descriptions
│       ├── clean/                   # Preprocessed feature descriptions
│       ├── boilerplate/             # Extracted phrase records
│       ├── model/                   # Saved Doc2Vec models
│       ├── requirement/             # Requirement datasets and generated text
│       ├── clean_text.py
│       ├── extract_phrases.py
│       ├── train_doc2vec.py
│       ├── cluster.py
│       ├── requirement_generator.py
│       ├── requirement_analysis.ipynb
│       ├── requirement_generator.ipynb
│       └── requirements.txt
├── results/
│   ├── A1_Individual_Summaries.csv
│   ├── A2_Individual_Percentages.csv
│   ├── A3_CRAFT_AI_Summaries.csv
│   ├── B1_All_27_Paired_Comparisons.csv
│   └── B2_Comparison_Categories.csv
├── CRAFT-AI_Requirements_Evaluation_Data_ForAnalysis.xlsx
├── Creativity_Framework_Evaluation_SecondStudyDataset_ForAnalysis.xlsx
├── Survey form.docx
└── README.md
```

The code directory also includes auxiliary scripts, additional input files, and exploratory resources not shown in this abbreviated tree.

## Artifact descriptions

| Artifact | Description |
| --- | --- |
| `code/CraftAI code/` | Python implementation and related resources for preprocessing, phrase extraction, Doc2Vec representation, BIRCH clustering, and combinational requirement generation. |
| `CRAFT-AI_Requirements_Evaluation_Data_ForAnalysis.xlsx` | Requirements evaluation data prepared for analysis. |
| `Creativity_Framework_Evaluation_SecondStudyDataset_ForAnalysis.xlsx` | Dataset for the second evaluation study. |
| `Survey form.docx` | Documentation of the human-evaluation survey instrument. |
| `results/A1_Individual_Summaries.csv` | Individual descriptive rating summaries. |
| `results/A2_Individual_Percentages.csv` | Percentages across rating categories. |
| `results/A3_CRAFT_AI_Summaries.csv` | Aggregate rating summaries. |
| `results/B1_All_27_Paired_Comparisons.csv` | Paired-comparison statistics, including adjusted p-values. |
| `results/B2_Comparison_Categories.csv` | Categorized comparison results. |

## Code organization and processing workflow

The supplied implementation contains the following stages:

1. **Input preparation:** Feature descriptions are provided in `*-features.txt` and `features/`. The `clean_text.py` script performs preprocessing; preprocessed files are also supplied.
2. **Phrase extraction:** `extract_phrases.py` uses spaCy to extract phrase-level information; intermediate records are included in `boilerplate/`.
3. **Text representation:** `train_doc2vec.py` trains Doc2Vec models; saved models are available in `model/`.
4. **Clustering:** `cluster.py` uses BIRCH to group feature representations.
5. **Requirement generation:** `requirement_generator.py` combines extracted information to construct requirement candidates; examples and related datasets are available in `requirement/`.
6. **Evaluation:** The two Excel workbooks, survey document, and CSV files in `results/` support inspection of the human evaluation and reported statistical summaries.

These stages describe the supplied implementation. They should not be interpreted as a verified one-command pipeline for reproducing every result in the manuscript.

## Environment and inspection

The code is written in Python and includes a dependency list at `code/CraftAI code/requirements.txt`. The implementation uses libraries including spaCy, NLTK, Gensim, and scikit-learn.

To inspect the code in a local environment:

```powershell
cd 'code/CraftAI code'
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

Review `requirements.txt` before installing dependencies: the supplied file uses UTF-16 encoding and pins older package versions that may require a compatible Python environment. Once converted to UTF-8 and checked for compatibility, dependencies can be installed using `python -m pip install -r requirements.txt`. The scripts also require language resources such as spaCy's `en_core_web_sm` and relevant NLTK corpora.

**Reproducibility scope:** The repository provides intermediate files, saved models, evaluation datasets, and exported results. Full end-to-end execution and regeneration of the five result CSV files have not been independently validated from the supplied archive. Some scripts require path corrections or other fixes before execution. Researchers can inspect the datasets and reported outputs directly without executing the generation pipeline.

## Evaluation materials and results

The Excel workbooks contain data prepared for empirical analysis. `Survey form.docx` documents the survey instrument. The five CSV files in `results/` provide descriptive and comparison outputs. The original analysis scripts used to export those five CSV files have not been identified in the supplied repository, so the CSVs should be treated as provided results rather than automatically reproducible outputs of the Python generation scripts.

## Citation

For anonymous review, cite the accompanying manuscript according to the conference's artifact-citation instructions. Full bibliographic details can be added after the review process.

## Survey form link:
https://docs.google.com/forms/d/e/1FAIpQLSdIzSQQE7VYuP_X5j8k1WfFNiGqCGF-hNeHpPqsDLQTC_ji_Q/viewform?pli=1


## Anonymity and usage

This repository is intended to be shared through an anonymous review link. The source repository and any author-identifying metadata should not be disclosed to reviewers. Before submission, verify that document properties, notebook outputs, file contents, commit metadata, and external survey links do not reveal author identities.

The presence of artifacts in this repository does not by itself grant a license for unrestricted redistribution. Consult the applicable licenses and permissions for individual components.
