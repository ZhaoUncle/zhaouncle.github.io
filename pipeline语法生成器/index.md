# Pipeline 片段语法生成器


<!--more-->

#方法一

内置的“片段生成器”工具有助于为各个步骤创建代码段，发现插件提供的新步骤，或者为特定的步骤尝试不同的参数。

片段生成器由 Jenkins 实例中可用的步骤动态添加。可用的步骤的数量依赖于安装的插件，这些插件显式地公开了流水线中使用的步骤。

要使用代码生成器生成一个步骤的片段：

从已配置好的流水线导航到 **流水线语法** 链接（见上），或访问 ``${YOUR_JENKINS_URL}/pipeline-syntax``。

#方法二：

创建一个 pipeline 的 job，然后拉到最下面，点击**Pipeline Syntax**

![image-20210415092616077](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415092616.png)

![image-20210415092627784](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210415092627.png)
