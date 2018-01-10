
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

> `git reflog` 用git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令

> `git diff HEAD -- README.md` 命令可以查看工作区和版本库里面最新版本的区别

> `git checkout -- README.md` 意思就是，把README.md文件在工作区的修改全部撤销，这里有两种情况：

> 一种是README.md自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；  一种是README.md已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

> 总之，就是让这个文件回到最近一次git commit或git add时的状态。 `git checkout -- file` 命令中的`--`很重要，没有--，就变成了`切换到另一个分支`的命令，我们在后面的分支管理中会再次遇到git checkout命令。

