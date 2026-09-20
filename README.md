# STADIOEquities-alert-triage-
## Repository structure

The repository is organised so that every artifact type has a clear location.
The table below maps each area to its folder. The SS2 modelling work (notebooks,
documentation, and processed data) is contained in the `notebooks/` folder,
while the remaining folders provide the standing structure for datasets, models,
experiments, and scripts.

| Area | Location in this repository |
|---|---|
| Modelling notebooks and documentation (SS2) | `/notebooks` |
| Datasets | `/data` |
| Models | `/models` |
| Experimental setup | `/experiments/setup` |
| Experimental results | `/experiments/results` |
| Statistical helper and comparison scripts | `/scripts/statistical` |
| Visualisation scripts | `/scripts/visualisation` |
| Literature review (related work + dataset selection) | `/literature-review` |
| Written reports and documents | `/docs` |
---
## Motivation
### Why this problem matters to STADIOEquities
STADIOEquities entire economic model rests on the trust of 2.3 million people who are registered for an account, but only 760,000 (33%) are funded and active, and 41% of customers who have never deposited at all. For a platform whose median client is 31 years old and whose core pitch is letting first time investors put real money into the market from their phone, trust in the platform’s handling of that money is not a soft metric, it is the product.

That trust is currently under direct fire from the compliance function. As STADIOEquities reports that rules-based transaction monitoring alerts have risen 140% year on year, with a large number of alerts being false positives, to the point that genuinely suspicious behaviour risks being lost in the noise while investigators burn out clearing the queue by hand. A false positive is however not an abstract operational inefficiency but for clients it means a legitimate withdrawal are delayed or blocked and flagged as suspicious activity. This is where the problem compounds rather than stays contained.

STADIOEquities separately notes that support-ticket volume has climbed from 46 to 63 per 1,000 active clients, with common queries being “why can’t I withdraw”. For a digital native, first time investor base that leans on peer reviews, app-store ratings, and word of mouth on platforms like Reddit and X to decide whether a brokerage can be trusted with their money, a visible pattern of withdrawal complaints is harmful reputationally in a way that reaches far beyond the client who filed the ticket. Potential sign-ups researching the platform before funding an account could now encounter these complaints, directly undermining the activation gap the board has already identified as its top priority, on the grounds that cheap sign-up mean nothing if they never fund. In other words, the alert triage problem and the activation problem, treated as separate priorities in the STADIOEquities 2030 strategy, are casually linked to unresolved false positives suppress the very funnel the business is most focused on fixing.

### Stakeholders affected
**Compliance investigators**- currently absorb the full weight of a 140% rise in alert volume with no prioritisation mechanism, leading to burnout and increased risk that genuine cases are missed.

**Existing funded clients**- experience delayed or blocked legitimate withdrawals when flagged incorrectly, driving the rise in “why can’t I withdraw” support tickets.

**Prospective clients**- encounter these complaints during due-diligence research before signing up or funding an account, at the exact point in the funnel STADIOEquatites is most trying to protect.

**Executive leadership and board**- accountable for two 2030 strategy priorities (activation and financial crime detection) that this project shows are more interdependent than the current strategy document treats them.

### Value this project will deliver to StadioEquitties.
A model that scores compliance alerts by likelihood of being genuine versus false positive rather than treating every rule triggered alert with equal urgency will let investigators work the queue by risk rather than by arrival order. The value delivered to each stakeholder group is as follows:

**Compliance investigators**: reduced false positive volume reaching manual review, erasing burnout and allowing the team to scale without headcount growing in lockstep with alert volume.

**Existing clients**: faster resolution of legitimate transactions, reducing the frustration and support ticket volume tied to withdrawal delays.

**Prospective clients**: an indirect but material benefit, as fewer unresolved complaints in public view removes friction at the due-diligence stage of the sign-up funnel.

**For leadership and the board**: sharper financial crime detection, in direct response to stated priority to ensure real threats “stand out from the noise”, delivered in a way that also supports the separately stated activation priority with a connection the 2030 strategy document does not draw explicitly, but one the operational data in this pack supports.

This project therefore does not treat compliance and growth as separate problems to be solved by separate teams, but positions alert triage as a lever that serves both.

