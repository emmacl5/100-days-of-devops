# Day 33 - Resolve Git Push Rejection and Fix Conflicts

## Background

Max attempted to push changes to the `story-blog` repository but encountered a rejection because the remote repository had updates that were not present locally.

Additionally, the `story-index.txt` file needed corrections:

* include all 4 story titles
* fix a typo ("Mooose" → "Mouse")
* remove leftover merge conflict markers

---

## Insight

This task demonstrated a very common real-world scenario:

> Push rejected because the remote branch is ahead of the local branch.

The correct workflow is:

1. Pull latest changes using rebase
2. Resolve any merge conflicts
3. Clean up the file content
4. Continue the rebase
5. Push updated changes

It also reinforced:

* conflict markers must be removed manually
* file content must be verified before pushing

---

## Progress

### 1. Connected to Storage Server as Max

```bash
ssh max@ststor01
cd /home/max/story-blog
```

---

### 2. Attempted push (rejected)

```bash
git push origin master
```

---

### 3. Pulled remote changes using rebase

```bash
git pull --rebase origin master
```

---

### 4. Resolved conflict in story-index.txt

Final content:

```text
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

Removed:

* conflict markers (`<<<<<<<`, `=======`, `>>>>>>>`)
* incorrect spelling "Mooose"

---

### 5. Continued rebase

```bash
git add story-index.txt
git rebase --continue
```

---

### 6. Pushed changes successfully

```bash
git push origin master
```

---

## Verification

```bash
git log --oneline -n 5
cat story-index.txt
```

Confirmed:

* latest commit pushed to `origin/master`
* file content is correct
* no conflict markers remain

---

## Explanation

The push failed because the remote repository had newer commits. Using `git pull --rebase`, local changes were replayed on top of the latest remote commits. During the process, a conflict occurred in `story-index.txt`, which was resolved manually.

After cleaning the file and completing the rebase, the changes were successfully pushed.

---

## What I Learned

* how to resolve push rejection errors
* how to use `git pull --rebase` effectively
* how to fix merge conflicts manually
* how to remove conflict markers properly
* importance of verifying file content before pushing

---

## Real-World Relevance

This scenario is very common in collaborative environments where multiple developers work on the same branch. Understanding how to resolve conflicts and maintain clean data is critical for DevOps and development workflows.

---

## Improvement / Automation Idea

Teams can:

* enforce pull request workflows instead of direct pushes
* use branch protection rules
* integrate CI checks to validate file content before merging

