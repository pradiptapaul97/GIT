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

### `git log` Options

| Option | Description | Example |
| :--- | :--- | :--- |
| **`--oneline`** | Shows each commit on a single line (hash and message). | `git log --oneline` |
| **`--graph`** | Displays a text-based graph of the branch history. | `git log --graph --oneline` |
| **`-p`** | Shows the "patch" (the actual code changes) for each commit. | `git log -p` |
| **`--stat`** | Shows stats like which files changed and how many lines were added/deleted. | `git log --stat` |
| **`-n <number>`** | Limits the log to a specific number of recent commits. | `git log -n 5` |
| **`--author="<name>"`** | Filters the log to show commits by a specific author. | `git log --author="pradipta"` |

**Example: `git log --oneline`**
```text
1930b76 (HEAD -> master) first commit
```

---

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

## Moving Between States (Undoing Changes)

### Staging Area → Working Area (Unstaging)
**Command:** `git restore --staged <file>`
**Description:** Moves a file from the staging area back to the working area.

---

### Commit Area → Staging Area (Undoing Commits)
**Command:** `git reset --soft HEAD~1`
**Description:** Undoes the last commit and moves the changes back to the staging area.

---

## The `git reset` Command

The `git reset` command is used to move your repository's history back to a specific point. It has three main options that determine what happens to your changes:

| Option | Commit Area | Staging Area | Working Area | When to use? |
| :--- | :--- | :--- | :--- | :--- |
| **`--soft`** | Removed | **Preserved** (Staged) | Preserved | Use when you want to **undo a commit but keep the changes ready** to commit again (e.g., to fix a commit message). |
| **`--mixed`** (Default) | Removed | Removed (Unstaged) | Preserved | Use when you want to **undo a commit and unstage the changes**, so you can edit them more before committing. |
| **`--hard`** | Removed | Removed | **Removed** (LOST) | Use only when you want to **completely discard all changes** and return to a previous state. **(Dangerous!)** |

---

### Detailed Example: `git reset --soft <hash>`

**Command:** `git reset --soft <hash>`
**Description:** Resets the repository history to a specific commit hash, keeping all changes made after that commit in the **Staging Area**.

**1. View history before reset:**
```text
$ git log
commit 4352d1229d7641f4e007cda90becda1115f329ee (HEAD -> master)
Author: pradipta <ppradipta65@gmail.com>
Date:   Mon Apr 20 09:36:09 2026 +0530

    third commit

commit 38e984f11fb4f0384d2f0be4c3f12f1137b59d16
Author: pradipta <ppradipta65@gmail.com>
Date:   Mon Apr 20 09:24:27 2026 +0530

    second commit

commit 1930b76cce27163c1817b048e549cd8bddc71c5b
Author: pradipta <ppradipta65@gmail.com>
Date:   Mon Apr 20 09:09:50 2026 +0530

    first commit
```

**2. View status before reset:**
```text
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")
```

**3. Run the reset command:**
```bash
git reset --soft 38e984
```

**4. View history after reset:**
```text
$ git log
commit 38e984f11fb4f0384d2f0be4c3f12f1137b59d16 (HEAD -> master)
Author: pradipta <ppradipta65@gmail.com>
Date:   Mon Apr 20 09:24:27 2026 +0530

    second commit

commit 1930b76cce27163c1817b048e549cd8bddc71c5b
Author: pradipta <ppradipta65@gmail.com>
Date:   Mon Apr 20 09:09:50 2026 +0530

    first commit
```

**5. View status after reset:**
```text
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   a.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md
```

### What happened after the reset?

1.  **History Changed:** In step 4, the `git log` shows that the "third commit" has been removed from the official history. The `HEAD` pointer has moved back to the "second commit".
2.  **Changes became Staged:** In step 5, `git status` shows `a.txt` under "Changes to be committed". This is because the changes that were in the "third commit" didn't disappear; Git just moved them from the **Commit Area** back into the **Staging Area**.
3.  **Working Area was Unaffected:** The `README.md` file, which was already modified in your working area, remains exactly as it was (unstaged).

---

## Branching

### `git branch`
**Description:** Lists all the branches in your repository. It shows you which branch you are currently on and what other branches exist locally.

**Example Output:**
```text
$ git branch
* master
```

---

### `git branch <branch-name>`
**Description:** Creates a new branch with the specified name. 

**Best Practice Naming Convention:**
A common way to name branches is to include the type of work, a tracking number (like a user story or bug ID), and a short description:
`bug/<story-number>-<description>`

