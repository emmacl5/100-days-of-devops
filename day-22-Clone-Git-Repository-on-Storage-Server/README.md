# Day 22 - Clone a Git Repository on the Storage Server

## Background

The development team needed a working copy of an existing Git repository on the Storage Server so they could begin using it for application development activities.

The repository already existed at `/opt/demo.git`, and the requirement was to clone it into `/usr/src/kodekloudrepos` using the `natasha` user without making unnecessary changes to permissions or repository contents.

---

## Insight

This task reinforced an important Git and Linux permissions concept:

* cloning as the wrong user can cause ownership issues
* cloning into the wrong path can fail the lab even if the repository itself is valid
* empty repositories still clone successfully and contain a `.git` directory with all repository metadata

A Git clone is not just about copying files — it is also about preserving the correct structure and execution context.

---

## Progress

### 1. Connected to the Storage Server

```bash
ssh natasha@ststor01
```

### 2. Ensured the parent directory existed and was owned by the correct user

```bash
sudo mkdir -p /usr/src/kodekloudrepos
sudo chown natasha:natasha /usr/src/kodekloudrepos
```

### 3. Cloned the repository as `natasha`

```bash
git clone /opt/demo.git /usr/src/kodekloudrepos/demo
```

---

## Verification

### Checked clone directory ownership

```bash
ls -ld /usr/src/kodekloudrepos/demo
```

Output:

```text
drwxr-xr-x 3 natasha natasha ...
```

### Checked repository contents

```bash
ls -a /usr/src/kodekloudrepos/demo
```

Expected:

```text
.  ..  .git
```

---

## Explanation

The repository at `/opt/demo.git` was cloned into `/usr/src/kodekloudrepos/demo` so that the repository name remained part of the directory structure. This matched the expected lab layout.

The clone was executed as the `natasha` user to ensure proper ownership and avoid permission mismatches caused by running Git with `sudo`.

---

## What I Learned

* how to clone a Git repository using a local path
* why the executing user matters in server-side Git operations
* how permissions and ownership affect repository setup
* why an empty repository still contains meaningful Git structure

---

## Real-World Relevance

This reflects how repositories are prepared on shared servers for developers, deployment jobs, or automation systems. Correct path structure and ownership are critical for smooth collaboration and predictable operations.

---

## Improvement / Automation Idea

This process can be automated with shell scripts or configuration management tools to standardize repository setup and ownership across environments.

