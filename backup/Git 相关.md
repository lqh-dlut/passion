### 项目上传与更新
> 添加内容到git暂存区

```git add [./PATH]``` 

> 撤销add的全部（部分）内容

`git reset [PATH]`

> 查看git状态

`git status`

> 提交注释并上传

```
git commit -m "comment"
git push <远程仓库/默认origin> <本地分支/默认当前>
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

