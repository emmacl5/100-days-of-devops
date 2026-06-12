# Day 72 - Creating a Parameterized Jenkins Job

## Background

The Nautilus DevOps team wanted to help a new DevOps Engineer understand how parameterized builds work in Jenkins.

To achieve this, a Jenkins job was created that accepts user input through parameters and displays those values during execution.

This demonstrates how Jenkins jobs can become reusable and dynamic.

---

## Objective

Create a Jenkins Freestyle Job named:

```text
parameterized-job
```

Requirements:

### String Parameter

```text
Name: Stage
Default Value: Build
```

### Choice Parameter

```text
Name: env
Choices:
- Development
- Staging
- Production
```

### Build Step

Execute a shell command that displays both parameter values.

### Validation

Run the job successfully using:

```text
Stage = Build
env = Development
```

---

## Solution

### Step 1 - Create Jenkins Job

Created a new Jenkins Freestyle Project:

```text
parameterized-job
```

---

### Step 2 - Enable Parameters

Enabled:

```text
This project is parameterized
```

This allows build-time user input.

---

### Step 3 - Add String Parameter

Configured:

```text
Name: Stage
Default Value: Build
```

Purpose:

Allows the user to specify the pipeline stage.

---

### Step 4 - Add Choice Parameter

Configured:

```text
Name: env
```

Choices:

```text
Development
Staging
Production
```

Purpose:

Allows selecting the target environment.

---

### Step 5 - Configure Build Step

Added:

```text
Build Step → Execute Shell
```

Command:

```bash
echo "Stage: ${Stage}"
echo "Environment: ${env}"
```

---

## Build Execution

Selected:

```text
Stage = Build
env = Development
```

Executed:

```text
Build with Parameters
```

---

## Verification

Opened:

```text
Build History
→ Latest Build
→ Console Output
```

Expected output:

```text
Stage: Build
Environment: Development
```

Build Result:

```text
SUCCESS
```

---

## Architecture

```text
User
  │
  ▼
Parameterized Jenkins Job
  │
  ├── Stage Parameter
  │
  ├── Environment Parameter
  │
  ▼
Execute Shell
  │
  ▼
Display User Input
```

---

## Key Learnings

* Jenkins Parameterized Builds
* String Parameters
* Choice Parameters
* Build-Time Variables
* Shell Execution in Jenkins
* Dynamic Job Configuration

---

## Real-World Relevance

Parameterized jobs are widely used for:

* Environment Selection
* Deployment Targets
* Application Versions
* Release Pipelines
* Infrastructure Automation
* CI/CD Workflows

Examples:

```text
Deploy to Development
Deploy to Staging
Deploy to Production
```

using the same Jenkins job.

---

## Result

✅ Freestyle Job Created

✅ String Parameter Added

✅ Choice Parameter Added

✅ Shell Command Configured

✅ Build Executed Successfully

✅ Parameter Values Verified

✅ Lab Validation Passed

