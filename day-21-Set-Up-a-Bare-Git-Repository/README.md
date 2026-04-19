# Day 21 - Set Up a Bare Git Repository

## Background

The Nautilus development team required a centralized Git repository to support a new application project.
To enable collaboration and version control, a shared repository needed to be created on the Storage Server.

In distributed environments, developers push code to a central location, which acts as the source of truth for the project.

---

## Insight

Instead of a regular Git repository, a **bare repository** was required.

A bare repository:

* does not contain a working directory
* is designed for collaboration
* acts as a central remote repository for pushing and pulling code

This is the standard setup for:

* team-based development
* CI/CD pipelines
* centralized version control systems

Understanding the difference between a **bare repo** and a **working repo** is essential in DevOps workflows.

---

## Progress

### 1. Connected to Storage Server

```bash
ssh natasha@ststor01
```

### 2. Installed Git

```bash
sudo yum install -y git
```

### 3. Created Bare Repository

```bash
sudo git init --bare /opt/cluster.git
```

---

## Verification

```bash
ls /opt/cluster.git
```

Expected structure:

```text
HEAD
config
objects
refs
```

---

## Explanation

The repository was initialized using the `--bare` flag, which creates a Git repository without a working directory. This allows multiple users or systems to push changes to the repository safely.

The repository was placed under `/opt/cluster.git`, making it accessible as a centralized location for version control operations.

---

## What I Learned

* difference between bare and non-bare Git repositories
* how centralized Git workflows are structured
* how to prepare infrastructure for team collaboration
* importance of proper repository setup in DevOps pipelines

---

## Real-World Relevance

Bare repositories are widely used in production environments as central Git servers.
They enable teams to collaborate, automate deployments, and maintain version control across distributed systems.

---

## Improvement / Automation Idea

This setup can be extended by:

* configuring SSH access for multiple users
* integrating with CI/CD pipelines
* automating repository creation using scripts or configuration management tools

