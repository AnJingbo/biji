

# Git/GitHub 笔记
## 一、Git 概述
Git 是一个免费的、开源的**分布式版本控制系统**，可以快速高效地处理从小型到大型的各种项目。

Git 易于学习，占地面积小，性能极快，它具有廉价的本地库、方便的暂存区域和多个工作流分支等特性，其性能优于 Subversion、CVS、perforce 和 ClearCase 等版本控制工具。
### 1.1 什么是版本控制
版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。

版本控制最重要的其实是可以记录文件修改的历史记录，从而让用户能够查看历史版本。
### 1.2 为什么需要版本控制
从个人开发过渡到团队协作。
### 1.3 版本控制工具
* 集中式版本控制工具

CVS、SVN、VSS 等

集中化的版本控制系统如：CVS、SVN 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连接到这台服务器，取出最新的文件或者提交更新。

这种做法带来了许多好处，每个人都可以在一定程度上看到项目中的其他人正在做什么，而且管理员也可以轻松掌握每个开发者的权限，并且管理一个集中化的版本控制系统，要远比在各个客户端上维护本地数据库来得轻松容易。

这种做法的缺点是中央服务器的单点故障，如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。
* 分布式版本控制工具

Git、Mercurial、bazaar、Darcs 等。

像 Git 这种分布式版本控制工具，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（到本地库），这样任何一处协同工作用的文件发生故障，事后都可以用其他客户端的本地仓库进行修复，因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。

