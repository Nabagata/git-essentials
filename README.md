# Essential Git Knowledge for Software Engineers

A comprehensive guide to git concepts and workflows every SWE should master.

## Table of Contents

1. [Fundamentals](#fundamentals)
2. [Branching Strategies](#branching-strategies)
3. [Commits](#commits)
4. [Merging and Rebasing](#merging-and-rebasing)
5. [Remote Workflows](#remote-workflows)
6. [Undoing Changes](#undoing-changes)
7. [Advanced Workflows](#advanced-workflows)
8. [Best Practices](#best-practices)

## Fundamentals

### What is Git?

Git is a distributed version control system that tracks changes in your codebase. Every developer has a complete history of the project, enabling collaboration and providing a safety net for code changes.

### Core Concepts

- **Repository**: A complete copy of the project history, stored locally or on a server
- **Commit**: A snapshot of changes at a specific point in time with a unique hash (SHA-1)
- **Branch**: A separate line of development pointing to a specific commit
- **HEAD**: A reference to the current commit you're viewing or working on
- **Index/Staging Area**: Where changes are prepared before committing

### Essential Commands

```bash
git init                    # Initialize a new git repository
git clone <url>            # Clone a remote repository
git status                 # Show current state of working directory
git add <files>            # Stage changes for commit
git commit -m "message"    # Create a commit with staged changes
git log                    # View commit history
git diff                   # Show unstaged changes
git diff --staged          # Show staged changes
```

## Branching Strategies

### Creating and Switching Branches

```bash
git branch <branch-name>                 # Create a new branch
git checkout <branch-name>               # Switch to a branch
git checkout -b <branch-name>            # Create and switch in one command
git switch <branch-name>                 # Modern alternative to checkout
git branch -d <branch-name>              # Delete a branch
git branch -D <branch-name>              # Force delete a branch
```

### Popular Branching Models

**Git Flow**
- `main`: Production-ready code
- `develop`: Integration branch for features
- `feature/*`: Individual feature branches
- `hotfix/*`: Emergency fixes for production

**GitHub Flow**
- `main`: Always deployable
- `feature-*`: Feature branches created from main
- Pull requests for code review before merging

**Trunk-Based Development**
- Single `main` branch
- Short-lived feature branches (1-2 days)
- Frequent merges and deploys
- Ideal for CI/CD pipelines

## Commits

### Writing Good Commit Messages

Format: `<type>(<scope>): <subject>`

```
feat(auth): add login validation
fix(api): resolve null pointer in user endpoint
docs(readme): update installation instructions
style: format code to match eslint rules
refactor(parser): simplify token processing
test(utils): add edge case tests for dateParser
chore: update dependencies
```

**Guidelines:**
- Use imperative mood ("add feature" not "added feature")
- Keep subject line under 50 characters
- Add body with details if changes are complex
- Reference issue numbers: "Fixes #123"
- Separate subject from body with blank line

### Commit Best Practices

- **Atomic commits**: Each commit should represent one logical change
- **Frequent commits**: Commit often to maintain a clear history
- **Meaningful messages**: Future developers (including yourself) will thank you
- **Avoid mixing concerns**: Don't combine refactoring with feature work

## Merging and Rebasing

### Merge

Combines two branches by creating a merge commit. Preserves complete history.

```bash
git merge <branch-name>              # Merge branch into current branch
git merge --no-ff <branch-name>      # Create merge commit even if fast-forward possible
git merge --squash <branch-name>     # Combine all commits into one before merging
```

**When to use:**
- Integrating feature branches to main
- When history preservation is important
- Collaborative branches with multiple contributors

### Rebase

Replays commits on top of another branch. Creates a linear history.

```bash
git rebase <branch-name>             # Rebase current branch on top of another
git rebase -i HEAD~3                 # Interactive rebase last 3 commits
git rebase --continue                # Continue after resolving conflicts
git rebase --abort                   # Cancel rebase operation
```

**When to use:**
- Cleaning up commit history before merging
- Local branches not yet pushed
- Keeping history linear and clean

### Merge vs Rebase

| Aspect | Merge | Rebase |
|--------|-------|--------|
| History | Preserves all history | Rewrites history |
| Readability | Merge commits can clutter | Clean linear history |
| Collaboration | Safe for shared branches | Only for local branches |
| Conflicts | Resolved once in merge commit | Resolved per commit |

## Remote Workflows

### Remote Basics

```bash
git remote -v                        # List all remote repositories
git remote add <name> <url>          # Add a new remote
git remote remove <name>             # Remove a remote
git fetch                            # Download objects from remote (no merge)
git pull                             # Fetch and merge remote changes
git push                             # Upload commits to remote
git push <remote> <branch>           # Push specific branch
git push -u <remote> <branch>        # Push and set upstream tracking
```

### Upstream Tracking

```bash
git branch -u <remote>/<branch>      # Set upstream for current branch
git branch --set-upstream-to=<remote>/<branch>
git push -u origin feature           # Push and set upstream in one command
```

### Handling Remote Changes

```bash
git fetch origin                     # Get latest from remote
git pull --rebase                    # Pull with rebase instead of merge
git push --force-with-lease          # Safe force push (checks remote first)
```

## Undoing Changes

### Before Committing

```bash
git restore <file>                   # Discard changes in working directory
git restore --staged <file>          # Unstage file
git reset                            # Unstage all files
git clean -fd                        # Remove untracked files and directories
```

### After Committing

```bash
git revert <commit>                  # Create new commit that undoes changes
git reset --soft HEAD~1              # Undo last commit, keep changes staged
git reset --mixed HEAD~1             # Undo last commit, keep changes unstaged
git reset --hard HEAD~1              # Undo last commit, discard changes
git reflog                           # View reference logs (recover lost commits)
```

### Important Safety Notes

- Use `revert` for shared/pushed commits
- Use `reset` only on local commits
- `reset --hard` is destructive; use with caution
- `reflog` can recover seemingly lost commits for 30 days

## Advanced Workflows

### Cherry-pick

Apply specific commits to current branch:

```bash
git cherry-pick <commit-hash>        # Apply single commit
git cherry-pick <start>..<end>       # Apply range of commits
git cherry-pick --continue           # Continue after conflict resolution
```

### Stashing

Save work without committing:

```bash
git stash                            # Stash current changes
git stash list                       # View all stashes
git stash pop                        # Apply and remove latest stash
git stash apply                      # Apply stash without removing it
git stash drop                       # Delete a stash
```

### Tagging

Mark important commits:

```bash
git tag <tag-name>                   # Create lightweight tag
git tag -a <tag-name> -m "message"   # Create annotated tag
git tag -l                           # List tags
git push origin <tag-name>           # Push specific tag
git push origin --tags               # Push all tags
```

### Searching History

```bash
git log --grep="pattern"             # Search commit messages
git log -S "code"                    # Search commit content
git blame <file>                     # Show who changed each line
git bisect                           # Binary search for bad commit
```

## Best Practices

### Daily Workflow

1. **Pull before working**: `git pull` to get latest changes
2. **Create feature branch**: `git checkout -b feature/my-feature`
3. **Make atomic commits**: Commit logical units of work
4. **Push regularly**: `git push` to backup your work
5. **Create pull request**: Request review before merging to main

### Code Review Standards

- Require at least one approval before merging
- Use pull request templates
- Automate checks (linting, tests, security scans)
- Discuss design decisions in PR comments
- Keep PRs focused and reasonably sized

### Commit Hygiene

- Never commit sensitive data (credentials, API keys)
- Use `.gitignore` for build artifacts and local files
- Review changes before committing: `git diff`
- One feature/fix per branch
- Write descriptive commit messages

### Security Considerations

```bash
git config --global core.safecrlf true    # Prevent line ending issues
git config --global core.excludesfile ~/.gitignore_global
```

- Use signed commits: `git commit -S`
- Verify GPG signatures: `git log --show-signature`
- Never force-push to main or shared branches
- Use branch protection rules on important branches

### Performance Tips

```bash
git gc                               # Optimize repository
git log --oneline                    # Concise history view
git log --graph --oneline --all      # Visual branch structure
git config --global credential.helper osxkeychain  # Store credentials securely
```

## Common Workflows

### Feature Development

```bash
git checkout -b feature/new-api
# Make changes
git add .
git commit -m "feat(api): add new endpoint"
git push -u origin feature/new-api
# Create pull request, get reviews, merge
```

### Hotfix in Production

```bash
git checkout main
git pull origin main
git checkout -b hotfix/urgent-fix
# Make fix
git commit -m "fix: resolve critical issue"
git push origin hotfix/urgent-fix
# Quick review and merge to main
git checkout main
git merge --no-ff hotfix/urgent-fix
git push origin main
```

### Syncing with Main

```bash
git fetch origin
git rebase origin/main
# or
git merge origin/main
```

## Debugging Git Issues

```bash
git reflog                           # Recover lost commits
git fsck --full                      # Check repository integrity
git log --all --oneline --graph      # Visualize all branches
git show <commit>                    # View specific commit details
git diff <branch1> <branch2>         # Compare branches
```

## Resources

- [Official Git Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book)
- [GitHub Learning Lab](https://lab.github.com)
- [Atlassian Git Tutorials](https://www.atlassian.com/git/tutorials)

---

**Remember**: Version control is about collaboration, history, and safety. Master these concepts and you'll be a more effective developer.
