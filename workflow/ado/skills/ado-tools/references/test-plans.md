# Test Plans Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## testplan_list_test_plans

List test plans in a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `filterActivePlans` | boolean | No | Only show active plans |
| `includePlanDetails` | boolean | No | Include plan details |
| `continuationToken` | string | No | Pagination token |

## testplan_create_test_plan

Create a new test plan.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `name` | string | **Yes** | Plan name |
| `iteration` | string | **Yes** | Iteration path |
| `areaPath` | string | No | Area path |
| `description` | string | No | Plan description |
| `startDate` | string | No | Start date |
| `endDate` | string | No | End date |

## testplan_list_test_suites

List test suites in a test plan (hierarchical structure).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `planId` | number | **Yes** | Test plan ID |
| `continuationToken` | string | No | Pagination token |

## testplan_create_test_suite

Create a test suite within a test plan.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `planId` | number | **Yes** | Test plan ID |
| `parentSuiteId` | number | **Yes** | Parent suite ID |
| `name` | string | **Yes** | Suite name |

## testplan_add_test_cases_to_suite

Add existing test cases to a test suite.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `planId` | number | **Yes** | Test plan ID |
| `suiteId` | number | **Yes** | Suite ID |
| `testCaseIds` | array | **Yes** | Array of test case IDs |

## testplan_list_test_cases

List test cases in a test suite.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `planid` | number | **Yes** | Test plan ID |
| `suiteid` | number | **Yes** | Test suite ID |
| `continuationToken` | string | No | Pagination token |

> **Note**: Parameter names use lowercase `planid` and `suiteid` (not camelCase).

## testplan_create_test_case

Create a new test case work item.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `title` | string | **Yes** | Test case title |
| `steps` | array | No | Test steps |
| `areaPath` | string | No | Area path |
| `iterationPath` | string | No | Iteration path |
| `priority` | number | No | Priority |
| `testsWorkItemId` | number | No | Work item ID to associate |

## testplan_update_test_case_steps

Update steps of an existing test case.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `id` | number | **Yes** | Test case ID |
| `steps` | array | **Yes** | Updated test steps |

## testplan_show_test_results_from_build_id

Get test results for a specific build.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildid` | number | **Yes** | Build ID |
| `outcomes` | array | No | Filter by outcome (`Failed`, `Passed`, `Aborted`) |

> **Note**: Parameter name uses lowercase `buildid` (not camelCase).

### Gotchas

- Test plan tools use the `testplan_` prefix, not `test_`.
- `testplan_list_test_cases` uses lowercase parameter names (`planid`, `suiteid`).
- `testplan_show_test_results_from_build_id` uses lowercase `buildid`.
- There is no `testplan_get_test_case` tool for retrieving a single test case by ID — use `wit_get_work_item` instead.
