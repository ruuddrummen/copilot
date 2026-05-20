# Advanced Security Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## advsec_get_alerts

Retrieve Advanced Security alerts for a repository.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `repository` | string | **Yes** | Repository name or ID |
| `confidenceLevels` | array | **Yes** | Confidence level filter |
| `alertType` | string | No | Alert type filter |
| `states` | array | No | Alert state filter |
| `severities` | array | No | Severity filter |
| `ruleId` | string | No | Filter by rule ID |
| `ruleName` | string | No | Filter by rule name |
| `toolName` | string | No | Filter by tool name |
| `ref` | string | No | Branch ref |
| `onlyDefaultBranch` | boolean | No | Only default branch alerts |
| `top` | number | No | Max results |
| `orderBy` | string | No | Result ordering |
| `validity` | string | No | Validity filter |
| `continuationToken` | string | No | Pagination token |

## advsec_get_alert_details

Get detailed information about a specific security alert.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `repository` | string | **Yes** | Repository name or ID |
| `alertId` | number | **Yes** | Alert ID |
| `ref` | string | No | Branch ref |

### Gotchas

- The tools are `advsec_get_alerts` (not `advsec_list_alerts`) and `advsec_get_alert_details` (not `advsec_get_alert`).
- `advsec_get_alerts` requires `confidenceLevels` — it's not optional.
- The `repository` parameter is used (not `repositoryId`).
