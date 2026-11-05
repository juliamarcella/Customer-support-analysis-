Flowbase Customer Support Analysis

What can support tickets tell a company about unhappy customers, and about customers who are about to leave?

This project analyses two years of customer support data from Flowbase, a made-up B2B SaaS company. I clean the data, explore it with eight business questions, test whether two hidden business events are visible, and build a simple model that flags customers at risk of churn.

Why it matters: 26% of customers churned in two years, and 62% of them left without giving a reason. Support tickets are one of the few early warning signs the business has.

The data

The data is synthetic: I generated it with a Python script (src/generate_dataset.py), so no real customers are involved. The script still builds in realistic patterns and noise, plus two hidden events: a platform outage (11 March 2025) and a price increase (1 September 2025).

File	Rows	One row per…
support_tickets.csv	17,381	support ticket
customers.csv	3,200	customer account
agents.csv	35	support agent

Period: 1 July 2024 to 30 June 2026. The full column list is in data/DATA_DICTIONARY.md.

Business questions
Speed: How fast does the team respond and resolve tickets? How often is the SLA missed?
Satisfaction: What leads to low CSAT scores?
Emotion: What is the emotion in customer messages? (next step)
Churn: Which customers leave, and can we predict it from their support history?
Business events: Are the outage and the price increase visible in the data?
Key findings

1. Speed
The median first reply takes 59.5 minutes, and the median resolution 14 hours.
11.2% of tickets miss the SLA. Urgent tickets are the weak spot: 34.2% miss their 45-minute target.
Bug Reports are the slowest to resolve (median 3.2 days), because they depend on Engineering.
Cancellation Requests wait the longest for a first reply (about 1.8 hours).

3. Satisfaction
A late first reply makes customers about 5 times as likely to be unhappy (28.9% vs 5.5%).
A reopened ticket has the same effect (28.3% vs 6.3%). Escalation matters much less (12.0% vs 7.6%).
The twist: Performance tickets looked like a product problem, but they were a speed problem. Only 3.7% of Performance customers were unhappy when the reply was on time, against 37.9% when it was late.

4. Churn
Billing tickets are the clearest warning sign outside of cancellations: 10.2% of them are followed by churn within 90 days, against 8.9% on average.
The plan matters even more: after a Billing ticket, Starter customers churn 17.6% of the time and Enterprise customers 2.7%.
A logistic regression catches 103 of 167 churners (recall 61.7%, ROC-AUC 0.70). Its list contains about 1.6 times more churners than a random pick.
The strongest warning signs: frequent contact with support, a Starter or Professional plan, monthly billing, never answering surveys, and many Billing tickets.

6. Business events
Outage: 268 tickets on 11 March 2025, about 11 times a normal day. 52.0% of Performance tickets in the outage days missed the SLA.
Price increase: Billing tickets almost doubled in the next 3 months (280 → 548). CSAT stayed good, but churn within 90 days rose from 8.6% to 13.9%. CSAT alone would have hidden this risk.
Recommendations
Answer Urgent and Performance tickets first, especially during incidents.
Solve problems properly the first time, so tickets don't get reopened.
Treat Billing tickets as an early churn warning, especially after a price change, and ask Customer Success to follow up.
Reply faster to Cancellation Requests.
Use the churn model to build a weekly check-in list for Customer Success.

How I worked
Data cleaning: checked why each value is missing (every gap had a reason, so nothing was filled), duplicates, text consistency, 19 logic checks and outliers. I also checked that skipping the CSAT survey is random.
Exploratory analysis: eight questions comparing categories, priorities and time periods, each with a short What I see and Why it matters.
Churn prediction: one row per customer, using only tickets from before churn to avoid data leakage. Logistic regression with balanced classes, evaluated with recall and precision instead of accuracy alone.

Tools: Python, pandas, NumPy, matplotlib, seaborn, scikit-learn, Jupyter

Repository structure
├── data/
│   ├── support_tickets.csv
│   ├── customers.csv
│   ├── agents.csv
│   ├── DATA_DICTIONARY.md
│   
├── notebooks/
│   └── Notebook.ipynb      # the full analysis
├── src/
│   └── generate_dataset.py # script that generates the synthetic data
└── README.md

Limitations
The data is synthetic, so patterns are cleaner than in a real company.
Only 39.5% of tickets have a CSAT score.
Before/after comparisons show a link, not proof of cause.
The churn model is moderate, because most reasons for leaving are not in support data.
Next steps
Text analysis (NLP) on customer messages to find each message's emotion (question 3).
Compare the model with others, such as a random forest.
Build a dashboard from the clean data, with alerts for ticket spikes.

Julia Silva · Junior Data Analyst · Portfolio
