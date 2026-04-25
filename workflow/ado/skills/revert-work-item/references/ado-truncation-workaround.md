# ADO Revision API Truncation Workaround

## The Problem

The `list_work_item_revisions` API truncates large text fields (Description, Acceptance Criteria) at **8,192 characters**. Restoring directly from revision data silently discards content beyond that limit.

## The Workaround: `get_work_item` with `asOf`

Use `get_work_item` with the `asOf` parameter to fetch the complete, untruncated field values as they existed at a specific point in time.

### Calculating the `asOf` Timestamp

To restore **revision N**, you need the state at the end of revision N — which is the moment just before revision N+1 was saved.

**Formula:** `asOf = ChangedDate of revision (N+1) minus 1 minute`

**Example:**

| Rev | System.ChangedDate           |
|-----|------------------------------|
| 4   | `2026-04-24T06:39:53Z`       |
| 5   | `2026-04-24T09:19:46Z`  ← accidental edit |

To restore rev 4: use `asOf: 2026-04-24T09:18:00Z` (1 minute before rev 5).

### Edge Case: Restoring the Latest Revision

If the user wants to restore the *current* revision (e.g. to fix truncation in a previous restore), use the current wall-clock time as `asOf`, or omit it entirely.

### Edge Case: Restoring the First Revision

If there is no next revision, use `asOf` set to the `ChangedDate` of revision 1 plus 1 minute.

## Verification

After restoring, re-fetch with `get_work_item` (no `asOf`) and compare the `System.Description` length to the value fetched via `asOf`. They should match. A mismatch indicates truncation occurred during the update call — this is rare but can happen if the update payload itself was malformed.
