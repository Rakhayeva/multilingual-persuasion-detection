# Multilingual Persuasion Detection
### English · Kazakh · Russian

A cross-lingual NLP study on detecting political manipulation in text,
with explainability analysis and documentation of the Central Asian
NLP resource gap.

## Motivation

The rapid proliferation of AI use raises concerns about its capacity
to generate content that influences political views through
sophisticated training techniques. Recent empirical research finds
that AI-generated persuasive content can change political views
significantly within a short period of time, and that post-training
techniques boost persuasive power by up to 51% (Hackenburg et al.,
2025). Moreover, the same mechanisms that make AI more persuasive
also lead it to generate less accurate information, which raises
concerns about targeted political manipulation at scale.

Consequently, an increasing volume of content, including news
articles and social media posts on political topics, incorporates
persuasive techniques (Da San Martino et al., 2020). Although NLP
studies on political persuasion detection have demonstrated promising
results (Bassi et al., 2024; Da San Martino et al., 2020), such
research remains predominantly focused on English. Cross-lingual
transfer of such models can produce insufficient results not only
due to language-specific differences but also due to the limited
availability of annotated data in other languages.

Therefore, this project investigates whether models for persuasion
technique detection trained on English political text can transfer
to Kazakh and Russian, and documents what happens when annotated
resources do not exist for targeted languages relevant for
Central Asia.

## Research Hypotheses

- **H1 (Cross-lingual gap):** A model trained on English persuasion
  data will show significantly lower F1 on Kazakh and Russian texts.
- **H2 (Feature analysis):** SHAP analysis will reveal which
  linguistic markers drive manipulation detection in English
  political text, providing a baseline for future multilingual
  comparison.
- **H3 (Resource gap):** A systematic survey of available corpora
  will confirm the absence of open persuasion-annotated datasets
  for Kazakh and Russian, directly motivating future thesis work.

## Datasets

