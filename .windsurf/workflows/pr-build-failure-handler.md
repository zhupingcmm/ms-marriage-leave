---
description: Handle pull request build failures using GitHub CLI
---

# PR Build Failure Handler

This workflow helps diagnose and address pull request build failures using the GitHub CLI.

## Prerequisites

1. Install GitHub CLI: `gh --version`
2. Authenticate with GitHub: `gh auth login`
3. Navigate to your repository directory

## Steps

### 1. Check PR Status and List Failed PRs
```bash
gh pr list --state open --json number,title,statusCheckRollup
```

### 2. Get Detailed Status for Specific PR
Replace `<PR_NUMBER>` with the actual PR number:
```bash
gh pr view <PR_NUMBER> --json statusCheckRollup,headRefName,baseRefName
```

### 3. View PR Checks and Build Logs
```bash
gh pr checks <PR_NUMBER>
```

### 4. Get Workflow Run Details
```bash
gh run list --branch <BRANCH_NAME> --limit 5
```

### 5. Download Build Logs
Get the run ID from step 4, then:
```bash
gh run download <RUN_ID> --name build-logs
```

### 6. View Specific Workflow Run
```bash
gh run view <RUN_ID> --log
```

### 7. Analyze Common Build Failures

Check for these common issues:
- **Compilation errors**: Look for `error:` in Java compilation logs
- **Test failures**: Search for `FAILED` or `AssertionError` in test output
- **Dependency issues**: Look for `Could not resolve dependency` or `ClassNotFoundException`
- **Linting issues**: Check for code style violations

### 8. Fix Common Issues

#### For Maven compilation errors:
```bash
mvn clean compile
mvn dependency:resolve
```

#### For test failures:
```bash
mvn test -Dtest=<FailedTestClass>
```

#### For dependency conflicts:
```bash
mvn dependency:tree
mvn dependency:analyze
```

### 9. Re-run Failed Checks
// turbo
```bash
gh pr checks <PR_NUMBER> --required
```

### 10. Re-run Specific Workflow
```bash
gh workflow run <WORKFLOW_NAME> --ref <BRANCH_NAME>
```

### 11. Add Comment to PR with Status Update
```bash
gh pr comment <PR_NUMBER> --body "🔧 Build failure investigated. Issue: [describe issue]. Status: [Fixed/In Progress]"
```

### 12. Request Re-review After Fix
```bash
gh pr ready <PR_NUMBER>
gh pr comment <PR_NUMBER> --body "✅ Build issues resolved. Ready for review."
```

## Advanced Troubleshooting

### Check PR Diff for Problematic Changes
```bash
gh pr diff <PR_NUMBER>
```

### Compare with Base Branch
```bash
gh pr view <PR_NUMBER> --json baseRefName,headRefName
git log --oneline <BASE_BRANCH>..<HEAD_BRANCH>
```

### Check CI Configuration
```bash
cat .github/workflows/*.yml
```

### View Recent Commits in PR
```bash
gh pr view <PR_NUMBER> --json commits
```

## Automation Scripts

### Create a script to check all open PRs:
```bash
#!/bin/bash
echo "Checking all open PRs for build failures..."
gh pr list --state open --json number,title,statusCheckRollup | jq '.[] | select(.statusCheckRollup.state == "FAILURE") | {number, title}'
```

### Auto-retry failed workflows:
```bash
#!/bin/bash
PR_NUMBER=$1
echo "Re-running failed checks for PR #$PR_NUMBER"
gh pr checks $PR_NUMBER --required | grep -i failed
gh workflow run ci --ref $(gh pr view $PR_NUMBER --json headRefName --jq .headRefName)
```
