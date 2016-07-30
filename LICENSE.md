　iframes 提供了一个简单的方式把一个网站的内容嵌入到另一个网站中。但我们需要慎重的使用iframe。iframe的创建比其它包括scripts和css的 DOM 元素的创建慢了 1-2 个数量级。下图显示创建 100 个不同的元素中iframe到底有多耗费时间。

　　


 

　　创建100个 elements 的耗时

　　使用 iframe 的页面一般不会包含太多 iframe，所以创建 DOM 节点所花费的时间不会占很大的比重。
　　但带来一些其它的问题：onload 事件以及连接池(connection pool)。

　　Iframes 阻塞页面加载

　　及时触发 window 的 onload 事件是非常重要的。onload 事件触发使浏览器的 “忙” 指示器停止，告诉用户当前网页已经加载完毕。
　　当 onload 事件加载延迟后，它给用户的感觉就是这个网页非常慢。

　　window 的 onload 事件需要在所有 iframe 加载完毕后(包含里面的元素)才会触发。在 Safari 和 Chrome 里，
　　通过 JavaScript 动态设置 iframe 的 SRC 可以避免这种阻塞情况。

　　唯一的连接池

　　浏览器只能开少量的连接到web服务器。比较老的浏览器，包含 Internet Explorer 6 & 7 和 Firefox 2，
　　只能对一个域名(hostname)同时打开两个连接。这个数量的限制在新版本的浏览器中有所提高。
　　Safari 3+ 和 Opera 9+ 可同时对一个域名打开 4 个连接，Chrome 1+, IE 8 以及 Firefox 3 可以同时打开 6 个。
　　你可以通过这篇文章查看具体的数据表：Roundup on Parallel Connections.

　　有人可能希望 iframe 会有自己独立的连接池，但不是这样的。绝大部分浏览器，主页面和其中的 iframe 是共享这些连接的。
　　这意味着 iframe 在加载资源时可能用光了所有的可用连接，从而阻塞了主页面资源的加载。如果 iframe 中的内容比主页面的内容更重要，
　　这当然是很好的。但通常情况下，iframe 里的内容是没有主页面的内容重要的。这时 iframe 中用光了可用的连接就是不值得的了。
　　一种解决办法是，在主页面上重要的元素加载完毕后，再动态设置 iframe 的 SRC。

　　美国前 10 大网站都使用了 iframe。大部分情况下，他们用它来加载广告。这是可以理解的，也是一种符合逻辑的解决方案，
　　用一种简单的办法来加载广告服务。但请记住，iframe 会给你的页面性能带来冲击。只要可能，不要使用 iframe。当确实需要时，
　　谨慎的使用他们。
