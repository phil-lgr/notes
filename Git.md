# Git

initialize git repo:
```
git init
```

add all changes:
```
git add -A 
```

status
```
git status
```

commit changes to tracked files
```
git commit
```

tells local repo to grab latest changes without merging
```
git fetch 
```

merge fetched changes in local code
```
git merge 
```

fetch and merge changes on the current branch
```
git pull 
```

list branches
```
git branch 
```

git checkout branch-name change to branch-name
```

git checkout -b branch-name create branch-name and checkout
```

git checkout - change to last branch
```

git merge branch-name merge current branch with branch-name
```

git branch -d branch-name delete branch-name
```

git stash save changes locally
```

git stash apply take stashed changes and reapply them
```

git log display last commits, navigate like in vim
```

git log --oneline formats git logs to one line
```

git show 21349j log a single commit changes
```

git log --decorate includes branch information of each commit
```

git log --graph display git branches as graphs
```
git log -p display details and code changes of each commit
```
git log -3 display only the three last commits
```
git log --grep="copyright" grep the commit messages for "copyright"
```
git diff show difference of commited changes vs local
```
git diff HEAD --stat display differences of local (cached and commited) vs the HEAD
```
git diff --stat only shows number of lines changes
```
git fetch && git diff origin/branch-name fetch changes and preview the changes that would be merged
```
git fetch && git diff origin/branch-name README.md for a specific file
```
git blame webpack.prod.config.js check who changed a file
```
git tag v.1.0.1 create reference to a commit
```
git push --tags push tags to remote
```
git push -u origin branch-name push branch and set upstream
```
git rebase -i origin/branch-name interactive rebase, pick and choose commits to squash into one commit
```
this one changes git history
```
git bisect start start bisect state
```
git bisect bad mark current state as bad, i.e. tests are failing now
```
git bisect good 2131k3  mark the last known commit as good, code was working properly back then
```
git bisect bad mark current commit as bad, so bisect can propose a new commit to test
```
git bisect reset exit bisect
```
