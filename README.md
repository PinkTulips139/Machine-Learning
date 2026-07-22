# Machine Learning Research Archive

This repository archives completed machine-learning coursework and research projects, with an emphasis on reproducible experiments, model evaluation, and transparent discussion of limitations.

## Projects

### IMDb Sentiment Analysis

A completed NLP research project comparing:

- TF-IDF + Multinomial Naive Bayes
- TF-IDF + Logistic Regression
- DistilBERT

The project evaluates predictive performance, training and inference cost, sensitivity to training-data size, and exploratory error patterns on the IMDb Large Movie Review Dataset.

➡️ See: [`IMDb-Sentiment-Analysis/`](./IMDb-Sentiment-Analysis/)

## Repository Structure

```text
Machine-Learning/
├── IMDb-Sentiment-Analysis/
│   ├── notebooks/
│   ├── results/
│   └── README.md
├── .gitignore
└── README.md
```

## Research Principles

This repository is maintained as a research archive rather than an actively developed software product. The main goals are to:

- preserve completed experiments and outputs;
- document methods and reproducibility constraints;
- distinguish implemented work from proposed future extensions;
- record limitations honestly;
- make the project understandable to supervisors, reviewers, and prospective research collaborators.

## Current Status

**Status:** Completed and archived.

The IMDb project is no longer under active development. Potential improvements and research extensions are documented in the project README, but they should not be interpreted as implemented results.

## Tools Used

- Python
- Jupyter Notebook
- pandas / NumPy
- scikit-learn
- PyTorch
- Hugging Face Transformers
- Matplotlib

## Notes

- The IMDb dataset is not redistributed in this repository.
- Hardware-dependent timing results should not be interpreted as hardware-neutral benchmarks.
- Some exploratory error-category outputs are retained for historical completeness but are explicitly marked as preliminary.
