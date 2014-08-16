# Distributing Your Apps {#distribution}

既然已经开发好应用，我们应该想办法发布给用户。在[introduction chapter](#introduction)章节我提到过，和Apple公司不同，Mozilla基金会并不强制你使用指定的渠道发布应用——你可以按照自己的意愿自由发布作品。本章中，我们将学习如何**在[Firefox Marketplace](http://marketplace.firefox.com)之外**发布我们自己的应用。

依我拙见，在这两种情况于Mozilla应用市场之外发布应用是明智的选择：

1.你开发的是一个在公司内部，或者在一个保密的/人数有限的团体内使用的应用。如果你将应用公开发布到Mozilla应用市场上，那么所有人都可以下载使用它。如果想把使用者限定在一定范围内，就需要配备诸如后端服务器之类的认证方案。例如，当第一次加载*Evernote* 应用时会先要求登录服务器。

2.在发布应用时可以利用你已经拥有的庞大用户群。比如在报纸行业，例如*Financial Times*，可以在官网上面向众多读者上发布他们的应用。记住，你可以同时在应用市场内外发布同一应用。所以你可以在自家发布渠道的之外，利用在Mozilla应用市场的发布吸引新用户。

发布在线和离线应用的流程相似，但是使用不同的API函数。这也是为什么我将他们分开讨论的原因。不区分离线和在线的话，大致的发布流程是：在自己的主页上添加一个诸如**通过点击安装应用**的按钮或链接，或者在用户点击时导向一个触发应用安装程序的特定URL。在这两种情况下，都会向用户弹出一个确认安装指定应用的对话框。

## 在线应用

<<[Code for hosted app installation](code/distribution/hosted_apps_distribution.js)

以上`manifestURL`包含了指向mainfest文件的链接。代码执行时，系统将要求用户确认自己安装相关应用的意愿，并根据用户的具体选择决定安装与否。

与该API函数的相关的更多内容参见[the MDN page about application installation](https://developer.mozilla.org/docs/Apps/JavaScript_API)。

## 离线应用

离线应用的安装过程类似，但是我们将用`mozApps.installPackage()`调用取代
`mozApps.install()`调用，如下图示：

<<[Code for packaged app installation](code/distribution/packaged_apps_distribution.js)

W> 注意：我记得在Firefox OS 1.0.1版本中不能在Mozilla市场之外安装离线应用。尽管官方已经发布该API函数，我自己还没有践行过。如果你试用过的话，请向我反馈以更新本书内容。

## 总结
本章讨论了在Firefox应用市场之外使用安装和管理API函数发布*开放Web应用程序*的各种选择。你还可以在网页上添加检测应用是否已安装的代码（这样就可以在已经安装的情况下隐藏*点击安装应用*的按钮）。相关API函数的更多内容参见[MDN page about application installation](https://developer.mozilla.org/docs/Apps/JavaScript_API) (没错，之前已经给出过这个链接了，快去浏览吧！那里包含了很多重要的源码)。

在下章中我们将学习如何在Firefox应用市场中发布应用。
