# Incident: 2025-04-08 12:08:00

## Summary

Between 12:08 and 12:25 UTC on April 8, 2025, the JWT Pizza service experienced a chaos test in the form of repeated login failures using an unknown user (`unknown@jwt.com`) and a flood of `GET /api/order/menu` requests. These requests were synthetic and unauthenticated, designed to simulate a system under stress.

The system responded correctly in all cases, returning appropriate `404` and `200` responses. However, no alerts were triggered, revealing gaps in our observability strategy. This low-severity incident did not affect real users, but highlighted the need for better alerting rules.

## Detection

This incident was detected manually while reviewing Grafana and Loki logs. No alert was triggered during the event.

While investigating other logs, the team noticed an unusual number of `unknown user` login failures and high-frequency menu queries.

To improve detection time in the future, we have added alert rules that trigger if more than 5 login failures occur within one minute or if more than 10 unauthenticated menu requests happen within two minutes. These will be owned and maintained by the JWT Pizza DevOps.

## Impact

For 17 minutes between 12:08 and 12:25 UTC on 04/08/25, the JWT Pizza service experienced a controlled chaos test. No real users were impacted.

- 0 support tickets
- 0 social media mentions
- 0 real authentication or order failures

## Timeline

All times are UTC:

- 12:08 - First login attempt with `unknown@jwt.com` triggers a 404
- 12:15 - High volume of `GET /api/order/menu` queries begins
- 12:25 - Incident identified as chaos test and resolved

## Response

After reviewing logs during regular monitoring, the incident was identified by the on-call engineer. Because the service was functioning correctly and the traffic was from a Chaos Monkey, no rollback or patch was required. The issue was acknowledged and logged.

## Root cause

A chaos test simulated login attempts using a non-existent user and high-frequency access to the menu endpoint. The root cause was not a bug in the code, but a **lack of sensitive alerting rules** to detect abnormal patterns in traffic and login failures.

## Resolution

The system handled the chaos test as designed. The only issue was observability-related. The resolution involved:

- Reviewing logs and confirming the incident scope
- Enhancing alerting thresholds and queries

## Prevention

No previous incidents share this specific root cause. This test has shown that although our system is resilient to malformed and repeated input, our monitoring was not responsive to behavioral anomalies.

## Action Items (Updated for Grafana Alert Rule system)

### ✅ Maintain and Improve Existing Request Latency Alert

- **Rule name**: `Request latency`
- **Metric**: `latency_milliseconds_total{source="jwt-pizza-service", key="endpoint"}`
- **Condition**: If latency > 500ms within a 1-minute window
- **Pending period**: 1 minute
- **Folder**: `GrafanaCloud`
- **Contact point**: `JWT Pizza DevOps`
- ⏱ Evaluated every 1 minute
- 🔔 Used to detect slow responses from API endpoints like `/api/order/menu`

---

### ✅ Add New Alert Rule for Login Failure Spike

- **Rule name**: `Login Failure Spike`
- **Metric**: Derived from Loki logs that contain `"unknown user"`
- **Query example** (Loki/LogQL):
```logql
{source="jwt-pizza-service"} |= "unknown user"
| count_over_time(1m)
```
- **Alert condition**: Trigger alert when login failures > 5 in 1 minute
- **Suggested action**: Integrate into Prometheus via Loki or Tempo; track using `login_failures_total` and alert on high counts
- **Evaluation group**: `jwt-pizza`
- **Contact point**: `JWT Pizza DevOps`

---

### ✅ Add New Alert Rule for Menu Query Spike

- **Rule name**: `Menu Query Spike`
- **Metric**: Derived from logs or application counter (e.g., `menu_request_count`)
- **Suggested alert condition**: More than 10 requests to `/api/order/menu` within a 2-minute window
- **Example Prometheus expression**:  
```promql
rate(menu_request_count[2m]) > 0.08
```
- **Pending period**: 1 minute
- **Label**: `type=chaos-detection`
- **Folder**: `GrafanaCloud`
- **Contact point**: `JWT Pizza DevOps`
