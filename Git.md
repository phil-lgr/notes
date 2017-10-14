```bash
# initialize git repo
git init

# add all changes
git add -A

# status of local changes
git status

# commit all tracked files
git commit

# git add with patch, only add specific changes
git add --patch
--> y to stage, q to quit, n to skip

# get latest changes without merging
git fetch

# merge changes in local code
git merge

# fetch and merge changes on the current branch
git pull

# fetch all branches from the remote and set tracking
git branch -r | grep -v '\->' | while read remote; do git branch --track "${remote#origin/}" "$remote"; done

# fetch all branches (once they are checked out locally)
git fetch --all

# update local branches with remote
git pull --all

# list branches
git branch

# change to branch-name
git checkout branch-name

# create branch-name and checkout
git checkout -b branch-name

# checkout from a differente remote
git checkout -b <branchname> --track upstream/<branchname>

# change to last branch
git checkout -

# merge current branch with branch-name
git merge branch-name

# merge only one particular commit
git cherry-pick <commit-hash>

# delete branch-name
git branch -d branch-name

# save changes locally
git stash

# merge stashed changes to local
git stash apply

# check which changes you are about to pull
git checkout feature/fonts
git fetch
git log --oneline --no-merges ..origin/feature/fonts

# display last commits, navigate like in vim
git log

# formats git logs to one line
git log --oneline

# log a single commit changes
git show 21349j

# includes branch information of each commit
git log --decorate

# display git branches as graphs
git log --graph

# prettier graph
git log --graph --all --decorate --stat --date=iso
git log --graph --all --date=iso

# tree graph with no commit info
git log --graph --all --date=iso --pretty=oneline --abbrev-commit

# display details and code changes of each commit
git log -p

# display only the three last commits
git log -3

# grep the commit messages for "copyright"
git log --grep="copyright"

# see what's everyone has been getting to
git log --all --oneline --no-merges
git log --all --since='2 weeks' --oneline --no-merges

# show who has committed
git shortlog -sn

# over a period of time
git shortlog -sn --since='10 weeks' --until='2 weeks'

# review what you are about to push
git fetch
git log --oneline --no-merges <remote>/<branch>..HEAD

# show difference of commited changes vs local
git diff

# diff without whitespace
git diff -w

# display differences of local (cached and commited) vs the HEAD
git diff HEAD --stat

# only shows number of lines changes
git diff --stat

# fetch changes and preview the changes that would be merged
git fetch && git diff origin/branch-name

# for a specific file
git fetch && git diff origin/branch-name README.md

# check who changed a file
git blame filename.md

# create reference to a commit
git tag v1.0.1

# push tags to remote
git push --tags

# push branch and set upstream
git push -u origin branch-name

# interactive rebase, pick and choose commits to squash into one commit
# (this one affects git history)
git rebase -i origin/branch-name

# start bisect state
git bisect start

# mark current state as bad, i.e. tests are failing now
git bisect bad

# mark the last known commit as good, code was working properly back then
git bisect good 2131k3

# mark current commit as bad, so bisect can propose a new commit to test
git bisect bad

# exit bisect
git bisect reset

# start ignoring file
git update-index --assume-unchanged ./path/to/file

# create alias
git config --global alias.unstage 'reset HEAD --'

# aliases
git config --global alias.c 'commit'
git config --global alias.ck 'checkout'
git config --global alias.a 'commit --all'
git config --global alias.p 'push'
git config --global alias.s 'status'
git config --global alias.up 'pull upstream master'
git config --global alias.tree-stat 'log --graph --all --decorate --stat --date=iso'
git config --global alias.tree 'log --graph --all --decorate=short --date=iso --oneline'
```
