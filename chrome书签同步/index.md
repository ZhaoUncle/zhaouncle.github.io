# Chrome 书签同步


<!--more-->



## 1. 使用 Chrome 插件：书签同步

- 点击下面的超链接就可以进入 Chrome 应用商店去安装和使用了。

  [https://chrome.google.com/webstore/detail/%E4%B9%A6%E7%AD%BE%E5%90%8C%E6%AD%A5/fbcbemgibdnpboehnfcnkegefaomnlbk?hl=zh-CN](https://chrome.google.com/webstore/detail/书签同步/fbcbemgibdnpboehnfcnkegefaomnlbk?hl=zh-CN)

- 使用这款 Chrome 插件之前你需要申请 github 账号，并配置 token 。

- 登录Github，在 Settings->Personal access tokens->Generate new token 生成一个访问token，点击 https://github.com/settings/tokens/new 这个超链接，就是创建你的token。

  ![image-20200902144750477](https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200902144750477.png)

- 可以通过一以下链接，创建一个 github 私有仓库 “Private” 就可以，这是这个插件好用的地方。

  https://github.com/new

  <img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200902145023927.png" width="600" hegiht="250" align=center/>

  

- 点击插件，依次输入“github 用户名”——“github token”——“github 仓库名称/bookmark.json”

  <img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/image-20200902145148139.png" width="600" hegiht="250" align=center/>

  1. “书签插件”，左键点击可以直接配置选项
  2. Username：表示github 用户名

  3. Axxess Token：github上生成的token
  4. Path是仓库名名称，或者仓库名称+“/”+文件名
  5. Upload：是上传到git上面(每次使用书签后 请点击上传)
  6. Download： 是下载网络上的书签 （换电脑以后可以点击download进行同步）
  7. Remember Me：是指记住这些配置
  8. 

- 上传结果演示

  ![image-20200902150629952](/Users/aomine/Library/Application%20Support/typora-user-images/image-20200902150629952.png)

  







## 2. 注意

至于是否需要先关闭chrome的同步功能，这个待测试，如果不影响，那我觉得不需要关闭，随它就行！！！

chrome://settings/syncSetup/advanced?search=%E5%90%8C%E6%AD%A5
