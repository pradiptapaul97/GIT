# Git Reference Documentation

This guide provides a list of common Git commands, their descriptions, and example outputs.

## Git Workflow: The Three States

Before diving into commands, it's important to understand the three main areas where your files reside:

1.  **Working Area:** This is where you do your work. Files are modified but not yet tracked or prepared for a commit.
2.  **Staging Area:** (After `git add`) This is a draft area where you gather all the changes you want to include in your next "save point" (commit).
3.  **Commit Area (Local Repository):** (After `git commit`) This is your official version history. Changes are permanently stored as a new version.

---

## Key Git Concepts

### What is `master`?
`master` is the default name for the first branch created when you initialize a Git repository. It is the main line of development where all your commits are stored by default.
*(Note: Newer versions of Git and platforms like GitHub often use `main` as the default branch name instead of `master`).*

### What is `HEAD`?
`HEAD` is a pointer that tells Git which branch or commit you are currently looking at. 
*   If `HEAD` points to `master` (shown as `HEAD -> master`), it means you are on the `master` branch and your next commit will be added to it.
*   You can think of `HEAD` as the "You are here" marker on a map.

---

## Commands

### `git init`
**Description:** Initialize a new, empty Git repository in the current directory. This creates a `.git` subdirectory which contains all of your necessary repository files.

---

### `git status` (Initial State)
**Description:** Displays the state of the working directory and the staging area. Untracked files are typically shown in **red**.

**Example Output:**
```diff
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
-        README.md
-        a.txt

nothing added to commit but untracked files present (use "git add" to track)
```

---

### `git add <file>`
**Description:** Adds a change in the working directory to the staging area. It tells Git that you want to include updates to a particular file in the next commit.

**Example:**
```bash
git add a.txt
```

---

### `git status` (After Staging)
**Description:** Files that are staged and ready to be committed are typically shown in **green**, while untracked files remain **red**.

**Example Output:**
```diff
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
+        new file:   a.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
-        README.md
```

---

### `git config` (Identity Configuration)
**Description:** Before you can commit for the first time, Git needs to know who you are. This information is attached to every commit you make.

**Commands to run:**
```bash
git config --global user.email "ppradipta65@gmail.com"
git config --global user.name "pradipta"
```

> [!NOTE]
> If you don't set this up, you will see an "Author identity unknown" error when trying to commit.

---

### `git commit -m "message"`
**Description:** Records a snapshot of the staged changes in the repository history. The `-m` flag allows you to add a short, descriptive message about the changes.

**Example Output:**
```text
[master (root-commit) 1930b76] first commit
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 a.txt
```

---

### `git log`
**Description:** Shows the commit history for the repository. It lists all the commits made, their unique hash IDs, the author, the date, and the commit message.

**Example Output:**
```text
commit 1930b76cce27163c1817b048e549cd8bddc71c5b (HEAD -> master)
Author: pradipta <ppradipta65@gmail.com>
Date:   Mon Apr 20 09:09:50 2026 +0530

    first commit
```

---

### `git status` (After Modifying a Tracked File)
**Description:** When you change a file that Git is already tracking (like `a.txt`), it shows up as "modified". Modified files in the working area are typically shown in **red** until they are staged.

**Example Output:**
```diff
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
-        modified:   a.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
-        README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

---
*Created as a quick reference for D:/GIT*
