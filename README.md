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

**Basic Example:**
If you are on `master` and want to merge changes from `bug/123-post`:
```bash
git merge bug/123-post
```

#### Merge Strategies

Git uses different "strategies" to combine branches depending on how their histories have diverged and the flags you use.

| Strategy | Description | Example Command | Test Case / Scenario |
| :--- | :--- | :--- | :--- |
| **Fast-forward**<br>(Default if possible) | Moves the current branch pointer forward to the target branch's latest commit. No new "merge commit" is created. | `git merge feature-branch` | **Given:** `master` is at commit A. `feature` branched from A and has commits B & C.<br>**Action:** On `master`, run `git merge feature`.<br>**Result:** `master` simply moves forward to commit C. No extra merge commit. |
| **No fast-forward**<br>(`--no-ff`) | Forces Git to create a new "merge commit" with two parents, even if a fast-forward was possible. Preserves the branch's historical grouping. | `git merge --no-ff feature-branch` | **Given:** `feature` has commits B & C. You want to see them grouped as a feature in history.<br>**Action:** On `master`, run `git merge --no-ff feature`.<br>**Result:** A new commit `M` is created on `master`. History clearly shows when the feature was merged. |
| **Squash**<br>(`--squash`) | Takes all changes from the target branch, stages them, and stops. You then make one new commit. The histories are not linked. | `git merge --squash feature-branch`<br>`git commit -m "Squashed changes"` | **Given:** `feature` has 5 messy "work in progress" commits.<br>**Action:** On `master`, run `git merge --squash feature` then commit.<br>**Result:** `master` gets 1 clean new commit containing all changes. `feature` history is ignored. |
| **3-Way Merge**<br>(Default if divergent) | Used when both branches have unique commits since they diverged. Git creates a new "merge commit" combining both histories. | `git merge feature-branch` | **Given:** `master` has new commit C. `feature` has new commit D (both diverged from B).<br>**Action:** On `master`, run `git merge feature`.<br>**Result:** Git creates commit `M` combining changes from C and D, automatically resolving if no conflicts exist. |
| **Octopus**<br>(`-s octopus`) | Used when merging **more than two branches** at once. It creates a single merge commit with multiple parents. It is the default strategy when passing multiple branches to merge. | `git merge feature1 feature2 feature3` | **Given:** You have 3 separate, completed feature branches (`f1`, `f2`, `f3`).<br>**Action:** On `master`, run `git merge f1 f2 f3`.<br>**Result:** `master` gets **one** new merge commit that ties in all 3 branches simultaneously, rather than creating 3 separate merge commits. |
| **Ours**<br>(`-s ours`) | Merges the histories, but **completely ignores all changes** from the target branch. The resulting files are exactly the same as your current branch. | `git merge -s ours obsolete-branch` | **Given:** You have an old branch whose history you want to link, but you don't want any of its actual code.<br>**Action:** Run `git merge -s ours obsolete-branch`.<br>**Result:** `master` gets a new merge commit, but no files are changed. *(Note: Different from `-X ours` conflict resolution)*. |
| **Subtree**<br>(`-s subtree`) | A specialized strategy used when you want to merge an entirely different project (or branch) into a specific sub-folder of your current project. | `git merge -s subtree foreign-project` | **Given:** You want to include a 3rd-party library's repo inside your `vendor/` folder.<br>**Action:** Run `git merge -s subtree foreign-project`.<br>**Result:** Git figures out the directory structure and merges the foreign project neatly into the subdirectory. |


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

## Undoing History: `git revert` vs `git reset`

When you need to undo a commit, you have two main choices:

### `git revert <commit-hash>`
**Description:** Creates a **new commit** that does the exact opposite of the specified commit. This effectively undoes the changes without deleting the history.
**When to use:** Use this when you have already pushed your code to a shared repository (like GitHub). It is safe because it doesn't change existing history.

**Example:**
```bash
git revert ce94632
```

---

