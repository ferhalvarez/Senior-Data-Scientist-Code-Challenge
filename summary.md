# Executive Summary

## Overview of the Approach
The objective of this project was to build a predictive model capable of identifying orders likely to be returned, enabling targeted interventions that reduce operational and logistics costs. The work progressed through four stages: establishing a baseline, reframing evaluation using business-aligned metrics, executing a structured model improvement process, and designing a deployment strategy. The evaluation centered on recall for returned items (class 1), expected financial savings, and maintaining a feasible intervention rate.

After experimenting with multiple algorithms and techniques, the final selected model is an Optuna-optimized Random Forest classifier with balanced class weighting. This model provides the strongest combination of minority-class detection, financial value, and robustness.

---

## Key Findings
The baseline logistic regression model achieved high accuracy but was essentially useless for detecting returns due to class imbalance. Re-evaluating performance using recall, F1, and cost-benefit metrics exposed the need for a model that prioritizes detecting returned items, even at the cost of more interventions.

Model improvements followed a systematic, hypothesis-driven structure:

- **Balanced Logistic Regression** began identifying returns but with limited effectiveness.
- **Balanced Random Forest** improved recall but plateaued around ~25%.
- **Feature-enhanced Random Forest** did not meaningfully improve performance.
- **Optimized Random Forest (final model)** achieved a significant breakthrough with **~71% recall** and **0.43 F1-score for class 1**, outperforming all previous models.

This final model successfully captures complex patterns in return behavior without overfitting and generalizes well on unseen data.

---

## Business Impact Estimate
Using the model’s final performance:

- Total real returns in the test set: **505**
- Recall (class 1): **0.71**

**True positives (returns correctly flagged):**  
TP = 0.71 × 505 ≈ **359 prevented returns**  
Savings = 359 × $15 = **$5,385**

**False positives (unnecessary interventions):**  
Precision = 0.31  
Estimated FP ≈ **801 interventions**  
Cost = 801 × $3 = **$2,403**

**Net financial impact:**  
$5,385 − $2,403 = **$2,982 net savings**  
Equivalent to **~$1.49 profit per order** on the test set.

This demonstrates substantial positive value, enabling earlier prevention of costly returns while maintaining controlled intervention costs.

---

## Deployment Recommendation
The optimized Random Forest should be deployed with a probability threshold tuned for maximum profit rather than accuracy. A monitoring system must track recall, precision, intervention rate, drift metrics, and profit-per-order over time. Retraining is recommended monthly or when drift persists over multiple weeks. A rollback mechanism should be in place to ensure service stability.

Given its high recall, strong financial impact, and robustness, the model is recommended for controlled production deployment with active monitoring and periodic retraining.
