# 第二章 使用Chrome浏览器开发者工具查看网页

## 2.1 HTTP协议

参考链接：https://www.cnblogs.com/an-wen/p/11180076.html

### 2.1.1 HTTP协议简介

​	超文本传输协议（英文：HyperText Transfer Protocol，缩写：HTTP）是一种用于分布式、协作式和超媒体信息系统的应用层协议。HTTP是万维网的数据通信的基础。

​	HTTP是一个客户端终端（用户）和服务器端（网站）请求和应答的标准（TCP）。通过使用网页浏览器、网络爬虫或者其它的工具，客户端发起一个HTTP请求到服务器上指定端口（默认端口为80）。

​	我们称这个客户端为用户代理程序（user agent）。应答的服务器上存储着一些资源，比如HTML文件和图像。

​	我们称这个应答服务器为源服务器（origin server）。在用户代理和源服务器中间可能存在多个“中间层”，比如代理服务

器、网关或者隧道（tunnel）。

![image-20220421163114107](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163114107.png)



### 2.1.2 HTTP工作原理

​	HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端。

​	HTTP协议采用了请求/响应模型。

​	客户端向服务器发送一个`请求报文`，请求报文包含请求的方法、URL、协议版本、请求头部和请求数据。

​	服务器以一个状态行作为`响应`，响应的内容包括协议的版本、成功或者错误代码、服务器信息、响应头部和响应数据。

![image-20220421163203476](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163203476.png)

### 2.1.3 HTTP无状态保存特性

​	HTTP是一种不保存状态,即无状态(stateless)协议。HTTP协议 自身不对请求和响应之间的通信状态进行保存。也就是说在HTTP这个级别,协议对于发送过的请求或响应都不做持久化处理。

​	通信时通过`cookies`和`session`机制保存访问状态，后面我们可以利用这个机制创建模拟登陆的爬虫来绕过反爬机制。



### 2.1.4 HTTP状态码

![image-20220421163327647](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163327647.png)

- 1xx消息——请求已被服务器接收，继续处理
- 2xx成功——请求已成功被服务器接收、理解、并接受
- 3xx重定向——需要后续操作才能完成这一请求
- 4xx请求错误——请求含有词法错误或者无法被执行
- 5xx服务器错误——服务器在处理某个正确请求时发生错误





## 2.2 HTTP请求和响应

### 2.2.1 请求方式

![image-20220421163426051](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163426051.png)



### 2.2.2 URL

​	统一资源定位符，又叫**URL（Uniform Resource Locator）**，是专为标识Internet网上资源位置而设置的一种

编址方式，我们平时所说的网页地址指的即是URL。



​	以http://www.luffycity.com:80/news/index.html?id=250&page=1 为例, 其中：

```
            http，是协议；
            www.luffycity.com，是服务器；
            80，是服务器上的默认网络端口号，默认不显示；
            /news/index.html，是路径（URI：直接定位到对应的资源）；
            ?id=250&page=1，是查询。
```



### 2.2.3 请求报文

![image-20220421163550789](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163550789.png)

![image-20220421163559659](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163559659.png)

​	请求头里面的内容举个例子：这个length表示请求体里面的数据长度，其他的请求头里面的这些键值对，陆续我们会讲的，大概知道一下就可以了，其中有一个user-agent，算是需要你记住的吧，就是告诉你的服务端，我是用什么给你发送的请求。



![image-20220421163628980](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163628980.png)

​	解决办法（伪装头信息）：

![image-20220421163646655](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163646655.png)



### 2.2.4 响应

​	响应 ，即**服务器返回的响应报文**。

![image-20220421163736572](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163736572.png)

![image-20220421163748766](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163748766.png)



### 2.2.5 HTTP头信息

​	HTTP头部信息(HTTP Header Fields) 是指在超文本传输协议(HTTP) 的请求和响应消息中的HTTP头部信息部分。头部信息中定义了一个超文本传输协议事务中的操作参数。

​	在爬虫中需要使用头部信息向服务器发送模拟信息，并通过发送模拟的头部信息将自己伪装成一般的客户端。

​	某网页的请求头部信息和响应头部信息如下:

![image-20220421163905389](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163905389.png)

![image-20220421163851183](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163851183.png)





**HTTP**的头域包括通用头、请求头、响应头和实体头四个部分。

#### (1) 通用头

​	通用头既适用于客户端的请求头，也适用于服务端的响应头。其与HTTP消息体内最终传输的数据是无关的，只适用于要发送的消息。

​	常用的标准通用头信息如下：

![image-20220421163937328](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421163937328.png)

#### (2) 请求头

​	请求头用于说明是谁或什么在发送请求、请求源于何处，或者客户端的喜好及能力。服务器可以根据请求头部给出的客户端信息，试着为客户端提供更好的响应。

​	常用的请求头信息如下：

![image-20220421164012629](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164012629.png)

![image-20220421164029645](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164029645.png)

![image-20220421164046433](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164046433.png)



#### (3) 响应头

​	响应头向客户端提供一些额外信息，比如谁在发送响应、响应者的功能，甚至与响应相关的一些特殊指令。这些头部有助于客户端处理响应，并在将来发起更好的请求。

