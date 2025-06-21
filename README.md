# FinanceHealth
# 💰 PersonalFinanceHealth: A SQL-Based Personal Finance Management System

**PersonalFinanceHealth** is a SQL project designed to help individuals manage and analyze their personal finances across income, expenses, savings, investments, and debts. It includes a structured schema, sample data, and insightful queries to evaluate financial health metrics.

## 📊 Project Objectives

- Track income, expenses, savings, investments, and debts by user
- Calculate monthly net income and financial health scores
- Identify high-interest debts and major spending areas
- Evaluate asset vs. liability distribution
- Enable data-driven personal finance decision-making

---

## 🧱 Database Schema

The database consists of 7 core tables:

| Table | Description |
|-------|-------------|
| `Users` | Stores basic user data and roles |
| `Income` | Logs income details by source and date |
| `Expenses` | Captures categorized expenses with optional notes |
| `Savings` | Records savings by category and date |
| `Investments` | Logs investment types and return rates |
| `Debts` | Tracks debt amounts, interest rates, and due dates |
| `FinancialHealth` | Scores and tracks the user's overall financial condition |

---

## 🗂️ Sample Data

Each table is preloaded with mock data for demonstration and testing purposes. For example:

```sql
-- Users
INSERT INTO Users (username, role, password_hash) VALUES
('john_doe', 'user', 'hashed_password_123'),
('jane_smith', 'user', 'hashed_password_abc');

````
## 🔍 Key SQL Queries

### 1. 🧮 Monthly Net Income Calculation
```sql
SELECT 
    u.username,
    COALESCE(SUM(i.amount), 0) AS total_income,
    COALESCE(SUM(e.amount), 0) AS total_expense,
    COALESCE(SUM(i.amount), 0) - COALESCE(SUM(e.amount), 0) AS net_income
FROM Users u
LEFT JOIN Income i ON u.user_id = i.user_id AND i.date BETWEEN '2023-10-01' AND '2023-10-31'
LEFT JOIN Expenses e ON u.user_id = e.user_id AND e.date BETWEEN '2023-10-01' AND '2023-10-31'
GROUP BY u.user_id;
````
## 2. 💸 High-Interest Debts (over 10%)

````sql
SELECT u.username, d.amount, d.interest_rate, d.due_date
FROM Users u
JOIN Debts d ON u.user_id = d.user_id
WHERE d.interest_rate > 10.0;
````
## 3. 🧠 Financial Health Score

```` sql
SELECT 
    u.username,
    f.score,
    f.evaluation,
    f.date
FROM Users u
JOIN FinancialHealth f ON u.user_id = f.user_id
WHERE f.date BETWEEN '2023-01-01' AND '2023-12-31';
````
## 4. 📉 Overspending Categories
````sql
SELECT 
    u.username, 
    e.category, 
    SUM(e.amount) AS total_spent
FROM Users u
JOIN Expenses e ON u.user_id = e.user_id
GROUP BY u.username, e.category
HAVING total_spent > 500;
````
## 5. 🏦 Assets vs. Liabilities

````sql
SELECT 
    u.username,
    COALESCE(SUM(s.amount), 0) + COALESCE(SUM(inv.amount), 0) AS total_assets,
    COALESCE(SUM(d.amount), 0) AS total_debts
FROM Users u
LEFT JOIN Savings s ON u.user_id = s.user_id
LEFT JOIN Investments inv ON u.user_id = inv.user_id
LEFT JOIN Debts d ON u.user_id = d.user_id
GROUP BY u.user_id;
````
## 💡 Future Enhancements
- Add stored procedures for automated scoring updates

- Integrate with Power BI for visual dashboards

- Set up alerts for due debts or unusual expenses

- Expand role-based access (admin vs user) with authentication simulation

## 
