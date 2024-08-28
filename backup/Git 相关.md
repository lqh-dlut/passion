### 项目上传与更新
> 添加内容到git暂存区

```git add [./PATH]``` 

> 撤销add的全部（部分）内容

`git reset [PATH]`

> 查看git状态

`git status`

> 从暂存区提交到本地仓库

```
git commit -m "comment"
```

> _如果只是对原来文件进行修改而没有新文件可以使用_

```
git commit -a -m "comment"
git commit -am "comment"
```

> 撤销commit的内容

```
git reset --soft HEAD~n  // 撤销n次commit
```

> 提交到远程仓库

`git push <远程仓库/默认origin> <本地分支/默认当前>`


> 查看git提交记录

```
git log [branch]   // 显示分支的commit记录，其他分支会包括master中的
git log --oneline // 只显示一行内容
git log --all --graph --oneline --decorate  // 图形化显示分支
```
### 项目与仓库的连接和重置

> 取消repos_name仓库连接

`git remote remove repos_name`

> 清除git历史记录并初始化

```
rm -rf .git
git init
```
> 添加新的仓库，查看已连接的仓库

```
git remote add name git@github.com:...
git remote -v
```

_网络加速器，vpn等开启时可能会莫名出现报错如下：_
```
ssh: connect to host github.com port 22: Connection refused
fatal: Could not read from remote repository.
```

### Git的一些概念
![image](https://github.com/user-attachments/assets/5962bdc3-6a99-4577-a331-82fde9ce0ff5)

#### Branch 分支
```
git branch // 查看分支列表
git branch branch-name // 创建branch
git switch branch-name  // 切换分支
git merge branch-name  // 合并分支（到当前所在分支）
git branch -d branch-name // 删除已经合并的分支
git branch -D branch-name // 强制删除未合并分支
```

> 合并分支后还需要**手动**删除分支

### Rebase 

> rebase后跟的branch2是不变的，branch1跟在后面

```
git switch branch1
git rebase branch2
```

### Cherry-pick

> 将其他branch中的其中一次commit单独复刻到自己的branch中

`git cherry-pick git-id`

### .gitignore


> 在gitignore文件中简化别名，git lg => git log --all --graph --oneline --decorate

```
[alias]
    lg = log --all --graph --oneline --decorate
```




