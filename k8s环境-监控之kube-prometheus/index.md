# K8s环境 监控之kube Prometheus


<!--more-->

# 环境

- k8s 1.20
- kube-prometheus release-0.7

在过去的几年中，云计算已经成为及分布式计算最火热的技术之一，其中 Docker、Kubernetes、Prometheus 等开源软件的发展极大地推动了云计算的发展。本文首先从 Prometheus 是如何监控 Kubernetes 入手，介绍 Prometheus Operator 组件。接着详细介绍基于 Kubernetes 的两种 Prometheus 部署方式，最后介绍服务配置、监控对象以及数据展示和告警。通过本文，大家可以在 Kubernetes 集群的基础上学习和搭建完善的 Prometheus 监控系统。

#对比

开源监控方式有[kube-prometheus](https://github.com/prometheus-operator/kube-prometheus)和[prometheus-operator](https://github.com/prometheus-operator/prometheus-operator)

两者的参考文档地址：https://prometheus-operator.dev/docs/prologue/introduction/

监控对比：

1. Prometheus Operator

Prometheus Operator 使用 Kubernetes 定制资源来简化 Prometheus、Alertmanager和相关监控组件的部署和配置。

2. kube-prometheus

[kube-prometheus](https://github.com/prometheus-operator/kube-prometheus) 提供了基于 Prometheus 和 Prometheus Operator 的完整集群监控堆栈的配置示例。这包括部署多个 Prometheus和 Alertmanager 实例、metrics exporters(如用于收集节点 metrics 的 node_exporters)、将 Prometheus 链接到各种度量端点的目标配置，以及用于通知集群中潜在问题的示例警报规则。

3. helm chart

The [prometheus-community/kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack) 提供了与 kube-prometheus 类似的功能集。该图表由Prometheus社区维护。有关更多信息，请参见图表的自述文件 [chart's readme](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack#kube-prometheus-stack)

#kube-prometheus安装

## 先决条件

默认情况下，假定kubelet使用令牌身份验证和授权，否则Prometheus需要一个客户端证书，它可以完全访问kubelet，而不仅仅是度量。令牌身份验证和授权允许更细粒度和更容易的访问控制。

kubelet配置必须包含以下标志：

- `--authentication-token-webhook=true`： 通过此标志，可以使用ServiceAccount令牌对kubelet进行身份验证。也可以通过将kubelet配置值authentication.webhook.enabled设置为true来启用它。
- `--authorization-mode=Webhook`：这个标志允许kubelet使用API执行一个RBAC请求，以确定是否允许请求实体(在本例中是Prometheus)访问资源，特别是这个项目的/metrics端点。这也可以通过设置kubelet配置值授权来启用。Webhook模式。

该堆栈通过部署Prometheus适配器提供资源指标。该适配器是扩展API服务器，需要启用Kubernetes的此功能，否则适配器无效，但仍在部署中。



安装

commit 版本：f5f72e1b5011830da821a7f6afff667c27b6fc37

```
git clone https://github.com/prometheus-operator/kube-prometheus.git
cd kube-prometheus
kubectl create -f manifests/setup
until kubectl get servicemonitors --all-namespaces ; do date; sleep 1; echo ""; done
kubectl create -f manifests/
```

临时暴露 web 页面：

Prometheus

```
kubectl --namespace monitoring port-forward svc/prometheus-k8s 9090
```

Then access via [http://localhost:9090](http://localhost:9090/)

Grafana

```
$ kubectl --namespace monitoring port-forward svc/grafana 3000
```

Then access via [http://localhost:3000](http://localhost:3000/) and use the default grafana user:password of `admin:admin`.

Alert Manager

```
$ kubectl --namespace monitoring port-forward svc/alertmanager-main 9093
```

Then access via [http://localhost:9093](http://localhost:9093/)

## 

改为 NodePort 类型，然后访问

改动方法一：直接在service.yaml 添加`type: NodePort`

```
cd kube-prometheus/manifests
sed -i '/selector:/i\\  type:\ NodePort' grafana-service.yaml 
sed -i '/selector:/i\\  type:\ NodePort' alertmanager-service.yaml
sed -i '/selector:/i\\  type:\ NodePort' prometheus-service.yaml
kubectl apply -f alertmanager-service.yaml 
kubectl apply -f grafana-service.yaml 
kubectl apply -f prometheus-service.yaml
```

方法二：命令行在线修改

```
kubectl edit svc grafana -n monitoring
kubectl edit svc prometheus-k8s -n monitoring
kubectl edit svc alertmanager-main -n monitoring
```

结果：

```
[root@node01 manifests]# kubectl get svc -n monitoring
NAME                    TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)                      AGE
alertmanager-main       NodePort    10.0.0.137   <none>        9093:31641/TCP               136m
alertmanager-operated   ClusterIP   None         <none>        9093/TCP,9094/TCP,9094/UDP   136m
blackbox-exporter       ClusterIP   10.0.0.149   <none>        9115/TCP,19115/TCP           136m
grafana                 NodePort    10.0.0.216   <none>        3000:30450/TCP               136m
kube-state-metrics      ClusterIP   None         <none>        8443/TCP,9443/TCP            136m
node-exporter           ClusterIP   None         <none>        9100/TCP                     136m
prometheus-adapter      ClusterIP   10.0.0.115   <none>        443/TCP                      136m
prometheus-k8s          NodePort    10.0.0.180   <none>        9090:31907/TCP               136m
prometheus-operated     ClusterIP   None         <none>        9090/TCP                     136m
prometheus-operator     ClusterIP   None         <none>        8443/TCP                     139m
```





nginx ingress 暴露端口访问

参考：https://github.com/prometheus-operator/kube-prometheus/blob/main/docs/exposing-prometheus-alertmanager-grafana-ingress.md

安装htpasswd

```
yum -y install httpd-tools
```



清除

```
kubectl delete --ignore-not-found=true -f manifests/ -f manifests/setup
```

![image-20210404184709478](https://cdn.jsdelivr.net/gh/ZhaoUncle/image@main/blog/20210404184709.png)

#prometheus-operator安装



# 参考

- https://www.infoq.cn/article/uj12knworcwg0kke8zfv

- https://github.com/prometheus-operator/kube-prometheus

- https://huangzhongde.cn/post/Kubernetes/Using_kube-prometheus_deploy_monitor_system/
- https://juejin.cn/post/6865504989695967245
- https://huisebug.github.io/2019/08/27/Prometheus-Operator/