| Source | Language | Task | Size | Access |
|--------|----------|------|------|--------|
| [PTC / SemEval-2020 Task 11](https://zenodo.org/records/3952415) | EN | Persuasion techniques in political news | 1147 paragraphs | Open |
| [baiangali/fake_news](https://github.com/baiangali/fake_news) | KZ | Fake/real news (proxy) | 101 texts | Open |
| [baiangali/fake_news](https://github.com/baiangali/fake_news) | RU | Fake/real news (proxy) | 103 texts | Open |

**Note on proxy datasets:** No open corpus with persuasion technique
annotation exists for Kazakh or Russian. The baiangali dataset is
used as a proxy task to study cross-lingual transfer, with fake/real
news labels serving as a proxy for manipulative content. This design
choice and its limitations are documented in notebooks 03 and 07.
SemEval-2023 Task 3, which includes Russian political news with
persuasion annotation, requires institutional affiliation for access,
confirming the resource barrier documented in this project.

## Project Structure
notebooks/
├── 01_data_loading.ipynb         # Data acquisition and parsing
├── 02_eda.ipynb                  # Exploratory data analysis
├── 03_preprocessing.ipynb       # Text cleaning and train/val/test split
├── 04_baseline_models.ipynb     # TF-IDF + Logistic Regression / SVM
├── 05_transformer_experiments.ipynb  # XLM-RoBERTa fine-tuning
├── 06_shap_explainability.ipynb  # SHAP analysis (H2)
└── 07_resource_gap.ipynb        # Resource gap documentation (H3)

## Results

### Baseline Models (TF-IDF + LR / SVM)

| Model | Train | Test | F1-macro | Hypothesis |
|-------|-------|------|----------|------------|
| LR (TF-IDF) | EN | EN | 0.761 | Baseline |
| SVM (TF-IDF) | EN | EN | 0.759 | Baseline |
| LR (TF-IDF) | EN | KZ proxy | 0.336 | H1 |
| SVM (TF-IDF) | EN | KZ proxy | 0.336 | H1 |
| LR (TF-IDF) | EN | RU proxy | 0.313 | H1 |
| SVM (TF-IDF) | EN | RU proxy | 0.313 | H1 |

### Transformer (XLM-RoBERTa)

| Model | Train | Test | F1-macro | Hypothesis |
|-------|-------|------|----------|------------|
| XLM-R | EN | EN | 0.774 | Baseline |
| XLM-R (zero-shot) | EN | KZ proxy | 0.331 | H1 |
| XLM-R (zero-shot) | EN | RU proxy | 0.322 | H1 |

**H1 confirmed:** F1-macro drops from 0.76-0.77 (in-language) to
0.31-0.34 (cross-lingual). Both classical and transformer models
predict only the majority class on KZ and RU test sets, confirming
complete transfer failure.

## SHAP Analysis: Manipulation Markers in English (H2)

![SHAP Top-20](https://drive.google.com/uc?export=view&id=ТВОЙ_FILE_ID](https://drive.google.com/file/d/1cHeIIy9E7c7570bU2Px6oboLYHRDHTqW/view?usp=sharing)

SHAP analysis of the LR model addresses the explainability challenge
identified by Bassi et al. (2024) as central to responsible
persuasion detection systems. No single word in the top-20 is a
reliable standalone marker of manipulation. The most interpretable
finding is that "our" correlates with group identity framing in
manipulative paragraphs, while "trump" correlates with neutral
factual reporting. Signed SHAP values are small (order of 0.001),
confirming that the model relies on combinations of weak signals
rather than strong individual markers (Da San Martino et al., 2020).

A parallel SHAP analysis for Kazakh and Russian is not possible at
this stage due to the absence of annotated persuasion corpora for
these languages, which is documented as part of Finding 3.

## Key Finding: Resource Gap for Central Asian NLP (H3)

A systematic survey of 11 datasets confirmed that the need for
multilingual persuasion detection, identified as a key future
challenge by Bassi et al. (2024), remains unaddressed for
Central Asian languages:

- **Kazakh:** No persuasion-annotated corpus exists in open access.
  Available resources cover sentiment, NER, and machine translation,
  but not persuasion technique detection.
- **Russian:** Two persuasion-annotated corpora exist (SemEval-2023
  Task 3, SlavicNLP 2025) but both require institutional affiliation
  for access. Open Russian datasets cover related tasks with
  channel-level labels, not span-level persuasion annotation.

This gap is not a limitation of this project. It is one of its
findings, and a primary motivation for the planned MSc thesis.
The full survey is available in `results/resource_gap_survey.csv`
and documented in `07_resource_gap.ipynb`.

## Connection to Research

This project is a preliminary empirical study for the MSc thesis
proposal:

> *"Reliability and Interpretability of NLP Models for Detecting
> Political Manipulation in Multilingual Contexts:
> A Central Asian Perspective"*

## How to Reproduce

```bash
git clone https://github.com/Rakhayeva/multilingual-persuasion-detection
```

Open notebooks in Google Colab in order: 01 → 02 → 03 → 04 → 05 → 06 → 07.

**Note:** Full reproduction requires downloading the following
datasets manually and placing them in your Google Drive:

- PTC / SemEval-2020 Task 11: [Zenodo](https://zenodo.org/records/3952415)
  — download `datasets-v2.tgz` and extract to `data/raw/`
- baiangali/fake_news: [GitHub](https://github.com/baiangali/fake_news)
  — clone or download `fake_real_news_annotated.json` to `data/raw/`

After downloading, update the `BASE` variable in each notebook to
point to your own Google Drive folder. Then run notebooks 01 and 03
to generate all processed datasets before running subsequent notebooks.

## References

Bassi, D., Fomsgaard, S., & Pereira-Farina, M. (2024). Decoding
persuasion: a survey on ML and NLP methods for the study of online
persuasion. Frontiers in Communication.
https://doi.org/10.3389/fcomm.2024.1457433

Da San Martino, G., Barron-Cedeno, A., Wachsmuth, H., Petrov, R.,
& Nakov, P. (2020). SemEval-2020 Task 11: Detection of propaganda
techniques in news articles. Proceedings of the 14th International
Workshop on Semantic Evaluation (SemEval 2020).
https://doi.org/10.18653/v1/2020.semeval-1.186

Hackenburg, K., Tappin, B. M., Hewitt, L., Saunders, E., Black, S.,
Lin, H., Fist, C., Margetts, H., Rand, D. G., & Summerfield, C.
(2025). The levers of political persuasion with conversational
artificial intelligence. Science.

Sambetbayeva, M., Nekessova, A., Yerimbetova, A., Bayangali, A.,
Kaldarova, M., Telman, D., & Smailov, N. (2025). A multi-level
annotation model for fake news detection: Implementing Kazakh-Russian
corpus via Label Studio. Big Data and Cognitive Computing, 9(8), 215.
https://doi.org/10.3390/bdcc9080215
