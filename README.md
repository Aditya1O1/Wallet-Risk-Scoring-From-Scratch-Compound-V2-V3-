
# Wallet Risk Scoring From Scratch (Compound V2)

## Overview

This project implements a wallet risk scoring system based on Compound V2 protocol activity. It evaluates the risk profile of Ethereum wallet addresses by analyzing their on-chain borrowing and repayment behavior. The result is a transparent, explainable score (0–1000) per wallet, suitable for DeFi credit risk assessment and automated analytics.

---

## Table of Contents

- [Tools & Technologies](#tools--technologies)  
- [Dataset](#dataset)  
- [Pipeline Steps & Implementation](#pipeline-steps--implementation)  
- [How to Run This Project](#how-to-run-this-project)  
- [Risk Scoring Model Details](#risk-scoring-model-details)  
- [Results](#results)
- [Attachments](#attachments)  
- [Notes & Future Improvements](#notes--future-improvements)  

---

## Tools & Technologies

- Python 3 (Colab friendly)
- Libraries: pandas, numpy, requests  
- [Covalent API](https://www.covalenthq.com/docs/api/) for fetching Compound V2 transaction data  
- Jupyter/Google Colab notebook for interactive analysis and code reproducibility

---

## Dataset

- **Wallet List:** 100 Ethereum wallet addresses (from `Wallet id.xlsx`)
- **On-chain Data:**  
  - Compound V2 transaction data (specifically: borrows and repays) fetched via Covalent API for each wallet

---

## Pipeline Steps & Implementation

### 1. Loading Wallets

- Load wallet addresses into a pandas DataFrame from Excel.

### 2. Fetching On-chain Transaction Data

- For each wallet, fetch Compound V2 events (focus: borrows and repays) using Covalent API and cToken contract addresses.
- Parse all relevant events (checking decoded logs only).

### 3. Data Preparation & Feature Engineering

- Preprocess and aggregate event data per wallet.
- Computed features include:
  - Total amount borrowed and repaid
  - Number of borrow and repay transactions
  - Average amounts (borrow, repay)
  - Activity span (first and last activity date, active days)
  - Borrow/repay frequency (events per active day)
  - Net outstanding balance (total_borrowed - total_repaid)
  - Repay to borrow ratio
  - Binary risk flag (`is_risky`) for wallets with clear risk patterns (low/no repay, high net borrow, etc.)

### 4. Risk Scoring

- Feature normalization using min-max scaling and robust handling of outliers.
- A rule-based scoring formula combines features: wallets with higher net borrow and lower repay ratios get higher risk scores; long, consistent, well-repaying wallets get lower risk scores.
- Final score scaled 0 (safe) to 1000 (risk).
- (Optional): Export additional readable summary stats for reporting.

### 5. Output Generation

- Save results as `wallet_risk_scores.csv` (columns: wallet_id, risk_score).
- Provide clear, documented code cells and feature explanations in the notebook.

---

## How to Run This Project

1. Download/upload the notebook and `Wallet id.xlsx` to your Colab or local Jupyter environment.
2. Register on CovalentHQ; add your API key in the notebook variable.
3. Run code cells in order:
   - Load wallet data
   - Retrieve Compound V2 transactions using Covalent
   - Feature engineering for risk analysis
   - Compute and export risk scores
4. Download `wallet_risk_scores.csv` for further use, sharing, or submission.

---

## Risk Scoring Model Details

- **Data Collection**: All activity is sourced from the Compound V2 protocol, using Covalent API for full event history per address.
- **Features**: Centered on borrow/repay volume, net balance, event frequency, recency, and repayment consistency—proven metrics for DeFi credit risk.
- **Scoring Logic**: Explicit rule-based score, rewarding wallets with good repayment/risk patterns and penalizing risky behaviors.
- **Normalization:** All features are standardized to maintain comparability regardless of wallet size or activity.
- *(Optional)*: Random Forest or other ML models may be added if true risk labels or larger datasets become available.

---

## Results

- **Output**: `wallet_risk_scores.csv` containing wallet_id and risk_score (0–1000 scale).
- **Interpretation:**
  - Riskier wallets (score~1000) = lots borrowed, little/missed repayment, concentrated or recent activity.
  - Safer wallets (score~0) = regular, fully repaid, spread out activity.

---
## Attachments

The following files are included in the repository to provide full context and reproducibility of the project:

- `raw_wallet_data.json` — Raw API transaction data fetched for analysis  
- `Wallet id.xlsx` — The list of 100 Ethereum wallet addresses analyzed  
- `wallet_risk_scores.csv` — Final output CSV containing wallet risk scores  
- `Zeru_Round_2_Wallet_Risk_Scoring_Assignment_(1).ipynb` — Primary Jupyter/Colab notebook implementing the full pipeline  
- `zeru_round_2_wallet_risk_scoring_assignment_.py` — Python script version of the notebook code for automation or local use

---

## Notes & Future Improvements

- Add more protocols (e.g., Aave V2/V3) data integration for fuller risk coverage.  
- Incorporate liquidation event detection for direct risk flags.  
- Explore graph/network features based on related wallet interactions.  
- Improve ML model labeling with actual default or liquidation outcomes.  
- Optimize API calls batching and caching for large-scale deployment.

---

## Author & Contact

- Aditya Kumar Pandey
- IIIT Bhagalpur, MTech CSE(AI & DS)
- Date: July 2025  
- For any questions or collaboration, please contact: aditya.240201001@iiitbh.ac.in

---

## References

- [Covalent API Documentation](https://www.covalenthq.com/docs/api/)  
- [The Graph Protocol](https://thegraph.com/docs/en/)  
- [Compound Protocol Documentation](https://compound.finance/docs)  
- [Data Science and Risk Engineering in DeFi](https://arxiv.org/abs/2109.02624)  

---

*This repository contains all code, data processing scripts, notebooks, and exports for wallet risk assessment tailored for Compound lending protocols.*

