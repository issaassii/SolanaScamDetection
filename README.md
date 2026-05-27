# Scam Token Detection on Solana Blockchain

A machine learning system that detects fraudulent tokens on the Solana blockchain using on-chain financial data and Logistic Regression.

## Overview

The Solana ecosystem sees a constant flood of new tokens, many of which are scams designed to rugpull investors within minutes. Traditional auditing is too slow for this environment. This project takes a data-driven approach — automatically fetching live token data, labeling it using domain-informed thresholds, and training a classifier to flag scam tokens before users lose money.

## Features

- Custom dataset generation pipeline using GeckoTerminal and DexScreener APIs
- Domain-informed scam labeling (liquidity traps, rugpull risk, FDV analysis)
- Logistic Regression with balanced class weights to handle imbalanced data
- Hyperparameter tuning (regularization strength C) to minimize overfitting
- Achieves **0.84 F1-score** on scam token detection

## Dataset

100 recently active Solana tokens were fetched and labeled using the following features:

| Feature | Description |
|---|---|
| Top 10 Holders % | High concentration = rugpull risk |
| Total Liquidity (USD) | Low liquidity = tokens can't be sold |
| FDV (Fully Diluted Valuation) | High FDV + low supply = liquidation risk |
| GeckoTerminal Risk Score | Platform-generated safety rating (0–100) |
| 24h / 5min Volume Change | Unusual volume spikes signal manipulation |
| 24h / 5min Price Change | Rapid price movement is a scam indicator |
| 24h Transaction Count | Activity level of the token |

Tokens were labeled as **scam** or **not scam** based on threshold rules derived from DeFi domain knowledge.

## Model

- **Algorithm:** Logistic Regression (scikit-learn)
- **Preprocessing:** StandardScaler (all numerical features)
- **Class Imbalance:** Handled via `class_weight='balanced'`
- **Train/Test Split:** 70/30
- **Tuned Hyperparameter:** Regularization strength C ≈ 1

## Results

| Metric | Not Scam | Scam |
|---|---|---|
| Precision | 0.14 | 0.95 |
| Recall | 0.50 | 0.75 |
| F1-Score | 0.22 | **0.84** |
| Accuracy | 73% | — |

> The low overall accuracy is expected given the heavily imbalanced nature of Solana memecoin data. The 0.84 F1-score on the scam class is the key performance indicator.

## Challenges

- Most free APIs (CoinGecko, SolScan, Helium) were unavailable or incomplete — required multiple pivots before settling on GeckoTerminal + DexScreener
- Public datasets for Solana scam tokens are scarce; the data pipeline was built from scratch
- Heavy class imbalance toward scam tokens required careful handling to avoid a biased model

## Future Work

- Incorporate social media signals (Twitter/X activity, Telegram mentions)
- Expand dataset with more tokens and additional features
- Experiment with other models (Random Forest, XGBoost, Neural Networks)
- Integrate paid APIs for richer data

## Tech Stack

- Python
- scikit-learn
- pandas
- GeckoTerminal API
- DexScreener API

## Author

Issa Assi
linkedin.com/in/issaassi/
