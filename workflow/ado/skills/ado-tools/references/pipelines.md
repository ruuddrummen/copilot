# Pipelines Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## pipelines_create_pipeline

Create a pipeline definition with YAML configuration.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `name` | string | **Yes** | Pipeline name |
| `yamlPath` | string | **Yes** | Path to YAML file in the repo |
| `repositoryType` | string | **Yes** | Repository type |
| `repositoryName` | string | **Yes** | Repository name |
| `folder` | string | No | Pipeline folder path |
| `repositoryConnectionId` | string | No | Repository connection ID |
| `repositoryId` | string | No | Repository ID |

## pipelines_get_builds

List builds for a project with optional filters.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `definitions` | array | No | Filter by definition IDs |
| `branchName` | string | No | Filter by branch |
| `buildNumber` | string | No | Filter by build number |
| `buildIds` | array | No | Filter by build IDs |
| `statusFilter` | string | No | Filter by status |
| `resultFilter` | string | No | Filter by result |
| `reasonFilter` | string | No | Filter by reason |
| `requestedFor` | string | No | Filter by requester |
| `repositoryId` | string | No | Filter by repository |
| `repositoryType` | string | No | Filter by repository type |
| `tagFilters` | array | No | Filter by tags |
| `queues` | array | No | Filter by queue |
| `top` | number | No | Max results |
| `continuationToken` | string | No | Pagination token |
| `maxBuildsPerDefinition` | number | No | Max builds per definition |
| `deletedFilter` | string | No | Deleted state filter |
| `queryOrder` | string | No | Result ordering |
| `minTime` | string | No | Min time filter |
| `maxTime` | string | No | Max time filter |
| `properties` | array | No | Properties to include |

## pipelines_get_build_status

Get status of a specific build.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |

## pipelines_get_build_log

Retrieve logs for a build.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |

## pipelines_get_build_log_by_id

Get a specific build log by log ID.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |
| `logId` | number | **Yes** | Log ID |
| `startLine` | number | No | Start line in log |
| `endLine` | number | No | End line in log |

## pipelines_get_build_changes

Get commits associated with a build.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |
| `top` | number | No | Max results |
| `includeSourceChange` | boolean | No | Include source change |
| `continuationToken` | string | No | Pagination token |

## pipelines_get_build_definitions

List build/pipeline definitions in a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `name` | string | No | Filter by name |
| `path` | string | No | Filter by folder path |
| `repositoryId` | string | No | Filter by repository |
| `repositoryType` | string | No | Filter by repository type |
| `top` | number | No | Max results |
| `continuationToken` | string | No | Pagination token |
| `definitionIds` | array | No | Filter by definition IDs |
| `queryOrder` | string | No | Result ordering |
| `includeLatestBuilds` | boolean | No | Include latest builds |
| `includeAllProperties` | boolean | No | Include all properties |
| `minMetricsTime` | string | No | Min metrics time |
| `builtAfter` | string | No | Built after date |
| `notBuiltAfter` | string | No | Not built after date |
| `taskIdFilter` | string | No | Filter by task ID |
| `processType` | number | No | Process type |
| `yamlFilename` | string | No | YAML file name filter |

## pipelines_get_build_definition_revisions

Get revision history of a build definition.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `definitionId` | number | **Yes** | Definition ID |

## pipelines_run_pipeline

Start a new pipeline run.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `pipelineId` | number | **Yes** | Pipeline ID |
| `pipelineVersion` | number | No | Pipeline version |
| `previewRun` | boolean | No | Preview only, don't run |
| `resources` | object | No | Pipeline resources |
| `stagesToSkip` | array | No | Stages to skip |
| `templateParameters` | object | No | Template parameters |
| `variables` | object | No | Pipeline variables |
| `yamlOverride` | string | No | YAML override |

## pipelines_get_run

Get details of a specific pipeline run.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `pipelineId` | number | **Yes** | Pipeline ID |
| `runId` | number | **Yes** | Run ID |

## pipelines_list_runs

List recent runs for a pipeline.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `pipelineId` | number | **Yes** | Pipeline ID |

## pipelines_update_build_stage

Update a build stage (cancel, retry, or run).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |
| `stageName` | string | **Yes** | Stage name |
| `status` | string | **Yes** | New status |
| `forceRetryAllJobs` | boolean | No | Force retry all jobs |

## pipelines_list_artifacts

List artifacts produced by a build.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |

## pipelines_download_artifact

Download a pipeline artifact.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `buildId` | number | **Yes** | Build ID |
| `artifactName` | string | **Yes** | Artifact name |
| `destinationPath` | string | No | Local path to save |

### Gotchas

- `pipelines_run_pipeline` uses `pipelineId`, not `definitionId`.
- `pipelines_list_artifacts` and `pipelines_download_artifact` use `buildId`, not `runId`.
- There is no generic `pipelines_list` or `pipelines_get` tool — use `pipelines_get_build_definitions` to list pipelines and `pipelines_get_build_status` or `pipelines_get_run` for details.
