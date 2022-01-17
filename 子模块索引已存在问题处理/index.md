# 子模块索引已存在问题处理

<!--more-->

问题： 'public' already exists in the index

场景：github repo 添加 submodule 子模块，使用命令 `git submodule add  git@github.com:test.git public` 报错

尝试解决：`rm -rf public` 删除目录在执行也不生效

最终解决办法：

`git rm -r --cached public`

`git submodule add  git@github.com:test.git public`

