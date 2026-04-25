# Work Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## work_list_iterations

List all iterations defined for a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `depth` | number | No | Depth of child iterations to include |
| `excludedIds` | array | No | Iteration IDs to exclude |

## work_create_iterations

Create new iterations in a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `iterations` | array | **Yes** | Array of iteration definitions to create |

## work_list_team_iterations

List iterations assigned to a specific team.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |
| `timeframe` | string | No | `past`, `current`, `future` |

## work_assign_iterations

Assign existing iterations to a team.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |
| `iterations` | array | **Yes** | Iterations to assign |

## work_get_iteration_capacities

Get capacity for all teams in an iteration.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `iterationId` | string | **Yes** | Iteration ID |

## work_get_team_capacity

Get capacity for a specific team in an iteration.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |
| `iterationId` | string | **Yes** | Iteration ID |

## work_update_team_capacity

Update team member capacity for an iteration.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | **Yes** | Team name |
| `teamMemberId` | string | **Yes** | Team member identity ID |
| `iterationId` | string | **Yes** | Iteration ID |
| `activities` | array | **Yes** | Activity capacities |
| `daysOff` | array | No | Days off entries |

## work_get_team_settings

Get team settings (default iteration, backlog iteration, default area path).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `team` | string | No | Team name |

### Gotchas

- `work_list_team_iterations` lists iterations assigned to a team; `work_list_iterations` lists all iterations in the project regardless of team assignment.
- There are no `work_get_team_members`, `work_get_boards`, or `work_get_board_columns` tools in the current toolset.
