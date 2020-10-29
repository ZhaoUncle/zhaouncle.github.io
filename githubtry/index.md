# Github 个人主页


<!--more-->

## 1. 个人主页

自定义个人主页之后你会发现除了项目的展示，还有个人的其他信息介绍。下面是小编设计的个人主页，初次尝试，请勿见笑！

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/github01.png" width="800" hegiht="250" align=center/>

## 2. 在你的 github 上创建一个新仓库

 这个新仓库的名称要和你登录账号名称保持一致。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/github03.png" width="800" hegiht="250" align=center/>

创建结果：

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/github04.png" width="800" hegiht="250" align=center/>

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/github05.png" width="800" hegiht="250" align=center/>

## 3. 将远程仓库拉到本地电脑上

用 git clone 一下仓库并且创建 README.md 文件，执行以下命令

`git clone git@github.com: 你用户名 / 你用户名. git`

## 4. 这个时候开始我们的自定义风格页面玩耍了

### 4.1 [在你的 README 中 获取动态生成的 GitHub 统计信息！](https://github.com/anuraghazra/github-readme-stats/blob/master/readme_cn.md)

上面的 README.md 我插入一个图片，可以实时展示你 github 仓库的一些数据信息，是通过一个链接实现的，如下图，记得设置链接的参数 username = 你的用户名

统计卡片示例：

[![Anurag's github stats](https://github-readme-stats.vercel.app/api?username=ZhaoUncle)](https://github.com/anuraghazra/github-readme-stats)



语言统计：

[![Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=ZhaoUncle)](https://github.com/anuraghazra/github-readme-stats)



## 5.完成上面几步之后，我们就可以开始推送到你的远程仓库

手动测试命令成功能够推送之后，直接一个 sh 脚本在当前实现就好啦，每次手动 sh update.sh 就能推送到 github 代码库了。

```shell
$ cat update.sh 
#!/bin/bash 
git add . 
git commit -m "update" 
git push -u origin master
```



上面的按照上面的步骤做完之后就可以查看属于你风格的个人主页了。

感兴趣的小伙伴们快去试试吧！
