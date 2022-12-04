# Git in Practice
by Mike McQuaid

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
