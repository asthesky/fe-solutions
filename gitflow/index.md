# git工作流

## git config
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

## git init
```
git init  // 目录变成Git可以管理的仓库
```

## git add
```
git add . // 添加修改过和添加的文件
git add --all // 添加所有文件
git add -A // 添加修改过和删除过的文件
```

## git commit
```
git commit -m "comment" // 提交文件
git commit -am "add and commit" // 如果没有删除过文件，可以合并添加和提交文件为一步
```

## git status
```
git status // 仓库当前的状态
git diff ~// 查看文件修改
git diff SHA1 SHA2 // 两次版本戳之间文件的修改
```

## git log
```
git log SHA(标识) // 历史记录
git reflog // 用来记录你的每一次命令
```

## git reset
```
git reset --hard HEAD^ / --hard HEAD~n / --hard SHA // 版本回退
--hard // 回退版本，代码也回退，忽略所有修改
--soft // 回退版本，代码不变，回退所有的 add 操作
--mixed // 回退版本，代码不变，保留 add 操作
```

## git checkout
```
git checkout -- file // 文件在工作区的修改全部撤销
rm file // 删除文件
```

## git push
```
git push origin master // 远程提交
git pull origin master // 先拉去远程仓库
git push -u origin master -f // 强制提交 -u把本地的master分支和远程的master分支关联起来
```

## git merge 
```
git merge branchname //把分支"branchname"合并到了当前分支里面
```

## git rebase
```
git rebase origin //命令会把分支里的每个提交取消掉，并且临时保存为补丁(patch)分支更新到最新的"origin"分支，再保存的这些补丁应用到分支上
git rebase --continue //git会继续应用(apply)余下的补丁
git rebase --abort //终止rebase的行动， 分支会回到rebase开始前的状态
```

## git branch
```
git branch // 查看当前所有分支
git branch -va // 查看当前和远程分支
git branch dev // 创建dev分支
git checkout dev // 切换dev分支
git checkout -b dev // 创建并切换dev分支
git checkout master // 切换分支
git merge dev // 把dev合并到当前分支
git merge --no-ff // 用普通模式合并，合并后的历史有分支，能看出来曾经做过合并
git branch -d dev // 删除dev分支
git log --graph //查看分支合并图
```

## git stash
```
git stash // 工作现场“储藏”
git stash list // 工作现场表
git stash pop // 回到工作现场
git stash apply stash@{0} / git stash drop // 回到某一工作现场
```

## git remote
```
git remote add origin git@github.com:accounts/learngit.git // 添加github远程仓库
git remote add origin https://github.com/accounts/Micro-Share // 添加origin远程仓库
git remote add mine http://your/path/to/git // 添加mine远程仓库
git fetch origin master // 拉取远程代码到 init 之后的 master 主干上
git commit -am "fist" git push -u mine master // 修改代码之后，提交到自己的仓库

git remote -v // 远程库的信息
git push origin master / dev // 本地提交推送到远程库
git checkout -b dev origin/dev // 创建远程origin仓库上的dev分支到本地
git branch --set-upstream dev origin/dev // 本地到远程dev连接
```

## git tag
```
git tag name // 用于新建一个标签，默认为HEAD
git push origin name / --tags // 推送一个/全部未推送过的本地标签
git tag -d v0.1 // 删除本地标签
git push origin :refs/tags/v0.9 // 删除远程标签
``` 


### git cherry-pick
```
git cherry-pick 38361a68 //在当前分支选取某一个分支中的一个或几个commit复制到当前分支
```