# git-查看某个文件的修改记录

## 查看某个文件的commit记录

```
git log filename
```


## 查看文件每次提交的diff

```
git log -p filename 
```


## 列出文件的所有改动历史

```
git log --pretty=oneline filename
```

## 只查看某次提交的文件变化

```
git show 提交生成的一次哈希值 filename
```
