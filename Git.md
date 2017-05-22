```bash
# initialize git repo
git init

# add all changes
git add -A

# status of local changes
git status

# commit all tracked files
git commit

# get latest changes without merging
git fetch

# merge changes in local code
git merge

# fetch and merge changes on the current branch
git pull

# list branches
git branch

# change to branch-name
git checkout branch-name

# create branch-name and checkout
git checkout -b branch-name

# change to last branch
git checkout -

# merge current branch with branch-name
git merge branch-name

# delete branch-name
git branch -d branch-name

# save changes locally
git stash

# merge stashed changes to local
git stash apply

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

# display details and code changes of each commit
git log -p

# display only the three last commits
git log -3

# grep the commit messages for "copyright"
git log --grep="copyright"

# show difference of commited changes vs local
git diff

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
```
