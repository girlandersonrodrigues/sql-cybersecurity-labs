# Investigation 02 - Shared Credit Cards

## Scenario

The Fraud Prevention team requested an investigation to identify payment cards being used by multiple customer accounts.

Although family members may legitimately share the same payment method, cards linked to several unrelated accounts may indicate account farming, bonus abuse, or fraudulent registrations.

---

## Objective

Identify credit cards associated with three or more different customer accounts.

The investigation should return:

- Card hash
- Last four digits
- Number of customer accounts
- Number of different names
- Number of different cities
- First registration date
- Last registration date

---

## SQL Query

```sql
SELECT
    credit_card_last4,
    card_hash,
    COUNT(DISTINCT customer_id) AS total_customers,
    COUNT(DISTINCT full_name) AS different_names,
    COUNT(DISTINCT city) AS different_cities,
    MIN(registration_datetime) AS first_registration,
    MAX(registration_datetime) AS last_registration
FROM customers_fraud_dataset_pro
WHERE card_hash IS NOT NULL
  AND card_hash <> ''
GROUP BY
    credit_card_last4,
    card_hash
HAVING COUNT(DISTINCT customer_id) >= 3
ORDER BY
    total_customers DESC,
    different_names DESC;
```

---

## Findings

The investigation identified the payment card **CARD9999** linked to **15 different customer accounts** distributed across **5 cities**.

The registrations occurred between **2026-01-02** and **2026-06-21**, indicating recurring reuse of the same payment method.

While card sharing can legitimately occur among family members, the number of associated accounts and geographic distribution increase the fraud risk.

---

## Recommendation

Further investigation is recommended by correlating:

- Address
- Phone number
- IP addresses
- Login history

  ## Query Result

![Result](result.png)
- Device fingerprints
- Purchase history

These additional indicators can help determine whether the activity represents legitimate family usage or fraudulent multi-account creation.
