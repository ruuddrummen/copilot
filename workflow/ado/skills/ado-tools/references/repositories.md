# Repositories Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## repo_list_repos_by_project

List all repositories in a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `repoNameFilter` | string | No | Filter by repository name |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |

## repo_get_repo_by_name_or_id

Get repository details by name or ID.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `repositoryNameOrId` | string | **Yes** | Repository name or ID |

## repo_list_branches_by_repo

List all branches in a repository.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `filterContains` | string | No | Filter branches containing this string |
| `top` | number | No | Max results |

## repo_list_my_branches_by_repo

List branches created by the current user.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `filterContains` | string | No | Filter branches containing this string |
| `top` | number | No | Max results |

## repo_get_branch_by_name

Get details of a specific branch.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `branchName` | string | **Yes** | Branch name |

## repo_create_branch

Create a new branch in a repository.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `branchName` | string | **Yes** | New branch name |
| `sourceBranchName` | string | No | Source branch to branch from |
| `sourceCommitId` | string | No | Source commit SHA |

## repo_search_commits

Search for commits with comprehensive filtering.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `repository` | string | **Yes** | Repository name or ID |
| `author` | string | No | Filter by author name |
| `authorEmail` | string | No | Filter by author email |
| `committer` | string | No | Filter by committer name |
| `committerEmail` | string | No | Filter by committer email |
| `searchText` | string | No | Search text in commits |
| `fromDate` | string | No | Start date filter |
| `toDate` | string | No | End date filter |
| `version` | string | No | Branch/tag/commit version |
| `versionType` | string | No | Version type |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `commitIds` | array | No | Filter by specific commit IDs |
| `fromCommit` | string | No | Starting commit |
| `toCommit` | string | No | Ending commit |
| `includeLinks` | boolean | No | Include links |
| `includeWorkItems` | boolean | No | Include linked work items |
| `historySimplificationMode` | string | No | History simplification mode |

## repo_list_directory

List files and folders in a repository directory.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `path` | string | No | Directory path (defaults to root) |
| `project` | string | No | Project name |
| `version` | string | No | Branch/tag/commit |
| `versionType` | string | No | Version type |
| `recursive` | boolean | No | Recurse into subdirectories |
| `recursionDepth` | number | No | Max recursion depth |

## repo_get_file_content

Get file content from a repository.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `path` | string | **Yes** | File path within the repo |
| `project` | string | No | Project name |
| `version` | string | No | Branch/tag/commit |
| `versionType` | string | No | Version type |

## repo_list_pull_requests_by_repo_or_project

List pull requests with optional filters.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | No | Repository name or ID |
| `project` | string | No | Project name |
| `status` | string | No | `active`, `completed`, `abandoned`, `all` |
| `sourceRefName` | string | No | Source branch ref |
| `targetRefName` | string | No | Target branch ref |
| `created_by_me` | boolean | No | Filter to PRs created by me |
| `created_by_user` | string | No | Filter by creator |
| `i_am_reviewer` | boolean | No | Filter to PRs where I'm reviewer |
| `user_is_reviewer` | string | No | Filter by reviewer |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |

> Either `repositoryId` or `project` must be provided.

## repo_list_pull_requests_by_commits

Find pull requests containing specific commits.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | **Yes** | Project name |
| `repository` | string | **Yes** | Repository name or ID |
| `commits` | array | **Yes** | Array of commit SHAs |
| `queryType` | string | No | Query type |

## repo_get_pull_request_by_id

Get details of a specific pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `project` | string | No | Project name |
| `includeWorkItemRefs` | boolean | No | Include linked work items |
| `includeLabels` | boolean | No | Include labels |
| `includeChangedFiles` | boolean | No | Include changed files summary |

## repo_get_pull_request_changes

Get file changes (diff) for a pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `project` | string | No | Project name |
| `iterationId` | number | No | Specific iteration (defaults to latest) |
| `compareTo` | number | No | Compare to another iteration |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `includeDiffs` | boolean | No | Include diff info (default: true) |
| `includeLineContent` | boolean | No | Include actual code lines (default: true) |

## repo_create_pull_request

Create a new pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `sourceRefName` | string | **Yes** | Source branch (e.g., `refs/heads/feature`) |
| `targetRefName` | string | **Yes** | Target branch (e.g., `refs/heads/main`) |
| `title` | string | **Yes** | PR title |
| `description` | string | No | PR description |
| `isDraft` | boolean | No | Create as draft |
| `labels` | array | No | Labels to apply |
| `workItems` | array | No | Work items to link |
| `forkSourceRepositoryId` | string | No | Fork source repo ID |

## repo_update_pull_request

Update pull request properties.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `title` | string | No | New title |
| `description` | string | No | New description |
| `status` | string | No | `active`, `completed`, `abandoned` |
| `targetRefName` | string | No | New target branch |
| `isDraft` | boolean | No | Draft state |
| `autoComplete` | boolean | No | Enable auto-complete |
| `deleteSourceBranch` | boolean | No | Delete source after merge |
| `mergeStrategy` | string | No | Merge strategy |
| `bypassReason` | string | No | Bypass reason |
| `transitionWorkItems` | boolean | No | Transition linked work items |

## repo_update_pull_request_reviewers

Add or remove PR reviewers.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `reviewerIds` | array | **Yes** | Reviewer identity IDs |
| `action` | string | **Yes** | `add` or `remove` |

## repo_vote_pull_request

Cast a vote on a pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `vote` | number | **Yes** | Vote value |

## repo_list_pull_request_threads

List comment threads on a pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `project` | string | No | Project name |
| `iteration` | number | No | Iteration filter |
| `baseIteration` | number | No | Base iteration |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `fullResponse` | boolean | No | Return full response |

## repo_list_pull_request_thread_comments

List comments in a specific PR thread.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `threadId` | number | **Yes** | Thread ID |
| `project` | string | No | Project name |
| `top` | number | No | Max results |
| `skip` | number | No | Results to skip |
| `fullResponse` | boolean | No | Return full response |

## repo_create_pull_request_thread

Create a new comment thread on a pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `content` | string | **Yes** | Comment content |
| `project` | string | No | Project name |
| `filePath` | string | No | File path for file-level comments |
| `rightFileStartLine` | number | No | Start line for inline comments |
| `rightFileStartOffset` | number | No | Start offset |
| `rightFileEndLine` | number | No | End line |
| `rightFileEndOffset` | number | No | End offset |
| `status` | string | No | Thread status |

## repo_update_pull_request_thread

Update a PR comment thread status.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `threadId` | number | **Yes** | Thread ID |
| `project` | string | No | Project name |
| `status` | string | No | `active`, `fixed`, `closed`, `byDesign`, `pending`, `wontFix` |

## repo_reply_to_comment

Reply to a comment on a pull request.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `repositoryId` | string | **Yes** | Repository name or ID |
| `pullRequestId` | number | **Yes** | Pull request ID |
| `threadId` | number | **Yes** | Thread ID |
| `content` | string | **Yes** | Reply content |
| `project` | string | No | Project name |
| `fullResponse` | boolean | No | Return full response |

### Gotchas

- Branch parameters for PRs require full ref paths: `refs/heads/main`, not just `main`.
- There are no `repo_create_file` or `repo_update_file` tools — use `repo_create_pull_request` with commits instead.
- `repo_list_pull_requests_by_repo_or_project` requires either `repositoryId` or `project` (not both required, but at least one).
- `repo_get_file_content` uses `version` for branch/tag, not `branch`.
