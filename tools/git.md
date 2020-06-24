git使用教程
=
## 一、本地git的使用
### 1.本地git仓库组成
+ 工作区：本地git仓库的编辑内容所在区域，所有可提交的文件都包含在此区域内。
+ 版本库：本初始化的本地git仓库，在对应文件夹内会生成.git文件夹，该.git文件夹即为版本库所在区域。版本库分为两部分：
    + 暂存区（stage或index）：在工作区中所作的修改提交，会先存入该空间中。
    + 分支区：暂存区中相对于当前主版本的修改内容，再进行提交，就会合并到当前主版本分支中。除了主版本分支外，还可以根据需求创建开发分支、测试分支、bug修复分支等。
### 2. git仓库配置与初始化
#### 2.1 git软件安装
+ Linux系统：编译git源码，从软件仓库直接安装，或下载git软件包安装
+ windows系统：下载git软件包安装
+ Mac系统：通过homebrew安装，或在Xcode中安装
#### 2.2 git账户配置
+ 配置账户：`git config --global user.name "[用户名]"`
+ 配置邮箱：`git config --global user.email "[用户邮箱]"`
+ 配置git命令行色彩显示：`git config color.ut true`
_注：`git config`命令中的`--global`参数，表示配置针对当前系统的所有git仓库。若无此参数，则表示配置仅针对当前git仓库进行配置。_
#### 2.3 git仓库的初始化
在存放git仓库的空目录路径下，使用`git init`命令初始化该git仓库。初始化后的git仓库目录中会生产一个_.git_文件夹，此文件夹为版本库空间。
### 3.本地git的使用
#### 3.1 修改内容的提交
+ 将修改内容的操作提交到版本库中的暂存区：`git add [修改文件]`
+ 将删除文件(夹)的操作提交到版本库中的暂存区：`git rm (-r) [删除文件]`
+ 将版本库中暂存区内的修改记录合并到版本库的当前版本分支中：`git commit -s -m "commit节点注释"`
_注1：提交删除文件操作的`git rm`命令的`-r`参数，表示删除的为文件夹；_
_注2：合并到当前版本分支的`git commit`命令的`-s`参数，表示在commit节点中添加提交账户；该命令的`-m`参数，表示向commit节点中添加节点注释。_
#### 3.2 内容回退
+ 未提交到版本库的暂存区中的修改操作：`git checkout -- [文件名]`（**慎用**）
+ 已提交到版本库的暂存区中的修改操作：`git reset HEAD [文件名]`(_仅回退版本库暂存区内容，工作区中修改内容仍然存在。_)
+ 已合并到版本库的版本分支中的修改操作：`git reset --hard HEAD^`
+ 当前版本分支不同commit节点内容间的跳转：`git reset --hard [commit_id]`
_注1：`HEAD`表示指向当前分支版本的当前commit节点的指针；_
_注2：`git reset`命令中的`--hard`参数，表示`HEAD`指针的指向将被移动；_
_注3：`HEAD`加n个`^`，或`HEAD~n`，可表示当前`HEAD`指针位置的前n个commit节点位置；_
_注4：`HEAD`指针回退过后，回退之前指向的commit节点依然存在，在未关闭当前命令行窗口前，仍然可以通过commit_id回退到该commit节点。_
#### 3.3 操作过程中的状态查看：
+ `git status`:查看当前的修改操作是否提交，是否合并到当前版本分支；
+ `git log:`查看当前版本分支的所有commit节点记录，使用`--pretty=oneline`参数，可减少commit节点显示的内容，使用`--graph`参数，可将所有分支版本以commit节点构成的网进行显示。
#### 3.4 本地git仓库的分支管理
+ 分支创建：`git branch [分支名]`
+ 分支切换：
    + 新版：`git switch [分支名]`
    + 旧版：`git checkout [分支名]`
+ 创建新分支并切换到新分支：
    + 新版：`git switch -c [分支名]`
    + 旧版：`git checkout -b [分支名]`
+ 分支合并：`git merge [指定分支名]`，将指定分支与当前分支合并。
_注1：使用`git merge`命令合并不同分支时，git默认采用Fast forward模式，在该模式下，删除分支会丢掉分支信息，同时使用`git log`命令也无法查看到被删除的分支的commit节点信息。_
_注2：`git merge`命令中，可使用`--no-ff`参数强制禁止Fast forward模式。_
+ 分支删除：
    + 已合并分支删除：`git branch -d [分支名]`
    + 未合并分支删除：`git branch -D [分支名]`
