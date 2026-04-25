# Day 30 - Git Hard Reset

## Background

The Nautilus development team had been using `/usr/src/kodekloudrepos/blog` as a test repository.
Several test commits were pushed, but the team wanted to clean the repository history and reset it back to a known good commit.

The requirement was to keep only two commits:

* `initial commit`
* `add data.txt file`

---

## Insight

This task demonstrated the difference between `git revert` and `git reset --hard`.

* `git revert` safely creates a new commit that undoes changes
* `git reset --hard` moves the branch pointer back and removes later commits from history

Because the requirement was to clean the commit history, `git reset --hard` was the correct approach.

---

## Progress

### 1. Opened the repository

```bash
cd /usr/src/kodekloudrepos/blog
```

### 2. Found the target commit

```bash
git log --oneline
```

### 3. Reset the repository to the required commit

```bash
git reset --hard 67ad92e
```

### 4. Pushed the rewritten history

```bash
git push origin master --force
```

---

## Verification

```bash
git log --oneline
```

Output:

```text
67ad92e add data.txt file
8beb0e0 initial commit
```

---

## Explanation

The repository history was reset to the commit with message `add data.txt file`.
This removed later test commits from the branch history and restored the repository to the required state.

Since the remote repository still had the old history, a force push was required to update the remote branch.

---

## What I Learned

* how to clean Git history using `git reset --hard`
* when to use reset instead of revert
* why force push is required after rewriting history
* how to verify commit history after reset

---

## Real-World Relevance

Hard resets are powerful but dangerous in shared repositories.
They are useful for test repositories or controlled cleanup tasks, but should be used carefully in production workflows because they rewrite shared history.

---

## Improvement / Automation Idea

Branch protection rules can prevent accidental force pushes on important branches, while test repositories can allow controlled resets when cleanup is required.

