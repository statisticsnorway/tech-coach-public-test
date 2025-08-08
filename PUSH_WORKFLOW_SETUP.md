# Push to Public Repo Workflow Setup

This document explains how to set up the GitHub Actions workflow that automatically pushes
content from this repository to the `tech-coach-publish-test` repository when a tag is pushed.

## How it works

When a tag is pushed to this repository (`tech-coach-internal-test`), the workflow:

1. **Detects the tag push** - Triggers automatically on any tag creation
2. **Creates a branch** - Creates a new branch in `tech-coach-publish-test` with name `b-{tag-name}`
3. **Copies content** - Copies all repository content to the new branch
4. **Creates a pull request** - Opens a PR from the new branch in the public repository

## Required Setup

### 1. Create a Personal Access Token (PAT)

You need to create a fine-grained GitHub Personal Access Token (PAT) by following these steps:

**Steps:**
1. Go to GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens
2. Click "Generate new token".
3. Set "token name" to: `PUBLISH_REPO_TOKEN` and "Resource owner" to `statisticsnorway`.
4. Set "Expiration" to 366 days.
5. Select "Only select repositories" and choose the -public version of this repository.
6. Select "Add permissions" and select `Read and write` access for
   - `Contents`
   - `Pull requests`
   - `Workflows`
7. Generate and copy the token.

### 2. Add Repository Secret

Add the PAT as a repository secret:
1. Go to this repository's Settings → Secrets and variables → Actions
2. Click "New repository secret"
3. Name: `PUBLISH_REPO_TOKEN`
4. Value: Paste the PAT created in step 1
5. Click "Add secret"

### 3. Ensure Target Repository Exists

Make sure the `tech-coach-publish-test` repository exists and has:
- A `main` branch (default branch)
- The PAT owner has write access to the repository

## Usage

To trigger the sync workflow:

1. Create and push a tag to this repository:
   ```bash
   git tag 2025.08
   git push origin 2025.08
   ```

2. The workflow will automatically:
   - Create branch `b-2025.08` in `tech-coach-publish-test`
   - Copy all content from the tagged commit
   - Create a pull request for review

## Workflow File

The workflow is defined in `.github/workflows/publish-to-public-repo.yml` and includes:
- Tag-based triggering
- Content synchronization
- Automated PR creation
- Proper git configuration and error handling

## Troubleshooting

**Common issues:**
- **403 Forbidden**: Check that the `PUBLISH_REPO_TOKEN` secret is set correctly
- **Repository not found**: Ensure `tech-coach-publish-test` exists and is accessible
- **Branch already exists**: The workflow will fail if a branch with the same name already exists

**Logs:**
Check the Actions tab in this repository to view workflow execution logs and debug any issues.
