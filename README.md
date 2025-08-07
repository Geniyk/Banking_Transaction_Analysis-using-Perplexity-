
## Banking_Transaction_Analysis Dashboard (using Perplexity)

# Dashoard Link: https://app.powerbi.com/groups/me/reports/4afca4d3-4066-472c-839b-bc665b87b4bd/404fe6924be0e50d4ef4?experience=power-bi

## Problem Statement:

This dashboard helps banking analysts and financial institutions understand customer behavior, account activity, and transactional trends more effectively. Using a relational SQL Server dataset integrated with Power BI, the report tracks account balances, transaction types, high-value outliers, inactive customers, and demographic insights to help detect anomalies, improve customer engagement, and monitor financial health across customer segments.

The dataset includes over 10,000 transactions, embedded with intentional data quality issues (mixed date formats, nulls, duplicates, inconsistent text casing, and foreign key mismatches) to simulate real-world messy datasets. This enables users to practice data cleaning and transformation using Power Query Editor.

The Power BI dashboard spans two pages, each showing 5 actionable KPIs to support decision-making on financial performance, customer management, and fraud detection.


### Steps followed:

- Step 1 : Generated a banking dataset using Perplexity AI by providing a detailed prompt.
The dataset included the following tables with ~10,000 records and intentional data quality issues:

- Customers
- Accounts
- Transactions
- Data issues introduced:
(a) Null values in columns like Email, Description

(b) Duplicate rows

(c) Mixed casing in text fields

(d) Inconsistent date formats

(e) Trailing spaces in City and Name columns

- Step 2 : Executed SQL queries from Perplexity to create and populate the tables inside SQL Server using Power_BI2 database.

- step 3: Connected Power BI Desktop to SQL Server database via "Get Data" → SQL Server → Connected to Power_BI2 and imported the 3 tables.

- step 4: Opened Power Query Editor in Power BI and enabled:

(a) Column Distribution

(b) Column Quality

(c) Column Profile

Also changed data profiling from 1000 rows to Entire Dataset for accurate insights.

- Step 5 : Cleaned the data using Power Query transformations:

(a) Removed duplicates

(b) Standardized date formats

(c) Trimmed text fields (Name, City)

(d) Replaced null values in Email with "Not Provided"

(e) Formatted mixed casing using Text.Proper()

- step 6:  Verified table relationships:

(a) Customers[CustomerID] → Accounts[CustomerID]

(b) Accounts[AccountID] → Transactions[AccountID]

- step 7: Created 2 Power BI Report Pages:

(a) Page 1: Account & Customer Overview

(b) Page 2: Transaction Insights

## 🔹 Page 1: Banking KPIs & Trends

- step 8: Measures Table was created:
(a) Transactions Count

Following DAX expression was written for the same,
Transactions Count = COUNT(CombinedBankingDataset[TransactionID])

(b) Monthly Transaction Amount

Following DAX expression was written for the same,
Monthly Transaction Amount = CALCULATE(SUM(CombinedBankingDataset[Amount]), ALLEXCEPT(CombinedBankingDataset, CombinedBankingDataset[TransactionDate].[Month]))

(c) Total Balance

Following DAX expression was written for the same,
Total Balance = SUM(CombinedBankingDataset[Balance])

(d) Inactive Accounts

Following DAX expression was written for the same,
Inactive Accounts = CALCULATE(DISTINCTCOUNT(CombinedBankingDataset[Account_AccountID]), FILTER(VALUES(CombinedBankingDataset[Account_AccountID]), CALCULATE(MAX(CombinedBankingDataset[TransactionDate])) < TODAY()-90))

(e) Average Balance

Following DAX expression was written for the same,
Average Balance = AVERAGE(CombinedBankingDataset[Balance])

# Snapshot of Dashboard_Page_1
![Image](https://github.com/user-attachments/assets/bec89783-bdef-442e-9771-56a8d5f50970)

## 🔹 Page 2: Customer & Account Demographics
- step 9: Measures Table was created:
(a) Customer Count by Gender

Following DAX expression was written for the same,
Customer Count by Gender = DISTINCTCOUNT(CombinedBankingDataset[CustomerID])

(b) Account Count by Type

Following DAX expression was written for the same,
Account Count by Type = COUNT(CombinedBankingDataset[Account_AccountID])

(c) Monthly Transaction Balance

Following DAX expression was written for the same,
Monthly Transaction Balance = CALCULATE(SUM(CombinedBankingDataset[Balance]), ALLEXCEPT(CombinedBankingDataset, CombinedBankingDataset[TransactionDate].[Month]))


- step 10: New Columns were added in CombinedBankingDataset to determine Number of Customers by Age-group.

(a) Customer Age

Following DAX expression was written for the same,
Customer Age = DATEDIFF(CombinedBankingDataset[DateOfBirth], TODAY(), YEAR)


(b) Customer Age Group

Following DAX expression was written for the same,
Customer Age Group = SWITCH(TRUE(), [Customer Age]<=25, "<=25", [Customer Age]<=35, "26-35", [Customer Age]<=50,"36-50","51+")


# Snapshot of Dashboard_Page_2
![Image](https://github.com/user-attachments/assets/068dc142-fa75-486d-9493-6209608ff08f)


## Insights:

A two page report was created on Power BI Desktop & it was then published to Power BI Service.

Following inferences can be drawn from the dashboard;

## Account Balances by Type

(a) Current accounts have a highly negative balance: -15,782.44K

(b) Savings accounts maintain a small positive balance: 1.48K

## Average Balance
(a) The average balance is negative: -39.45K

(b) Range: from -78.90K to 0.00

## Inactive Accounts (By Year and Month)

(a) Inactive accounts climbed rapidly from 2 in January 2024 to a steady 5 from July 2024 onwards

(b) The peak growth was between April and July 2024

## Total Amount by Name

(a) Sarah Khan: positive net amount of 0.26M

(b) Priya Singh: negative net amount of -0.25M

## Monthly Transactions Amount by Months

(a) Highest transaction months: January and July (both at 4.0M)

(b) Lowest: May (0.2M)

(c) Stable or recovering through the second half of the year (2.8M–2.9M per month)

## Transactions Count by Transaction Type

(a) Debit: 5K (50%)

(b) Credit: 5K (50%)

(even split between debit and credit transactions)

## Monthly Transactions Balance by Months

(a) Consistently negative balances throughout the year (range: -0.95M to -1.74M)

(b) Lowest: May and October (-1.74M)

(c) Best months (least negative): July, November, December (all at -0.95M)

## Account Count by Account Type
(a) Equal number of Current and Savings accounts: 200 each

## Customer Segmentation
By Age Group:

(a) 36–50 years: 200 customers

(b) 26–35 years: 100 customers

(Majority are in the 36–50 age group)

By Gender:

(a) Female: 25%

(b) Male: 25%

(c) Unspecified/Blank: 50%

## Total Balance by Name

(a) Mark Lee: positive balance of 5K

(b) Priya Singh: highly negative balance of -15,787.44K

(c) Sarah Khan: neutral/zero total balance

