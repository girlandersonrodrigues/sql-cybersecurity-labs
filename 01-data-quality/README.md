# Data Quality Analysis

## Overview

This project focuses on evaluating the quality of data stored in cybersecurity-related datasets.

Before performing threat detection, fraud analysis, or security investigations, it is important to confirm that the data is complete, consistent, valid, and reliable.

Poor-quality data can generate incorrect conclusions, false positives, and inaccurate security reports.

---

## Objectives

The objectives of this analysis are to identify:

- Missing values
- Duplicate records
- Invalid values
- Inconsistent data formats
- Unexpected values
- Incomplete records
- Data that may affect security investigations

---

## Dataset

The analysis was performed using cybersecurity-related tables such as:

- `authentication_logs`
- `firewall_logs`
- `customers`
- `employees`

The main focus was verifying whether the data could be reliably used in security and fraud investigations.

---

## Data Quality Dimensions

### Completeness

Checks whether important fields contain missing or null values.

Examples:

- Missing usernames
- Missing source IP addresses
- Missing countries
- Missing timestamps

### Uniqueness

Checks whether records that should be unique appear more than once.

Examples:

- Duplicate users
- Duplicate email addresses
- Repeated customer IDs
- Duplicate log records

### Validity

Checks whether values follow the expected business rules.

Examples:

- Invalid authentication status
- Invalid country values
- Incorrect IP address formats
- Unexpected firewall actions

### Consistency

Checks whether related information follows the same format across the dataset.

Examples:

- `Brazil`, `BRAZIL`, and `brazil`
- `FAILED`, `Failed`, and `failed`
- Different date formats
- Inconsistent department names

---

## SQL Checks

The SQL queries used in this analysis are available in:

### Identify Missing Values

```sql
SELECT *
FROM authentication_logs
WHERE username IS NULL
   OR source_ip IS NULL
   OR country IS NULL
   OR status IS NULL
   OR login_time IS NULL;
```

---

### Find Duplicate Authentication Records

```sql
SELECT
    username,
    source_ip,
    status,
    login_time,
    COUNT(*) AS duplicate_count
FROM authentication_logs
GROUP BY
    username,
    source_ip,
    status,
    login_time
HAVING COUNT(*) > 1
ORDER BY duplicate_count DESC;
```

---

### Validate Authentication Status

```sql
SELECT DISTINCT status
FROM authentication_logs
ORDER BY status;
```

---

### Detect Invalid Status Values

```sql
SELECT *
FROM authentication_logs
WHERE status NOT IN ('SUCCESS', 'FAILED')
   OR status IS NULL;
```

---

### Analyze Data Distribution

```sql
SELECT
    country,
    COUNT(*) AS total_records
FROM authentication_logs
GROUP BY country
ORDER BY total_records DESC;
```

## Security Relevance

Data quality is important in cybersecurity because incomplete or incorrect information may:

- Hide malicious activity
- Generate false positives
- Affect incident investigation
- Produce inaccurate dashboards
- Compromise fraud detection
- Prevent correct user identification
- Affect alert prioritization

For example, a missing `source_ip` may prevent an analyst from identifying the origin of a suspicious login attempt.

---

## Key Learning

Before analyzing security threats, analysts must validate whether the available data is reliable.

Data quality analysis is an important first step in:

- Threat hunting
- Fraud detection
- Incident response
- Security monitoring
- Risk analysis
- Executive reporting

---

## Tools

- PostgreSQL
- DBeaver
- SQL
- GitHub

---

## Author

Girlanderson Rodrigues

Cybersecurity Analyst | CompTIA Security+ | SQL | Threat Detection