## Problem Statement
### 1. Context
STADIOEquities already operates the infrastructure that generates the data this problem needs. Its Data & Technology Landscape includes a dedicated compliance monitoring data source transaction monitoring alerts, their dispositions, and case outcomes, with four years of history available. In principle, this is the exactly the kind of historical labelled data (each alert paired with a known outcome of genuine or false positive) that a classification model needs to learn from.

In practice, this system is currently used to generate alerts, not to learn from them. STADIOEquities reports that alert volume has risen 140% year on year, with the vast majority being false positives, and that investigators work through the resulting queue manually and in the order alerts arrive, with no mechanism to prioritise by risk before a case is opened.

This reflects a structural limitation common to rules-based monitoring systems because rules are deliberately set broad to avoid missing genuine cases, they tend to avoid missing genuine cases as well as generate a high volume of false positives as a side-effect. STADIOEquities experience is therefore not an isolated operational failure but a recognisable pattern in how rule-based alerting behaves which suggests a data driven, learned approach to triage is a reasonable direction to investigate, rather than a purely novel proposition.

### 2. Issue
The core issue is that STADIOEquities compliance team has no reliable way to distinguish, at the point an alert is generated, which alerts represent genuine financial crime risk and which are false positives. This has three compounding effects, each already visible in the operating data:

1. Investigators risk missing genuine cases, since real threats are diluted within an overwhelming false positive volume.
2. Investigator capacity is consumed disproportionally by cases that ultimately resolve as non-issues, at a time when alert volume is rising faster than the compliance team is scaling.
3. Legitimate clients experience delayed or blocked withdrawls when a false positive is triggered on their account, a pattern reflected in the rise of “why can’t I withdraw” support tickets from 46 to 63 per 1000 active clients.

What is not currently known is whether transaction and account level data of the kind STADIOEquities already collects contains learnable patterns that reliably separate genuine cases from false positives and, if so, how much of the current false positive burden could realistically be reduced without weakening detection of real threats.

### 3. Relevance
This problem sits at the intersection of two priorities the STADIOEquities 2030 strategy currently treats as separate: sharpening financial crime detection, and closing the activation gap that keeps 41% of registered accounts from ever funding. As argued in the motivation above, these are not in fact, independent problems as unresolved false positives generate the withdrawal complaint that plausibly discourages prospective clients during due diligence research, directly undermining the boards top stated growth priority. Addressing alert triage is there not only a compliance efficiency exercise; it is a plausible, if currently unmeasured, lever on client acquisition and retention.

This problem also lends itself naturally to a Data Science project specifically for three reasons: it is structured, historical documented classification task where each past alert has a known outcome; the company already collects the relevant features (transaction size, frequency, account age, and behavioural patterns) as part of normal operations; and the central question which alerts are genuine which is precisely the kind of supervised learning problem I will address.

### 4. Objectives
The research aims to:
1.	Determine whether transaction and account level features can reliably predict whether a compliance alert is a genuine case or a false positive.
2.	Identify features which are most predictive of true risk, to inform which signals the compliance team should weight most heavily
3.	Build and evaluate a classification model capable of ranking alerts by estimated risk, rather than treating all alerts as equally urgent.
4.	Assess the model using metrics appropriate to a rare- event, imbalanced classification problem (precision, recall, and PR-AUC in particular), rather than accuracy alone, given that missing a genuine case and flooding investigators with false positives carry different costs.
5.	Translate model outputs into practical triage recommendation that STADIOEquaties compliance team could plausibly act on.

---

---

## Part B: Modelling Documentation

The following documents describe the modelling pipeline. Each links to its
corresponding notebook and explains how to run it.

- [Preprocessing](notebooks/Preprocessing.MD)
- [Feature Engineering](notebooks/FeatureEngineering.MD)
- [Model 1 : Logistic Regression](notebooks/Model1.MD)
- [Model 2 : Random Forest](notebooks/Model2.MD)

---

## Part C: Model Performance & Comparison

The following documents present the performance results of each model on the
Part A dataset, and a comparison between them. Each links to the notebook that
produces the results.

- [Model 1 Performance](notebooks/Model1Performance.MD)
- [Model 2 Performance](notebooks/Model2Performance.MD)
- [Comparison of Model 1 and Model 2](notebooks/Comparison.MD)

