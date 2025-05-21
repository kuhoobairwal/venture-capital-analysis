---
layout: default
title: A Data-Driven Look at VC Fundraising
---

# A Data-Driven Look at VC Fundraising 

Hi, I'm Kuhoo — a data science and finance enthusiast with a background in computer science and entrepreneurship. This project bridges my interest in **venture capital** and **data-driven decision making** by asking one core question:

> **Can we predict how much capital a VC fund will raise based on its structure, strategy, and firm characteristics?**

---

## 🔍 Project Overview
- Dataset of ~4,800 VC funds from FactSet
- Regression models to predict capital raised
- Explores how AUM, fund age, strategy, and geography relate to fundraising success
- Tools: Python, scikit-learn, Plotly, Lasso Regression

---

## 📊 Explore the Analysis
- [EDA: AUM Distribution (Raw)](aum-raw-distribution.html)
- [EDA: AUM Distribution (Cleaned)](aum-clean-distribution.html)
- [Fund Type vs AUM](fund-type-vc.html)
- [Fund Geography Overview](fund-country-vc.html)
- [AUM vs # of Funds (Colored by Age)](funds_v_aum.html)

---

## 🧠 Key Insights
- Multi-stage funds tend to raise more capital than single-stage ones
- Nonlinear effects between AUM/Fund Age and fundraising success
- Geographic focus (esp. US) is a strong signal of fund size

---

## 📈 Final Model
- Lasso Regression with polynomial + encoded features
- Best R² on test set: 0.6684
- Includes multi-hot encoded fund types and polynomial features for AUM and fund age

---

## 🚀 What's Next?
- Explore sentiment analysis on press releases
- Add diversity and team composition data
- Apply tree-based models (e.g., XGBoost) for comparison

---

## 📂 Full Code + Notebook
🔗 [GitHub Repository](https://github.com/kuhoobairwal/vc-fundraising)

---

*Thanks for reading! I’d love to connect if you're interested in VC, data science, or finance strategy.*
