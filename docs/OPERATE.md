# Operate

## Signals covered
- **Risky users (updates)**
- **Risky sign-ins (updates)**

Both sections share the same lookback window and default filters (**Medium** and **High** only).

---

## Tuning
- **Concurrency:** set the trigger concurrency to `1` to avoid bursty Teams posts.
- **Payload size:** cap each list (e.g., 10–20 items) to stay under the Teams ~28 KB card limit.
- **No updates → no post:** add a condition before the Teams action to skip sending when all counts are `0`.

Example condition (pseudo):
```
@and(equals(outputs('CountRiskyUsers'), 0), equals(outputs('CountRiskySignIns'), 0))
```

---

## Troubleshooting
- **Runs history → Inputs/Outputs:** verify field mappings and API responses.
- **Connector auth:** if Teams actions fail, open the Teams connector in the workflow and re-sign.
- **Rate limiting (if you call Graph):** wrap calls with simple retry (Delay + Retry) and short backoff.

---

## Operations
- **Change control:** update parameters and re-deploy; workflows are idempotent.
- **Monitoring:** enable diagnostic logs to Log Analytics (`WorkflowRuntime` table).
- **Rollback:** disable the workflow or remove the playbook from Sentinel alert rules.
