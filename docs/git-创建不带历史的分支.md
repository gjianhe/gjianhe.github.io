
# git-创建不带历史的分支


有些时候想要创建一个不带历史记录的 git 分支，比如要从原来在本地开发的项目中，将代码 push 到 GitHub 开源，不想分享糟糕的历史提交记录，那就可以创建一个不带历史记录的分支。

查看 `git checkout --help` 的帮助说明，可以看到其中有一个选项是`--orphan`，就是创建一个孤立的分支，这个分支上的第一个提交不回有任何的 parents 节点。如下：

```
--orphan <new-branch> 
Create a new orphan branch, named <new-branch>, started from <start-point> and switch to it. The first commit made on this new branch will have no parents and it will be the root of a new history totally disconnected from all the other branches and commits.
```

所以我们可以做如下的操作：

```
git checkout --orphan new_branch
git add .
git commit -m "Init commit"
git push -u origin new_branch
```