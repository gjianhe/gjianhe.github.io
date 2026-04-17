
# git-diff 和 apply 使用详解

## diff 和 patch 的区别  

git 提供了两种补丁方案，一种是通过 `git diff` 生成的 .diff 文件，第二种是通过 `git format-patch` 生成的 .patch 文件。

通过 git diff 生成的文件不含有  commit 信息，可以指定文件生成 diff，也可以指定单个 commit， 多个 commit 生成 。通过 git format-patch 生成的 .patch 文件 含有 commmit 信息。一个 commit 对应一个 patch 文件。

在开发当中，有时候，我们需要进行代码迁移，这时候就可以使用补丁，方便又快捷。
 

## 一、git diff：指定文件生成 patch 文件  
 

1、举例子：比如我们修改了 Test.java,Test1.java 文件，我们只想 patch Test.java 文件，那么我们可以使用以下的命令

> git diff Test.java > test.patch  
  
2、想把所有的修改文件打成 patch，即 Test.java,Test1.java 文件，只需要使用下面的命令

> git diff  > test.patch  
  
3、指定 commit id 生成 patch  
使用命令行

> git diff  老：【commit sha1 id】 新：【commit sha1 id】 >  【diff文件名】

## 二、应用patch

git apply   xxx.patch