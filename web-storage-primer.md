#  <译>Web Storage初探

[Web Storage](http://www.w3.org/TR/webstorage/)是相对比较新的一种可以将数据存储在用户电脑（客户端）的一种方式。

Web Storage给网站/应用提供了很多好处。比如，Web Storage可以用来跨网站/应用监测用户的行为而不需要服务端脚本和数据库。Web Storage也能在用户即使突然断网的情况下保存一部分web应用的能力，让你不会因为网络连接的问题受到影响

## Web Storage vs. Cookies

传统的用来在客户端保存数据的方式是通过HTTP cookies。

Web Storage和cookies之间存在着很多的区别。让我们把注意力放在这两种客户端数据存储的方法的真正的区别上。

### 存储原理

Cookies是一种结构化的数据，是由web服务器返回给请求的一个服务器响应的一部分。Cookie可以通过设置HTTP响应头来进行设置。无论什么时候发送请求，浏览器都会把cookie作为请求头的一部分返回给服务器。

简单的来说，请求发送的目的是为了能够更新cookie里面的值。同时，无论数据有没有改变，cookies也总是会占据HTTP响应头的一部分。

另一方面，Web Storage的创建和管理完全是由客户端控制的。因此，尽管有服务器的参与有很多其他的优势，但Web Storage还是避免了web服务器的参与。这个方法产生了很多有益的结果，最明显的莫过于理论上导致了更好的web性能

**相关：[Web性能文章](http://sixrevisions.com/category/web-performance/)**:

同时，因为Web Storage可以在没有HTTP请求/响应的情况下工作（摆脱了网页初始与服务器的交互），在良好的实现情况下，存储在用户浏览器的数据可以在失去网络连接的情况下也能安全的更新和修改。

### 不同的浏览器实例

Web Storage能够解决用户开启多个浏览器窗口/标签的同步问题。在一个浏览器窗口在存储或者更新数据能够同时在其他浏览器窗口上同步，只要其他浏览器窗口在同一个网站上。

Cookies，相反，设计之初就不是用来处理设计多个浏览器窗口的情况。

### 存储大小限制

HTTP cookies和Web Storage对存储大小的限制在不同浏览器之间是不同的。但是，一般为了能够好的跨浏览器支持把存储大小限制在4.0KB左右是最佳的实践方案。

（这里有一种[cookie测试工具](http://browsercookielimits.squawky.net/)可以让你测试浏览器的cookie的存储大小限制）

W3C Web Storage标准并没有推荐一个默认的存储限制，让浏览器自己决定。但是，现实中Web Storage完虐cookies的4.0KB的限制，一般情况下对于Web Storage对象的限制时5MB左右。

也就是说，Web Storage的大小限制是cookies的124527%倍那么大。

（这里也有一种[Web Storage测试工具](http://dev-test.nemikor.com/web-storage/support-test/)可以让你测试浏览器Web Storage的存储大小限制）

## Storage的种类

Web Storage有两种方式来存储客户端数据：sessionStorage和localStorage.

**Web Storage** **存储数据的生命周期**  **数据结构** **数据类型**
sessionStorage   只在回话中保存     键值对  字符串

localStorage     持久保存      键值对   字符串