![image-20220421164201614](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164201614.png)

![image-20220421164213390](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164213390.png)

![image-20220421164230790](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164230790.png)



#### (4) 实体头

​	实体头部提供了有关实体及其内容的大量信息，从有关对象类型的信息，到能够对资源使用的各种有效的请求方法。

![image-20220421164352407](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164352407.png)



## 2.3 Chrome开发者工具（入门）

### 2.3.1 调用开发者工具

​		方式一：按F12调出

​		方式二：右键检查（或快捷键Ctrl+Shift+i）调出

![image-20220421164946844](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421164946844.png)

### 2.3.2 开发者工具初步介绍

​	chrome开发者工具最常用（爬虫中）的四个功能模块：

![image-20220421165018739](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165018739.png)

​	`元素（ELements）`、`控制台（Console）`、`源代码（Sources）`，`网络（Network）`。‘



​	我们这次课主要先介绍**元素（Elements）**，**网络（Network）**功能的基本操作，在后续的课程中，我们会逐步介绍开发者工具的详细操作。



​	Ps：**控制台（Console）**和  **源代码（Sources）**会在JS逆向的课中详细介绍，有兴趣的同学可以提前熟悉一下。



### 2.3.3 元素（Elements）

​	点击左上角的箭头图标（或按快捷键Ctrl+Shift+C）进入选择元素模式

![image-20220421165258607](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165258607.png)

​	当图标变蓝(如下图)，则已经进入元素模式

![image-20220421165314257](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165314257.png)

​	元素模式下，光标移动到网页中的任意位置，网页会显示该元素的信息，并且会用蓝框把该元素内容显示出来，同时显示元素信息（**元素名**，**color**，**Font**等）。



![image-20220421165347609](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165347609.png)

​		点击蓝色方框，会直接定位到该元素源代码的具体位置。

![image-20220421165410102](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165410102.png)





<img src="C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165422644.png" alt="image-20220421165422644" style="zoom:80%;" />

​	从源代码中读到的只是一部分显式声明的属性，要查看该元素的所有属性，可以在右边的侧栏中查看。

![image-20220421165455929](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165455929.png)

![image-20220421165503434](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165503434.png)



### 2.3.4 网络（Network）  

​	**网络（Network）**是爬虫中的重点。

​	当我们使用Python进行爬虫的时候，其实就是一个模拟的资源访问返回过程，使用第三方库用目的url向所在的服务器发出请求，网站的服务器接收到这个请求后进行处理和分析，然后返回响应。



​	响应中包含了页面的源代码等内容，然后我们在对次进行解析和处理，从中得到我们想要的信息。每个网站根据自己所展示的内容的不同，会有不同级别的反爬手段，我们就要对此进行分析，才能正确的得到自己想要的返回响应，为了更直观的说明这个过程，使用Chrome浏览器的Network监听组件来进行分析。



​	在Network页面下方出现了一个个的条目，其中一个条目就代表一次发送请求和接收响应的过程。（Network就是一个抓包工具）

![image-20220421165606531](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165606531.png)

#### (1) 控制器

​	控制面板的外观与功能

 ![image-20220421165656608](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165656608.png)

 ![image-20220421165707966](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165707966.png)





#### (2) 过滤器

​	用于过滤网络连接信息，过滤请求列表中显示的资源。

![image-20220421165749386](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165749386.png)



​	比如,选择所有JavaScript数据包

![image-20220421165832698](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165832698.png)



​	Doc：选择所有Doc文件，包括文本，**超文本HTML文件（爬虫请求url实际爬取到的文件）**

![image-20220421165859347](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421165859347.png)



#### (3) 请求列表

​	可以看到所有的资源请求，包括网络请求，图片资源，html,css，js文件等请求，可以根据需求筛选请求项，一般多用于网络请求的查看和分析，分析后端接口是否正确传输，获取的数据是否准确，请求头，请求参数的查看



**a.   请求列表每列的含义：**

- Name：资源的名称
- Status：HTTP状态代码
- Type：请求的资源的MIME类型
- Initiator：发起请求的对象或进程。
- Size：服务器返回的响应大小（包括头部和包体），可显示解压后大小
- Time：总持续时间，从请求的开始到接受响应中的最后一个字节
- Waterfall：各请求相关活动的直观分析图



![image-20220421170008964](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421170008964.png)





**b.   每个请求文件的具体说明**

![image-20220421170053843](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421170053843.png)

**Headers模块---重点**

​	Header面板列出资源的请求url、HTTP方法、响应状态码、请求头和响应头及它们各自的值、请求参数等等

![image-20220421174012024](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421174012024.png)

![image-20220421174020571](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421174020571.png)



**Preview：预览面板，用于资源的预览。**

![image-20220421174041138](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421174041138.png)



**Response：原始的响应数据（这也是爬虫获取的数据）**

![image-20220421174106253](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421174106253.png)



**Cookies：客户端的cookie信息，用户的身份信息**

![image-20220421174126996](C:\Users\ACG1314\AppData\Roaming\Typora\typora-user-images\image-20220421174126996.png)