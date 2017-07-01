# blog
# 从 URL 输入到页面展现发生了什么
从在浏览中输入URL到按下回车再到看到网页，中间的过程很复杂，但是对于程序员来说，有必要弄清楚大致的过程，而且在前端面试会常考这道题。

##URL是什么：

  我们经常在浏览器里输入的网址就是URL，中文名叫统一资源定位符，它用于定位互联网上的资源。
  基本URL包含模式（或称协议）、域名（或IP地址）、路径和文件名,比如：http://example.com:8080/test/test.html
  http是网络协议，example.com是域名，8080是端口，/test/test.html是资源路径
  http是最为常见的网络协议--超文本传输协议，除此之外还有https（用安全套接字层传送的超文本传输协议），ftp(文件传输协议),file（本地电脑或网上分享文件的协议）等等协议。

##域名解析：

  IP地址对应着网络上一台计算机，如果访问的地址是域名而不是IP地址的话，就需要通过DNS（域名解析系统）将该域名解析成IP地址，这样才能访问。

##DNS查找过程如下：
浏览器缓存 – 浏览器会缓存DNS记录一段时间。 有趣的是，操作系统没有告诉浏览器储存DNS记录的时间，这样不同浏览器会储存各自固定的一个时间（2分钟到30分钟不等）。
系统缓存 – 如果在浏览器缓存里没有找到需要的记录，浏览器会做一个系统调用（windows里是gethostbyname），从Hosts文件中查找是否有该域名对应的IP
路由器缓存 – 一般路由器也会缓存域名信息，如果前面的查询没有结果，那么请求会发向路由器。
ISP DNS 缓存 – 接下来要查找的就是ISP缓存DNS的服务器，比如电信的DNS服务。在这一般都能找到相应的缓存记录。
递归搜索 – 你的ISP的DNS服务器从根域名服务器开始进行递归搜索，从.com顶级域名服务器到example的域名服务器。一般DNS服务器的缓存中会有.com域名服务器中的域名，所以到顶级服务器的匹配过程不是那么必要了。
发送请求

  当我们想要访问一个网页，输入URL后按下回车键浏览器会向服务器发出一个http协议请求。比如我们要访问百度，http请求包含的内容如下：

Accept:text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Encoding:gzip, deflate, sdch, br
Accept-Language:zh-CN,zh;q=0.8
Cache-Control:max-age=0
Connection:keep-alive
Cookie:（Cookie省略）
Host:www.baidu.com
Upgrade-Insecure-Requests:1
User-Agent:Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.99 Safari/537.36
##服务器处理：
  请求在进入到真正的应用服务器前，可能还会先经过负责负载均衡的机器，它的作用是将请求合理地分配到多个服务器上，同时具备具备防攻击等功能；或者被反向代理服务器比如Nginx接受然后代理到其他的服务器。
  最终的应用服务器接收到请求，然后处理并返回一个响应。
  这里所说的应用服务器指的是Web服务器软件，比如IIS，Apache，NodeJs等等。以 Apache 为例，在接收到请求后会交给一个独立的进程来处理，我们可以通过编写 Apache 扩展来处理，但这样开发起来太麻烦了，所以一般会调用 PHP 等后端脚本语言来进行处理，它可以读取http请求的请求参数和cookie，然后生成一个HTML响应返回给客户端浏览器。如果请求的是静态HTML文件，服务器就不会进入到后端语言处理部分，而是直接返回给客户端这个HTML文件。

##浏览器处理：
  HTML字符串被浏览器接受后会被一句一句的解析出来，如果解析到link标签就会根据href发送请求获取css文件，解析到script标签会发送请求获取JavaScript文件。
  最后浏览器根据HTML和CSS计算得到渲染树，绘制到屏幕上，并执行JavaScript代码。