# Day 34 - Automate Release Tagging with Git Hooks

## Background

The Nautilus development team needed to automate part of their release workflow for the repository located at:

/usr/src/kodekloudrepos/beta

They required that whenever changes are pushed to the `master` branch, a release tag should automatically be created using the current date.

---

## Insight

This task introduced **Git hooks**, specifically the `post-update` hook.

Git hooks are scripts that run automatically when certain events occur in a repository.

In this case:

* event: push to master branch
* action: create a release tag

Key idea:

> Git hooks allow you to automate workflows directly inside the repository.

---

## Progress

### 1. Navigated to repository

```bash id="3tn8zz"
cd /usr/src/kodekloudrepos/beta
```

---

### 2. Marked repositories as safe

```bash id="k7q1pb"
git config --global --add safe.directory /usr/src/kodekloudrepos/beta
git config --global --add safe.directory /opt/beta.git
```

---

### 3. Created post-update hook

```bash id="p9czgd"
vi /opt/beta.git/hooks/post-update
```

Hook content:

```bash id="zsd5p8"
#!/bin/bash

for ref in "$@"
do
  if [ "$ref" = "refs/heads/master" ]; then
    tag_name="release-$(date +%F)"
    git tag -f "$tag_name" refs/heads/master
  fi
done
```

---

### 4. Made hook executable

```bash id="9l3c2c"
chmod +x /opt/beta.git/hooks/post-update
```

---

### 5. Merged feature branch into master

```bash id="4wyfh8"
git checkout master
git merge feature
```

---

### 6. Pushed changes to trigger hook

```bash id="7dy79l"
git push origin master
```

---

## Verification

```bash id="f4f1r7"
git ls-remote --tags origin
```

Output example:

```text id="2g84mq"
refs/tags/release-2026-04-29
```

Confirmed:

* tag created automatically
* correct naming format used
* hook executed successfully

---

## Explanation

A `post-update` hook was configured in the bare repository (`/opt/beta.git`). This script listens for updates to the `master` branch and automatically creates a release tag using the current date.

When changes were pushed to `master`, the hook executed and generated the tag without any manual intervention.

---

## What I Learned

* how Git hooks work
* difference between working repo and bare repo hooks
* how to automate tagging using shell scripting
* how to trigger hooks via Git push
* importance of executable permissions for scripts

---

## Real-World Relevance

Git hooks are widely used in DevOps to automate tasks such as:

* tagging releases
* running validations
* triggering deployments

This is a foundational concept for CI/CD pipelines and automated release management.

---

## Improvement / Automation Idea

This workflow can be extended by:

* integrating hooks with CI/CD pipelines
* automatically pushing tags to remote repositories
* adding versioning logic (semantic versioning)

