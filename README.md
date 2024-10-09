
# 🏦 Bank Loan Analysis Project

This repository provides an in-depth analysis of bank loan data using **MS SQL Server**. Key performance indicators (KPIs) such as loan applications, funded amounts, and received payments are calculated to assess the health of the loan portfolio. This analysis is further visualized using **Power BI** to make informed data-driven decisions.

---

## 📊 Project Dashboards

The analysis is supported by interactive dashboards that summarize the findings and provide detailed insights into the loan portfolio:

- **Summary Dashboard**  
  ![Bank Loan Summary](https://harsh-kittu.github.io/bank-loan-analysis/report-images/bank_loan_summary.png)

- **Overview Dashboard**  
  ![Bank Loan Overview](https://harsh-kittu.github.io/bank-loan-analysis/report-images/bank_loan_overview.png)
  
  - **Details Dashboard**  
  ![Bank Loan Details](https://harsh-kittu.github.io/bank-loan-analysis/report-images/bank_loan_details.png)

These dashboards offer a visual representation of KPIs, loan performance, and key metrics for quick insights into lending activities.

---

## 📋 Table of Contents

1. [Key Performance Indicators (KPIs)](#key-performance-indicators-kpis)
2. [Loan Status Analysis](#loan-status-analysis)
3. [Overview Reports](#overview-reports)
4. [DAX Queries for Power BI](#dax-queries-for-power-bi)
5. [Tools Used](#tools-used)
6. [Conclusion](#conclusion)
7. [Author](#author)
8. [License](#license)

---

## 📊 Key Performance Indicators (KPIs)

### 1. Total Loan Applications
```sql
SELECT COUNT(id) AS Total_Applications FROM bank_loan_data;
```

### 2. Month-to-Date (MTD) Loan Applications
```sql
SELECT COUNT(id) AS Total_Applications FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

### 3. Previous Month-to-Date (PMTD) Loan Applications
```sql
SELECT COUNT(id) AS Total_Applications FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

### 4. Total Funded Amount
```sql
SELECT SUM(loan_amount) AS Total_Funded_Amount FROM bank_loan_data;
```

### 5. MTD Total Funded Amount
```sql
SELECT SUM(loan_amount) AS Total_Funded_Amount FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

### 6. PMTD Total Funded Amount
```sql
SELECT SUM(loan_amount) AS Total_Funded_Amount FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

### 7. Total Amount Received
```sql
SELECT SUM(total_payment) AS Total_Amount_Collected FROM bank_loan_data;
```

### 8. MTD Total Amount Received
```sql
SELECT SUM(total_payment) AS Total_Amount_Collected FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

### 9. PMTD Total Amount Received
```sql
SELECT SUM(total_payment) AS Total_Amount_Collected FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

### 10. Average Interest Rate
```sql
SELECT AVG(int_rate) * 100 AS Avg_Int_Rate FROM bank_loan_data;
```

### 11. MTD Average Interest Rate
```sql
SELECT AVG(int_rate) * 100 AS MTD_Avg_Int_Rate FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

### 12. PMTD Average Interest Rate
```sql
SELECT AVG(int_rate) * 100 AS PMTD_Avg_Int_Rate FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

### 13. Average Debt-to-Income (DTI) Ratio
```sql
SELECT AVG(dti) * 100 AS Avg_DTI FROM bank_loan_data;
```

### 14. MTD Average DTI Ratio
```sql
SELECT AVG(dti) * 100 AS MTD_Avg_DTI FROM bank_loan_data
WHERE MONTH(issue_date) = 12;
```

### 15. PMTD Average DTI Ratio
```sql
SELECT AVG(dti) * 100 AS PMTD_Avg_DTI FROM bank_loan_data
WHERE MONTH(issue_date) = 11;
```

---

## 📈 Loan Status Analysis

### Good Loan Issued

#### 1. Good Loan Percentage
```sql
SELECT
    (COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN id END) * 100.0) / 
    COUNT(id) AS Good_Loan_Percentage
FROM bank_loan_data;
```

#### 2. Good Loan Applications
```sql
SELECT COUNT(id) AS Good_Loan_Applications FROM bank_loan_data
WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';
```

#### 3. Good Loan Funded Amount
```sql
SELECT SUM(loan_amount) AS Good_Loan_Funded_Amount FROM bank_loan_data
WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';
```

#### 4. Good Loan Amount Received
```sql
SELECT SUM(total_payment) AS Good_Loan_Amount_Received FROM bank_loan_data
WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';
```

---

### Bad Loan Issued

#### 1. Bad Loan Percentage
```sql
SELECT
    (COUNT(CASE WHEN loan_status = 'Charged Off' THEN id END) * 100.0) / 
    COUNT(id) AS Bad_Loan_Percentage
FROM bank_loan_data;
```

#### 2. Bad Loan Applications
```sql
SELECT COUNT(id) AS Bad_Loan_Applications FROM bank_loan_data
WHERE loan_status = 'Charged Off';
```

#### 3. Bad Loan Funded Amount
```sql
SELECT SUM(loan_amount) AS Bad_Loan_Funded_Amount FROM bank_loan_data
WHERE loan_status = 'Charged Off';
```

#### 4. Bad Loan Amount Received
```sql
SELECT SUM(total_payment) AS Bad_Loan_Amount_Received FROM bank_loan_data
WHERE loan_status = 'Charged Off';
```

---

### Loan Status Overview
```sql
SELECT
    loan_status,
    COUNT(id) AS LoanCount,
    SUM(total_payment) AS Total_Amount_Received,
    SUM(loan_amount) AS Total_Funded_Amount,
    AVG(int_rate * 100) AS Interest_Rate,
    AVG(dti * 100) AS DTI
FROM
    bank_loan_data
GROUP BY
    loan_status;
```

---

## 📅 Overview Reports

### Monthly Report
```sql
SELECT 
    MONTH(issue_date) AS Month_Number, 
    DATENAME(MONTH, issue_date) AS Month_Name, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
GROUP BY MONTH(issue_date), DATENAME(MONTH, issue_date)
ORDER BY MONTH(issue_date);
```

### State-wise Report
```sql
SELECT 
    address_state AS State, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
GROUP BY address_state
ORDER BY address_state;
```

### Loan Term Analysis
```sql
SELECT 
    term AS Term, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
GROUP BY term
ORDER BY term;
```

### Employment Length Analysis
```sql
SELECT 
    emp_length AS Employee_Length, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
GROUP BY emp_length
ORDER BY emp_length;
```

### Loan Purpose Breakdown
```sql
SELECT 
    purpose AS PURPOSE, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
GROUP BY purpose
ORDER BY purpose;
```

### Home Ownership Analysis
```sql
SELECT 
    home_ownership AS Home_Ownership, 
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
FROM bank_loan_data
GROUP BY home_ownership
ORDER BY home_ownership;  
```

---

## 📊 DAX Queries for Power BI

### 1. **Total Loan Applications**
```DAX
Total_Applications = COUNT(bank_loan_data[id])
```
- **Explanation**: This DAX function calculates the total number of loan applications by counting the `id` field.

### 2. **Month-to-Date (MTD) Loan Applications**
```DAX
MTD_Loan_Applications = CALCULATE(
    COUNT(bank_loan_data[id]),
    DATESMTD(bank_loan_data[issue_date])
)
``

`
- **Explanation**: This query uses the `DATESMTD` function to filter loan applications for the current month and counts the total applications.

### 3. **Previous Month-to-Date (PMTD) Loan Applications**
```DAX
PMTD_Loan_Applications = CALCULATE(
    COUNT(bank_loan_data[id]),
    PREVIOUSMONTH(bank_loan_data[issue_date])
)
```
- **Explanation**: The `PREVIOUSMONTH` function is used here to get the count of loan applications from the previous month.

### 4. **Total Funded Amount**
```DAX
Total_Funded_Amount = SUM(bank_loan_data[loan_amount])
```
- **Explanation**: Sums up the total funded amount by adding all the values in the `loan_amount` column.

### 5. **Month-to-Date (MTD) Total Funded Amount**
```DAX
MTD_Funded_Amount = CALCULATE(
    SUM(bank_loan_data[loan_amount]),
    DATESMTD(bank_loan_data[issue_date])
)
```
- **Explanation**: Uses `DATESMTD` to sum the loan amounts for the current month.

### 6. **Previous Month-to-Date (PMTD) Total Funded Amount**
```DAX
PMTD_Funded_Amount = CALCULATE(
    SUM(bank_loan_data[loan_amount]),
    PREVIOUSMONTH(bank_loan_data[issue_date])
)
```
- **Explanation**: Sums up the total loan amounts funded in the previous month using `PREVIOUSMONTH`.

### 7. **Total Amount Received**
```DAX
Total_Amount_Collected = SUM(bank_loan_data[total_payment])
```
- **Explanation**: Sums up all loan payments received from borrowers.

### 8. **Month-to-Date (MTD) Total Amount Received**
```DAX
MTD_Amount_Collected = CALCULATE(
    SUM(bank_loan_data[total_payment]),
    DATESMTD(bank_loan_data[issue_date])
)
```
- **Explanation**: Collects the total payments received in the current month using the `DATESMTD` function.

### 9. **Previous Month-to-Date (PMTD) Total Amount Received**
```DAX
PMTD_Amount_Collected = CALCULATE(
    SUM(bank_loan_data[total_payment]),
    PREVIOUSMONTH(bank_loan_data[issue_date])
)
```
- **Explanation**: Collects the total payments received in the previous month.

### 10. **Average Interest Rate**
```DAX
Avg_Interest_Rate = AVERAGE(bank_loan_data[int_rate]) * 100
```
- **Explanation**: Calculates the average interest rate across all loans and multiplies by 100 to express it as a percentage.

### 11. **Month-to-Date (MTD) Average Interest Rate**
```DAX
MTD_Avg_Interest_Rate = CALCULATE(
    AVERAGE(bank_loan_data[int_rate]),
    DATESMTD(bank_loan_data[issue_date])
) * 100
```
- **Explanation**: Averages the interest rate for the current month and multiplies by 100 to convert it to a percentage.

### 12. **Previous Month-to-Date (PMTD) Average Interest Rate**
```DAX
PMTD_Avg_Interest_Rate = CALCULATE(
    AVERAGE(bank_loan_data[int_rate]),
    PREVIOUSMONTH(bank_loan_data[issue_date])
) * 100
```
- **Explanation**: Averages the interest rate for the previous month.

### 13. **Average Debt-to-Income (DTI) Ratio**
```DAX
Avg_DTI = AVERAGE(bank_loan_data[dti]) * 100
```
- **Explanation**: Averages the DTI ratio and expresses it as a percentage.

### 14. **Month-to-Date (MTD) Average DTI Ratio**
```DAX
MTD_Avg_DTI = CALCULATE(
    AVERAGE(bank_loan_data[dti]),
    DATESMTD(bank_loan_data[issue_date])
) * 100
```
- **Explanation**: Averages the DTI for the current month and multiplies by 100 to express it as a percentage.

### 15. **Previous Month-to-Date (PMTD) Average DTI Ratio**
```DAX
PMTD_Avg_DTI = CALCULATE(
    AVERAGE(bank_loan_data[dti]),
    PREVIOUSMONTH(bank_loan_data[issue_date])
) * 100
```
- **Explanation**: Averages the DTI for the previous month and expresses it as a percentage.

### 16. **Good Loan Percentage**
```DAX
Good_Loan_Percentage = 
    (COUNTROWS(FILTER(bank_loan_data, bank_loan_data[loan_status] = "Fully Paid" || bank_loan_data[loan_status] = "Current")) 
    * 100.0) / COUNTROWS(bank_loan_data)
```
- **Explanation**: Calculates the percentage of good loans that are either fully paid or current.

### 17. **Bad Loan Percentage**
```DAX
Bad_Loan_Percentage = 
    (COUNTROWS(FILTER(bank_loan_data, bank_loan_data[loan_status] = "Charged Off")) 
    * 100.0) / COUNTROWS(bank_loan_data)
```
- **Explanation**: Calculates the percentage of bad loans that are charged off.

---

## 🛠️ Tools Used

- **MS SQL Server**:  
   - Used for writing complex queries to extract, clean, and aggregate loan data.
   - Key SQL operations include `GROUP BY`, `SUM`, `AVG`, and filtering data by month and loan status.
  
- **Power BI**:  
   - Connected to SQL Server to create interactive dashboards for data visualization.
   - Used DAX for advanced calculations and to create visual reports on KPIs, trends, and loan performance.

---

## 📈 Conclusion

The **Bank Loan Analysis** project leverages SQL queries and Power BI to provide insights into the loan portfolio’s performance. By calculating key metrics such as loan applications, funded amounts, and repayment data, this analysis helps stakeholders make informed decisions on loan offerings and risk management.

---

## 👤 Author

[HARSH]  
[kittuharsh75@gmail.com]  
[www.linkedin.com/in/harsh-kit2]

---



