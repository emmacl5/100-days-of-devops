# Day 29 - Implement Pull Request Workflow with Review and Merge

## Background

The development team wanted to enforce a proper Git workflow where developers cannot push directly to the `master` branch. Instead, all changes must go through a review process before being merged.

Max had completed a new story in a feature branch:

```
story/fox-and-grapes
```

The goal was to integrate this work into the `master` branch using a Pull Request (PR) and a review process.

---

## Insight

This task introduced a key DevOps principle:

> Production code should never be updated directly — it must go through review and approval.

The workflow used:

* feature branch development
* pull request creation
* reviewer assignment
* approval and merge

This ensures:

* code quality
* collaboration
* controlled deployments

---

## Progress

### 1. Connected to Storage Server as Max

```bash
ssh max@ststor01
```

---

### 2. Verified repository and commit history

```bash
git log --oneline --decorate -n 10
```

Confirmed:

* existing commit history
* Max’s branch `story/fox-and-grapes` exists

---

### 3. Created Pull Request (Gitea UI)

* Logged in as **max**
* Opened repository `sarah/story-blog`
* Created PR with:

```text
Title: Added fox-and-grapes story
Source: story/fox-and-grapes
Destination: master
```

---

### 4. Assigned reviewer

* Added **tom** as reviewer through UI

---

### 5. Reviewed and merged PR

* Logged out as max
* Logged in as **tom**
* Reviewed the PR
* Approved and merged into `master`

---

## Verification

* PR shows status: **Merged**
* Commit successfully merged into `master`
* Reviewer `tom` recorded in PR
* Feature branch changes now part of main branch

---

## Explanation

Instead of pushing directly to `master`, Max created a Pull Request to propose his changes. Tom reviewed the changes and approved them before merging. This ensures that all updates to the main branch are validated and controlled.

---

## What I Learned

* how to create and manage Pull Requests
* importance of code reviews in Git workflows
* how to assign reviewers in a Git platform
* how controlled merges protect production code

---

## Real-World Relevance

This is a standard workflow in modern DevOps teams. Pull Requests enforce collaboration, improve code quality, and ensure that only approved changes reach production environments.

---

## Improvement / Automation Idea

This workflow can be enhanced with:

* mandatory approvals before merge
* automated CI checks
* branch protection rules

