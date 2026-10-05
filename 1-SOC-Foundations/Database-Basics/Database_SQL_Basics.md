# 🛢️ Database Infrastructure & SQL Basics

**Module:** Software Basics  
**Chapter:** Introduction to Databases / Café SQL  
**Objective:** Understand data persistence, analyze relational table structures (tables, rows, columns), and engineer multi-stage structured search queries (`SELECT`, `FROM`, `WHERE`, `ORDER BY`).

---

## 🏗️ Relational Database Infrastructure

When tracking thousands of system operations, user logins, or network events, storing information in standard flat text files becomes slow and inefficient. Databases solve this scalability challenge by organizing data into rigid, indexed structures that a computer can search, sort, and parse in seconds.

### 📊 Structural Breakdown of a Table
Inside a database, information lives within structured grids called **Tables**. 

*   **Columns (Attributes):** The vertical headers at the top of a table. They define the explicit data type or category stored (e.g., `price`, `drink`, `time`).
*   **Rows (Records / Tuples):** The horizontal entries across a table. **One single row represents a complete, unique transaction or historical record** (e.g., a single customer order profile).

---

## 🧭 The Café SQL Schema

The lab environment provides a dual-table sandbox environment mapping out sales transactions and catalog items:
*   **`Orders` Table Columns:** `id`, `drink`, `price`, `time`
*   **`Menu` Table Columns:** `drink`, `price`

---

## ⌨️ Practical SQL Query Playbook

**SQL (Structured Query Language)** is the language utilized to construct specific, conditional questions—called **Queries**—to pull telemetry results out of a database without changing the underlying files.

### 1. Retrieve Global Records (Select All)
Pulls every column and row from the targeted table. The asterisk `*` represents a global wildcard for all attributes.
```sql
SELECT * FROM Orders;
```

### 2. Isolate Target Attributes (Specific Column Projections)
Narrows down results to display only the requested informational columns, saving processing overhead.
```sql
SELECT drink, price FROM Orders;
```

### 3. Record Filtering & Extraction Constraints (`WHERE`)
Applies strict logic evaluation criteria to display only the rows that match an explicit value.
```sql
SELECT * FROM Orders WHERE drink = 'Coffee';
```

### 4. Sequential Sorting Metrics (`ORDER BY`)
Sorts results based on an attribute column. By default, it organizes in ascending order (lowest to highest). Appending `DESC` forces a reverse descending order sequence (highest to lowest).
```sql
-- Lowest price first
SELECT * FROM Orders ORDER BY price;

-- Highest price first
SELECT * FROM Orders ORDER BY price DESC;
```

### 5. Multi-Stage Filter & Sort Integration
Combines row-level conditional mapping with an active sorting engine to extract high-value telemetry profiles cleanly.
```sql
SELECT * FROM Orders WHERE drink = 'Coffee' ORDER BY price DESC;
```
