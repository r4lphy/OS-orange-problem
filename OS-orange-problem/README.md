# PES-VCS: Designing a Version Control System from the Ground Up

**Author:** Ralph Ethan Dsouza  
**SRN:** PES2UG24AM131  
**Class:** AIML-B  

---

## 0. What This Project Is About

This assignment is not just about writing code — it’s about **recreating the core ideas behind Git** using operating system and filesystem principles.

You will build a **local version control system (PES-VCS)** that:
- Tracks file changes  
- Stores snapshots efficiently  
- Maintains full commit history  

All implementations directly reflect concepts like:
- File storage  
- Hashing  
- Directory structures  
- Atomic operations  

**Environment:** Ubuntu 22.04  

---

## 1. Setup Guide

### Install Dependencies

```bash
sudo apt update
sudo apt install -y gcc build-essential libssl-dev
```

---

### Repository Instructions (Important)

This repository is meant to be used as a **template**, not forked.

### Steps:
1. Click **“Use this template” → “Create a new repository”**
2. Name it like:
   ```
   PES2UG24AM131-pes-vcs
   ```
3. Set visibility to **public**
4. Clone your repo:
   ```bash
   git clone <your-repo-url>
   ```
5. Work entirely inside this directory  

⚠️ **Commit frequently** — minimum **5 commits per phase**

---

## 2. Build Commands

```bash
make          # Builds pes executable
make all      # Builds everything including tests
make clean    # Removes compiled files
```

---

## 3. Author Identity Configuration

Set your identity for commits:

```bash
export PES_AUTHOR="Ralph Ethan Dsouza <PES2UG24AM131>"
```

If not set:
```
PES User <pes@localhost>
```

---

## 4. Project File Responsibilities

| File | Purpose | Action |
|------|--------|--------|
| `pes.h` | Core structures/constants | Do not modify |
| `object.c` | Object storage system | Implement functions |
| `tree.h` | Tree interface | Do not modify |
| `tree.c` | Tree creation/serialization | Implement functions |
| `index.h` | Index interface | Do not modify |
| `index.c` | Staging system | Implement functions |
| `commit.h` | Commit interface | Do not modify |
| `commit.c` | Commit logic | Implement functions |
| `pes.c` | CLI entry | Do not modify |
| `test_objects.c` | Phase 1 tests | Do not modify |
| `test_tree.c` | Phase 2 tests | Do not modify |
| `test_sequence.sh` | Integration test | Do not modify |
| `Makefile` | Build config | Do not modify |

---

## 5. Core Idea: How Git Actually Works

Instead of storing differences, Git stores **complete snapshots**.

Efficiency comes from:
1. **Content-based storage (hashing)**
2. **Tree structures (reuse unchanged data)**

---

### Object Types

#### Blob (File Content)

```
blob <size>\0<data>
```

- Stores raw file data
- No filename or metadata
- Identified using SHA-256 hash

---

#### Tree (Directory Structure)

Represents folders and their contents.

```
100644 blob <hash> README.md
040000 tree <hash> src
```

Modes:
- `100644` → normal file  
- `100755` → executable file  
- `040000` → directory  

---

#### Commit

Contains:
- Pointer to tree
- Parent commit
- Author info
- Message

---

### Commit Chain

```
C3 → C2 → C1 → (start)
```

Each commit points to its parent → forms history.

---

### Index (Staging Area)

Acts as a buffer before committing:

Workflow:
```
Working Directory → Index → Commit
```

Commands:
- `pes add` → stage files  
- `pes commit` → snapshot staged files  

---

### Storage Structure

```
.pes/
├── objects/
├── refs/
│   └── heads/main
├── index
└── HEAD
```

---

## 6. Commands You Will Implement

```
pes init
pes add <file>
pes status
pes commit -m "msg"
pes log
```

---

## 7. Development Phases

---

### Phase 1: Object Storage

Concepts:
- Hashing
- File storage
- Integrity verification

Implement:
- `object_write`
- `object_read`

Test:
```bash
make test_objects
./test_objects
```

---

### Phase 2: Trees

Concepts:
- Directory representation
- Recursive structures

Implement:
- `tree_from_index`

Test:
```bash
make test_tree
./test_tree
```

---

### Phase 3: Index (Staging Area)

Concepts:
- File tracking
- Metadata comparison
- Atomic writes

Implement:
- `index_load`
- `index_save`
- `index_add`

Test manually:
```bash
./pes init
./pes add file.txt
./pes status
```

---

### Phase 4: Commits

Concepts:
- Linked structures
- Snapshot history

Implement:
- `commit_create`

Test:
```bash
./pes commit -m "message"
./pes log
```

---

## 8. Expected Output Example (`pes status`)

```
Staged changes:
  staged: file.txt

Unstaged changes:
  modified: README.md

Untracked files:
  untracked: notes.txt
```

---

## 9. Analysis Questions (Theory Only)

### Branching
- How checkout works
- Handling uncommitted changes
- Detached HEAD behavior

### Garbage Collection
- Removing unused objects
- Race conditions in deletion

---

## 10. Submission Checklist

### Screenshots

| Phase | Requirement |
|------|------------|
| 1 | Object test + storage |
| 2 | Tree test + raw tree |
| 3 | Status + index |
| 4 | Log + refs + objects |

---

### Required Code Files

- `object.c`
- `tree.c`
- `index.c`
- `commit.c`

---

### Written Answers

- Q5.1, Q5.2, Q5.3  
- Q6.1, Q6.2  

---

## 11. Submission Rules

### GitHub Repository
- Must be **public**
- Maintain proper structure

---

### Report
- Include screenshots + answers
- File name:
  - `README.md` OR
  - `report.pdf`

---

### Commit Requirement

Minimum:
- **5 commits per phase**

Preferred:
- More detailed commits showing step-by-step progress

---