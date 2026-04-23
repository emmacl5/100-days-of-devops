# Day 27 - Revert the Latest Git Commit Safely

## Background

The development team reported an issue with the most recent commit in the Git repository located at:

/usr/src/kodekloudrepos/games

Instead of rewriting history or deleting commits, the requirement was to safely undo the latest change while preserving the repository history.

---

## Insight

This task reinforced a critical Git concept:

> `git revert` is safer than `git reset` in shared repositories.

Why?

* `git revert` creates a new commit that undoes the changes from a previous commit
* it preserves commit history
* it is the preferred approach when working in collaborative environments

It also highlighted an important Git detail:

* `-m` means **mainline** in `git revert`, not **message**

---

## Progress

### 1. Connected to the Storage Server and opened the repository

```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/games
```

### 2. Marked the repository as safe

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/games
```

### 3. Reviewed commit history

```bash
git log --oneline -n 3
```

### 4. Reset local mistakes back to the remote state

```bash
git reset --hard origin/master
```

### 5. Reverted the latest commit

```bash
git revert HEAD --no-edit
```

### 6. Updated the revert commit message

```bash
git commit --amend -m "revert games"
```

---

## Verification

```bash
git log --oneline -n 3
```

Output:

```text
6a58521 revert games
27d119a add data.txt file
82675a4 initial commit
```

This confirmed:

* the latest commit was reverted
* the new revert commit message matched the requirement exactly
* previous history remained intact

---

## Explanation

The latest commit was undone using `git revert`, which created a new commit reversing the changes introduced by the previous commit. This preserved the repository history and ensured the rollback was traceable.

The revert commit message was then updated to:

```text
revert games
```

to match the task requirement exactly.

---

## What I Learned

* how to safely undo changes in Git using `git revert`
* why `git revert` is better than `git reset` in shared repositories
* how to correct commit messages using `git commit --amend`
* how to recover from extra local commits by resetting back to the remote state

---

## Real-World Relevance

In production and team environments, rollback operations must be safe and auditable. `git revert` provides a clean way to undo mistakes without erasing project history, making it ideal for shared repositories and CI/CD workflows.

---

## Improvement / Automation Idea

This process can be supported by branch protection rules and CI checks so problematic commits are identified early and rollbacks can be handled in a controlled workflow.

