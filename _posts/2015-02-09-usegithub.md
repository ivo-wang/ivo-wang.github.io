---
layout: post
title: git使用小记
tags:
    - git
---
	
- **1.配置github**

参考github官方文档[https://help.github.com/articles/set-up-git/ ](https://help.github.com/articles/set-up-git/) 配置了HTTPS和SSH连接。

- **2.在github中创建版本库**

可参见github官方文档[https://help.github.com/articles/create-a-repo](https://help.github.com/articles/create-a-repo)

- **3.初始化本地版本库**

进入项目根目录，输入：

    git init

- **4. 将项目文件添加到版本库中**

还是在项目根目录中输入：

    git add .

- **5. 添加项目改动说明**

还是在项目根目录中输入：

    git commit -m “第一次提交，创建项目。”

- **6.将本地仓库与github项目仓库xxx相关联**

在本地项目仓库的根目录中，输入：

    git remote add origin git@github.com:yourname/xxx.git

- **7.将本地库的内容同步到github上**

    git push -u origin master

注意:由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

如果发生了这样的错误： 无法推送一些引用到 'git@github.com:yourname/xxx.git'
提示：更新被拒绝，因为远程版本库包含您本地尚不存在的提交。这通常是因为另外
提示：一个版本库已向该引用进行了推送。再次推送前，您可能需要先整合远程变更
提示：（如 'git pull ...'）。

则使用强行更新 +master:

    git push -u origin +master

- **8.将本地库与github项目仓库相关联的另外一种办法**

在本地仓库的根目录中，输入：

    git clone https://github.com/yourname/xxx.git

进入xxx目录中，将所有文件复制到上一层目录中：

    cd xxx

    cp -r * ../

回到本地仓库根目录，删除xxx目录的所有文件

    rm -rf xxx/

这样也能将本地仓库和远程仓库相关联，接下来可以添加文件到本地仓库，再提交内容到github。
8 git的回滚操作
 **8.1 先使用git log查看日志，找到想要回滚的版本**

在项目目录中，输入

    git log

结果如下图所示：
enter image description here
  **8.2 再使用git reset回滚到指定版本**

    git reset –hard 4bb7bbc07f4b3792b48a6001bdfcc2b694cd3c81
