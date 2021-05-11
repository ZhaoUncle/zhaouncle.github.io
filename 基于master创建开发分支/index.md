# git 基于 master 创建删除 develop 分支


<!--more-->

# 应用场景：

开发过程中经常用到从master分支copy一个本地分支作为开发分支

# 步骤：

自己先 clone 代码下来，这里不教

1. 切换到被copy的分支（master），并且从远端拉取最新版本

   ```
   git checkout master
   git pull
   ```

2. 从当前分支拉copy开发分支

   ```
   git checkout -b develop
   ```

3. 把新建的分支push到远端

   ```
   git push origin develop
   ```

4. 关联

   ```
   git branch --set-upstream-to=origin/develop
   ```

5. 再次拉取验证

   ```
   git pull
   ```

   

# 以下如有需要在使用

1. 删除 github 或者 gitlab 上面的远端分支

   ```
   git push origin  --delete develop
   ```

2. 删除本地创建的分支，记得要先切换到其他分支在进行删除

   ```
   git checkout master
   git branch -d develop
   ```


