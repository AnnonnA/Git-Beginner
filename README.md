#### 1. 创建版本库 (Repository)

创建版本库其实很简单，首先选择需要管理的目录，比如，在 /d/Git/ 下创建一个 learngit 文件进行管理：

```
$ mkdir learngit
$ cd learngit
$ pwd
/d/Git/learngit
```

然后，通过通过 `git init` 命令把这个目录变成 Git 可以管理的仓库：

```
$ git init
Initialized empty Git repository in /d/Git/learngit/.git/
```

这样就建好了一个空仓库，并且 learngit 文件下多了一个 `.git` 的目录，这就是用来跟踪管理版本库的，很重要，但它默认是隐藏的，可以通过 `ls -ah` 命令看到。

#### 2. 追踪修改

Git 可以被分成三个分区，working directory、stage (暂存区) 和 repository (分为本地仓库和远程仓库)，我们在 workplace 进行修改，然后暂存到 stage，最后提交到仓库中，如果有远程仓库，可以将本地仓库 push 到远程仓库上，也可以从远程仓库 clone / fetch 到本地仓库中。Git 最基本的操作都是围绕这三个分区。

想要追踪修改，一定要放到 `learngit` 目录下 (子目录也行)，否则 Git 追踪不到该修改。比如，创建一个 readme.txt 文件：

```
$ touch readme.txt # 这时文件在 working directory，还未被追踪
```

可以使用 `$ git status` 来查看当前文件的状态：

```
$ git status
On branch master # 默认的分支 master
Untracked files: # readme.txt 还没有被追踪，需要使用 git add 来将其添加至 stage
  (use "git add <file>..." to include in what will be committed)
        readme.txt

nothing added to commit but untracked files present (use "git add" to track)
```

使用 `git add`  来将其添加至 stage：

```
$ git add readme.txt
$ git status
On branch master
Changes to be committed: # 已被追踪但还未提交至 repository
  (use "git restore --staged <file>..." to unstage)
        new file:   readme.txt

# git add 可以同时添加多个文件至 stage
$ git add readme_1.txt readme_2.txt
```

使用 `git commit` 来提交至 repository：

```
$ git commit -m "add readme.txt" # -m 表示 可以给提交添加 message，即后面引号内的提交说明
[master 097b868] add readme.txt # 提交说明
 1 file changed, 0 insertions(+), 0 deletions(-) # 修改说明，1 个文件变化，即创建 readme.txt
 create mode 100644 readme.txt
 
$ git status
On branch master
nothing to commit, working tree clean # 此时，working directory 中没有未被追踪的文件了，即 readme.txt 已被追踪
```

这样，一个修改的追踪过程就完成了。

如果，想要对 readme.txt 添加内容，可以直接使用 echo 命令：

```
$ echo "hello world" >> readme.txt # echo 后面引号内是添加内容，>> 是追加，其后面是路径
$ cat readme.txt
hello word
$ echo "cover hello world" > readme.txt # > 是先删除原有内容再添加内容
$ cat readme.txt
cover hello world
```

也可以直接使用 `vi readme.txt`，打开 vim 面板来修改文件内容：

```
$ vi readme.txt
```

执行上述命令后，会切换至 vim 界面：

```
cover hello world
~
~
~
readme.txt [unix] (12:57 23/05/2022)                                       1,1 All
"readme.txt" [unix] 1L, 18B
```

按 a 进行修改内容，修改结束后按 Esc，再按 :wq，保存并退出，或 :q 为不保存直接退出。

修改结束后，我们看下文件状态：

```
$ git status
On branch master
Changes not staged for commit: # 被追踪的文件在 working directory 被修改，还没有被 add 到 stage
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme.txt # readme.txt 被修改

no changes added to commit (use "git add" and/or "git commit -a")
```

如果该文件的改动还是需要被追踪的，那就继续上述 `git add` 和 `git commit` 的操作。

#### 3. 撤销修改

对应追踪修改，就一定会有撤销修改的需求，即，将提交文件从 repository 中撤销，暂存文件从 stage 中撤销，修改文件从 work directory 中撤销。

其实从上述 `git status` 指令中的提示中就可以找到答案：

```
(use "git restore <file>..." to discard changes in working directory)
(use "git restore --staged <file>..." to unstage)
```

以撤销 readme.txt 为例：

```
# 如果想撤销在 work directory 中修改的 readme.txt，使用：
$ git restore readme.txt
# 如果想撤销已暂存到 stage 中的 readme.txt，使用：
$ git restore --staged readme.txt
```

#### 4. 版本回退

如果想要撤销已提交至 repository 的 readme.txt，也就是将版本回退到上一个，需要使用：

```
# HEAD 是指向当前版本的指针，HEAD^ 表示上一个版本，HEAD^^ 表示上上个版本
# HEAD~n 表示前 n 个版本
$ git reset --hard HEAD^
```

