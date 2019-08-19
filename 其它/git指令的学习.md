## git指令的操作

> 分支 操作
>
> ```shell
> # 查看分支
> git branch 
> # 切换分支
> git checkout 分支名
> # 创建并切换分支
> git checkout -b 分支名
> # 删除分支
> git branch -d 分支名
> # 删除远程分支
> git push origin -d 远程分支名
> ```
>
> 查看版本
>
> ```shell
> # 看版本
> git log   
> # 记录每一次命令
> git reflog  
> ```
>
> 回滚版本
>
> ```shell
> # 回滚到指定的版本
> git reset --hard e377f60e28c8b84158 (后边是版本号)
> # 撤销特定commit
> git revert : 撤销特定commit
> ```
>
> git报错```error: src refspec dev does not match any.``` 无法push到远程分支
>
> > 解决方式
> >
> > 1. git checkout -b name    #name为分支名称 再进行 git push name（name为远端分支的名称）
> > 2. git push 远端git名字 本地分支名:远端分支名
> >
> > 强制覆盖远程的东西
> >
> > git push 远端git名字 本地分支名:远端分支名 --force
>
> git合并项目的一般流程
>
> ```shell
> # master分支
> git pull
> # 切换到自己分支
> git checkout 分支
> # 合并
> git merge master
> ```
>
> 自己写代码后提交的一般流程
>
> ```shell
> # 查看有什么更改
> git status
> # 添加到本地分支
> git add .
> # 添加提交信息
> git commit -m 'test'
> # 提交到远程
> git push origin + 远程分支名
> ```
>
> 

