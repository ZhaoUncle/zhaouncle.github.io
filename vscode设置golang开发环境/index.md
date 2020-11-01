# 【Golang】vscode 设置 go 开发环境


<!--more-->

# 步骤

## 1. 始

Golang 语言开发选择一款合适的编辑器，能加速你敲字的灵感，这里推荐微软的 Visual Studio Code，简称 vscode。

## 2. 安装 go 插件

首先需要安装 go 语言插件，在 vscode 扩展中搜索 “go”，如下图，下载安装go插件

![image-20201101170130488](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101170130488.png)

![image-20201101170447244](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101170447244.png)



## 3. 设置 GOPROXY

如果想要 vscode 在 Go语言开发的时候为我们提供诸如代码提示、代码自动补全等功能，需要安装 go tools，但是安装 tools 需要设置 goproxy，否则会因为网络问题无法下载 tools 工具。

在此之前请先设置`GOPROXY`，打开终端执行以下命令：

```bash
go env -w GOPROXY=https://goproxy.cn,direct
```

## 4. 安装 Go语言开发工具包

安装 Golang Tools，按下 Ctrl/Cmd+P，输入`> Go: Install/Update Tools`，然后回车，选择你要安装的 tools 插件

![image-20201101172850059](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101172850059.png)

![image-20201101173030140](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101173030140.png)

## 5. 插件介绍

| tools 名称 |  | 用途 |
| :----------- | ---- | ------------ |
|gocode    		|  github.com/mdempsky/gocode | 代码自动完成 |
| gopkgs       |  github.com/uudashr/gopkgs/v2/cmd/gopkgs    | 该工具为未导入的软件包提供自动补全功能 |
| go-outline   |  github.com/ramya-rao-a/go-outline    | 此工具提供了[文档大纲](https://github.com/golang/vscode-go/blob/master/docs/features.md#document-outline)功能以及当前文件功能中的[转到符号](https://github.com/golang/vscode-go/blob/master/docs/features.md#go-to-symbol)。 |
| go-symbols   |  github.com/acroca/go-symbols    | 此工具提供了工作空间中的[转到符号](https://github.com/golang/vscode-go/blob/master/docs/features.md#go-to-symbol)功能。 |
| guru         |  golang.org/x/tools/cmd/guru    | 该工具提供[查找参考](https://github.com/golang/vscode-go/blob/master/docs/features.md#find-references)和[查找接口实现](https://github.com/golang/vscode-go/blob/master/docs/features.md#find-interface-implementations)功能。<br />它也可用于通过设置提供[转到定义](https://github.com/golang/vscode-go/blob/master/docs/features.md#go-to-definition)[`"go.docsTool"`](https://github.com/golang/vscode-go/blob/master/docs/settings.md#go.docsTool)。 |
| gorename     |  golang.org/x/tools/cmd/gorename    | 此工具提供了[重命名符号](https://github.com/golang/vscode-go/blob/master/docs/features.md#rename-symbol)功能。 |
| gotests      |  github.com/cweill/gotests/...     | 该工具为[`Go: Generate Unit Tests`](https://github.com/golang/vscode-go/blob/master/docs/features.md#generate-unit-tests)命令集提供支持。 |
| gomodifytags |  github.com/fatih/gomodifytags     | 该工具支持[`Go: Add Tags to Struct Fields`](https://github.com/golang/vscode-go/blob/master/docs/features.md#add-or-remove-struct-tags)和[`Go: Remove Tags From Struct Fields`](https://github.com/golang/vscode-go/blob/master/docs/features.md#add-or-remove-struct-tags)命令。 |
| impl         |  github.com/josharian/impl     | 该工具为[`Go: Generate Interface Stubs`](https://github.com/golang/vscode-go/blob/master/docs/features.md#generate-interface-implementation)命令提供支持。 |
| fillstruct   |  github.com/davidrjenni/reftools/cmd/fillstruct     | 该工具提供了对[`Go: Fill struct`](https://github.com/golang/vscode-go/blob/master/docs/features.md#fill-struct-literals)命令的支持。 |
| goplay       |  github.com/haya14busa/goplay/cmd/goplay    | 该工具为[`Go: Run on Go Playground`](https://github.com/golang/vscode-go/blob/master/docs/features.md#go-playground)命令提供支持。 |
| godoctor     |  github.com/godoctor/godoctor    | 该工具提供了[重构](https://github.com/golang/vscode-go/blob/master/docs/features.md#refactor)功能。<br />它不支持Go模块，因此我们希望[`gopls`](https://github.com/golang/vscode-go/blob/master/docs/gopls.md)它将提供此功能（[golang / go＃37170](https://github.com/golang/go/issues/37170)）。 |
| dlv          |  github.com/go-delve/delve/cmd/dlv    | 这是Go语言的调试器。它用于提供此扩展的[调试](https://github.com/golang/vscode-go/blob/master/docs/debugging.md)功能。 |
| gocode-gomod |  github.com/stamblerre/gocode    |  |
| goreturns    |  github.com/sqs/goreturns    |  |
| golint       |  golang.org/x/lint/golint     |  |

## 6. 打开vscode设置

![image-20201101174435968](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101174435968.png)

![image-20201101174527975](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101174527975.png)

## 7. settings.json 配置

```go
  "go.inferGopath": true,
  "go.autocompleteUnimportedPackages": true,
  "go.gocodePackageLookupMode": "go",
  "go.gotoSymbol.includeImports": true,
  "go.useCodeSnippetsOnFunctionSuggest": true,
  "go.useCodeSnippetsOnFunctionSuggestWithoutType": true,
  "go.docsTool": "guru",
```

![image-20201101174633072](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101174633072.png)



## 8. settings.json 参数介绍：

### 8.1 跳转到定义

- go.docsTools：这里有三个选项，默认使用 **gogetdoc**，不知为何我这里选择之后无法使用 `ctrl/cmd +鼠标左键`点击跳转函数或者源码，于是我选择了 **guru**

![image-20201101175010106](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/image-20201101175010106.png)



## 9. 效果

![QQ20201101-181724-HD](https://zhaouncle.com/image/blog/video/QQ20201101-181724-HD.gif)



# 参考链接：

https://github.com/microsoft/vscode-go

https://github.com/golang/vscode-go

https://github.com/golang/vscode-go/blob/master/docs/tools.md

https://golang.google.cn/
