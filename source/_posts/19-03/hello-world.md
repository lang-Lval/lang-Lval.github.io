---
title: Git的基本使用
categories: 
- web前端
---
![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day2/0.jpg)

“ 很早之前便注册了github，一直没有使用，正好这次搭建blogs可以熟悉下好久没用的git，当然，我对git的理解还是很粗浅的，主要还是通过Git教程 - 廖雪峰的官方网站来学习。”

git的安装就不做介绍了，都是一直下一步的操作，我们直接进入git的操作。

<!-- more -->
### 第一步

先创建一个本地的版本库（就是一个文件夹，随便在哪建一个）

![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day2/0.png)

   ### 第二步
   通过git init 将这个文件夹变成git可管理的厂库 进入文件夹内右击打开git bash命令行窗口


![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day2/01.png)

  ### 第三步

此时你会发现文件夹内多了一个.git的文件，这是用来跟踪和管理版本库的。它是默认隐藏的，如果你看不到，那就需要设置一下让隐藏文件可见
![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day2/02.png)

到了这一步，你就可以将自己的项目文件复制进来了然后通过git status 查看当前状态
![avatar](https://raw.githubusercontent.com/langhuonan/wechat/master/mdImages/day2/03.jpg)

   ### 第四步、

    看到文件状态后，再通过git add . 将改文件下的所有目录添加到厂库，然后用git commit -m "注释"把项目提交到仓库。-m后面的引号内是你本次提交的注释，这个可以随便写，或者空着

### 第五步
    将Git仓库与本地仓库进行关联（这个操作只用执行一次，关联后下次可直接进行提交）

git remote add origin https://github.com/langhuonan/langhuonan.github.io.git 

git remote add origin 后面跟的是你github的仓库地址


### 第六步 
关联好后我们就可以吧本地库的内容推送到远程仓库github上啦

    git push -u origin master (第一次推送由于远程库是空的，所以要加上-u这个参数，下次再传的时候只需要git push origin master)

    至此就完成了将本地项目上传到远程库的整个过程