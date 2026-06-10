# Developer Setup Guide

## Prerequisites

- Python 3.9+
- Git

## One-Time Setup

### 1. Install pre-commit hooks (includes notebook output stripping)

```bash
pip install pre-commit nbstripout
pre-commit install
```

### 2. Install nbstripout as a git filter (belt-and-suspenders with pre-commit)

```bash
nbstripout --install --attributes .gitattributes
```

This ensures notebook outputs are **always** stripped, even if you forget to run pre-commit.

### 3. Verify setup

```bash
# Check that nbstripout is configured
nbstripout --status

# Run all pre-commit hooks on existing files
pre-commit run --all-files
```

## How It Works

### Notebook Output Stripping

Three layers of protection ensure notebooks never have outputs committed:

1. **`.gitattributes` + nbstripout filter** — Git automatically strips outputs during `git add` (invisible to the developer)
2. **`.pre-commit-config.yaml`** — Pre-commit hook double-checks on `git commit`
3. **GitHub Actions CI** — The `notebook-output-check` job fails the PR if any notebook has outputs

### Quality Checks on Every PR

| Check | What it does |
|-------|--------------|
| Markdown lint | Validates formatting consistency |
| Link check | Verifies URLs aren't broken |
| YAML lint | Validates configuration files |
| Notebook output check | Ensures notebooks are clean |

### Automated Maintenance

| Automation | Frequency |
|------------|-----------|
| Dependabot security updates | Continuous |
| Dependabot version updates | Weekly (Monday) |
| Dependabot auto-merge (patch/minor) | On PR creation |
| Full link scan | Weekly (Wednesday) |
| Stale issue/PR cleanup | Weekly (Monday) |
