# Detecting Successful Logins Outside Business Hours

## Business Scenario

The Security Operations Center (SOC) requested an investigation to identify successful logins performed outside normal business hours.

Although employees may occasionally work overtime or remotely, authentication events occurring during unusual hours may indicate compromised accounts, unauthorized access, or insider threats. The objective is to identify these events for further analysis.

---

## Objective

Identify successful authentication events that occurred:

- Before **06:00 AM**
- After **10:00 PM**

The investigation returns:

- Username
- Customer ID
- Login timestamp
- Source IP
- Country
- Authentication method
- Device type
- Alert classification

---

## SQL Query

```sql
SELECT
    username,
    customer_id,
    login_time,
    source_ip,
    country,
    auth_method,
    device_type,
    CASE
        WHEN login_time::time < TIME '06:00:00'
            THEN 'Login before business hours'
        WHEN login_time::time > TIME '22:00:00'
            THEN 'Login after business hours'
    END AS alert_type
FROM sql_authentication_investigation_dataset
WHERE status = 'SUCCESS'
    AND username IS NOT NULL
    AND username <> ''
    AND login_time IS NOT NULL
    AND (
        login_time::time < TIME '06:00:00'
        OR login_time::time > TIME '22:00:00'
    )
ORDER BY login_time DESC;
```

---

## Query Result

> Insert the SQL query result screenshot below.

![Result](result.png)

---

# Findings

The investigation identified **six successful authentication events** performed outside normal business hours.

Among these events, successful logins originated from IP addresses geolocated in countries such as **Germany** and **New Zealand**, which may represent unusual behavior depending on each user's historical activity.

While unusual login times alone do not confirm malicious activity, they represent behavioral anomalies that deserve additional investigation when combined with other indicators.

---

## Risk Assessment

Authentication outside business hours should **not** automatically be considered a security incident.

Legitimate situations include:

- Remote work
- Overtime
- Maintenance activities
- Business travel
- Time zone differences
- Corporate VPN usage

Likewise, IP geolocation alone is not sufficient to determine the user's physical location, since VPNs, proxies, cloud providers, and ISP routing may influence the reported country.

However, the risk increases significantly when these events are correlated with additional indicators, such as:

- First login from a new country
- Unknown device
- Suspicious IP reputation
- Previous failed authentication attempts
- MFA disabled or bypassed
- Sensitive actions performed after authentication

---

## Recommendations

Recommended next steps include:

- Compare the login with the user's historical behavior.
- Verify IP reputation using VirusTotal or AbuseIPDB.
- Validate whether the user was traveling or working remotely.
- Correlate successful logins with previous failed authentication attempts.
- Review activities performed after authentication.
- Investigate changes to passwords, MFA configuration, permissions, or account settings.
- Prioritize privileged accounts for immediate review.
- Block sessions or reset credentials only if additional indicators of compromise are identified.

---

## Conclusion

Six successful authentication events were identified outside standard business hours.

Although these events alone do not confirm unauthorized access, they represent behavioral anomalies that warrant further investigation. Correlating login time with IP reputation, user history, authentication method, device information, and post-login activities enables more accurate detection of potential account compromise while reducing false positives.

---

## Skills Demonstrated

- SQL Filtering
- CASE Expressions
- Timestamp Analysis
- Time-Based Detection
- Authentication Log Analysis
- Behavioral Analysis
- UEBA Concepts
- Security Monitoring
- Threat Hunting
- Cybersecurity Reporting
