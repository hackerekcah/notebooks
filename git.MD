# Learning Git
## collaborating
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
## [Undo](https://www.atlassian.com/git/tutorials/resetting-checking-out-and-reverting)
* think with in mind **3 threes in git**: working directory, staged snapshot, commit history
### `reset` vs `checkout`
* `reset` move **three threes** to match a specific commit
* `checkout` move HEAD ref point to commit
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

## Integrate changes
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

## 查看目前状态
```
git status
```
## 查看所有的commit 的历史
```
git log
```
## 查看当前所有的branch
```
git branch
```
## 在没有uncommited changes前提下，将整个目录回滚到某个branch/commit，可以用
```
git checkout <branch-name>
git checkout <commit-hash>
```
## 丢掉目前的所有staged but uncommited changes,
```
git reset --hard <commit-hash>
```
## unstage staged changes
```
git reset <file>
git reset <commit-hash>
```
## add a new git repo
```
on github, create a new repo, then:
git remote add origin https://github.com/hackerekcah/xxxxxxx.git
git push -u origin master
```
