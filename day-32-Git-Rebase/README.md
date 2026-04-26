# Day 32 - Rebase Feature Branch with Master

## Background

The Nautilus development team was working on a repository located at:

/usr/src/kodekloudrepos/demo

A developer was actively working on a `feature` branch while new changes were being pushed to the `master` branch. The requirement was to update the `feature` branch with the latest changes from `master` without creating a merge commit.

---

## Insight

This task introduced **Git rebase**, a powerful way to maintain a clean commit history.

Unlike `git merge`, which creates a merge commit, `git rebase`:

* replays feature branch commits on top of another branch
* creates a linear commit history
* avoids unnecessary merge commits

Key principle:

> Rebase rewrites history to make it appear as if changes were built on top of the latest base branch.

---

## Progress

### 1. Navigated to repository

```bash
cd /usr/src/kodekloudrepos/demo
```

---

### 2. Marked repository as safe

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/demo
```

---

### 3. Switched to feature branch

```bash
git checkout feature
```

---

### 4. Rebasing feature onto master

```bash
git rebase master
```

---

### 5. Pushed rebased branch

```bash
git push origin feature --force
```

---

## Verification

```bash
git log --oneline --graph --all -n 10
```

Output:

```text
* Add new feature
* Update info.txt
* initial commit
```

This confirmed:

* feature commits are now on top of master
* no merge commit was created
* history is clean and linear

---

## Explanation

The `feature` branch was rebased onto the `master` branch. This reapplied the feature work on top of the latest changes from master, ensuring that the branch contained up-to-date code without introducing a merge commit.

Since rebase rewrites history, a force push was required to update the remote branch.

---

## What I Learned

* how to use `git rebase` to update a branch
* difference between `merge` and `rebase`
* why rebase creates cleaner commit history
* why `--force` is required after rebasing
* how to verify commit structure visually

---

## Real-World Relevance

Rebase is widely used in professional DevOps and development workflows to maintain clean and readable commit histories. It is especially useful in feature branch workflows and before merging code into main branches.

---

## Improvement / Automation Idea

Teams can enforce rebasing before merging using CI/CD pipelines or Git hooks to ensure a consistent and linear project history.

