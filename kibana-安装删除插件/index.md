# 【ELK】kibana 安装删除插件


<!--more-->

## 安装插件

使用以下命令安装插件：

```shell
bin/kibana-plugin install <package name or URL>
```

当您指定的插件名没有带 URL，插件工具将会尝试去下载 Elastic 官方插件。例如：

```shell
$ bin/kibana-plugin install x-pack
```

### 通过指定的 URL 地址安装插件

您可以简单的指定插件名称来下载 Elastic 官方插件。也可以指定插件具体的 URL 来下载安装，例如：

```shell
$ bin/kibana-plugin install https://artifacts.elastic.co/downloads/packs/x-pack/x-pack-6.0.0.zip
```

您可以在 URL 中指定多种协议，例如 HTTP 、 HTTPS 或者 `文件` 协议。

### 向指定的目录安装插件

在 `install` 命令后面通过 `-d` 或者 `--plugin-dir` 选项指定插件安装目录，例如：

```shell
$ bin/kibana-plugin install file:///some/local/path/x-pack.zip -d path/to/directory
```

如果目录不存在，这条命令会创建这个目录。

### 通过 Linux 安装包安装插件

Kibana 服务需要有 `optimize` 目录的写权限。如果您使用 sudo 或者 su 安装插件，您需要确保这些命令使用 `kibana` 用户执行。这个用户已经默认为您添加了，它用于包的安装。

```shell
$ sudo -u kibana bin/kibana-plugin install x-pack
```

如果插件使用了不同的用户安装且服务又没有运行起来，您就需要修改这些文件的所属用户：

```shell
$ chown -R kibana:kibana /path/to/kibana/optimize
```

# 删除插件

通过删除当前版本重装新的插件来升级插件。

通过 `remove` 命令来删除插件：

```shell
$ bin/kibana-plugin remove x-pack
```

您也可以通过手动删除 `plugins/` 目录下的插件子目录来手动删除插件。

删除插件之后将会在下一次 Kibana 启动的时候触发一次 “优化（optimize）” 动作，可能会使启动有点延迟。



##  关闭插件

使用如下命令来关闭插件：

```shell
./bin/kibana --<plugin ID>.enabled=false 
```



关闭或打开插件将会在下一次 Kibana 启动的时候触发一次 “优化（optimize）” 动作，可能会使启动有点延迟。

> 您可以在 `package.json` 文件中通过 `name` 属性查看插件的 ID
