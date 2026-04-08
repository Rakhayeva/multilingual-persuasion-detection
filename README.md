# Multilingual Persuasion Detection
### English · Kazakh · Russian

A cross-lingual NLP study on detecting political manipulation in text,
with explainability analysis and documentation of the Central Asian
NLP resource gap.

## Motivation
Most persuasion detection research is English-only. This project
investigates whether models trained on English political text can
transfer to Kazakh and Russian and documents what happens when
annotated resources do not exist for a target language.

## Research Hypotheses
- **H1 (Cross-lingual gap):** A model trained on English persuasion
  data will show significantly lower F1 on Kazakh and Russian texts.
- **H2 (Feature analysis):** SHAP analysis will reveal which linguistic
  markers drive manipulation detection in English political text.
- **H3 (Resource gap):** A systematic survey of available corpora will
  confirm the absence of open persuasion-annotated datasets for Kazakh
  and Russian directly motivating future thesis work.

## Datasets

| Source | Language | Task | Size | Access |
|--------|----------|------|------|--------|
| [PTC / SemEval-2020 Task 11](https://zenodo.org/records/3952415) | EN | Persuasion techniques in political news articles | 1147 paragraphs | Open |
| [baiangali/fake_news](https://github.com/baiangali/fake_news) | KZ | Fake/real news *(proxy)* | 101 texts | Open |
| [baiangali/fake_news](https://github.com/baiangali/fake_news) | RU | Fake/real news *(proxy)* | 103 texts | Open |

**Note on Kazakh and Russian data:** No open corpus with persuasion
technique annotation exists for either language. The baiangali dataset
is used as a proxy task (fake/real news) to study cross-lingual
transfer. SemEval-2023 Task 3, which includes Russian political news
with persuasion annotation, requires institutional affiliation for
access, confirming the resource barrier documented in this project.

## Project Structure
```
notebooks/
├── 01_data_loading.ipynb     # Data acquisition and parsing
├── 02_eda.ipynb              # Exploratory data analysis
├── 03_preprocessing.ipynb   # Text cleaning and train/val/test split
├── 04_baseline_models.ipynb # TF-IDF + Logistic Regression / SVM
├── 05_transformer.ipynb     # XLM-RoBERTa fine-tuning
├── 06_shap.ipynb            # Explainability analysis (SHAP)
└── 07_resource_gap.ipynb    # Resource gap documentation
```

## Results

| Model | Train | Test | F1-macro | Hypothesis |
|-------|-------|------|----------|------------|
| LR (TF-IDF) | EN | EN | — | Baseline |
| LR (TF-IDF) | EN | KZ proxy | — | H1 |
| LR (TF-IDF) | EN | RU proxy | — | H1 |
| XLM-R | EN | EN | — | Baseline |
| XLM-R | EN | KZ proxy | — | H1 |
| XLM-R | EN | RU proxy | — | H1 |

*Results will be updated as experiments are completed.*

## Key Finding: Resource Gap for Central Asian NLP

A systematic search for open persuasion-annotated corpora revealed:

- **Kazakh:** No persuasion-annotated corpus exists in open access.
- **Russian:** The only known corpus (SemEval-2023 Task 3) requires
  institutional verification for access.

This gap is not a limitation of this project — it is one of its
findings, and a primary motivation for the planned MSc thesis.

## SHAP Analysis
*To be updated after Step 6.*

## Connection to Research
This project is a preliminary empirical study for the MSc thesis proposal:

> *"Reliability and Interpretability of NLP Models for Detecting
> Political Manipulation in Multilingual Contexts:
> A Central Asian Perspective"*
>

## How to Reproduce
```bash
git clone https://github.com/YOUR_NAME/multilingual-persuasion-detection
# Open notebooks in Google Colab
# Mount Google Drive and set BASE path
# Run notebooks in order: 01 → 02 → 03 → 04 → 05 → 06 → 07
```

## Citation
- Da San Martino et al. (2020). SemEval-2020 Task 11. 
- Sambetbayeva et al. (2025). baiangali/fake_news corpus.
