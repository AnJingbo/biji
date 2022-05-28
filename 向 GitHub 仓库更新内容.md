

### 1. 首先在gitHub上创建一个仓库：
（1）登录到个人账号上，点击 New repository；
![图示](https://img-blog.csdnimg.cn/271c988f1d7d4b9299ef3951c26977bc.jpg)
（2）填写仓库名字、描述等，一般选择 Public，填写完毕点击 create repository，就完成了仓库的创建；
![tushi](https://img-blog.csdnimg.cn/2bd81059fc7a4cb6bf45a0af55758151.jpg?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
（3）完成后就可以在个人主页看到刚创建好的仓库了。

### 2. 通过 git 命令向远程仓库推送更新：
（1）``git clone https://github.com/...``，这个路径可以点击进入仓库，在仓库的这一部分复制即可：
![图示](https://img-blog.csdnimg.cn/67af17655b8e42d3a4125608050b3f54.jpg?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
（2）命令行窗口出现下面的通知就说明克隆成功，同时你的桌面上会多一个名字为远程仓库名字的文件夹：

![图示](https://img-blog.csdnimg.cn/20dacd8025964b7eb779ba204885e3ab.jpg?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
（3）向桌面文件夹（即新增的名字为远程仓库名字的文件夹）中添加要更新的文件，在命令行中将工作目录切换到克隆到本地的仓库下：``cd ...``（或者直接在桌面文件夹内右击打开 Git Bash Here）
![tishi](https://img-blog.csdnimg.cn/9aa4928eb80f4a6f8248e9a3380c2f6b.png)
（4）使用 ``git init`` 进行初始化：
![图示](https://img-blog.csdnimg.cn/51c342e86ce3492c990743bb7b418f57.png)
（5）使用 ``git add .`` 放入缓存区：
![图示](https://img-blog.csdnimg.cn/f34a57217a9f4b15bfa05ca1cb376628.jpg)
（6）加入完毕后，使用 ``git commit -m "日志信息" `` 提交到本地库：
![图示](https://img-blog.csdnimg.cn/f687b5f925d644c3809eac3976df8264.jpg?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)
（7）最后输入 ``git push`` 完成推送，刷新网页，仓库目录下会多出你想要上传的文件：
![tushi](https://img-blog.csdnimg.cn/7cc3284dca564d0a83065376cc8a6331.jpg?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5bSU5rOi5rOi5ZWK,size_20,color_FFFFFF,t_70,g_se,x_16)