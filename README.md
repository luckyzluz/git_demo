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

一些常见的git命令如下

```shell
常见git命令
git init                        #把当前目录变成git可以管理的仓库（初始化）
git add readme.txt              #添加一个文件，也可以添加文件夹
git add -A                      #添加全部文件
git rm test.txt                 #删除一个文件，也可以删除文件夹
git commit -a -m "some commit"  #提交修改
git status                      #查看是否还有未提交
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

