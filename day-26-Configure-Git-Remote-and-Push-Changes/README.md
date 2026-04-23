# Day 26 - Configure Git Remote and Push Changes

## Background

The development team made updates to a project hosted in a Git repository located at `/opt/beta.git`.
A working copy of this repository existed on the Storage Server at:

/usr/src/kodekloudrepos/beta

To support a new workflow, the DevOps team introduced an additional remote repository that needed to be configured. The task involved updating the repository configuration, adding new content, and pushing changes to the new remote.

---

## Insight

This task highlighted the importance of Git remotes in managing multiple environments.

A Git remote represents a separate repository location, which can be used for:

* development
* staging
* production

Adding multiple remotes allows teams to:

* push code to different environments
* manage workflows more efficiently
* separate concerns between repositories

It also reinforced that:

> pushing to the correct remote is critical — even if the commit is correct.

---

## Progress

### 1. Connected to Storage Server and navigated to repo

```bash
ssh natasha@ststor01
cd /usr/src/kodekloudrepos/beta
```

---

### 2. Configured repository as safe

```bash
git config --global --add safe.directory /usr/src/kodekloudrepos/beta
```

---

### 3. Added new remote

```bash
git remote add dev_beta /opt/xfusioncorp_beta.git
```

---

### 4. Verified remote configuration

```bash
git remote -v
```

---

### 5. Switched to master branch

```bash
git checkout master
```

---

### 6. Copied file into repository

```bash
cp /tmp/index.html .
```

---

### 7. Added and committed changes

```bash
git add index.html
git commit -m "Add index.html to beta repo"
```

---

### 8. Pushed changes to new remote

```bash
git push dev_beta master
```

---

## Verification

### Checked remote configuration

```bash
git remote -v
```

### Checked commit history

```bash
git log --oneline -n 3
```

Confirmed:

* new commit exists
* push to `dev_beta` completed successfully

---

## Explanation

A new remote named `dev_beta` was added to the repository, pointing to `/opt/xfusioncorp_beta.git`.

A file (`index.html`) was copied into the repository, committed on the `master` branch, and pushed to the new remote. This ensures that changes are available in a different repository location, simulating multi-environment workflows.

---

## What I Learned

* how to add and manage Git remotes
* how to push code to a specific remote
* how multiple remotes support different environments
* importance of targeting the correct remote during push

---

## Real-World Relevance

This setup reflects real DevOps workflows where code is pushed to different environments such as development, staging, or production repositories. Managing multiple remotes is essential in CI/CD pipelines and deployment strategies.

---

## Improvement / Automation Idea

This workflow can be automated using CI/CD pipelines to automatically push changes to specific remotes based on branch or environment rules.