**Example:**
```bash
git branch bug/123-post
```
*   `bug/`: The type of branch (e.g., feature, bug, hotfix).
*   `123`: The user story or ticket number for tracking.
*   `post`: A short description of the specific changes.


---

### `git checkout`
**Description:** Used to switch between branches or restore working tree files. It is the primary way to move your `HEAD` pointer to a different branch or commit.

**Common Options:**

| Command | Description |
| :--- | :--- |
| **`git checkout <branch>`** | Switch to an existing branch. |
| **`git checkout -b <branch>`** | **Create** a new branch and **switch** to it immediately. |
| **`git checkout <commit-hash>`** | Switch to a specific commit. |
| **`git checkout <file>`** | Discard local changes in a file. |


---

### `git switch`
**Description:** A newer, more specific command for switching branches. It was introduced to separate the "switching branches" functionality of `git checkout` from its other uses.

**Common Options:**

| Command | Description |
| :--- | :--- |
| **`git switch <branch>`** | Switch to an existing branch. |
| **`git switch -c <branch>`** | **Create** a new branch and **switch** to it immediately. |


---

### Comparison: `git branch` vs `git checkout` vs `git switch`

| Command | Primary Purpose | Best Used For... |
| :--- | :--- | :--- |
| **`git branch`** | **Management** | Listing, creating, or deleting branches. |
| **`git checkout`** | **Navigation & Restore** | Switching branches, going to specific commits, or discarding file changes. |
| **`git switch`** | **Branch Navigation** | Switching branches or creating/switching in one step (modern alternative to `checkout`). |


---

### `git merge <branch-name>`
**Description:** Combines the changes from another branch into your current branch. This is commonly used to bring feature or bugfix changes back into the `master` or `main` branch.

**Example:**
If you are on `master` and want to merge changes from `bug/123-post`:
```bash
git merge bug/123-post
```


---

## SSH Setup for GitHub

To securely connect your local repository to GitHub, you need to set up SSH keys.

### 1. Generate SSH Key
**Command:** `ssh-keygen`
**Description:** Generates a new pair of public and private keys.

**Example Output:**
```text
$ ssh-keygen
Generating public/private ed25519 key pair.
Enter file in which to save the key (/c/Users/pprad/.ssh/id_ed25519):
Created directory '/c/Users/pprad/.ssh'.
Enter passphrase (empty for no passphrase):
Your public key has been saved in /c/Users/pprad/.ssh/id_ed25519.pub
```

### 2. View and Copy Your Public Key
**Command:** `code ~/.ssh/id_ed25519.pub` (using VS Code) or `cat ~/.ssh/id_ed25519.pub`
**Description:** You need to copy the content of this file and add it to your GitHub account settings under "SSH and GPG keys".

### 3. Test the Connection
**Command:** `ssh -T git@github.com`
**Description:** Verifies if your SSH key is correctly configured and authenticated with GitHub.

**Example Output:**
```text
$ ssh -T git@github.com
The authenticity of host 'github.com (20.207.73.82)' can't be established.
ED25519 key fingerprint is: SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi pradiptapaul97! You've successfully authenticated, but GitHub does not provide shell access.
```


---

## Remote Repositories

### `git remote -v`
**Description:** Lists the remote connections you have to other repositories. It shows the shorthand name (like `origin`) and the URL associated with it for both fetching and pushing data.

**Example Output:**
```text
$ git remote -v
origin  https://github.com/pradiptapaul97/GIT.git (fetch)
origin  https://github.com/pradiptapaul97/GIT.git (push)
```


### `git push -u origin master`
**Description:** Uploads your local branch commits to the remote repository. The `-u` flag sets the "upstream" tracking, so you can just use `git push` next time.

**Example Output:**
```text
$ git push -u origin master
Enumerating objects: 25, done.
Counting objects: 100% (25/25), done.
Delta compression using up to 4 threads
Compressing objects: 100% (21/21), done.
Writing objects: 100% (23/23), 3.27 KiB | 1.09 MiB/s, done.
Total 23 (delta 10), reused 5 (delta 1), pack-reused 0 (from 0)
remote: Resolving deltas: 100% (10/10), completed with 1 local object.
To https://github.com/pradiptapaul97/GIT.git
   25f63d3..ce94632  master -> master
branch 'master' set up to track 'origin/master'.
```

---
*Created as a quick reference for D:/GIT*