分布式的版本控制系统出现之后，解决了集中式版本控制系统的缺陷：
1、服务器断网的情况下也可以进行开发，因为版本控制是在本地进行的。
2、每个客户端保存的都是整个完整的项目（包含历史记录），更加的安全。
### 1.4 Git 简史
![图示](https://img-blog.csdnimg.cn/20210706110301886.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.5 Git 工作机制
![图示](https://img-blog.csdnimg.cn/20210706110357845.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 1.6 Git 和代码托管中心
代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为远程库。
![图示](https://img-blog.csdnimg.cn/20210706111532363.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 二、Git 常用命令
![图示](https://img-blog.csdnimg.cn/20210707144627883.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 2.1 设置用户签名
基本语法：
* `git config --global user.name 用户名`
* `git config --global user.email 邮箱`
* 可以通过命令：`cat ~/.gitconfig` 去查看设置。或者去 C:\Users\当前正在使用的用户\\.gitconfig 去查看设置。

说明：签名的作用是区分不同的操作者身份。用户的签名信息在每一个版本的提交信息中能够看到，以此确认本次操作是谁做的。**Git 首次安装必须设置一下用户签名，否则无法提交代码。**

注意：这里设置的用户签名和将来登录 GitHub（或者其他代码管理中心）的账号没有任何关系。
### 2.2 初始化本地库
基本语法：`git init`

实例：
![实例](https://img-blog.csdnimg.cn/20210707150933805.png)
注：要在保存本地库的文件下面右击打开 Git Bash Here（我的是在 D/Git/GitSpace/ 目录下右击打开），然后执行 git init 命令。新建的 .git 文件在 windows 里面默认是隐藏的，需要在 `查看` 里面去设置显示`隐藏的项目`。
### 2.3 查看本地库状态
基本语法：`git status`
### 2.4 将工作区的文件添加到暂存区
基本语法：`git add 文件名`
### 2.5 将暂存区的文件提交到本地库
基本语法：`git commit -m "日志信息" 文件名`

注：修改过文件后，还需要再次添加到缓存区，并提交到本地库。

注：修改过一次文件之后，文件还是只有一个。**因为 Git 控制版本用的不是副本，底层是利用指针来控制。**
### 2.6 查看版本信息
基本语法：
* `git reflog`：查看版本信息
* `git log`：查看日志版本详细信息。
### 2.7 版本穿梭
基本语法：`git reset --hard 版本号`

**Git 切换版本，底层其实是移动的 HEAD 指针。HEAD 指针一直指向的是当前分支（此时是 master），当前分支（master）的指针一直在变化。**
![图示](https://img-blog.csdnimg.cn/20210710104004471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 三、Git 分支操作
![图示](https://img-blog.csdnimg.cn/20210710110740365.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 3.1 什么是分支
在版本控制过程中，有的时候需要同时推进多个任务，那么我们就可以为每个任务创建每个任务单独的分支。使用分支意味着程序员可以把自己的工作从开发主线上分离开来，开发自己分支的时候，不会影响到主线分支的运行。（分支底层其实也是指针的引用，HEAD 指针一直指向的是当前分支。）
### 3.2 分支的好处
（1）同时并行推进多个功能开发，提高开发效率。

（2）各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响，失败的分支删除重新开始即可。
### 3.3 分支的操作
![图示](https://img-blog.csdnimg.cn/20210710111419935.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 3.4 合并分支
基本语法：`git merge 分支名`

注意：如果要把 hot-fix 分支合并到 master 分支，那么当前分支必须是 master 分支。
![图示](https://img-blog.csdnimg.cn/20210710140713335.png)
### 3.5 冲突合并
* 冲突产生的表现：后面状态为 MERGING。如下图：
![图示](https://img-blog.csdnimg.cn/20210710141504674.png)
* 产生冲突的原因：合并分支时，两个分支在同一个文件有两套不同的修改，Git 无法替我们决定使用哪一个，不知道以哪一个修改为标准，因此**必须人为决定新代码的内容**。
* 解决冲突：
（1）编辑有冲突的文件，删除特殊符号，决定要使用的内容
&nbsp;&nbsp;特殊符号：
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<<<<<<< HEAD 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;当前分支的代码
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=======
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;合并过来的代码
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\>>>>>>> hot_fix
![图示](https://img-blog.csdnimg.cn/20210710143319933.png)
（2）添加到暂存区
![图示](https://img-blog.csdnimg.cn/2021071014200517.png)
（3）执行提交到本地库（**注意：此时使用 git commit 命令的时候不能带文件名**）
![图示](https://img-blog.csdnimg.cn/20210710142113377.png)
## 四、Git 团队协作
### 4.1 团队内协作
![图示](https://img-blog.csdnimg.cn/20210710150436558.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 4.2 跨团队协作
![图示](https://img-blog.csdnimg.cn/20210710151037639.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
## 五、GitHub 操作
![图示](https://img-blog.csdnimg.cn/20210710161515698.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 5.1 创建远程库别名
基本语法：
* `git remote add 别名 远程地址`。远程地址是在创建完远程库后生成的链接。
* `git remote -v` ：查看当前所有远程别名地址
### 5.2 推送本地分支到远程仓库
基本语法：`git push 别名 分支`
### 5.3 拉取远程库到本地库
基本语法：`git pull 别名 分支`

拉取会自动帮你提交到本地库。

>注意：push 是将本地库代码推送到远程库，如果本地库代码和远程库代码版本不一致，push 的操作是会被拒绝的。也就是说，要想 push 成功，一定要保证本地库的版本要比远程库的版本高！**因此一个成熟的程序员在动手改本地代码之前，一定会先检查远程库和本地库代码的区别。如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！**
>pull 是拉取远程仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。
### 5.4 克隆远程仓库到本地
* 基本语法：`git clone 远程地址`
* 克隆代码是不需要登陆 Github 的
* 克隆会做如下操作：
（1）拉取代码
（2）初始化本地仓库
（3）创建别名，默认是 origin
### 5.5 SSH 免密登陆
我们可以看到远程仓库中还有一个 SSH 的地址，因此我们也可以使用 SSH 进行访问。
具体操作如下：
（1）进入当前用户的目录（C:\Users\当前用户名）
（2）点击 Git Bash Here，输入命令`ssh-keygen -t rsa -C atguiguyueyue@aliyun.com `，然后连续敲 3 次回车，生成 .ssh 密钥目录（注意：这里的 -C 这个参数是大写的 C）
## 六、IDEA 集成 Git
### 6.1 配置 Git 忽略文件
* 问什么要忽略它们？
与项目的实际功能无关，不参与服务器上的部署运行。把它们忽略掉能够屏蔽开发工具之间的差异。
* 怎么忽略？
（1）创建忽略规则文件 xxx.ignore（前缀名随便起，建议是 git.ignore）。这个文件的存放位置原则上哪里都可以，为了便于让 ~/.gitconfig 文件引用，建议也放在用户目录下（C:\Users\当前用户名）。
git.ignore 文件模板内容网上有。
（2）在 .gitconfig 文件中引用忽略配置文件。
![图示](https://img-blog.csdnimg.cn/20210711111857179.png)
### 6.2 定位 Git 程序
![图示](https://img-blog.csdnimg.cn/20210711112029682.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
### 6.3 初始化本地库
* 选择要创建本地库的工程。
![图示](https://img-blog.csdnimg.cn/2021071114351747.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQ5NzUwMw==,size_16,color_FFFFFF,t_70)
* 切换版本：左下角 Git ---> Log
* 分支(Branch)相关：右击文件，选择 Git，里面有分支相关的操作，或者点击右下角的按钮
![图示](https://img-blog.csdnimg.cn/20210711153626888.png) 
* 其余的操作可以 右击文件，选择 Git。
## 七、IDEA 集成 GitHub
### 7.1 设置 GitHub 账号
* IDEA 在`File---> Settings ---> Version Control ---> GitHub`中关联账户。
* 用 Idea 登陆 GitHub 账号的时候，如果直接输入账号密码会因为网络原因而很难登陆上去，因此我们需要使用口令（token）的方式登陆上去。
（1）点击小加号，在 `Log In with Token`里面就有 Generate new token，直接点击即可。
（2）口令（token）在 `GitHub网页上---> 点击头像下的Settings ---> Developer settings ---> Personal access tokens -> Generate new token(生成的时候把所有按钮都点上)`
### 7.2 分享项目到 GitHub
`VCS位置的Git ---> Share Project on GitHub`按钮
### 7.3 push 推送本地库到远程库
`Git ---> Push`按钮
### 7.4 pull 拉取远程库到本地库
`Git ---> Pull`按钮
### 7.5 push 和 pull 使用注意事项
>push 是将本地库代码推送到远程库，如果本地库代码和远程库代码版本不一致，push 的操作是会被拒绝的。也就是说，要想 push 成功，一定要保证本地库的版本要比远程库的版本高！**因此一个成熟的程序员在动手改本地代码之前，一定会先检查远程库和本地库代码的区别。如果本地的代码版本已经落后，切记要先 pull 拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！**

>pull 是拉取远程仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。
#### 7.6 clone 远程库到本地
`Git ---> clone`按钮
## 八、国内代码托管中心-码云 Gitee
### 8.1 码云 Gitee 简介
因为 GitHub 服务器在国外，所以使用 GitHub 作为代码托管网站，如果网速不好的话会严重影响使用体验，甚至会出现登陆不上的情况。针对这个情况，大家也可以使用国内的代码托管网站-码云（Gitee） 。

码云是开源中国推出的基于 Git 的代码托管服务中心，使用方式和 GitHub 一样，而且它还是一个中文网站。
### 8.2 Idea 集成 码云
Idea 默认不带码云插件，因此第一步先要在`File ---> Settings ---> Plugins`里安装 Gitee 插件，然后你安装完成之后你可以在`File ---> Settings ---> Version Control`里面找到 Gitee 了，然后就可以登陆你自己的 Gitee 账号。
### 8.3 码云复制 GitHub 项目
码云提供了直接复制 GitHub 项目的功能，方便我们做项目的迁移和下载。
具体操作：`点击右上角加号 ---> 新建仓库 ---> 点击导入已有仓库`