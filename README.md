
## git命令

> `pwd` 命令用于显示当前目录。

> `ls -ah` 可以查看包含隐藏文件的全部文件夹

> `git add README.md` 添加文件到仓库

> `git commit -m "wrote a readme file"` 把文件提交到仓库。 `-m` 后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。

> `git status` 命令可以让我们时刻掌握仓库当前的状态，上面的命令告诉我们，readme.md被修改过了，但还没有准备提交的修改。

> `git diff README.md`  顾名思义就是查看difference，显示的格式正是Unix通用的diff格式，可以从上面的命令输出看到修改的内容部分

> `git log` 查看历史版本, git log命令显示从最近到最远的提交日志，我们可以看到3次提交，最近的一次是append GPL，上一次是add distributed，最早的一次是wrote a readme file。

> `git log --pretty=oneline` 查看的是类似一大串类似3628164...882e1e0的是commit id（版本号）

> `git reset --hard HEAD^`  现在，我们要把当前版本“append GPL”回退到上一个版本“add distributed”，就可以使用git reset命令：

> 注意：首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交3628164...882e1e0（注意我的提交ID和你的肯定不一样），上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

> `git reset --hard (e9af1a8f5e commit id)` 指定回到未来的某个版本,版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。

> `git reset HEAD README.md` 是将暂存区的文件撤回到工作区, 可以把暂存区的修改撤销掉（unstage），重新放回工作区,这种状态是 git add 之后，可以把add到暂存区的操作撤销

> `git reflog` 用git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令

> `git diff HEAD -- README.md` 命令可以查看工作区和版本库里面最新版本的区别

> `git checkout -- README.md` 意思就是，将工作区的修改恢复到之前未修改的状态（即之前的状态）。把README.md文件在工作区的修改全部撤销，这里有两种情况：

> 一种是README.md自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；  一种是README.md已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

> 总之，就是让这个文件回到最近一次git commit或git add时的状态。 `git checkout -- file` 命令中的`--`很重要，没有--，就变成了`切换到另一个分支`的命令，我们在后面的分支管理中会再次遇到git checkout命令。


```
小结

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD file，就回到了场景1，第二步按场景1操作 (git checkout -- file)。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节(git reset --hard (e9af1a8f5e commit id))，不过前提是没有推送到远程库。
```


> `git rm test.txt`  从版本库中删除该文件, 确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit： 文件就从版本库中被删除了。

> 另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本： $ git checkout -- test.txt, git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。


### 远程仓库

> `sh-keygen -t rsa -C tzvuf@163.com`  创建SSH Key。在用户主目录(C:\Users\Administrator\.ssh)下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

> 把本地库的所有内容推送到远程库上：把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

> `git remote add origin https://github.com/ataotao/git.git`

> `git push -u origin master`

> `git push origin master` 从现在起，只要本地作了提交，就可以通过此简化命令

> `git clone git@github.com:ataotao/git.git`  从远程库克隆


### 分支管理

> `git checkout -b dev` 创建dev分支，然后切换到dev分支

> git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```
> `git branch` 命令会列出所有分支，当前分支前面会标一个*号。

> `git merge dev`命令用于合并指定分支到当前分支。合并后，再查看readme.txt的内容，就可以看到，和dev分支的最新提交是完全一样的

> `git branch -d dev` 删除分支

Creating a new branch is quick & simple.
