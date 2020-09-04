# Learning Git
## 0. Three trees of git
### working directory:
### index
* temporary area to prepare next commit
* stores `snapshots of changes` to be commited
### commit history
## 1. collaborating
### `remote`: manage `repository` connection info
```
git remote # show remote connections
git remote add <name> <url>
```
### `fetch`, download without merge
* internally, local and remote contents are seperated
```
git branch    #list local branch
git branch -r #list remote brach
```
* fetch from remote
```
git fetch
git fetch <remote-repo> <brach>
```
* after fetch, we can inspect it, but you are in `detached HEAD state`
```
git checkout remote-repo/branch-name
```
### `pull`, download and merge immediately
* git pull == git fetch <remote> and git merge origin/<current-brach>
* [--rebase](https://www.atlassian.com/git/tutorials/syncing/git-pull)
```
git pull            #a new merge commit will be add
git pull --rebase   #use `rebase` instead of merge
```

### `push`, push to the remote
```
git push <remote>
git push <remote> --all #push all braches
```
## 2. [Undo](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)
* think with in mind **3 threes in git**: working directory, staged snapshot, commit history
### `reset` vs `checkout`
* `reset` move **three threes** to match a specific commit
* `checkout` move HEAD ref point to commit, must commit or stash any changes before using it
### `reset`
* control what to match by option
```
git reset --mixed   #by default, match staged snapshot, working dir untack
git reset --hard    # match both staged snapshot and working dir
```
* disrecard all changes after last commit
```
git reset HEAD --hard
git reset HEAD~2 --hard   # reset HEAD backwards by 2 commit
```

## 3. Integrate changes
```
git checkout feature
git merge master

or

git checkout feature
git rebase master
```
### `merge` vs `rebase`
* `merge`, add new commit history to the end, non-destructive
* `rebase`, add brand new commit, destructive, but looks cleaner
* `rebase` is useful to rewrite history of a prifvate branch, never use it in publich branch

## 4. 'Stash'
* wanna a save of current working dir and index, but not commit
* after stash, working dir is clean, matching HEAD
* list, show, apply
```
git stash list  #list stack stack
git stash apply stash@{0}   #apply the most recent stash
git stash drop              #drop the last stash from stack
```
* stash before pull to avoid warning, stash back what is working on
```
git stash
git pull
git stash pop   # == git apply + git drop
```

## 5. `git rm`
* remove file from `index` or both `working tree and index`
* file to be removed must be identical to `tip of the brach`, unless use --cache
```
# --cached: remove index only, not directory, -- just indicate files are file not command
git rm --cached -- <files>
```

## 6. `branch`: list, create, delete branch
* list
```
git branch
# or
git branch --list
```
* create
```
git branch <new_brach>   # new a branch, will not checkout automatically
```
* rename
```
git branch -m <old> <new> # use -D if <new> already exists
```
* delete
```
git branch -d <branch>  # -D == --delete + --force
```

## 7. `.git/refs`, git is all about commits, refs makes it easy to refer to a commit, like pointer
* forms fo refs: `hashes`, `tags`, `branch`
* tip of branch is stored in `.git/refs/heads/<branch>`
### special refs
* in directory `.git/` could find these
```
HEAD
FETCH_HEAD
ORIG_HEAD
...
```
### `refspecs`
* map local branch to remote branch
* see `.git/config`, entries like \[+\]<src>: <dst>
  
### relative refs
* grandparents of HEAD
```
git show HEAD~2
```
## 8. [`merge`](https://git-scm.com/docs/git-merge)
merge history of another branch/commit to current branch
### merge
```
git merge     # without <commit> arg, will merge traching upstream
git merge MERGE_HEAD #merge commit from .git/MERGE_HEAD, which is created by last `git fetch`
```
### Fast-Forward Merge
* a special merge, that current branch tip is ancestor of the named commit.
* Fast-Forward Merge will not generate merge commit

### Conflict presentation 
* `<<<<<<`: yours
* `=====` : split line
* `>>>>>` : theirs

### Merge Conflict, abort or resolve
* when met conflict, can abort with 'git merge --abort'

### merge strategy
* specify using `-s`, possible strategy are `resolve`,`recursive(by default)`,`octopus` and some more

## 9. Config
* caching github password using credential.helper, need to set cache time expire
```
git config --global credential.helper cache
git config --global credential.helper 'cache --timeout=3600'

# after done this, will save credential on next push, and remember for 2hours
```
## 10.Workflow
* new branch and checkout immediately
```
git checkout -b new_feature_branch
```
* push to remote
```
git push origin new_feature_branch
```
* new a pull request and peer comment & review
* others could pull new branch and checkout it
```
git pull    #by default, will pull new branch, but wont create local new branch by default
git checkout new_feature_branch   # == git checkout -b new_feature_branch --track origin/new_feature_branch
```
* merge in commandline
```
1. my-feature-branch up-to-date
git checkout my-feature-branch
git pull origin my-feature-branch
2. up to date with master also, merge if necessary
git pull origin master
3. master up-to-date
git checkout master
git pull origin master
git merge --no-ff my-feature-branch
```
* delete my-feature-branch both for `local` and `remote`
```
git branch -d my-feature-branch
git push origin --delete my-feature-branch
```




## -1. QA
* 查看目前状态
```
git status
```
* 查看所有的commit 的历史
```
git log
```
* 查看当前所有的branch
```
git branch
```
* 在没有uncommited changes前提下，将整个目录回滚到某个branch/commit，可以用
```
git checkout <branch-name>
git checkout <commit-hash>
git checkout -- <file>  # discard uncommited changes of a file
```
* 丢掉目前的所有staged but uncommited changes,
```
git reset --hard <commit-hash>
```
* unstage staged changes
```
git reset <file>
git reset <commit-hash>
```
* add a new git repo
```
on github, create a new repo, then:
git remote add origin https://github.com/hackerekcah/xxxxxxx.git
git push -u origin master
```

* `cherry-pick`, designed to pick one/multipy existing commits of branch1 to apply to branch2
```
git checkout branch2
git cherry-pick -ff <commits from branch1>
```

* show a remote repo detail
```
git remote show origin
```

* show diff in last commit
```
git show <COMMIT hash>
```

* diff modified
```
git diff <file_name>
```

* amend/edit last commit message(only if not pushed to remote)
```
# will open text editor, can edit message
git commit --amend
```

* difference between `git clone` and download zip
```
`clone` also get .git file, so you have the full repository. while downloaded zip wouln't
```

* git diff two branches
```
# comapre tips of two branches
git diff branch_1..branch_2
```

