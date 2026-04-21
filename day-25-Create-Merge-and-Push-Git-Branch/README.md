# Day 25 - Create, Merge, and Push a Git Branch

## Background

The Nautilus development team needed to add a new file to the project repository while keeping the change isolated first.
The requirement was to create a separate branch, add the file there, commit it, merge it back into `master`, and then push both branches to the remote repository.

This reflects a standard feature-branch workflow used in collaborative development.

---

## Insight

This task demonstrated a complete Git workflow:

* create a branch from `master`
* make changes in the new branch
* commit the changes
* merge the branch back into `master`
* push both branches to the remote repository

It reinforced an important DevOps principle:

> Good version control is not just about saving code — it is about managing change safely.

---

## Progress

### 1. Connected to the Storage Server and opened the repository

```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/cluster
```

### 2. Marked the repository as safe for Git

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/cluster
```

### 3. Switched to the `master` branch

```bash
git checkout master
```

### 4. Created a new branch from `master`

```bash
git checkout -b datacenter master
```

### 5. Copied the required file into the repository

```bash
cp /tmp/index.html .
```

### 6. Added and committed the file

```bash
git add index.html
git commit -m "Add index.html in datacenter branch"
```

### 7. Merged the branch back into `master`

```bash
git checkout master
git merge datacenter
```

### 8. Pushed both branches to origin

```bash
git push origin master
git push origin datacenter
```

---

## Verification

### Checked local and remote branches

```bash
git branch -a
```

### Checked commit history

```bash
git log --oneline --decorate --all -n 5
```

### Confirmed push output

* `master -> master`
* `datacenter -> datacenter`

---

## Explanation

A new branch named `datacenter` was created from `master` so the file could be added in isolation.
After committing the change, the branch was merged back into `master` to include the update in the main codebase.

Finally, both `master` and `datacenter` were pushed to the remote repository so the changes were available centrally.

---

## What I Learned

* how to create a branch from a specific base branch
* how to add and commit a new file in Git
* how to merge a feature branch back into `master`
* how to push multiple branches to a remote repository
* how feature-branch workflows support safer collaboration

---

## Real-World Relevance

This is a very common Git workflow in DevOps and software teams.
Developers create branches for isolated work, merge changes after validation, and push updates to a shared remote repository used by collaborators and CI/CD systems.

---

## Improvement / Automation Idea

This workflow can be improved by adding pull-request reviews and CI checks before merges, helping teams validate changes before they reach the main branch.