+ 分支查看：`git log --graph --pretty=oneline`
+ 跨分支提交：`git cherry-pick [commit_id]`，可将非当前分支的commit节点修改内容，合并到当前分支中。
+ stash暂存的使用：
    1. 使用`git stash`命令，可将当前未合并到当前分支的修改工作暂存到当前分支上（此时使用`git status`无法查看到修改状态）；
    2. 向当前分支中恢复暂存到stash中的内容：
        + 使用`git stash apply`命令，可将stash暂存中的修改工作回复到当前分支，但stash暂存中的修改工作记录不会被删除；
        + 使用`git stash pop`命令，可将stash暂存中的修改工作回复到当前分支，同时stash暂存中的修改工作记录也会被删除；
    3. 当切换回当前分支后，可使用`git stash list`查看暂存到stash上的内容：
#### 3.5 本地git仓库的标签操作
    1. 标签创建
    + 在当前分支当前节点处创建标签：`git tag [标签名]`
    + 任意commit节点处创建标签：`git tag [标签名] [commit_id]`
    + 带注释标签创建：`git tag -a [标签名] -m [commit_id]`
    2. 标签删除：`git tag -d [标签名]`
    3. 标签查看：
        + `git tag`：将所有标签按照名称排序列出；
        + `git show [标签名]`：查看特定标签所指向的commit节点的状况。
## 二、服务器端git的使用
### 1.密钥的生成
`ssh-keygen -t rsa -C [用户邮箱]`，运行该命令后，将在本地生成一个`.ssh`目录，在该目录内包含`id_rsa`和`id_rsa.pub`，`id_rsa`存储私钥，`id_rsa.pub`存储公钥。
### 2.本地git服务器的搭建
#### 2.1 搭建git服务器
1. 安装git软件；
2. 创建git管理用户：`sudo adduser [用户名]`；
3. 创建登录证书：将所有用户电脑上的`id_rsa.pub`文件中的公钥导入到`.ssh`目录下的`authorized_keys`文件中，文件中每一行存储一个用户的公钥；
4. 初始化git仓库：
    1. 选择存放仓库的空白目录，在命令行的该目录路径下执行`sudo git init --bare [仓库名].git`命令，初始化git仓库；
    2. 使用`sudo chown -R [管理用户分组]:[管理用户用户名] [仓库名].git`命令，修改仓库管理账户；
    _注：`-R`参数表示对目录以迭代模式进行操作。_
5. 禁止本地shell登录，改为远程ssh登录：将`/etc/passwd`文件的`:/home/git:/bin/bash`改为`:/home/git:/bin/git-shell`
#### 2.2 管理公钥
+ 开发人员少：将所有开发人员的公钥导入到`.ssh`目录下的`authorized_keys`文件中；
+ 开发人员多：Gitosis软件
#### 2.3 管理权限
使用Gitosis软件
### 3.在线git服务器
#### 3.1 在线git服务器配置
1. 创建在线git服务账号；
2. 将本机git公钥导入在线git服务账号中；
3. 创建线上git仓库；
4. 将本地仓库与线上仓库关联：`git remote add [线上仓库本地命名] [线上仓库地址]`
#### 3.2 git服务器的使用
+ 本地仓库的提交：
    + 第一次：`git push -u [线上仓库本地命名] [本地仓库分支名]`
    + 非第一次：`git push [线上仓库本地命名] [本地仓库分支名]`
    _注：`git push`命令的`-u`参数，是将本地仓库分支与线上仓库分支关联。_
    + 使用`git remote`命令可查看所有线上分支情况。
+ 远程仓库克隆：`git clone [线上仓库地址]`
_注：线上仓库地址有ssh、https等多种协议，其中ssh协议是最快的。_
+ 标签操作：
    - 推送一个本地标签：`git push [线上仓库本地命名] [标签名]`
    - 推送所有本地未推送的标签：`git push [线上仓库本地命名] [标签名] --tags`
    - 删除一个远程标签：`git push [线上仓库本地命名] :refs/tags/[标签名]`