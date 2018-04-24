# GIT Training

A brief introduction for using GIT for source control

---

### Contents

- Basic Concepts
- Divergence and Conflicts
- Working with local repository
- Working with remote repository
- Git Workflows
- Tips
- Basic VIM operations

---

### Basic Concepts
- Overview
- Getting Started
- Special GIT files
- Commit
- Branch
- Basic check-in flow
- HEAD

![Press Down Key](assets/down-arrow.png)

+++?image=assets/overview.png&title=Overview&size=contain&color=white

+++

### Getting Started

Two ways to get started
- Clone an existing remote repository
```
git clone <clone url>
```
- Init a new repository
```
git init .
```

+++
### Special GIT files

- .git folder
- .gitignore

+++
### Commit

- Identified by 

```plain
sha1 (
    commit message,
    committer,
    commit date,
    author,
    author date,
    source tree hash,
    parents (can be 0, 1 or 2 parents)
)
```

+++
### Commit (cont'd)

- Immutable
- Commits form a tree (independent of branches)
- Exists in local and remote repos
- Push / Fetch to sync between repos

+++
### Demo: making a commit

```bash
git init .
echo "Some Content" > somefile.txt
git add .
git commit -m "Initial Commit with Some Content"
```

@[1](Initialize a local git repo)
@[2](Prepare some content)
@[3](Stage the changes)
@[4](Make a commit with message)

+++
### Branch
- Pointer to a commit
- Exists in local and remote repositories
- Default master branch is created for new repo
- Upstream

+++
### Branch - comparison with TFVC
- TFS: Branch as a container for changesets / files
- GIT: Commit tree exists independently, branches are just pointers to commits

+++?image=assets/tfsgitcomparison.png&title=TFS vs GIT&size=contain&color=white

+++
### Demo: making a branch
```bash
git checkout -b anotherbranch
git branch -vv

* anotherbranch 7be053a Initial Commit with Some Content
  master        7be053a Initial Commit with Some Content
```

@[1](Create a new branch)
@[2](Display branches (use -vv for "very verbose" output))
@[3-5](Output)
+++
### Demo: creating a commit on the new branch

```bash
echo "Another file" > file2.txt
git add .
git commit -m "Another file"
git branch -vv

* anotherbranch 6f3503c Another file
  master        7be053a Initial Commit with Some Content
```

@[1](Prepare another file)
@[2](Stage the changes)
@[3](Make another commit)
@[4](Display branches)
@[5-7](Output)

+++
### Basic check-in flow
- Create changes in WORKSPACE
- Stage changes in INDEX
- Commit staged changes into LOCAL REPO
- Push commits to REMOTE REPO

+++?image=assets/git-transport.png&title=GIT workflow&size=contain&color=white

+++
### Demo: basic check in

```bash
git clone https://github.com/NowcomCorporation/playground-git-exercise.git .
echo "some content" >> file1.txt
git status
git add .
git status
git commit -m "content 1"
git status
git branch -vv
git push origin master:master
git branch -vv
```

@[1](Clone the remote repo)
@[2-3](Create changes in WORKSPACE)
@[4-5](Stage changes in INDEX)
@[6-8](Commit staged changes into LOCAL REPO)
@[9-10](Push commits to REMOTE REPO)

+++
### HEAD
- Tells which branch or commit you are working against
- Usually point to a branch (Attached HEAD)
- Can point to a commit directly (Detached HEAD)
- Move HEAD around to go back in time

+++
### Demo: moving HEAD around

