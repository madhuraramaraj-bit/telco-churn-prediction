# Telco Customer Churn Prediction

Predicting which telecom customers are likely to leave, and what the company can do about it.

## About this project

This is a beginner machine learning project. I used the public **Telco Customer Churn** dataset from Kaggle (7,043 customers, 21 columns) to build a model that flags customers who are likely to churn (leave the company), and I turned the results into a short business memo.

**Tools:** Python, pandas, scikit-learn, Google Colab

> The dataset is not included in this repository. It can be downloaded from Kaggle: search for "Telco Customer Churn" (file: `WA_Fn-UseC_-Telco-Customer-Churn.csv`).

## What I did

1. **Loaded and explored the data** and found that 27% of customers churn (1,869 of 7,043).
2. **Cleaned the data:** fixed blank values in `TotalCharges` (new customers who had not been billed yet) and converted the column to numbers.
3. **Prepared the data:** converted text columns into numbers (one-hot encoding), removed `customerID`, and split the data into 80% training and 20% test sets.
4. **Trained and compared three models** (see results below).
5. **Checked what drives churn** using the model's coefficients, then confirmed the main patterns directly in the data.
6. **Wrote a business memo** with findings and recommendations (below).

## Model results (churners only)

Accuracy alone is misleading here, because 73% of customers stay. A model that always guessed "stays" would be 73% accurate and useless. So I compared **recall** (how many of the real churners the model finds) and **precision** (how many of the customers it flags really churn).

| Model | Accuracy | Recall | Precision |
|---|---|---|---|
| Logistic Regression | 0.82 | 0.60 | 0.68 |
| Random Forest | 0.79 | 0.46 | 0.66 |
| Logistic Regression (balanced) | 0.75 | **0.82** | 0.51 |

The simple Logistic Regression beat the Random Forest, so a more complex model was not automatically better. Telling Logistic Regression to give extra weight to churners (`class_weight='balanced'`) raised recall from 0.60 to 0.82, at the cost of more false alarms. Which model is best depends on what a missed churner costs the business compared with an unnecessary retention offer.

---

# Customer Churn: Findings and Recommendations

## 1. The problem

Out of every 100 customers, 27 churn (leave the company). Each customer who leaves takes their monthly income with them. If we can find out who is likely to leave *before* they go, we can offer them a discount and keep them.

## 2. Key findings

- Customers on month-to-month contracts leave far more often than customers on two-year contracts: 42.7% against 2.8%.
- Customers with fiber optic internet leave the most (41.9%), compared with DSL customers (19.0%) and customers with no internet (7.4%).

## 3. Model result

The model finds 82% of the customers who are going to leave. The cost is that some of the customers it flags would have stayed anyway (about half of the flagged customers), so the company gives some discounts that weren't needed. This is acceptable, because losing a customer costs more than a discount.

## 4. Recommendations

1. Offer month-to-month customers a discount for switching to a one- or two-year contract.
2. Find out why fiber optic customers leave, for example by checking their service quality and pricing.
3. Give the retention team a weekly list of customers the model flags as likely to leave, so they can contact them with a discount.

## 5. Estimated impact

If the plan saves just 10% of the customers who would have left, that is about 187 customers kept. At an example value of 65 per month (780 per year) per customer, this protects about 145,860 in yearly income. These numbers are an example to show the scale, and the real value depends on the company's actual prices and costs.
