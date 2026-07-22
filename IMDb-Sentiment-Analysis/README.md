# IMDb Sentiment Analysis

This project compares traditional machine learning models and DistilBERT for IMDb movie review sentiment classification.

## Project Overview

The project builds a complete NLP text classification workflow, including data preprocessing, exploratory data analysis, feature extraction, model training, evaluation, and error analysis.

## Models

- Multinomial Naive Bayes
- Logistic Regression
- DistilBERT

## Tech Stack

- Python
- scikit-learn
- PyTorch
- Hugging Face Transformers
- pandas
- matplotlib

## Results

- Best DistilBERT F1 score: 0.9145
- DistilBERT with 20% training data F1 score: 0.8971
- Compared Accuracy, Precision, Recall, F1, training time, and inference time
- Analyzed 5,465 misclassified samples
- Extracted representative error examples for error pattern analysis

## Files

- 
otebooks/IMDb_sentiment_analysis.ipynb: main experiment notebook
- esults/: experiment result tables, charts, and error analysis files

## Research Output

This project supported the paper *Traditional Machine Learning and DistilBERT for Sentiment Analysis*, accepted by CONFC_DCS.
