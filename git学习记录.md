# 如何使用git管理项目—学习笔记(2025/11/16)

## git核心概念

1. **Git**: 一个**版本控制系统**，可以理解为“代码的时光机”。它负责记录每次代码的变更（谁、什么时候、改了什么）。Git 是在你本地电脑上运行的。
2. **GitHub**: 一个基于 Git 的**在线代码托管平台**。你可以把本地的 Git 仓库（Repository）“推送”到 GitHub 上，这样代码就有了一个云端备份，其他人也可以看到和参与你的项目。
3. **仓库（Repository）**: **一个项目对应的文件夹**，里面包含了项目的所有文件、修改历史、提交记录等。
4. **提交（Commit）**: 代表一次代码的变更记录，类似于游戏中的“存档点”。每次提交都需要写一条简短的说明，描述这次修改的内容。
5. **分支（Branch）**: 项目开发的一条独立线。默认的主分支通常叫 `main`或 `master`。你可以创建新的分支来开发新功能或修复 Bug，而不会影响主分支的稳定。开发完成后，再通过“合并”将分支的改动整合回主分支。



## git工作流程

1. **安装 Git**: 从官网下载并安装。

2. **配置用户信息**: 在命令行中设置你的用户名和邮箱，这样你的提交才会带有身份信息。

   ```
   git config --global user.name "你的用户名"
   git config --global user.email "你的邮箱"
   ```

3. **创建 SSH 密钥**（推荐）：这样每次和 GitHub 通信时就不需要输入密码了。教程在 GitHub 官方文档里有。这里列出简单的流程：

   ```
   ssh-keygen -t rsa -C "xxxxxxx@xxx.com"
   ```

   然后一直回车即可，成功会生成.ssh文件夹，进入打开id_rsa.pub， 复制里面所有的信息。打开github，打开Settings，在Access里面点击SSH and GPG keys，点击New SSH keys，然后id_rsa.pub所有的信息全部复制Key里面，Title自己取名。之后的设置git代理：

   ```
   设置代理
   git config --global http.proxy 'http://127.0.0.1:端口号'
   git config --global https.proxy 'https://127.0.0.1:端口号'
   
   
   ssh -T git@github.com
   出现下面的提示代表链接成功
   Hi xxx! You've successfully authenticated, but GitHub does not provide shell access.
   ```

   这里的端口号查询：本地电脑-打开设置-打开网络和Internet-点击代理-手动设置代理-打开-复制端口号即可

## **单个项目的流程**

1. **创建仓库**:

   ​	•**线上仓库创建**: 在 GitHub 网站上点击 “New Repository”。

   ​	•**本地仓库创建**: 进入想推送的仓库的文件夹中，或者创建一个新的文件夹。

2. **添加文件到暂存区**: 初始化版本库，添加指定文件到版本库中（**暂存区**）或添加所有文件到版本库中。具体命令如下：

   ```
   1. git init                    (初始化版本库)   
   2. git add <file_name>         (如果要上传单个文件，示例: git add test.py， 推送所有文件，示例: git add . )
   3. git add <dir_name>          (如果要上传一个文件夹)  
   ```

3. **提交变更**: 使用 `git commit`命令，创建一个“存档点”。

   ```
   git commit -m "这里写一句清晰的提交说明，比如：修复了登录页面的按钮bug"`
   ```

4. **推送到远程仓库（GitHub）**: 首先需要将本地库和github上的远程库关联，之后将本地的“存档点”同步到 GitHub 上。如果是第一次推送，还需要将**本地分支**与**远程同名分支相关联**，具体命令如下：

   ```
   git remote add origin https://github.com/xxx/xxx.git   (关联本地库和远程库)
   git push -u origin main                                (首次推送需要)
   git push                                               (后续不切换库可以直接push,git会自动推向origin/main)
   ```

 5. 从远程仓库拉取代码：可以使用pull命令：

    ```
    git fetch <remote_repo> <branch>                       (从远程仓库拉取最新代码到本地，但不合并代码)
    git merge <branch>                                     (拉取并合并代码)
    git pull --rebase                                      (多人开发会用到，使得本地代码永远处于最新状态)
    git pull <remote_repo> <branch>                        (从远程仓库拉取代码，默认为origin，默认为当前分支)       
    ```

    

 **其他命令：**

1. **远程仓库相关操作：**

``` 
git checkout -b <new_branch_name>                    (创建新分支，并切换到该分支)
git checkout <branch_name>                           (切换branch分支,示例: git checkout master/main)
git remote -v                                        (查看关联的远程仓库)
git remote show                                      (查看关联的指定远程仓库详细信息)
git remote rename <old_name> <new_name>              (将已配置的远程仓库重命名)  
git remote remove <remote_name>                      (删除指定的远程仓库)
```

2. **branch增删查改相关操作：**

```
git push <remote_name> <local_branch_name>:<remote_branch_name>         (若远程分支与本地分支名可省略前者)
git push <remote_name> <local_branch_name>
git branch -a                                                           (查看所有远程和本地分支)
git branch                                                              (查看所有分支)
git branch -r                                                           (查看所有远程分支)
git push origin -delete <branch_name>                                   (删除主机和远程的branch)
git branch -d <branch_name>                                             (删除本地分支)
```



参考链接：

1. [MajestyV/GitTutorial: Guidance for rookies to realize project management through GitHub](https://github.com/MajestyV/GitTutorial/tree/main?tab=readme-ov-file)

2. [Git 教程 | 菜鸟教程 (runoob.com)](https://www.runoob.com/git/git-tutorial.html)

3. [GitHub 入门文档 - GitHub 文档](https://docs.github.com/zh/get-started)