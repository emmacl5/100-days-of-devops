# Day 28 - Cherry-Pick a Specific Commit from Feature Branch

## Background

The Nautilus development team was working on a repository located at:

/usr/src/kodekloudrepos/games

A developer working on the `feature` branch had made several changes, but only one specific commit needed to be moved to the `master` branch while keeping the rest of the feature work isolated.

---

## Insight

This task introduced the concept of **cherry-picking** in Git.

Cherry-picking allows:

* selecting a specific commit from one branch
* applying it to another branch
* without merging the entire branch

This is useful when:

* a feature is partially complete
* only one fix or change is needed immediately
* full branch integration is not yet ready

---

## Progress

### 1. Connected to Storage Server and opened the repository

```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/games
```

---

### 2. Configured repository as safe

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/games
```

---

### 3. Identified the required commit in feature branch

```bash
git log feature --oneline
```

Located commit:

```text
Update info.txt
```

---

### 4. Switched to master branch

```bash
git checkout master
```

---

### 5. Cherry-picked the required commit

```bash
git cherry-pick <commit-hash>
```

---

### 6. Pushed changes to origin

```bash
git push origin master
```

---

## Verification

```bash
git log --oneline -n 5
```

Output:

```text
9afd76b Update info.txt
4317e74 Add welcome.txt
a5f1d5e initial commit
```

Confirmed:

* only the required commit was added to master
* feature branch was not merged entirely
* history remained clean

---

## Explanation

The commit containing "Update info.txt" was selectively applied from the `feature` branch to the `master` branch using `git cherry-pick`. This allowed the team to integrate a specific change without merging incomplete work from the feature branch.

---

## What I Learned

* how to use `git cherry-pick` to apply specific commits
* difference between merging and cherry-picking
* how to manage partial feature integration
* importance of controlled code movement in Git

---

## Real-World Relevance

Cherry-picking is commonly used in production environments when:

* urgent fixes need to be applied
* features are partially complete
* selective updates are required without full branch merges

---

## Improvement / Automation Idea

This workflow can be integrated into CI/CD pipelines to automatically promote specific commits (such as hotfixes) across environments.

