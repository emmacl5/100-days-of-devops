# Day 31 - Restore Git Stashed Changes

## Background

The Nautilus development team had a Git repository located at:

`/usr/src/kodekloudrepos/games`

One developer had saved some in-progress work using Git stash. The team later requested that one specific stash, identified as `stash@{1}`, be restored, committed, and pushed to the remote repository.

---

## Insight

Git stash is used to temporarily save uncommitted work without committing it to the repository.

This is useful when developers need to:

* switch branches
* save incomplete work
* clean their working directory temporarily
* come back to changes later

A key detail in this task was restoring the correct stash:

`stash@{1}`

Stash indexes matter because `stash@{0}` and `stash@{1}` can contain completely different changes.

---

## Progress

### 1. Navigated to the repository

```bash
cd /usr/src/kodekloudrepos/games
```

### 2. Listed available stashes

```bash
git stash list
```

### 3. Applied the required stash

```bash
git stash apply stash@{1}
```

### 4. Added restored changes

```bash
git add .
```

### 5. Committed the changes

```bash
git commit -m "Restore stashed changes"
```

### 6. Pushed changes to origin

```bash
git push origin master
```

---

## Verification

```bash
git log --oneline -n 3
```

Output:

```text
47f2ef5 Restore stashed changes
2dd484e initial commit
```

The commit was also pushed to origin:

```text
HEAD -> master, origin/master
```

---

## Explanation

The required stash was restored using `git stash apply stash@{1}`.
After applying the stash, the restored changes were staged, committed, and pushed to the remote `master` branch.

This preserved the stash while also making the restored work part of the project history.

---

## What I Learned

* how to list Git stashes
* how to apply a specific stash
* difference between `stash apply` and `stash pop`
* how to restore temporary work and commit it properly
* why stash identifiers matter

---

## Real-World Relevance

Git stash is commonly used when developers need to pause work, switch context, or temporarily clean their workspace. Knowing how to recover specific stashed changes is important in real development and DevOps workflows.

---

## Improvement / Automation Idea

Teams can reduce stash confusion by encouraging descriptive stash messages using:

```bash
git stash push -m "description"
```

This makes it easier to identify and restore the correct stash later.

