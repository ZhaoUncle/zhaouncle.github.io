# 保持自己github的forks自动和上游仓库同步并推送到 gitee


<!--more-->

# 一、自动拉取 fork 源上游

## 方法一：apps pull

📢：只要你不改fork 库的内容，那么是纯净同步模式，时间点都和上游仓库一致。同步更新时间 3 个小时以上。

### 安装 github app

访问地址：https://github.com/apps/pull

开源地址：https://github.com/wei/pull#readme

![image-20220123120123689](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231201782.png)

![image-20220123120230081](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231202125.png)

![image-20220123120259166](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231202207.png)

### 如何打开进入安装好的 APP

![image-20220123120525050](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231205095.png)

![iShot2022-01-23 12.06.01](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231208795.png)

上游仓库改变，过了几个小时后，自动同步成功

![image-20220123201914666](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201232019716.png)

## 方法二：github Actions

| 列表                                                         | github 商店                                          |      |
| ------------------------------------------------------------ | ---------------------------------------------------- | ---- |
| [Fork Sync](https://github.com/tgymnich/fork-sync)           | https://github.com/marketplace/actions/fork-sync     |      |
| [Github Action: Upstream Sync](https://github.com/aormsby/Fork-Sync-With-Upstream-action) | https://github.com/marketplace/actions/upstream-sync |      |

### Fork Sync

自用配置

```
# .github/workflows/sync.yml
name: Sync Fork

on:
  push: # push 时触发, 主要是为了测试配置有没有问题
  schedule:
    - cron: '* */3 * * *' # 每3小时触发, 对于一些更新不那么频繁的项目可以设置为每天一次, 低碳一点
jobs:
  repo-sync:
    runs-on: ubuntu-latest
    steps:
      - uses: TG908/fork-sync@v1.6.3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          owner: ZhaoUncle # fork 的上游仓库 user
          head: main # fork 的上游仓库 branch
          base: main # 本地仓库 branch

```

![iShot2022-01-24 22.40.12](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201242244658.png)

# 二、手动拉取 fork 源上游

## 情况一：代码不冲突

### 方法一：Fetch upstream

📢：同步是干净的，完全同步

![image-20220123124833425](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231248532.png)

![image-20220123124852658](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231248726.png)

### 方法二：Pull requests

📢：注意，一定是从源仓库，也叫做上游，——>> merge 到，本地仓库，箭头方向一定要对。

![image-20220123125053571](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231250643.png)

![image-20220123125348122](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231253167.png)

![image-20220123125457153](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231254198.png)

![image-20220123125515344](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231255390.png)

![image-20220123125531576](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231255621.png)

![image-20220123125548247](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231255292.png)

####遇到的问题

##### 如果你显示的不全，需要点击“compare across forks”

![image-20220123125820763](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231258806.png)

##### 该方法会导致 commit 记录比 fork 的源多，如果你要保持该仓库的干净整洁，这种 merge pull requests 的情况就不合适了。

![image-20220123130139463](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201231301502.png)



### 方法三：命令行同步复制

参考官方文档：https://docs.github.com/cn/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork

指定fork 的上游仓库

```
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```

验证

```
$ git remote -v
```

验证，显示结果

```
> upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
> upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

进入本地仓库

从上游获取提交数据

```
git fetch upstream
```

检出 fork 的本地默认分支，这里用 main 测试

```
git checkout main
```

将上游默认分支，本例中为 upstream/main 的更改合并到本地默认分支，记得要按 q 退出编辑模式，不会就直接关闭当前命令行在进入

```
git merge upstream/main
```

推送到 github

```
git push origin main
```



# 三、自动推送到 gitee

## github Actions 方法



| 推荐列表                                                     | github 商店                                                  |      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ---- |
| [Hub Mirror Action](https://github.com/Yikun/hub-mirror-action) | https://github.com/marketplace/actions/hub-mirror-action     |      |
| [repository-mirroring-action](https://github.com/marketplace/actions/mirroring-repository) | https://github.com/pixta-dev/repository-mirroring-action     |      |
| [Git Mirror Action](https://github.com/wearerequired/git-mirror-action) | https://github.com/marketplace/actions/mirror-a-repository-using-ssh |      |
| [mirror-action](https://github.com/yesolutions/mirror-action) | https://github.com/marketplace/actions/mirror-repository     |      |
| [Mirror to GitLab and trigger GitLab CI](https://github.com/SvanBoxel/gitlab-mirror-and-ci-action) | https://github.com/marketplace/actions/mirror-to-gitlab-and-run-gitlab-ci#mirror-to-gitlab-and-trigger-gitlab-ci |      |
|                                                              |                                                              |      |



## Hub Mirror Action

### 参考列表：

- [Hub Mirror Action](https://github.com/Yikun/hub-mirror-action) ：github 源码库
- [Sync GitHub to other hub](https://github.com/yi-Xu-0100/hub-mirror) ：一个用于展示如何使用这个action的模板仓库
- [自动镜像 GitHub 仓库到 Gitee](https://github.com/ShixiangWang/sync2gitee)：一个关于如何使用这个action的介绍
- [巧用Github Action同步代码到Gitee](http://yikun.github.io/2020/01/17/巧用Github-Action同步代码到Gitee/): Github Action第一篇软文

### 过程

1. 一开始在 github 商店找 actions 方法，还是打算直接通过集成实现，因为 gitee 免费版不具备相关自动化的功能。

https://github.com/marketplace?type=actions&query=gitee+

然后发现，stars 数最高的一个项目，参考下，可以支持多 repo 仓库，那么可以单独拆开，也可以设置一个统一的推送仓库来配置，非常不错

![image-20220123234100222](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201232341293.png)

2. 代码核心流程

   ![image-20220124133035101](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201241330192.png)

### 个人使用模板

要注意的地方： on.push.branches 或者 on.pull_request.branches 不管是哪个仓库，由于 github 上面旧的仓库还是 master 而不是 main，所以你要改成对应的

```
name: Gitee repos mirror periodic job

on:
  push: # 有 push 代码的时候会执行
    branches: [ main ]
  pull_request: # 有 pull_request 代码的时候会执行
    branches: [ main ]
  schedule: 
    # 每3个小时跑一次
    - cron:  '0 */3 * * *'

jobs:
  build:

    runs-on: ubuntu-latest

    steps:

    - name: Mirror the Github organization repos to Gitee.
      uses: Yikun/hub-mirror-action@v1.0
      with:
        src: github/ZhaoUncle	# 必选，需要同步的Github用户（源）
        dst: gitee/qingfeng689 # 必选，需要同步到的Gitee的用户（目的）
        dst_key: ${{ secrets.GITEE_PRIVATE_KEY }} # 必选，Gitee公钥对应的私钥，https://gitee.com/profile/sshkeys
        dst_token:  ${{ secrets.GITEE_TOKEN }} # 必选，Gitee对应的用于创建仓库的token，https://gitee.com/profile/personal_access_tokens
        account_type: user # 如果是组织，指定组织即可，默认为用户user
        timeout: 600 # git 命令超时时间
        debug: true # 测试用，正常可以关掉
        force_update: true # 强制推送到 dst 仓库
        white_list: "test2,community.applications" # 白名单机制，可以用于更新某些指定库

```

### 配置参数：

我直接借用大佬们的截图，我这里就不做展示了，请见谅

#### 参考一：

https://github.com/marketplace/actions/hub-mirror-action

![image-20220124131638919](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201241316027.png)

#### 参考二：

https://github.com/yi-Xu-0100/hub-mirror

用到的地址：

- github ssh key 配置地址：https://github.com/settings/keys
- gitee ssh key 配置地址：https://gitee.com/profile/sshkeys
- github toekn 配置地址：https://github.com/settings/tokens
- gitee token 配置地址：https://gitee.com/profile/personal_access_tokens

![image-20220124131741921](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201241317019.png)

### 想简单使用，可以直接使用以下方式

fork 该仓库到你本地，然后修改 相关参数就好了，截图如下

![image-20220124133456241](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/202201241334323.png)





# 四、推荐阅读

[Github进行fork后如何与原仓库同步：重新fork很省事，但不如反复练习版本合并](https://github.com/selfteaching/the-craft-of-selfteaching/issues/67)