---

## Part D: Recommendations Report

A formal business report addressing which model is best, how it can be improved,
how it adapts to STADIOEquities' own data, and how the results align with the
literature.

- [Recommendations Report (PDF)](docs/Recommendations_Report.pdf)



## RAAIDD Log

### Risks

| Risk | Description |
|------|-------------|
| Proxy dataset not representative | The public proxy dataset (Credit Card Fraud / PaySim) may differ from STADIOEquities' real alert data, limiting how well the findings transfer to the actual business. |
| Severe class imbalance | Genuine fraud cases are rare, so the model could show high accuracy while failing to detect the true positives that matter most. |
| Cutting false positives too aggressively | Tuning the model to reduce false positives could suppress alerts representing real financial crime whic is a more costly error for the business. |
| Scope creep across the capstone | Attempting to model activation, dormancy, and alert triage together would dilute focus and jeopardise completion of the six-part project. |
| Timeline pressure | Underestimating the data-cleaning and modelling stages could compress later stages such as report writing and the presentation. |

### Actions

| Action | Description |
|--------|-------------|
| Source and load the proxy dataset | Obtain the chosen proxy dataset (Credit Card Fraud Detection and/or PaySim) and load it into Google Colab. |
| Clean and prepare the data | Handle missing values, inconsistent formatting, and irrelevant variables to produce a modelling-ready dataset |
| Perform exploratory data analysis | Visualise the class imbalance and key feature distributions to understand the data before modelling. |
| Engineer features and select an algorithm | Create relevant features and choose a suitable classification algorithm, justified against the problem |
| Train, evaluate, and tune the model | Build the model, evaluate it with imbalance-appropriate metrics, and tune it to balance precision and recall |
| Write the report and prepare the presentation | Synthesise motivation, methodology, results, and recommendations into the final report and prepare presentation |

### Assumptions

| Assumption | Description |
|------------|-------------|
| Proxy data is structurally comparable | The proxy dataset's rare-event, imbalanced, transaction-level structure is assumed similar enough to STADIOEquities' alert data for the approach to transfer. |
| Historical outcomes are reliable labels | The "genuine vs. false positive" labels are assumed accurate enough to train a trustworthy model. |
| Client could supply the requested data | In a real engagement, STADIOEquities is assumed able to provide the labelled alert, transaction, and account data specified in the Data Request |
| Patterns are learnable from the features | Transaction- and account-level features are assumed to contain enough signal to separate genuine cases from false positives. |
| Course tools are sufficient | The Python stack and algorithms I've learned in the course are assumed adequate to build a credible triage model. |

### Issues

| Issue | Description |
|-------|-------------|
|No real client dataset could be obtained | STADIOEquities' own alert data is unavailable, as the client is fictional. This is being managed by substituting a structurally comparable real-world proxy dataset (Credit Card Fraud / PaySim) for the modelling work. |

### Decisions

| Decision | Description |
|----------|-------------|
| Chose alert triage over other candidate problems | Among the problems in the STADIOEquities pack (activation, dormancy, segmentation, alert triage), fraud/AML alert triage was selected for its strong fit with available proxy data, its established research base, and its alignment with the financial-crime detection priority. |
| Chose proxy datasets and evaluation approach | Decided to use the Credit Card Fraud Detection and/or PaySim datasets as proxies, and to evaluate with precision, recall, and PR-AUC rather than accuracy, given the imbalanced, cost-sensitive nature of the problem. |

### Dependencies

| Dependency | Description |
|------------|-------------|
| Data sourcing → data preparation | The dataset must be sourced and loaded first; only then can cleaning and preparation begin. |
| Data preparation → exploratory data analysis | The dataset must be fully cleaned before meaningful exploratory analysis can be carried out. |
| Exploratory analysis → feature engineering and algorithm choice | EDA must be completed first, as its insights drive which features are engineered and which algorithm is selected. |
| Feature engineering and algorithm choice → model training | Features must be engineered and an algorithm chosen before the model can be trained, evaluated, and tuned. |
| Model training → reporting and presentation | The model must be trained and evaluated before the report and presentation can be finalised, since results and recommendations flow from it. |