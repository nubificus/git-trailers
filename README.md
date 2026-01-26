# git-trailers

This action parses information for a pull request and adds git trailers to
persist PR metadata history in the commit messages.

## Example usage

Create a workflow to run the action on PR approval.

eg:
`.github/workflows/pr-approval.yml`:

```yaml
name: Add Git trailers for PRs

on:
  pull_request_review:
    types: [submitted]

jobs:
  git-trailers:
    if: github.event.review.state == 'approved'
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Add git trailers
        uses: nubificus/git-trailers@feat_gh_checkout_rebase
```

Upon approval, the action will edit the PR commit messages adding trailers for
the PR number, reviewers and approvers and update the PR (by force-pushing the
final branch).