See [Move HEAD example](https://github.com/NowcomCorporation/playground-git/wiki/Example-MoveHead) in wiki

---

### Divergence and Conflicts

- How divergence happens
- Resolving divergence
- Demo - handle divergence
- Fast forward merge
- Conflict resolution

![Press Down Key](assets/down-arrow.png)

+++
### How divergence happens
1. John clones the repo locally
2. John makes changes and commit locally
3. Mary clones the repo locally
4. Mary makes changes and commit locally
5. Mary pushes the commit
6. John tries to push commit

** John is said to be "1 behind" and "1 ahead" of origin master

+++?image=assets/divergence/divergence-1.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-2.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-3.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-4.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->

+++
### Resolving divergence

Two options: 
- Option 1: Merge Commit
- Option 2: Rebase

+++
### Merge Commit

+++?image=assets/divergence/divergence-4.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-5.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-6.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-7.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->

+++
### Rebase

+++?image=assets/divergence/divergence-4.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-8.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-9.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->
+++?image=assets/divergence/divergence-10.png&size=contain&color=white
<!-- .slide: data-background-transition="none" -->


+++
### Demo: handle divergence

See [Merge example](https://github.com/NowcomCorporation/playground-git/wiki/Example-Merge) in wiki

+++
### Fast forward merge

- When there is a straight path from the commit the branch is currently pointing to, to the destination commit
- Push to remote can only happen when fast forward is possible
- Otherwise, it is a "force push", i.e. history rewrite

+++
### Conflict resolution

Steps to cause conflict:
```bash
git init .
echo line0 > file.txt
git add .
git commit -m "Initial commit"
git checkout -b branch1
echo line1 >> file.txt
git commit -am "Line 1"
git checkout master
git checkout -b branch2
echo line2 >> file.txt
git commit -am "Line 2"
```

+++
### What it looks like after the initial set up
```plain
  o branch1
 /
o master
 \
  o branch2
```
+++
### Merge commit
```bash
git checkout branch2
git merge branch1
[[ fix merge ... ]]
git add .
git commit --no-edit
```
+++
### Rebase commit
```bash
git checkout branch2
git rebase branch1
[[ fix merge ... ]]
git add .
git rebase --continue
```

---

### Working with local repository

- Visibility to local repository
- Basic operations
- More commits operations
- More on squash merge
- Things to pay attention to

![Press Down Key](assets/down-arrow.png)

+++
### Visibility to local repository
- git branch -vv (--all)
- git log --format=short
- git log --oneline (--graph)
- git status
- git rev-list
- git reflog
- git remote -vv

+++
### Basic operations
- git add .
- git commit -m / -am {{message}}
- git checkout {{branch}}
- git checkout -b {{branch}}
- git checkout {{file}}
- git branch -u {{remote name}}/{{remote branch}}
- git branch -D {{branch name }}
- git reset {{--hard / soft / mixed }} {{commit/branch}}

+++
### More Commits Operations
- Amend {{--no-edit}}
- Cherry-pick
- Merge --squash
- Show
- Diff

+++

### More on Squash merge
Purpose: to "squash" multiple commits into one to avoid unnecessary details in the main branch history

+++
### Squash merge illustrated

```plain
          D - E - F   feature
         /
A - B - C    master

git checkout master
git merge feature --squash
git commit --no-edit

A - B - C - G   master
(where G is squashed commit of D,E and F)
```
@[1-3](Initial State)
@[5](Switch to master branch)
@[6](Squash merge feature branch)
@[7](Make a commit of the squashed changes - no-edit to accept default commit message)
@[8-10](Outcome)

See example in [wiki](https://github.com/NowcomCorporation/playground-git/wiki/Example-Squash)

+++
### Things to pay attention to
- Use git fetch and rebase, or git pull --rebase. Avoid git pull only
- Feel free to create branch locally for feature development
- Squash merge to main branches to avoid cluttering with unnecessary details

+++
### cont'd
- Delete unused local branch
- Push yur commit to remote regularly (onto your own remote branch)
- Prefix your remote branch with your user ID for easy identification

---

### Working with remote repository

- Listing remotes
- Fetching, Pulling
- Push
- Other operations

![Press Down Key](assets/down-arrow.png)

+++
### Listing remotes

- git remote -vv
- git remote add

Listing branches in remote as well
- git branch -vv --all

Get remote branch into local and set upstream
- git checkout -b {{ branch name }} -t

+++
### Fetching, Pulling

Fetching from all remotes, and remove the deleted remote branch references

```
git fetch --all --prune
```

Pull latest commits, if there are divergence, automatically make a merge commit - should be avoided generally to keep things linear

```
git pull
```
+++

Pull rebase = fetch + rebase
```
git pull --rebase
```

However, prefer to do fetch and rebase in two steps so in between, you can check what was updated in remote by doing:
```
git rev-list master..origin/master
```

+++
### Push
General form - can push from any local branch to any remote (provided FF merge is possible):
```
git push {{remote name}} {{local branch name}}:{{remote branch name}}
```

+++
After creating a local feature branch and you want to push it to remote:

```
git push -u {{remote name}}
```

This creates a branch on remote with the same name as local branch, set upstream, and push commits

+++
### Clean up

Delete remote branch:

```
git push {{remote name}} :{{branch to delete}}
```

---
### Forward and Backward

- Four places for source:
  - Workspace
  - Index
  - Local Repo
  - Remote Repo

+++

## Workspace - Index

Workspace => Index (stage changes)

```plain
git add .
```

Workspace <= Index (unstage changes)
```plain
git checkout .
git clean -df
```

Note: checkout allow unstaging changes made to existing files. Clean allows removing new files.

+++

### Index - Local repo

Index => Local repo (take staged changes in index and create commit)
```plain
git commit -m "Commit Message"
```

Index <= Local repo (update index to use the same content as what's in commit)
```plain
git reset .
```

+++

### Index - Local repo (cont'd)

If you make a commit by mistake, then you need to first point to the previous commit using reset. The default reset mode "mix" will update the index as well.

```plain
git reset HEAD~1
```

+++

### Local repo - Remote repo

Local repo => Remote repo (push commit)
```plain
git push
```

Local repo <= Remote repo (pull / fetch)
```plain
git fetch
```

---
### Git Workflows
- Feature Branch Flow
- Git-Flow - https://github.com/nvie/gitflow
- GitHub Flow - https://guides.github.com/introduction/flow/
- Forking Flow
- For us? TBD

---
### Tips recap

- If you messed up locally and wanted to get back to a certain commit - run this:

```
git reset --hard {{commit_hash}}
git clean -df
```

- Fetch is 100% safe to do - do it more often
```
git fetch --all --prune
```

- Always know what branch you are working on
```
git branch -vv
```

+++
- Keep things linear - always rebase local branch

```
git pull --rebase
 - or -
git fetch --all --prune
git rebase origin/master
```

- Squash the details

```
git merge --squash
```

+++

- Clean up feature branches no longer in use (i.e. those merged into main branches already)

```
git branch -d branch_name
git push origin :branch_to_delete
```

- Double check what commits you are pushing to remote before actually pushing
- Do not rebase "shared" branches

---

### VIM operations

- "i" to enter edit mode, "esc" to exit edit mode
- ":q" to quit. ":q!" to quit without saving
- "dd" to delete line
- ":w" to save changes. ":wq" to save and quit
