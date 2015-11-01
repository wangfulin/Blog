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

| **Web Storage** |**存储数据的生命周期** |  **数据结构** | **数据类型** |
|---------------- |---------------------|--------:|--------:|
| sessionStorage  |  只在回话中保存      | 键值对  | 字符串 |
| localStorage    |  持久保存       |   键值对 | 字符串 |

### sessionStorage

sessionStorage是设计用来保存浏览器会话这个时间段的客户端数据的。换句话说数据只在用户还在网站浏览的时候才保存起来。

sessionStorage在实践中主要用来暂时的数据 存储。比如说，用户在网页表单上的输入能够在用户浏览器会话的阶段保存在sessionStorage对象当中。这样的话，数据就能够在浏览的整个过程中获得到而不需要把数据存储到服务器当中。也就是说，当用户不小心关闭或者刷新浏览器窗口，暂时的数据存储可以防止用户不得不重复输入数据。

### localStorage

从实现的角度来说，localStorage和sessionStorage的实现方式是基本类似的。

localStorage和sessionStorage分享一套javascript方法（比如说：`getItem`,`setIte`m等等），并且它也以键值对的方式存储数据。

但是，把数据存储在localStorage对象上意味着数据可以在用户的会话之间一直保存着。换句话说，数据可以在用户下一次访问这个网站的时候依然可以获取到。

## Web Storage的浏览器支持

Caniuse.com的结论是Web Storage有良好的浏览器支持。

Web Storage浏览器支持表
|浏览器|版本|
|:-----|:----|
|Internet Explorer|IE8及IE8以上|
|Mozilla Firefox|Firefox 3.5及以上|
|Google Chrome|Chrome 4及以上|
|Apple Safari|Safari 4及以上|
|Opera|Opera 11.5及以上|

目前，Web Storage标准是*W3C Candidate Recommendation*.总共有五个级别的推荐。"Candidate Recommendation"是这五个级别中的第三个级别。目前的Web Storage已经相当成熟因为这已经不是一个工作草案啦，但是与此同时，这个也不是最终的方案。

支持旧的没有Web Storage这个特性的浏览器可以通过使用polyfill来支持。对于localStorage的支持可以选择[Store.js](https://github.com/marcuswestin/store.js)。

##　数据隐私和安全考虑

Ｗeb Storage和cookies有一样的禁止跨域的策略。这意味着一个网站的Web Storage不能被其他网站访问。这对于数据安全来说是有益的。但是这也导致了我们在需要子域的时候出现问题。对于这个问题，有其他的解决方案，比如由Zendesk开发的一个开源包[Cross-Storage](https://github.com/zendesk/cross-storage)。

就像任何其他的客户端数据存储机制一样，保护存储的数据也是一个很重要的需要考虑的点。保存用户私密或者敏感的数据是不推荐的。这也让能够接触设备的人更方便的获取到本地数据。尤其是在一些其他人可以用公共的网络环境比如大学，图书馆，工作电脑等访问你的网站的地方更应该格外小心。

数据完备性也是一个考虑因素。必须要有在存储事务失败的时候的解决方案。当用户的Web Storage被关闭时候，或者当用户的存储空间不足的时候，或者超过浏览器的Web Storage限制的时候都有可能造成失败。Web Storage规范说明了当触发存储失败事件的标准的错误/警告输出，比如QuotaExceededError异常。

## IndexedDB怎么样呢？

对于客户端存储来说，[Indexed Database API](http://www.w3.org/TR/IndexedDB/)(IndexedDB)提供了很多可以从Web Storage衍生出来的相同的有点。但是IndexedDB不是Web Storage规范的一部分所以这也超出了我们所要讨论的范围。

但是。IndexedDB也很值得我们简单的聊一下，因为它和Web Storage实现有很多共享的特点以及未来可能有联系的可能。

通过IndexedDB存储数据相对于Web Storage来说可能会更加复杂一些。但是，它也打开了通往更加复杂数据结构和关系的大门。

通过IndexedDB，数据会像你喜欢的客户端数据管理系统比如`MYSQL`,`MSSQ`L,`PostgreSQ`L等等数据库一样通过索引存储起来

**相关：[最好的五中数据库管理工具](http://sixrevisions.com/tools/top-five-best-database-management-tools/)**

除此之外，如果你实现了一个可以处理IndexedDB数据库的查询语言，你也可以想服务端数据库一样检索数据库。

相对于IndexedDB来说，Web Storage的数据获取能力是很基础的。Web Storage只有两个内置的获取数据的方法：`.getItem`和`.key`。（除了能够可以获取Web Storage对象的`length`属性）。因此，当你考虑到需要更加复杂的数据存储的时候，你可以考虑IndexedDB而不是Web Storage。

## 原文链接
[web-storage-primer](http://sixrevisions.com/web-technology/web-storage-primer/)


