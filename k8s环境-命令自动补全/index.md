# K8s环境命令行自动补全


<!--more-->

#kubectl， helm  命令自动补全

##kubectl 命令自动补全：

```
yum install -y bash-completion
locate bash_completion

echo "source /usr/share/bash-completion/bash_completion" >> /etc/profile
echo "source <(kubectl completion bash)" >> /etc/profile
```

若要马上生效，直接执行 source 的命令而不是添加到文件里面

##helm 自动补全：

```
echo "source <(helm completion bash)" >> /etc/profile
```






