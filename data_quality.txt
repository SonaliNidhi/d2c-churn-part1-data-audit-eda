# Data Quality Report

## 1. Executive Summary

This report evaluates the quality of customer, order, support-ticket, web activity, intervention history, and churn-label datasets prior to exploratory analysis and model development.

The audit focuses on:

* Missing values
* Duplicate records
* Invalid values
* Outliers
* Join integrity
* Date consistency
* Data leakage risks

---

## 2. Missing Value Assessment

### customers.csv

| Column       | Missing Count | Missing % | Impact               |
| ------------ | ------------- | --------- | -------------------- |
| loyalty_tier | XXX           | XX%       | Affects segmentation |
| skin_type    | XXX           | XX%       | Limited impact       |

### orders.csv

| Column | Missing Count | Missing % | Impact                           |
| ------ | ------------- | --------- | -------------------------------- |
| rating | XXX           | XX%       | May affect satisfaction analysis |

### Recommendation

* loyalty_tier → Create "Unknown" category
* skin_type → Impute with mode or retain as missing
* rating → Preserve missing as "Not Rated"

---

## 3. Duplicate & Duplicate-Like Records

### orders.csv

Detected duplicate-like records identified using order_id patterns.

Examples:

| order_id | customer_id |
| -------- | ----------- |
| XXX      | XXX         |

### Business Impact

Duplicate orders can inflate:

* revenue calculations
* frequency metrics
* customer value estimates

### Recommendation

Investigate before aggregation.

---

## 4. Invalid & Unusual Values

Checked:

* Negative revenue values
* Future dates
* Invalid loyalty tiers
* Invalid ratings
* Invalid return indicators

### Findings

Document all unusual values discovered.

### Recommendation

Apply validation rules before modeling.

---

## 5. Outlier Assessment

### gross_amount

Detected extreme purchase values.

Potential causes:

* bulk purchases
* data-entry issues
* corporate purchases

### Impact

Can distort:

* monetary metrics
* RFM scoring
* model training

### Recommendation

Use IQR analysis and winsorization if required.

---

## 6. Join & Key Integrity Validation

Validated customer_id across:

* customers
* orders
* support_tickets
* web_events
* intervention_history
* churn_labels

### Checks

Customers without orders

Customers without support history

Customers without web activity

Orphan records

### Recommendation

Retain left joins and flag sparse histories.

---

## 7. Date Consistency Audit

Verified:

* order dates
* support ticket dates
* intervention dates
* snapshot date alignment

### Findings

Post-snapshot transactions detected.

### Business Impact

Can introduce leakage into churn models.

---

## 8. Leakage Risk Assessment

### High-Risk Leakage Sources

1. Orders after snapshot date
2. Future interventions
3. Post-target-period activity

### Rule

Only information available on or before 2025-09-30 can be used for feature creation.

### Recommendation

Exclude all post-snapshot records from modeling.

---

## 9. Data Quality Risk Ranking

| Issue                | Severity |
| -------------------- | -------- |
| Leakage Risk         | High     |
| Missing Loyalty Tier | Medium   |
| Duplicate Orders     | Medium   |
| Revenue Outliers     | Medium   |
| Missing Ratings      | Low      |

---

## 10. Final Recommendations

1. Remove post-snapshot records from modeling.
2. Investigate duplicate-like orders.
3. Treat missing categorical values explicitly.
4. Review extreme order values.
5. Maintain join integrity checks during feature engineering.
6. Implement validation rules before production deployment.
