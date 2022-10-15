# git项目教程

## 一、账号配置

Git同时配置Gitee和GitHub

### 1.  配置（清除）git全局设置

设置全局属性的命令：

```shell
git config --global user.name "账户名字"                      
git config --global user.email "账户邮箱"

执行后会在windows生成C:\Users\lenovo\.gitconfig文件
注：--global 表示全局属性，所有的git项目都会共用属性。
设置本地机器默认commit的昵称与Email. 请使用有意义的名字与email.
```

可以通过`git config --global --list`来查看是否设置过。

```shell
git config --global --unset user.name "你的名字"
git config --global --unset user.email "你的邮箱"
```

### 2. 生成SSH  keys

#### 2.1 GitHub 的钥匙

```shell
ssh-keygen -t rsa -f ~/.ssh/id_rsa.github -C "邮箱"
```

#### 2.2 Gitee 的钥匙

这里的邮箱我用的和上面github的邮箱是一样的

```shell
ssh-keygen -t rsa -f ~/.ssh/id_rsa.gitee -C "邮箱"
```

完成后会在~/.ssh / 目录下生成以下文件。

- id_rsa.github
- id_rsa.github.pub
- id_rsa.gitee
- id_rsa.gitee.pub

### 3 识别 SSH keys 新的私钥

默认只读取 id_rsa，为了让 SSH 识别新的私钥，需要将新的私钥加入到 SSH agent 中。**这一步也需要再探索一下，我没有设置也可以成功。**

```shell
ssh-agent bash
ssh-add ~/.ssh/id_rsa.github
ssh-add ~/.ssh/id_rsa.gitee
```

### 4 多账号配置 config 文件

创建config文件**（解决 ssh冲突）**：

```shell
#Default gitHub user Self
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa.github

# gitee
Host gitee.com
    Port 22
    HostName gitee.com
    User git
    IdentityFile ~/.ssh/id_rsa.gitee
```

### 5. 添加 ssh

分别添加SSH到Gitee和Github：

Github：
https://github.com/settings/keys
将 id_rsa.github.pub 中的内容填进去，起名的话随意。

Gitee:
https://gitee.com/profile/sshkeys
将 id_rsa.gitee.pub 中的内容填进去，起名的话随意。

### 6 账号测试

```shell
ssh -T git@gitee.com
ssh -T git@github.com
```

![git账号测试成功截图](https://s1.ax1x.com/2022/09/14/vvvYtJ.png)

------



## 二、常见的git命令

```shell
git init                        #初始化git仓库（把当前目录变成git可以管理的仓库）
git add readme.txt              #添加xxx文件（夹）到仓库
git add .                       #把所有更改的文件添加到仓库
git rm test.txt                 #删除一个文件，也可以删除文件夹
git commit -a -m "some commit"  #提交文件修改
git status                      #查看状态(非必须)，查看是否还有未提交
git log                         #查看最近日志
git reset --hard HEAD^          #版本回退一个版本
git reset --hard HEAD^^         #版本回退两个版本
git reset --hard HEAD~100       #版本回退多个版本
git remote add origin +地址     #远程仓库的提交（第一次链接）
git push -u origin master       #仓库关联
git push                        #远程仓库的提交（第二次及之后）
git checkout -b xxxx            #新建 xxx 分支
git push --set-upstream  origin  xxx   #第一次往 xxx 分支推送代码
```





## 三、git 经典案例

### 1.  项目bug修复

​       一般来说，我们开发完了一个app（项目）,上线之后那就是迭代功能开发了,如果上线的app出现了一个严重的bug,要你放下手头新功能的开发去解决这个bug,然后在发布一个新版本,如果你要是就在你要迭代功能的项目上进行修改发布的话,那肯定是不行的,且先不谈有没有新的bug出现,时间是也是不允许的,发布的前提还要把新功能完善好才行,要是删掉新功能的代码也不怎么现实,要是业务逻辑少一点还好说,要是多的话那还真是有点无从下手了,所以git的分支就很好的解决了这个问题; 如下图:
master就是我们的主代码,一直优化,到v1.4版本发布了,然后接着往下开发v2.0,v2.1版本,但是v1.4版本出现了一个严重的bug,这时候我们就在这个v1.4版本创建一个分支developer来对bug进行修复,到了v1.6版本bug修复好发布出去,然后在跟原来的master主代码进行合并一下把代码添加到v2.1版本就OK了,剩下就接着迭代开发了;

Ps.假设某一版本出现bug，需要不影响既有开发进度，进行修复



![3333](https://cdn.jsdelivr.net/gh/luckyzluz/luckyzluz.github.io@zblog/img/home_top_bg.webp)









## 附录

### git add .与-A的区别（不同版本下的区别）

1.0的版本中add .包含了修改的文件和新文件但是不会监听删除的文件，而-A则是可以提交所有的文件包含了删除的，但是最近在2.0的版本中使用的过程中发现了git add .也可以提交删除的文件。

Git Version 1.x:

|    命令    | 新文件 | 修改的文件 | 删除的文件 |                  描述                  |
| :--------: | :----: | :--------: | :--------: | :------------------------------------: |
| git add -A |   √    |     √      |     √      | 暂存所有（新的，修改的，已删除的）文件 |
| git add .  |   √    |     √      |     ×      |       仅暂存新文件和修改过的文件       |
| git add -u |   ×    |     √      |     √      |            仅修改和删除文件            |


Git Version 2.x:

|            命令            | 新文件 | 修改的文件 | 删除的文件 |                  描述                  |
| :------------------------: | :----: | :--------: | :--------: | :------------------------------------: |
|         git add -A         |   √    |     √      |     √      | 暂存所有（新的，修改的，已删除的）文件 |
|         git add .          |   √    |     √      |     √      | 暂存所有（新的，修改的，已删除的）文件 |
|         git add -u         |   ×    |     √      |     √      |            仅修改和删除文件            |
| git add --ignore-removal . |   √    |     √      |     ×      |       仅暂存新文件和修改过的文件       |



图片测试

![](https://cdn.jsdelivr.net/gh/luckyzluz/zblogBasics/images/202201112213232.svg+xml;charset=utf-8)

![333](https://webcall.oss-cn-qingdao.aliyuncs.com/41052/2022/9/16/e83bddeb6ab14940b0fef49c0e9a4af7.png)