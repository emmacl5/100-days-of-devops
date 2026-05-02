# Day 38 - Docker Image Tagging

## Background

The Nautilus development team planned to test a containerized environment for a new project. As part of the setup, they required a specific Docker image to be pulled and re-tagged for testing purposes on Application Server 3.

---

## Insight

This task introduced the concept of **Docker image tagging**.

Docker tags act as labels that reference specific versions or variants of an image.

Key idea:

> Tags do not create new images — they simply point to the same image with a different name.

This allows flexibility in:

* version control
* environment-specific naming
* release management

---

## Progress

### 1. Connected to Application Server 3

```bash
ssh banner@stapp03
```

---

### 2. Pulled BusyBox image with musl tag

```bash
sudo docker pull busybox:musl
```

---

### 3. Re-tagged the image

```bash
sudo docker tag busybox:musl busybox:news
```

---

### 4. Verified images

```bash
sudo docker images
```

Output showed:

```text
busybox   musl
busybox   news
```

---

## Verification

* Both tags (`musl` and `news`) point to the same image
* Image IDs are identical
* No new image was created, only a new reference

---

## Explanation

The `docker tag` command was used to assign a new tag (`news`) to the existing `busybox:musl` image. This enables the same image to be referenced using multiple tags without duplicating storage.

---

## What I Learned

* how to pull specific Docker image tags
* how to re-tag images
* how Docker tags work internally
* difference between image creation and tagging
* how to verify image tags

---

## Real-World Relevance

Image tagging is widely used in DevOps for:

* versioning applications
* managing environments (dev, staging, prod)
* labeling releases

---

## Improvement / Automation Idea

Tagging strategies can be automated in CI/CD pipelines to:

* assign version numbers
* create release tags
* manage environment-specific builds

