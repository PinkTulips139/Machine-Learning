# IMDb Sentiment Analysis

## Project Status

**Completed research project / archival repository**

This project compares traditional machine-learning baselines and a compact Transformer model for binary sentiment classification on the IMDb Large Movie Review Dataset. It was designed to study not only predictive performance, but also the trade-off between model effectiveness, computational cost, data scale, and error characteristics.

The project is no longer under active development. The repository preserves the completed notebook, result files, figures, and research notes. Proposed improvements listed below were **not implemented unless explicitly stated**.

---

## Research Questions

The completed study investigated:

1. How accurately do traditional machine-learning models and DistilBERT classify IMDb reviews?
2. How much training and inference time does each model require?
3. How does performance change when only 20%, 50%, or 100% of the training data is used?
4. What types of reviews appear difficult for the evaluated models?

---

## Dataset

The project uses the **IMDb Large Movie Review Dataset** for binary sentiment classification.

- 25,000 training reviews
- 25,000 test reviews
- Balanced positive and negative labels
- Document-level movie reviews containing long-form opinions, sentiment shifts, mixed evaluation, and contextual language

The original dataset is not redistributed in this repository. The dataset can be obtained from the official [IMDb Large Movie Review Dataset](https://ai.stanford.edu/~amaas/data/sentiment/) page. Before running the notebook, please download the dataset locally and update the dataset path accordingly.

---

## Models

### 1. TF-IDF + Multinomial Naive Bayes

A lightweight probabilistic text-classification baseline.

### 2. TF-IDF + Logistic Regression

A stronger sparse linear baseline with a favorable performance-efficiency balance.

### 3. DistilBERT

A compact Transformer model fine-tuned for sentiment classification.

Key DistilBERT settings used in the completed experiment:

- checkpoint: `distilbert-base-uncased`
- epochs: 2
- batch size: 16
- learning rate: `2e-5`
- weight decay: `0.01`
- maximum sequence length: 256
- checkpoint selection: best validation F1

The traditional models used lowercased and cleaned text with TF-IDF unigram and bigram features, a maximum vocabulary size of 20,000, English stop-word removal, `min_df=2`, and `max_df=0.95`.

---

## Main Results

### Full Training-Data Setting

| Model | Accuracy | Precision | Recall | F1 | Training Time (s) | Inference Time (s) |
|---|---:|---:|---:|---:|---:|---:|
| TF-IDF + Naive Bayes | 0.849840 | 0.864842 | 0.829280 | 0.846688 | 9.649062 | 4.096136 |
| TF-IDF + Logistic Regression | 0.882840 | 0.879952 | 0.886640 | 0.883284 | 10.305945 | 4.292568 |
| DistilBERT | 0.913720 | 0.906341 | 0.922800 | 0.914496 | 1810.676478 | 219.141574 |

DistilBERT achieved the highest F1 score, while Logistic Regression remained the strongest efficient baseline. Relative to Logistic Regression, DistilBERT improved F1 by roughly 3.12 percentage points but required substantially more training and inference time.

> Timing results are hardware-dependent wall-clock measurements. The traditional baselines ran on CPU, while DistilBERT used CUDA acceleration. These values should not be treated as hardware-neutral benchmarks.

### F1 under Different Training-Data Sizes

| Training Data | Naive Bayes | Logistic Regression | DistilBERT |
|---|---:|---:|---:|
| 20% | 0.830359 | 0.857255 | 0.897070 |
| 50% | 0.840883 | 0.872116 | 0.905369 |
| 100% | 0.846688 | 0.883284 | 0.914496 |

DistilBERT remained the best-performing model at all three training-data ratios.

---

## Error-Pattern Analysis

Among the 25,000 test reviews, **5,465 reviews were misclassified by at least one model**.

The completed study grouped these reviews by model-level error pattern, such as:

- Naive Bayes wrong, stronger models correct
- traditional models wrong, DistilBERT correct
- traditional models correct, DistilBERT wrong
- all models wrong
- Logistic Regression wrong, others correct
- only Logistic Regression correct
- only Naive Bayes correct

This analysis showed that DistilBERT was not uniformly superior for every review and that the models exhibited potentially complementary error patterns.

---

## Important Limitation: Representative Error Samples

Two representative-sample files were created for qualitative inspection:

- `Representative_error_examples_by_pattern.csv`
- `Representative_error_examples_with_categories.csv`

These files must be interpreted carefully.

### What they are useful for

- manually reading representative mistakes;
- exploring possible linguistic phenomena;
- developing preliminary annotation ideas;
- selecting qualitative case studies;
- checking how different model error patterns look in practice.

### What they are **not** suitable for

- estimating the natural distribution of error categories in all 5,465 misclassified reviews;
- proving that one error category accounts for a fixed percentage of the full error pool;
- serving as final human-annotated ground truth;
- directly training a difficulty predictor or routing system;
- supporting population-level statistical claims.

The representative set contained 210 examples selected in a balanced inspection design rather than a natural-distribution sample. The resulting category percentages therefore describe only that sampled set.

---

## Important Limitation: Preliminary Error Categories

The original error categories were assigned with keyword-based rules and were explicitly exploratory.

Examples included:

- negation / rhetorical framing;
- genre-specific vocabulary;
- long review / truncation risk;
- ambiguous or mixed sentiment;
- weak or implicit sentiment;
- sentiment shift / contrast;
- other context-dependent error.

These labels have several limitations:

1. **Keyword presence is not the same as error causation.** A review containing words such as `dark`, `violent`, `not`, or `however` does not prove that those words caused the model error.
2. **Some categories combine different mechanisms.** Negation and rhetorical or sarcasm-like framing should not automatically be treated as the same phenomenon.
3. **The system forced a single category.** A review may simultaneously contain negation, contrast, mixed sentiment, genre-specific vocabulary, and long-context risk.
4. **Rule priority can hide secondary phenomena.** The first matched rule may determine the final label even when another phenomenon is more relevant.
5. **Some lexical rules were overly broad.** Words such as `while` or `though` do not always indicate sentiment contrast.
6. **No formal annotation-reliability study was conducted.** There was no independent second annotator, Cohen's kappa, or systematic self-consistency check.
7. **The full error pool was not manually categorized.** Most misclassified reviews did not receive a reliable category label.

For these reasons, the following legacy files should be treated as **historical exploratory outputs**, not final taxonomy evidence:

- `Error_category_summary_table.csv`
- `Error_category_summary_bar_chart.png`
- `Representative_error_examples_with_categories.csv`

---

## Additional Project Limitations

### 1. Single-run dependence

The error pool was created from one completed set of model runs. A review that was misclassified once may not be consistently difficult under different random seeds or retraining runs.

### 2. Single dataset and single domain

The study used only IMDb movie reviews. Results may not generalize to product reviews, social media, news comments, or other domains.

### 3. Limited model coverage

The experiment included Naive Bayes, Logistic Regression, and DistilBERT, but not stronger encoders, lighter Transformers, or large language models.

### 4. Long-text claims were not fully validated

DistilBERT used a maximum length of 256 tokens, but the project did not systematically compare 128, 256, and 512 tokens or verify whether decisive sentiment information appeared after the truncation point.

### 5. Limited uncertainty information

The archived outputs do not provide a complete analysis of probability margins, entropy, calibration, or prediction confidence.

### 6. No formal statistical significance testing

The project did not include multi-seed mean ± standard deviation, bootstrap confidence intervals, or McNemar tests.

### 7. Hardware-dependent efficiency comparison

Traditional models and DistilBERT were evaluated on different compute devices, so the timing comparison is practical but not hardware-neutral.

---

## Feasible Improvements

### A. Rebuild the Diagnostic Dataset

- sample from the full error pool using explicit strata;
- preserve a stable `review_id`;
- remove exact and near duplicates;
- include 100–200 correctly classified control examples;
- report both the natural population distribution and the diagnostic-sample distribution;
- use sampling weights if estimating full-pool proportions.

### B. Redesign the Error Taxonomy

Build a literature-supported annotation guideline with:

- `primary_error_type`
- `secondary_error_type`
- structural-risk flags
- `possible_label_noise`
- `annotator_confidence`
- `annotation_note`

Keyword rules should only assist candidate selection, not define final labels.

### C. Validate Annotation Reliability

Possible designs:

- two independent annotators and Cohen's kappa;
- partial second annotation;
- delayed self-consistency re-annotation;
- supervisor audit of boundary cases.

### D. Repeat Training across Seeds

For each test example, save:

- prediction for every run;
- number of wrong predictions;
- error rate;
- prediction flip count;
- model agreement rate.

This would distinguish accidental mistakes from persistent failures.

### E. Add Confidence and Calibration Analysis

Store and evaluate:

- class probabilities;
- prediction margin;
- entropy;
- calibration error;
- reliability diagrams.

### F. Expand the Model Set by Research Role

- MiniLM or TinyBERT for efficient Transformer inference;
- RoBERTa for a stronger encoder baseline;
- LLM zero-shot or few-shot prediction on a diagnostic subset;
- long-context models for truncation-sensitive cases.

### G. Run Long-Text Ablations

Compare:

- maximum lengths of 128, 256, and 512;
- performance on short, medium, and long subsets;
- whether decisive sentiment appears after the truncation point.

### H. Add Statistical Tests

- mean and standard deviation across seeds;
- bootstrap confidence intervals;
- McNemar tests;
- feature and component ablations.

---

## Proposed Research Extensions and Innovation Ideas

These ideas were discussed as possible continuations but were **not implemented in this archived project**.

### 1. Failure-Stability-Aware Error Analysis

Instead of treating every one-time mistake as equally difficult, repeat model training and estimate whether each review is:

- an accidental one-shot error;
- an unstable or borderline case;
- a stable model-specific failure;
- a persistent cross-model hard case;
- possible label noise.

### 2. Five-Level Difficulty Taxonomy

| Level | Meaning |
|---|---|
| D1 Stable Easy | Most models and runs predict correctly |
| D2 Weak-Model Sensitive | Lightweight models fail, stronger models succeed |
| D3 Unstable / Borderline | Predictions change across seeds or models |
| D4 Strong-Model Hard | Strong Transformers frequently fail |
| D5 Persistent Hard / Possible Label Noise | Multiple models and runs fail, or the label may be questionable |

### 3. Cost-Adaptive Model Routing

A future system could route reviews by predicted difficulty:

- D1 → Logistic Regression
- D2 → MiniLM or DistilBERT
- D3 → RoBERTa
- D4 → LLM
- D5 → abstain or human review

The goal would be to approach strong-model performance while reducing average inference cost.

### 4. Selective Prediction and Abstention

Rather than forcing a binary prediction for every input, the system could identify uncertain or potentially mislabeled samples and return:

- `abstain`
- `needs human review`
- `possible label noise`

### 5. Efficiency-Robustness Trade-off Analysis

A future study could measure whether model compression, shorter sequence length, or faster inference disproportionately harms difficult linguistic categories.

---

## Reproducibility Notes

To rerun the archived experiment:

1. Download and extract the official IMDb dataset.
2. Open the notebook in `notebooks/`.
3. Update local dataset and output paths where necessary.
4. Confirm the Python environment and installed package versions.
5. Run the notebook cells in order.
6. Compare generated outputs with the archived files under `results/`.

Because package versions, CUDA environments, and hardware differ, exact timing values may not reproduce across machines.

---

## Repository Structure

```text
IMDb-Sentiment-Analysis/
├── notebooks/   # Jupyter notebooks and experiment workflow
├── results/     # Exported metrics, tables, and figures
└── README.md    # Project description, findings, limitations, and archive notes
```

---

## Conclusion

The completed project established a reproducible comparison between traditional text-classification baselines and DistilBERT, showing a clear performance-efficiency trade-off. Its error analysis was useful as an exploratory starting point, but the representative samples and rule-based categories should not be treated as population-level or manually validated evidence.

The most important research lesson from this archive is that strong overall F1 does not necessarily imply robust language understanding. More rigorous future work would require repeated training, a carefully sampled diagnostic set, literature-supported human annotation, uncertainty analysis, and explicit cost-aware evaluation.
