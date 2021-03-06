
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

> `git log --graph` 命令可以看到分支合并图。, 当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

> `git merge --no-ff -m "test" dev`  合并dev分支，请注意--no-ff参数，表示禁用Fast forward：合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

> `git stash` 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作, 执行完毕后，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。 首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：流程如下

```
 git checkout master
 git checkout -b issue-101
 /*
 在master基础上创建的issue-101分支里修复bug
 修复完成继续下面动作
 */
 git checkout master
 git merge --no-ff -m "merged bug fix 101" issue-101
 git branch -d issue-101
```

> `git stash list` 修复完成后，`git checkout dev` `git status` 工作区是干净的，刚才的工作现场存到哪去了？用git stash list命令看看：

> 工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

> 一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

> 另一种方式是用`git stash pop`，恢复的同时把stash内容也删了, 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场。

> `git branch -D dev` 如果分支还没有被合并，删除，将丢失掉修改，直接执行`git branch -d feature-vulcan`会报错提示 ，如果要强行删除，需要使用命令(大写的D)`git branch -D dev`。

### 远程协助

> `git remote` 查看远程库的信息

> `git remote -v` 查看远程库的信息 `-v`显示更详细的信息

> `git push origin master` 推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上

> `git push` 当命令行不指定使用参数推送的位置时，将查询当前分支的branch.*.remote配置以确定要在哪里推送。 如果配置丢失，则默认为origin。

> `git push -u origin master` 如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。

```
    /*
    但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？
    1、master分支是主分支，因此要时刻与远程同步；
    2、dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
    3、bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
    4、feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。
    总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！
    */
```

### 标签

> `git tag v1.0` 首先，切换到需要打标签的分支上：`git tag v1.0`

> `git tag` 可以用命令git tag查看所有标签


> 默认标签是打在最新提交的commit上的。有时候，如果忘了打标签，比如，现在已经是周五了，但应该在周一打的标签没有打，怎么办？方法是找到历史提交的commit id，然后打上就可以了：

> `git log --pretty=oneline --abbrev-commit` 获取历史commitID, `git tag v0.9 6224937` 对应相应的id打上标签即可

> `git show v0.0`注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

> `git tag -a v0.1 -m "version 0.1 released" e9af1a8` 还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

> `git tag -s v0.2 -m "signed version 0.2 released" fec145a` 还可以通过-s用私钥签名一个标签： 签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错：

> `git tag -d v0.1` 如果标签打错了，也可以删除：因为创建的标签都只存储在本地，不会自动推送到远程。所以，打错的标签可以在本地安全删除。

> `git push origin v0.1` 如果要推送某个标签到远程，使用命令git push origin <tagname>：

> `git push origin --tags` 一次性推送全部尚未推送到远程的本地标签：

> `git tag -d v0.9`(本地删除) `git push origin :refs/tags/v0.9`(远程删除) 如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

> `git config --global color.ui true`  让Git显示颜色，会让命令输出看起来更醒目：