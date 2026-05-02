# Install `gh` CLI in a devcontainer

`gh` is installed by adding a devcontainer feature entry to your `devcontainer.json`:

```jsonc
"ghcr.io/devcontainers/features/github-cli:1": {}
```

Idempotency check before editing: if `ghcr.io/devcontainers/features/github-cli` is already in the `features` object, skip and report "already present".

## Variants

### a) `features` object already exists

Add the entry as a new key inside the existing `features` object.

**Before:**

```jsonc
{
  "features": {
    "ghcr.io/devcontainers/features/common-utils:2": {}
  }
}
```

**After:**

```jsonc
{
  "features": {
    "ghcr.io/devcontainers/features/common-utils:2": {},
    "ghcr.io/devcontainers/features/github-cli:1": {}
  }
}
```

### b) No `features` object

Create the `features` object with the entry.

**Before:**

```jsonc
{
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu"
}
```

**After:**

```jsonc
{
  "image": "mcr.microsoft.com/devcontainers/base:ubuntu",
  "features": {
    "ghcr.io/devcontainers/features/github-cli:1": {}
  }
}
```

## Notes

- `gh auth login` must be run interactively after the container starts — it cannot be automated. Remind the user.
