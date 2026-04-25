# Wiki Tools

> Last verified against [TOOLSET.md](https://github.com/microsoft/azure-devops-mcp/blob/main/docs/TOOLSET.md): 2026-04-23

## wiki_list_wikis

List wikis in the organization or a project.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `project` | string | No | Project name (omit to list all org wikis) |

## wiki_get_wiki

Get details of a specific wiki.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `wikiIdentifier` | string | **Yes** | Wiki name or ID |
| `project` | string | No | Project name |

## wiki_list_pages

List pages in a wiki.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `wikiIdentifier` | string | **Yes** | Wiki name or ID |
| `project` | string | **Yes** | Project name |
| `top` | number | No | Max results |
| `continuationToken` | string | No | Pagination token |
| `pageViewsForDays` | number | No | Include page view stats for N days |

## wiki_get_page

Get wiki page metadata (without content).

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `wikiIdentifier` | string | **Yes** | Wiki name or ID |
| `project` | string | **Yes** | Project name |
| `path` | string | **Yes** | Page path (e.g., `/Overview`) |
| `recursionLevel` | string | No | `none`, `oneLevel`, `full` |

> This tool returns metadata only, not page content. Use `wiki_get_page_content` for content.

## wiki_get_page_content

Retrieve wiki page content.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `wikiIdentifier` | string | No | Wiki name or ID |
| `project` | string | No | Project name |
| `path` | string | No | Page path |
| `url` | string | No | Direct URL to the page |

> Provide either `url` OR (`wikiIdentifier` + `project`).

## wiki_create_or_update_page

Create or update a wiki page with content.

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `wikiIdentifier` | string | **Yes** | Wiki name or ID |
| `path` | string | **Yes** | Page path |
| `content` | string | **Yes** | Page content (Markdown) |
| `project` | string | No | Project name |
| `branch` | string | No | Branch name |
| `etag` | string | No | ETag for concurrency (required for updates) |

### Gotchas

- There are no separate `wiki_create_page` and `wiki_update_page` tools — use `wiki_create_or_update_page` for both.
- There is no `wiki_delete_page` tool in the current toolset.
- `wiki_get_page` returns metadata only; use `wiki_get_page_content` for actual content.
- When updating an existing page, provide the `etag` from the previous read to handle concurrency.
