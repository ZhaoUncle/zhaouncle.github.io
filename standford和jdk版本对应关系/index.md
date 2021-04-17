# standford parser 和 jdk 版本对应关系


<!--more-->

#问题如下：

```
Caused by: java.lang.UnsupportedClassVersionError: org/springframework/web/SpringServletContainerInitializer : Unsupported major.minor version 52.0 (unable to load class org.springframework.web.SpringServletContainerInitializer)
```

**首先解释一下报错原因**：

stanford parser和jdk版本对应关系

```
J2SE 8 = 52,
J2SE 7 = 51,
J2SE 6.0 = 50,
J2SE 5.0 = 49,
JDK 1.4 = 48,
JDK 1.3 = 47,
JDK 1.2 = 46,
JDK 1.1 = 45
```

Unsupported major.minor version 52.0: 看到Unsupported你是不是会想到jdk高版本能兼容低版本，但是低版本不能兼容高版本，不错，猜对了，其实就是这个意思。这个错误意思是你项目用JDK1.8运行过，现在又在本地的eclipse等开发工具或者本地环境变量为低版本的jdk1.7或者jdk1.6下运行，eclipse会说：“抱歉，我本地jdk版本太低，不支持这个高级版本jdk1.8编译过的项目运行”。

同理，如果你在高版本的 jdk 比如 11 版本编译了 war 包，然后丢到8 版本的 tomcat 跑，那么也一样会报错。