### `git reset <commit-hash>`
**Description:** Moves the branch pointer back to a previous commit, essentially **deleting** the commits that came after it.
**When to use:** Use this only for local changes that haven't been shared yet.

**Reset Options (Summary):**
*   `--soft`: Keeps changes in the Staging Area.
*   `--mixed`: Keeps changes in the Working Area.
*   `--hard`: Deletes all changes.

**Example:**
```bash
git reset --hard 1930b76
```


---

### Comparison: When to use `revert` vs `reset`?

| Feature | `git revert` | `git reset` |
| :--- | :--- | :--- |
| **Main Action** | Creates a **NEW** commit that undoes changes. | Moves the current branch **BACK** to a previous commit. |
| **History** | **Preserves history.** You see both the mistake and the fix. | **Rewrites history.** The mistakes disappear from the log. |
| **Shared Code** | **Safe** to use on branches shared with others (GitHub). | **Dangerous** on shared branches (causes conflicts for others). |
| **Outcome** | Your file changes are undone, but the commit count increases. | Your file changes are undone, and the commit history is shortened. |
| **Condition** | Use when the code is already **pushed** to a remote server. | Use for **local** mistakes that you haven't shared yet. |


---

## Understanding `HEAD~n` Notation

When using commands like `git reset` or `git revert`, you often see `HEAD~1`, `HEAD~2`, etc. This is a shorthand way to refer to previous commits relative to where you are now.

| Notation | Meaning |
| :--- | :--- |
| **`HEAD`** | The current commit you are on. |
| **`HEAD~1`** | The commit immediately before the current one (1 commit back). |
| **`HEAD~2`** | Two commits before the current one (2 commits back). |
| **`HEAD~n`** | **n** commits before the current one. |

**Visual Example:**
*   `HEAD` (Latest Commit)
*   `HEAD~1` (Parent of latest)
*   `HEAD~2` (Grandparent of latest)


---

### `git fetch`
**Description:** Downloads all the changes (commits, branches, tags) from the remote repository (like GitHub) to your local machine, but **does not merge** them into your current work. 
**When to use:** Use this when you want to see what others have done without affecting your local code yet.

**Example:**
```bash
git fetch origin
```


---

## The Connection: `fetch` + `merge` = `pull`

It is important to understand how these commands work together to update your local repository:

1.  **`git fetch`**: Only **downloads** the data. Your local files do not change.
2.  **`git merge`**: **Applies** those downloaded changes to your current branch.

### The `git pull` Command
**`git pull`** is a shortcut that performs both `git fetch` and `git merge` in a single step.

| Command | Action | Description |
| :--- | :--- | :--- |
| **`git fetch`** | Download | Gets the latest code from GitHub but doesn't change your files. |
| **`git merge`** | Integrate | Combines the fetched changes into your current work. |
| **`git pull`** | **Fetch + Merge** | Downloads the latest code and integrates it immediately. |

---

### Deleting Branches

**Description:** Once you have merged a branch or decided to discard the work, you should delete it to keep your repository clean. You **cannot** delete a branch that you are currently standing on.

**Common Options for Deleting:**

| Command | Description | Example |
| :--- | :--- | :--- |
| **`git branch -d <branch>`** | **Safe Delete (Local):** Deletes the local branch *only* if its changes have already been safely merged into another branch. | `git branch -d feature-x` |
| **`git branch -D <branch>`** | **Force Delete (Local):** Deletes the local branch forcefully, permanently throwing away any unmerged changes. | `git branch -D messy-experiment` |
| **`git push <remote> --delete <branch>`**<br>*(or `-d`)* | **Delete Remote Branch:** Removes the branch from the remote server (e.g., GitHub). This does not delete your local copy. | `git push origin -d feature-x` |

**Example:**
```bash
$ git branch -d bug/123-post
Deleted branch bug/123-post (was 38e984f).
```

## Merge vs Rebase vs Squash Merge

When integrating changes from one branch into another, you have three main options. Here is a comparison of how they affect your commit history:

