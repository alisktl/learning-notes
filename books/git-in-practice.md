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
