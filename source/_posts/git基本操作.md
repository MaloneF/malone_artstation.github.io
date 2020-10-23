***
### 获取ssh公钥
* 1.查看系统本用户路径下有没有.ssh目录，一般在C:\Users\admin 下。
* 2.创建ssh key。 在任意位置打开Git Bush终端，运行 
     ssh-keygen -t -C "youremail@example.com"
* 3.查看.ssh目录，可以看到里面有id_rsa和id_rsa.pub两个文件。id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，待会要用到。
* ***
### 连接远程仓库
#### 已有远程仓库，拉取到本地做开发：
*   获取到远程仓库的地址
*   运行命令    git clone 远程地址
#### 本地项目推送到gitHub
* 1.创建本地仓库。
* 2.连接远程仓库，把本地仓库与远程仓库连接起来，运行命令：git remote add origin 
* 3.运行    git remote -v 查看连接的远程仓库信息。
> -v选项(译注:此为 –verbose 的简写,取首字母),显示对应的克隆地址。origin 为远程地址的别名。

* 4.git push 即可。
* ***
### git常用命令
#### 基本概念
* 工作区：在电脑上能看见的文件目录。
* 暂存区：英文叫 stage 或 index。一般存放在 .git 目录下的 index 文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。
* 工作区有一个隐藏目录 .git，这个不算工作区，而是 Git 的版本库。
![](https://github.com/MaloneF/Socure/raw/master/img/1.png)
* 当对工作区修改（或新增）的文件执行 git add 命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的ID被记录在暂存区的文件索引中。
* 当执行提交操作（git commit）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 指向的目录树就是提交时暂存区的目录树。
* 当执行 git reset HEAD 命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
* 当执行 git rm --cached <file> 命令时，会直接从暂存区删除文件，工作区则不做出改变。
* 当执行 git checkout . 或者 git checkout -- <file> 命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
* 当执行 git checkout HEAD . 或者 git checkout HEAD <file> 命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。
* *** 
#### git创建仓库
 *        git init
> 该命令执行完后会在当前目录生成一个 .git 目录。
   
* 使用我们指定目录作为Git仓库。 
    git init newrepo
> 初始化后，会在 newrepo 目录下会出现一个名为 .git 的目录，所有 Git 需要的数据和资源都存放在这个目录中。
***
#### git clone
将远端仓库克隆到本地

* git clone git@github.com:fsliurujie/* test.git         --SSH协议
* git clone git://github.com/fsliurujie/test.git          --GIT协议
* git clone https://github.com/fsliurujie/test.git      --HTTPS协议
***
#### 提交与修改
![](https://github.com/MaloneF/Socure/raw/master/img/2.png)

* workspace：工作区
* staging area：暂存区/缓存区
* local repository：或本地仓库
* remote repository：远程仓库
    git diff 比较文件的不同，即暂存区和工作区的差异。
    git commit 提交暂存区到本地仓库。（-m 添加备注）
    git reset 回退版本。
    git mv 移动或重命名工作区文件。
    git fetch 从远程获取代码库。
***
#### git fetch
从远程获取代码库。 
该命令执行完后需要执行 git merge 远程分支到你所在的分支。
从远端仓库提取数据并尝试合并到当前分支：

    git merge
该命令就是在执行 git fetch 之后紧接着执行 git merge 远程分支到你所在的任意分支。
假设你配置好了一个远程仓库，并且你想要提取更新的数据，你可以首先执行:

    git fetch [alias]  [alias]是别名，一般用origin
如果是从github上clone下来的仓库，则只需git fetch 即可。

以上命令告诉 Git 去获取它有你没有的数据，然后你可以执行：

    git merge [alias]/[branch]

以上命令将服务器上的任何更新（假设有人这时候推送到服务器了）合并到你的当前分支。 
***
#### git pull
用于从远程获取代码并合并本地的版本。
git pull 其实就是 git fetch 和 git merge FETCH_HEAD 的简写。 命令格式如下：
 
    git pull <远程主机名> <远程分支名>:<本地分支名>
实例

更新操作：

    git pull
    git pull origin

将远程主机 origin 的 master 分支拉取过来，与本地的 brantest 分支合并。

    git pull origin master:brantest

如果远程分支是与当前分支合并，则冒号后面的部分可以省略。

    git pull origin master
***
#### git pull
git push 命用于从将本地的分支版本上传到远程并合并。
git pull 其实就是 git fetch 和 git merge FETCH_HEAD 的简写。 命令格式如下:

    git push <远程主机名> <本地分支名>:<远程分支名>
实例

以下命令将本地的 master 分支推送到 origin 主机的 master 分支。

    git push origin master

相等于：

     git push origin master:master

如果本地版本与远程版本有差异，但又要强制推送可以使用 --force 参数：

    git push --force origin master

删除主机但分支可以使用 --delete 参数，以下命令表示删除 origin 主机的 master 分支：

    git push origin --delete master
***
#### Git分支管理
 创建分支命令：

    git branch (branchname)

切换分支命令:

    git checkout (branchname)

当你切换分支的时候，Git 会用该分支的最后提交的快照替换你的工作目录的内容， 所以多个分支不需要多个目录。

合并分支命令:

    git merge 


你可以多次合并到统一分支， 也可以选择在合并之后直接删除被并入的分支。 
列出分支

列出分支基本命令：

    git branch

没有参数时，git branch 会列出你在本地的分支。

    git branch
    master

如果创建新分支后来又有更新提交， 再切换到了新分支，Git 将还原你的工作目录到你创建分支时候的样子。

创建新分支并立即切换到该分支下：

     git checkout -b (branchname)
删除分支命令：

    git branch -d (branchname)
当合并分支时如果出现冲突需要手动修改后在用git add 将文件提交。
***
#### 查看Git提交历史
 * git log - 查看历史提交记录。
* git blame <file> - 以列表形式查看指定文件的历史修改记录。
可以用 --oneline 选项来查看历史记录的简洁的版本。
* git reset +id 回滚到存档
* 我们还可以用 --graph 选项，查看历史中什么时候出现了分支、合并。以下为相同的命令，开启了拓扑图选项。
* 也可以用 --reverse 参数来逆向显示所有日志。
* 如果要查看指定文件的修改记录可以使用 git blame 命令，格式如下：git blame <file>
* git tag v1.0 创建标签
* git tag 查看已有标签
* git tag -d v1.1 删除标签
* git show v1.0 查看此版本修改内容
 