| Feature | `git merge` | `git rebase` | `git merge --squash` |
| :--- | :--- | :--- | :--- |
| **Action** | Combines two branches with a new "merge commit". | Moves the entire feature branch to begin on the tip of the target branch. | Combines all commits from the feature branch into a single, new commit on the target branch. |
| **History Structure** | Preserves exact history, showing when branches diverged and merged. | Rewrites history to be completely linear. No merge commits. | Rewrites history. The detailed commits of the feature branch are lost. |
| **Commit Volume** | High (keeps all individual commits + merge commit). | High (keeps all individual commits, but linearly). | Low (only 1 single commit added). |
| **Best Used For** | Merging completed, significant features or shared branches where historical context is important. | Keeping a clean, linear history for personal or un-shared feature branches before merging. | Merging small features or pulling changes into `main` without cluttering the history with WIP commits. |
| **Warning** | Can create a messy, "train track" history if overused. | **Dangerous** on shared branches. Never rebase commits that have been pushed publicly. | You lose the detailed, step-by-step history of the work done on the feature branch. |

## Detailed Merge Strategies with Graphs

### 1. Fast-forward Merge (Default if possible)
**Command**: `git merge feature`
**Description**: Moves the `HEAD` pointer of the current branch forward to the target branch. No new merge commit is created.
**Graph**:
```text
Before Merge:
      A---B---C feature
     /
D---E main

After Fast-forward:
D---E---A---B---C main, feature
```

### 2. No Fast-forward (`--no-ff`)
**Command**: `git merge --no-ff feature`
**Description**: Forces Git to create a merge commit, even if a fast-forward is possible. This preserves the historical grouping of the feature branch commits.
**Graph**:
```text
Before Merge:
      A---B---C feature
     /
D---E main

After --no-ff Merge:
      A---B---C feature
     /         \
D---E-----------M main
```

### 3. Squash Merge (`--squash`)
**Command**: `git merge --squash feature`
**Description**: Combines all commits from the feature branch into a single new commit on the target branch. The feature branch's individual history is not linked.
**Graph**:
```text
Before Merge:
      A---B---C feature
     /
D---E main

After Squash Merge:
D---E---S main (S contains changes from A, B, and C)
```
*(The feature branch remains separate, but its changes are condensed into one commit on main)*

### 4. 3-Way Merge (Default when diverged)
**Command**: `git merge feature`
**Description**: Git creates a new merge commit combining both histories. This happens automatically when both branches have new commits since they diverged.
**Graph**:
```text
Before Merge:
      A---B---C feature
     /
D---E---F---G main

After 3-Way Merge:
      A---B---C feature
     /         \
D---E---F---G---M main
```

### 5. Octopus Merge (`-s octopus`)
**Command**: `git merge branch1 branch2 branch3`
**Description**: Used to merge more than two branches at once. It creates a single merge commit with multiple parents, keeping the history less cluttered than multiple sequential merges.
**Graph**:
```text
      A---B branch1
     /
    / C---D branch2
   / /
  / / E---F branch3
 / / /
X-------M main
```
*(M is a single merge commit combining branch1, branch2, and branch3)*

### 6. Ours Merge (`-s ours`)
**Command**: `git merge -s ours feature`
**Description**: Merges the history of the feature branch, but completely ignores any code changes from it. The file tree of the resulting merge commit is identical to the current branch.
**Graph**:
```text
Before Merge:
      A---B---C feature
     /
D---E---F---G main

After Ours Merge:
      A---B---C feature
     /         \
D---E---F---G---M main
```
*(History looks like a normal 3-way merge, but `M` contains ONLY the code from `main`, ignoring all changes introduced in `A`, `B`, and `C`)*

### 7. Subtree Merge (`-s subtree`)
**Command**: `git merge -s subtree external-project`
**Description**: Merges another project or branch into a specific subdirectory of your current repository, adjusting the paths automatically.
**Concept Graph**:
```text
X---Y---Z external-project (e.g., a library)
         \
          \ (merged into 'vendor/library/' folder)
           \
A---B---C---M main
```

---
*Created as a quick reference for D:/GIT*
