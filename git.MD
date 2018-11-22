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
