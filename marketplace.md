# Firefox Marketplace

![Firefox Marketplace](images/originals/marketplace.png)

[Firefox Marketplace](http://marketplace.firefox.com)是一个在线商店，用户可在上面购买或下载Firefox OS、Firefox和Android版Firefox的应用。这是发布Firefox OS应用的主要渠道，但不是唯一渠道。如果你想在Firefox Marketplace外发布应用，请参考[上一章](#distribution)。

要提交应用到Marketplace，首先需要通过[Mozilla Persona](https://login.persona.org/about)的验证。点击**登录**，按照指示操作即可。验证成功后，你就可以准备提交了。

## 产生提交应用的想法之前就应该核对好的清单

所有提交到Marketplace的应用都有个审批流程（听起来吓人而已！）。托管应用的审批没privileged应用那么严格，因为前者没有涉及敏感API。提交前，先看看Marketplace的[审查准则](https://developer.mozilla.org/en-US/docs/Web/Apps/Publishing/Marketplace_review_criteria)。最重要的部分是(个人愚见)：

* Firefox OS设备没有Android和桌面浏览器那种**返回按钮**。假如用户点进了应用的某个地方，但没办法返回上处（即被困住了），你的应用将被驳回。
* 应用需要提供一个60x60的图标和清晰的描述。
* 应用的功能应和描述一致。描述的功能若和实际提供的功能不一致，应用将被驳回。
* 如果应用申请了某权限，那么你必须在代码中使用它。把你的应用标记为privileged应用，但是代码却没有使用任何privileged API的话，应用会被驳回，并要求你重新提交为普通应用。
* 应用必须包含*隐私政策*说明。
* Manifest文件的MIME类型必须能被服务器正确识别，并和托管应用放在同一域下。

上述链接还提到了其他准则 - Mozilla有权随时修改，不会另行通知。该页面内容值得你花时间阅读。要是因为一些容易修复的小问题而被驳回，那可真是太浪费时间了。最好第一次就把全部东西准备妥当（好的应用很受审核员青睐！）

## 准备提交应用

托管应用和打包应用的提交步骤是不同的。就托管应用而言，只需确保拥有正确的MIME类型和manifest文件，并能被在线访问到。打包应用则需要以*zip*格式压缩，且还有些注意事项。

很多开发者会错把包含应用文件的文件夹打包了。这导致压缩包包含一个文件夹，该文件夹包含应用。这是错误的打包方式。正确做法是：打包后，manifest文件位于压缩包的根目录。Mac OS X和Linux用户可使用terminal导航到应用文件夹，运行命令，如“zip -r myapp.zip *”，来正确打包应用，如下图所示：

![Correctly zipping the files](images/originals/marketplace-preparing-packaged-app.png)

这个zip压缩包就是我们要提交到Marketplace的。

## 提交应用到Marketplace
 
应用已经准备就绪，并且你确定它是遵循审查准则的，那么是时候提交到Marketplace了。点击Marketplace页面顶部的齿轮，选择“我的应用”。

![My Submissions](images/originals/marketplace-my-submissions.png)

在应用管理页面，点击顶部菜单的**提交一个应用**。

![Submit An App](images/originals/marketplace-new-app.png)

这个链接会把你带到提交新应用的表单，如下图所示。

![Submit New App](images/originals/marketplace-step-1.png)

在此页面有以下选项可选：

* 托管应用还是打包应用
* 免费还是付费（或者应用内购买）
* 支持哪些平台（Firefox OS, Firefox桌面版, Firefox手机版, Firefox平板版）

选择完后自动进入下一屏。本书着重讲解打包应用的发布，但其实托管应用的流程大体相同。

在本章剩余的内容里，我们假设要发布的是免费的Firefox OS打包应用。这种情况下，我们需要上传前面准备好的zip压缩包。

上传完毕，它会开始自动处理，生成一个包含很多选项的报告。

![After the zip upload](images/originals/marketplace-step-1_5.png)

如上图所示，刚才提交的应用没出错，但包含了6个警告。为了继续演示，我们忽略掉，接着来勾选*应用最低需求*。在这里，最后的*智能手机尺寸的屏幕(有qHD分辨率)*不要勾选，因为我们的应用可适配各种屏幕尺寸。

下一步是**步骤 #3: 详情**，你需要补充完整应用的信息，例如分类、描述、截图等。

![Filling details](images/originals/marketplace-step-3.png)

填写完详情后，整个提交过程结束。现在只需等待Marketplace审核员的通过了。恭喜你提交了一个Firefox OS应用！

你可以到[应用管理页面](https://marketplace.firefox.com/developers/submissions)查看提交状态，有需要的话还可以修改详情。

若想深入了解如何提交应用到Firefox Marketplace，可参考[Firefox OS developer hub](https://marketplace.firefox.com/developers/docs/submission)里的文档。

## 总结

恭喜！！！你在Firefox Marketplace发布了一个新应用，这是对一个全新市场的探索。

我希望你喜欢这本快速指南。我打算经常更新和扩展该指南，所以请持续关注，订阅更新吧。如果你的书是在Leanpub下载的，很好，每次更新你都会收到邮件。如果是在别处下载的，请考虑在[Leanpub的官方页面](http://leanpub.com/quickguidefirefoxosdevelopment)获取更新，以及用邮件订阅。本书完全免费，而且你绝对不会收到任何垃圾邮件，我保证。

欢迎反馈。这本书是我在一个科技大会召开前，抽出两晚通宵赶出来的，所以你能体会我是有多享受这个项目，多想看它成功。如有反馈，可去Twitter[@soapdog](http://twitter.com/soapdog)，或者发送邮件到[fxosquickguide@andregarzia.com](mailto:fxosquickguide@andregarzia.com)。我的个人主页是[http://andregarzia.com](http://andregarzia.com)。

现在你是Firefox OS应用开发者的一员了，来成为我大Mozilla社区的一员吧，Mozilla社区致力于打造自由的网络，对用户完全开放的网络。来[http://www.mozilla.org/contribute/](http://www.mozilla.org/en-US/contribute/)加入我们，帮助Firefox OS成长！
