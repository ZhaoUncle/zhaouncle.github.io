# Notion 绑定自定义域名


<!--more-->

# Notion 绑定自定义域名

自定义域名是个很常见的需求，很多 Notion 用户拿 Notion 作博客，想给自己绑定个性化的域名却发现没有很好的途径，最后域名只能做个转发，跳转到 Notion 官方页面。

考虑到很多 Notion 用户并非程序员，所以这个文章不会解释过多的技术细节，是新手向的教程。

按照下面的教程做完，将会实现。

1. 通过自己的域名访问 Notion
2. 浏览器地址上显示自己的域名
3. 访问页面时会有一定程度的加速效果

## 原理说明

官方目前不打算推出自定义域名的特性。如果你将自己的域名设置为隐式转发会在前端看到一个这样的提示。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/notion01.png" width="800" hegiht="250" align=center/>

其主要的检验手段是在前端 js 做校验， Notion 是一个典型的单页应用，利用 Cloudflare 的 worker 做代理，将 js 中的 [notion.so](http://notion.so/) 全部替换成自己的域名，这样即可绕过校验。

1. 如果访问 app.js，则替换 js 中的域名，使前端域名校验失效。
2. 如果访问其它路径，则返回原有内容。

这么做是有局限的。

1. 只读，无法登陆，无法评论。仅仅做展示用。
2. 部分第三发接口无法调用（反正是只读，也没啥问题）

## 准备工作

- 域名一个

- Cloudflare 帐号一枚 （后面简称 cf）

### 操作过程

1. 域名 DNS 服务器设置为 cf （略）。
2. 为域名配置一条 a 记录，ip 地址随便填（不知道填什么就 ping 下 www.cloudfare.com ，例如 104.17.209.9 ），但是要通过 cf proxy，打开 Cloudflare 的「DNS」编辑页面，添加一条记录。如下图：

注意，其中的 IPv4 地址部分，虽然你可以改成任意 DNS 的 IP，或者是你自己的服务器，但是当你的 IP 为国内 IP 时，有概率会因为备案问题而 404。

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/notion02.png" width="800" hegiht="250" align=center/>

3. 创建 Workers，查看如下的“worker 代码”

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/notion03.png" width="800" hegiht="250" align=center/>

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/notion04.png" width="800" hegiht="250" align=center/>

4. 添加一条 route，并指向上方的 worker rule

<img src="https://cdn.jsdelivr.net/gh/ZhaoUncle/images/blog/notion05.png" width="800" hegiht="250" align=center/>



### Worker 代码

配置 worker：https://www.bilibili.com/video/av64011915/

cloudfare网址链接：[https://dash.cloudflare.com](https://dash.cloudflare.com/)



**复制下面代码**

第一行改为你的域名。

第二行改为你想让访问者看到的首页地址。

例如 我想要用户 访问 link.zhaouncle.com 时，跳转到指定的页面（Home 页）

例如：https://www.notion.so/e00f6db6166e47279acab60c32094e37?v=efe4879069ed4d5b880c7118fb3a370a

```shell
const MY_DOMAIN = "link.zhaouncle.com"
const START_PAGE = "https://www.notion.so/e00f6db6166e47279acab60c32094e37?v=efe4879069ed4d5b880c7118fb3a370a"

addEventListener('fetch', event => {
  event.respondWith(fetchAndApply(event.request))
})

const corsHeaders = {
  "Access-Control-Allow-Origin": "*",
  "Access-Control-Allow-Methods": "GET, HEAD, POST,PUT, OPTIONS",
  "Access-Control-Allow-Headers": "Content-Type",
}

function handleOptions(request) {
  if (request.headers.get("Origin") !== null &&
    request.headers.get("Access-Control-Request-Method") !== null &&
    request.headers.get("Access-Control-Request-Headers") !== null) {
    // Handle CORS pre-flight request.
    return new Response(null, {
      headers: corsHeaders
    })
  } else {
    // Handle standard OPTIONS request.
    return new Response(null, {
      headers: {
        "Allow": "GET, HEAD, POST, PUT, OPTIONS",
      }
    })
  }
}

async function fetchAndApply(request) {
  if (request.method === "OPTIONS") {
    return handleOptions(request)
  }
  let url = new URL(request.url)
  let response
  if (url.pathname.startsWith("/app") && url.pathname.endsWith("js")) {
    response = await fetch(`https://www.notion.so${url.pathname}`)
    let body = await response.text()
    try {
      response = new Response(body.replace(/www.notion.so/g, MY_DOMAIN).replace(/notion.so/g, MY_DOMAIN), response)
      // response = new Response(response.body, response)
      response.headers.set('Content-Type', "application/x-javascript")
      console.log("get rewrite app.js")
    } catch (err) {
      console.log(err)
    }

  } else if ((url.pathname.startsWith("/api"))) {
    response = await fetch(`https://www.notion.so${url.pathname}`, {
      body: request.body, // must match 'Content-Type' header
      headers: {
        'content-type': 'application/json;charset=UTF-8',
        'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_13_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36'
      },
      method: "POST", // *GET, POST, PUT, DELETE, etc.
    })
    response = new Response(response.body, response)
    response.headers.set('Access-Control-Allow-Origin', "*")
  } else if (url.pathname === `/`) {
        let pageUrlList = START_PAGE.split("/")
    let redrictUrl = `https://${MY_DOMAIN}/${pageUrlList[pageUrlList.length-1]}`
    return Response.redirect(redrictUrl, 301)
  } else {
    response = await fetch(`https://www.notion.so${url.pathname}`, {
      body: request.body, // must match 'Content-Type' header
      headers: request.headers,
      method: request.method, // *GET, POST, PUT, DELETE, etc.
    })
  }

  return response
}
```



**参考网站：**

https://gine.me/posts/466ca19681634b09b123067f880ea396?nsukey=cgA3O8FAmy0q877VvsS2i1DFyHR%2Bk9iwyM%2F%2B7M5zbx9ii20yrqWZwmmKVzd2a2n80FH9U9OaZ1rHrF6UKlksZ6e57XVwZKydlJ1TDg3NtQLHBaeBiAn9%2FIQ9v%2FeKcQKvQfjgpr7xuwiuo8LPMSu6WEyBl3bIUTeXEwj5qHQ8hKXZvERUVYXFT54%2BmbaxpC4RoS7TIgdiB8AAc098g0WUWw%3D%3D



https://www.notion.so/URL-e1620aa7a9204c289e0be7c65eaeef48
