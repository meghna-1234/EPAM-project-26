# Assignment 4 - Git and GitHub for Developers

---

## Overview
This document contains the complete collection of Git commands, workflows, internal object mechanics, branching strategies, and remote repository operations extracted from the practical terminal lab sessions.

---

## Course Completion Certificate

### Certificate Details
- **Awarded To:** Meghna Patel
- **Course Title:** Git and GitHub for Developers
- **Issuing Organization:** Infosys Springboard (*Infosys Limited*)
- **Completion Date:** August 8, 2026
- **Issued Date:** Tuesday, August 11, 2026
- **Authorized Signatory:** Satheesha B. Nanjappa (*Senior Vice President and Head, Education, Training and Assessment, Infosys Limited*)
- **Certificate Verification:** [https://verify.onwingspan.com](https://verify.onwingspan.com)

---

## 1. Local Git Repository Lifecycle

### Commands Workflow
```bash
# 1. Navigate to working directory and create project folder
cd /c/wamp/www
mkdir ~/public_html
cd ~/public_html/

# 2. Create index.html file
echo "My website is alive" > index.html

# 3. Initialize Git repository
git init

# 4. Configure Git user credentials
git config --global user.name "Omkar"
git config --global user.email "datar.omkar@gmail.com"

# 5. Check status, stage and commit
git status
git add index.html
git commit -m "initial contents of public_html is recorded"
git status

# 6. Removing a file and committing deletion
git rm index.html
git status
git commit -m "the index file is deleted"
git status

# 7. Local Cloning
git clone ~/public_html my_website
```

### Key Concepts
- **`git init`**: Initializes a `.git` metadata directory in the current workspace.
- **`git config --global`**: Sets user identity (`user.name`, `user.email`) attached to all subsequent commits.
- **`git rm`**: Removes the specified file from both the working directory and the Git staging index.

---

## 2. Git Internals & Object Storage (`.git` Architecture)

### Commands Workflow
```bash
# Navigate to project and inspect Git objects directory
cd /c/wamp/www/hello
git init
find .git/objects

# 1. Create file (untracked files do NOT create Git objects)
echo "hello world" > hello.txt
find .git/objects

# 2. Create subfolder and place file
mkdir subdir
cp hello.txt subdir/

# 3. Staging creates compressed blob objects
git add subdir/hello.txt

# 4. Write current staging area into a tree object
git write-tree
# Output SHA-1: 501bed3428ca7ad1d7db901f0d6d6935135f2226

# 5. Pretty-print tree object contents using full or short SHA-1
git cat-file -p 501bed3428c
git cat-file -p 501b
```

### Key Concepts
- **Blob Object**: Stores compressed raw file data (created on `git add`).
- **Tree Object**: Represents directories, mapping filenames to blob hashes and folder modes.
- **`git write-tree`**: Low-level plumbing command that creates a tree object from the staging index.
- **`git cat-file -p <hash>`**: Pretty-prints the type, contents, or size of any Git object using its SHA-1 hash.

---

## 3. Git Branching Mechanics in Empty vs Committed Repositories

### Commands Workflow
```bash
# In an empty repository before the first commit:
git init

# 1. Attempting to create a branch fails (no commit object exists yet)
git branch prs
# Output: fatal: Not a valid object name: 'master'.

# 2. Changing the unborn HEAD symbolic reference
git checkout -b "demo"

# 3. Listing branches returns empty
git branch
git show-branch

# 4. Switching back to master fails (master ref not recorded until first commit)
git checkout master
# Output: error: pathspec 'master' did not match any file(s) known to git.

# 5. Switching back to master reference
git checkout -b "master"
```

### Key Concepts
- **Unborn Branch**: When a repository is freshly initialized, `HEAD` points to `refs/heads/master`, but no actual branch reference file exists until the root commit is created.
- **`git show-branch`**: Displays branch comparison matrix and commit hierarchy across branches.

---

## 4. Git Diff & Modification Tracking

### Commands Workflow
```bash
# 1. Create branch and project directory
git checkout -b "diff"
mkdir diff_example
cd diff_example/

# 2. Create and commit initial files
echo "foo" > file1
echo "bar" > file2
git add ./
cd ..
git commit -m "ADD FILE1 AND FILE2"

# 3. Modify file content
cd diff_example/
echo "abc" > file1
cat file1

# 4. View working tree diff vs last commit
git diff

# 5. Stage and commit modification
git add file1
git commit -m "file has been modified"

# 6. Verify diff is now empty
git diff
```

### Key Concepts
- **`git diff`**: Shows line-by-line unified diff between the working tree and the staging area / `HEAD`.
- Lines prefixed with `-` represent removed lines, and `+` represent inserted lines.

---

## 5. Branch Merging, Fast-Forwarding & Bitbucket Push

### Commands Workflow
```bash
# --- Merging & Fast-Forwarding ---
git checkout master

# Fast-forward merge demo branch into master
git merge demo

# Verify commit status ahead of remote
git status

# Push master commits to remote Bitbucket repository
git push origin master

# --- Feature Branching & Upstream Push ---
git checkout -b "newbranch"
echo "new file created" > file1
git add file1
git commit -m "new file added"

# View branch matrix
git show-branch

# Create alternate branch and merge
git checkout -b "alternate"
echo "File again created" > example.txt
git merge newbranch

# Switch to master and merge newbranch
git checkout master
git merge newbranch

# Push changes to Bitbucket
git push origin master
```

### Key Concepts
- **Fast-Forward Merge**: When the target branch has no divergent commits, Git simply advances the target pointer to the latest commit of the merged branch.
- **`Already up-to-date`**: Indicates the branch already contains all commits from the source branch.
- **`git push origin HEAD`**: Pushes the current local active branch to a remote branch of the same name.

---

## 6. GitHub Remote Integration Workflow

### Commands Workflow
```bash
# 1. Clone empty remote repository
cd /c/wamp/www
git clone https://github.com/radhikaomkar/demoProject.git
cd demoProject/

# 2. Create initial introduction file
echo "Hello world" > introduction.txt
git add introduction.txt
git commit -m "Introduction added"

# 3. Push root commit to GitHub master branch
git push origin master

# 4. Create feature subbranch
git checkout -b "subbranch"
echo "new file" > file.txt
git add file.txt
git commit -m "New file added in new branch"

# 5. Push subbranch to GitHub remote
git push origin HEAD

# 6. View commit log history
git log
```

### Key Concepts
- **`git clone <url>`**: Clones a remote Git repository onto local filesystem and sets up remote `origin`.
- **`git log`**: Displays chronological commit history including Author, Date, Commit SHA-1, and commit messages.

---

## 7. Git Hooks Structure & Automated Staging (`git commit -a`)

### Commands Workflow
```bash
# 1. Initialize hook testing directory
mkdir hooktest
cd hooktest
git init

# 2. Create multiple files simultaneously
touch a b c
git add a b c
git commit -m "added a, b and c"

# 3. Inspect Git hooks structure
find .git/hooks

# 4. Modify tracked files
echo "perfectly fine" > a
echo "broken" > b

# 5. Automatically stage and commit tracked modified files in one command
git commit -a -m "test commit -a"

# 6. Check clean working directory
git status
```

### Key Concepts
- **`.git/hooks/`**: Contains client-side and server-side hook sample scripts (`pre-commit.sample`, `pre-push.sample`, `commit-msg.sample`) used for automated testing, linting, and policy enforcement.
- **`git commit -a` / `git commit -am "..."`**: Automatically stages all modified and deleted tracked files before committing, bypassing explicit `git add` steps for already tracked files.
