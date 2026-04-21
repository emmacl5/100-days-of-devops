# Day 23 - Fork a Git Repository Using Gitea

## Background

A new developer joined the Nautilus project team and needed access to begin contributing to an existing codebase.
Instead of working directly on the original repository, the task required creating a personal copy (fork) of the repository using the Gitea web interface.

Forking allows developers to work independently without affecting the original project until changes are reviewed and merged.

---

## Insight

Forking is a key concept in modern Git workflows, especially in collaborative and open-source environments.

A fork:

* creates a copy of a repository under a different user account
* allows safe experimentation and development
* enables contribution via pull requests

This approach ensures:

* code integrity in the main repository
* controlled collaboration
* proper review processes before merging changes

Understanding forking is essential for working in teams and contributing to shared projects.

---

## Progress

### 1. Accessed Gitea Web Interface

Opened the Gitea UI from the lab environment.

---

### 2. Logged in as the new developer

```text
Username: jon
Password: Jon_pass123
```

---

### 3. Located the repository

Navigated to:

`sarah/story-blog`

---

### 4. Forked the repository

* Clicked the **Fork** button
* Selected **jon** as the owner

---

### 5. Verified the fork

Confirmed that the repository was successfully created under:

```text
jon/story-blog
```

---

## Verification

* Logged in as **jon**
* Verified the existence of the forked repository
* Confirmed correct ownership (`jon/story-blog`)
* Captured screenshot as proof of completion

---

## Explanation

Forking creates a separate copy of a repository under a different user account. This allows developers to work on features or fixes independently without impacting the original repository.

Once changes are complete, they can be proposed back to the original repository via pull requests, enabling structured collaboration.

---

## What I Learned

* how to fork repositories using a web-based Git platform
* how collaborative Git workflows are structured
* the difference between cloning and forking
* importance of isolating development work from the main codebase

---

## Real-World Relevance

Forking is widely used in open-source projects and team-based development. It allows developers to safely contribute to projects while maintaining the stability of the main repository.

---

## Improvement / Automation Idea

This workflow can be extended by:

* cloning the forked repository locally
* creating feature branches
* submitting pull requests for review

