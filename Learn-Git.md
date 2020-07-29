# `git`学习

`git`跟踪管理的是修改，而非文件。

* 工作区：就是在电脑里能看到的目录。
* 版本库：即隐藏目录`.git`，版本库里存了很多东西，其中最重要的就是称为`stage`或者叫`index`的暂存区，还有自动创建的第一个分支`master`以及指向`master`的一个指针叫`HEAD`。

## 1. 添加命令

### `git add`

* `git add -A`

  提交所有变化。

* `git add -u`

  提交被修改和被删除的文件，不包括新文件。

* `git add . `

  提交新文件和被修改的文件，不包括被删除的文件。

* `git add readme.txt`

  提交指定文件。

* `git add file1.txt`

* `git add file2.txt`

* `git add file3.txt`

  可以同时添加几个文件。

## 2. 提交命令

### `git commit`

* `git commit -m "message"`

  `message`代表提交说明，可以输入任何内容 。

## 3. 查看状态

### `git status`

掌握仓库当前的状态

## 4. 查看修改

### `git diff`

`git diff readme.txt`查看difference。

## 5. 查看日志

### `git log`

`git log`可以显示最近到最远的提交日志。

`git log --pretty=oneline`

可以简略好看的显示日志。

## 6. 版本回退

### `git reset`

`HEAD`指向的版本就是当前版本，`git`允许在历史版本之间穿梭，使用命令`git reset --hard commit_id`。版本回退的速度非常快，因为`HEAD`是一个指向当前版本的指针，当回退版本时，仅仅是把`HEAD`换了指向。

`commit_id`没必要写全，写前几位就可以了，`git`会自动去找，当然也不能致只写前一两位，因为`git`会找到多个版本号，就无法确定是哪一个了。

用`git log`查看提交历史，以便确定回退到哪个版本。

`git reflog`查看命令历史，忘掉commit_id时可以用到。

## 7. 撤销修改

### `git checkout`

当改乱了工作区某个文件的内容，想直接丢弃工作区的修改是，用`git checkout -- readme.txt`，让这个文件回到最近一次`git commit`或者`git add`时的状态。

当改乱了工作区某个文件的内容，还添加到了暂存区，想丢弃修改，第一步用`git reset HEAD readme.txt`,回到上面，在进行上面的操作。

## 8. 远程推送

### `git push`

关联一个远程仓库，使用命令`git remote add origin git@server-name:path/repo-name.git`

第一次推送`master`分支的所有内容：`git push -u origin master`。`origin`代表的是远程库。

`git remote`

查看远程库信息。

`git remote -v`

显示更详细的信息。

`git push origin master`

推送到远程库分支。

`git push origin dev`

推送到其他分支。

## 9. 克隆

通过`ssh`支持的原生`git`协议速度最快。

## 10. 分支管理

`git branch dev`

创建分支。

`git checkout dev`

切换到分支。

`git branch`

查看当前分支。

`git merge dev`

合并指定分支到当前分支。

`git branch -d dev`

删除分支。

`git log --graph`查看分支合并图。

通常合并分支时，如果可能`git`会采用`fast forward`模式，但这种模式下，删除分支后会丢掉分支信息。

强制禁用`fast forward`模式，`git`会在`merge`时生成一个新的`commit`，这样从分支历史上就可以看出分支信息。

`git merge --no-ff -m "merge with no-ff" dev`

`--no-ff`参数，表示禁用`fast forward`模式。

## 11. 储存现场

### `git stash`

可以把当前工作现场先储存起来，等以后恢复现场后再继续工作。

`git stash list`

查看工作现场。

`git stash apply`

恢复工作现场，但是`stash`内容不会删除，需要用`git stash drop`来删除。

`git stash pop`

恢复的同时把`stash`内容也删了。

## 12. 标签管理

### `git tag`

首先切换到需要打标签的分支上，然后`git tag v1.0`，就给分支打上了`v1.0`的标签。

`git tag`

可以查看所有标签。

`git tag -d v0.1`

删除标签。

创建的标签都只存储在本地，不会自动推送到远程。

`git push origin tagname`

推送一个本地标签。

`git push origin --tags`

推送全部未推送过的本地标签。

`git tag -d tagname`

删除一个本地标签。

`git push origin :refs/tags/tagname`

删除一个远程标签。