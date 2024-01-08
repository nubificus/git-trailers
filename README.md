This action parses the PR branch and PR info and adds git trailers
to persist PR metadata history in the commit messages.

Example usage:

Create a workflow file for an action to be triggered on PR approval.

eg:
`.github/workflows/pr-approve.yml`:

```
name: Add Git trailers for PRs

on:
  pull_request_review:
    types: [submitted]

jobs:
  git-trailers:
    runs-on: [self-hosted]
    if: github.event.review.state == 'approved'

    steps:
      - name: Cleanup previous jobs
        run: |
          echo "Cleaning up previous runs"
          sudo rm -rf ${{ github.workspace }}/*
          sudo rm -rf ${{ github.workspace }}/.??*

      - name: Checkout Repository
        uses: actions/checkout@v2
        with:
          fetch-depth: 0

      - name: Do git-trailers
        uses: nubificus/git-trailers@v1
```

Upon approval, the action would overwrite current commits against `main` (or `master`)
adding trailers for reviewers, approvers and the PR#.
