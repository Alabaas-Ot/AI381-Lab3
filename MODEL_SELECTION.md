# Model selection

## The question
This model predicts whether a telecom customer is likely to leave the service (churn). A false negative may cause the company to lose a customer, while a false positive may lead to unnecessary retention offers and costs.

## Candidates
| Candidate | Why I tried it | AUC | F1 | F1 per slice (lowest) | p95 latency (ms) | Size (KB) | MLflow run ID |
|---|---|---:|---:|---:|---:|---:|---|
| Logistic Regression | Simple, fast, and easy to interpret | 0.798 | 0.495 | 0.250 (one-year, two-year) | 4.5 | 4 | feaef66d5f634d32a17c3ee1acecb6f6 |
| Random Forest | To capture nonlinear patterns | 0.759 | 0.454 | 0.222 (one-year) | 17.5 | 3160 | 080e859357fc4d6088abf495daa2f8e8 |
| Gradient Boosting | To test boosting performance | 0.748 | 0.417 | 0.222 (two-year) | 4.3 | 142 | 372ad8ba7e1b4bcc882a5e44f0f9a4fe |

## The champion
I selected Logistic Regression as the champion. It achieved the highest AUC (0.798) and F1 (0.495), while also having the smallest model size (4 KB) and low p95 latency (4.5 ms). I accepted its slightly higher latency than Gradient Boosting and its low F1 for one-year and two-year contracts.

## My promotion rules
- Latency budget (p95): 25 ms, because the prediction API should remain responsive.
- Minimum gain to replace the champion (F1 points): 0.02 absolute F1, because a small improvement may not justify replacing a simpler and smaller model.

## What I would check next
I would investigate performance for one-year and two-year contract customers, because their F1 scores are low. I would also monitor changes in customer behavior and data over time.
