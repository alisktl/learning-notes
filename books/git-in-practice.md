# Git in Practice
by Mike McQuaid

1. Part 1. Introduction to Git
    1. [Chapter 1: Local Git](#chapter-1-local-git)
        1. [Initial setup](#initial-setup)
        2. [Technique 1: `git init`](#technique-1-creating-a-repository-git-init)
        3. [Technique 2: `git add`](#technique-2-building-a-new-commit-in-the-index-staging-area-git-add)
        4. [Technique 3: `git commit`](#technique-3-committing-changes-to-files-git-commit)
        5. [Technique 4: `git log`](#technique-4-viewing-history-git-log-gitk-gitx)
        6. [Technique 5: `git diff`](#technique-5-viewing-the-differences-between-commits-git-diff)
    2. [Chapter 2: Remote Git](#chapter-2-remote-git)
        1. [Technique 6: `git remote add`](#technique-6-adding-a-remote-repository-git-remote-add)
        2. [Technique 7: `git push`](#technique-7-pushing-changes-to-a-remote-repository-git-push)
        3. [Technique 8: `git clone`](#technique-8-cloning-a-remotegithub-repository-onto-your-local-machine-git-clone)
        4. [Technique 9: `git pull`](#technique-9-pulling-changes-from-another-repository-git-pull)
        5. [Technique 10: `git fetch`](#technique-10-fetching-changes-from-a-remote-without-modifying-local-branches-git-fetch)
        6. [Technique 11: `git branch`](#technique-11-creating-a-new-local-branch-from-the-current-branch-git-branch)
        7. [Technique 12: `git checkout`](#technique-12-checking-out-a-local-branch-git-checkout)
        8. [Technique 13: Pushing a local branch remotely](#technique-13-pushing-a-local-branch-remotely)
        9. [Technique 14: `git merge`](#technique-14-merging-an-existing-branch-into-the-current-branch-git-merge)
        10. [Technique 15: Deleting a remote branch](#technique-15-deleting-a-remote-branch)
        11. [Technique 16: Deleting the current local branch after merging](#technique-16-deleting-the-current-local-branch-after-merging)
2. Part 2. Git essentials
    1. [Chapter 3: Filesystem interactions](#chapter-3-filesystem-interactions)
        1. [Technique 17: `git mv`](#technique-17-renaming-or-moving-a-file-git-mv)
        2. [Technique 18: `git rm`](#technique-18-removing-a-file-git-rm)
        3. [Technique 19: `git reset`](#technique-19-resetting-files-to-the-last-commit-git-reset)

---

## Chapter 1: Local Git
### Initial setup
#### Setup global config
```
git config --global user.name "Mike McQuaid"

git config --global user.email mike@mikemcquaid.com
```

#### Get global *user.email* config
```
git config --global user.email
```

### Technique 1: Creating a repository: `git init`
```
git init GitInPracticeRedux
```

### Technique 2: Building a new commit in the index staging area: `git add`
#### Add a change in the *working directory* to the *staging area*
```
git add GitInPractice.asciidoc
```

#### Display the state of the working directory and the staging area
```
git status
```

### Technique 3: Committing changes to files: `git commit`
#### Capture a snapshot of the project's currently staged changes.
```
git commit --message 'Initial commit of book.'
```

### Technique 4: Viewing history: `git log`, `gitk`, `gitx`
#### Show a list of all the commits made to a repository
```
git log
```

### Technique 5: Viewing the differences between commits: `git diff`
#### Show changes between commit *master~1* (before commit *master*) and *master* commit
```
git diff master~1 master
```

#### Show changes between commits and a brief overview of how many lines were involved in the changes
```
git diff --stat master~1 master
```

#### Show changes between commits and show modifications per word rather than per line
```
git diff --word-diff master~1 master
```

---

## Chapter 2: Remote Git
### Technique 6: Adding a remote repository: `git remote add`
#### Add a new remote repository
```
git remote add origin https://github.com/alisktl/GitInPracticeRedux.git
```

#### Verify that this remote has been created successfully
```
git remote --verbose
```

### Technique 7: Pushing changes to a remote repository: `git push`
```
git push --set-upstream origin master
```

### Technique 8: Cloning a remote/GitHub repository onto your local machine: `git clone`
```
git clone https://github.com/alisktl/GitInPracticeRedux.git
```

### Technique 9: Pulling changes from another repository: `git pull`
```
git pull
```
> **Note:** `git pull` performs two actions: fetching the changes from a remote repository and merging them into the current branch.

### Technique 10: Fetching changes from a remote without modifying local branches: `git fetch`
```
git fetch
```

### Technique 11: Creating a new local branch from the current branch: `git branch`
#### See all branches
```
git branch
```

#### Create a new branch `chapter-two`
```
git branch chapter-two
```

### Technique 12: Checking out a local branch: `git checkout`
#### Switch to a newly created branch
```
git checkout chapter-two
```
> **Note:** the `HEAD` pointer is updated to point to the current, `chapter-two` branch pointer, which in turn points to the top commit of that branch.

### Technique 13: Pushing a local branch remotely
```
git push --set-upstream origin chapter-two
```

### Technique 14: Merging an existing branch into the current branch: `git merge`
#### Change current branch to `master`
```
git checkout master
```

#### Merge `chapter-two` branch to `master`
```
git merge chapter-two
```

### Technique 15: Deleting a remote branch
```
git push origin :chapter-two
```
or
```
git push --delete origin chapter-two
```

### Technique 16: Deleting the current local branch after merging
```
git branch --delete chapter-two
```

---

## Chapter 3: Filesystem interactions
### Technique 17: Renaming or moving a file: `git mv`
```
git mv old-name.file new-name.gile
```
```
git commit -m "Rename old-name.file to new-name.file"
```

### Technique 18: Removing a file: `git rm`
```
git rm old-name.file
```
```
git commit -m "Remove old-name.file"
```

### Technique 19: Resetting files to the last commit: `git reset`
#### Reset both the index `staging area` and the `working directory` to the state of the previous commit on the current branch.
```
git reset --hard
```
#### Reset the index `staging area` but not the contents of the `working directory`.
```
git reset
```
or
```
git reset --mixed
```

### Technique 20: Deleting untracked files: `git clean`
#### View the files that are currently tracked
```
git ls-files
```
#### View the currently untracked files
```
git ls-files --others
```
or
```
git ls-files -o
```
#### Delete untracked files (files that are in `working directory`)
```
git clean --force
```

### Technique 21: Ignoring files: `.gitignore`
#### Ignore all *.tmp* files.
Create an `.gitignore` file and add line **.tmp*
```
*.tmp
```

### Technique 22: Deleting ignored files
#### Delete all files that are ignored by `.gitignore`
```
git clean --force -X
```
#### Delete all ignored files and all the untracked files (files that are in `working directory`)
```
git clean -x
```
