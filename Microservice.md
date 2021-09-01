## 微服务
经常翻阅微服务材料的话，总会碰到 microservices.io 这个网站，总结了微服务方方面面的设计模式。网站的作者是 Chris Richardson。

这些相关的经验在 2018 年成为了《微服务架构设计模式》这本书，并且 2019 年引进国内。当时我第一时间购入了这本书，中间断断续续地一直没看完。最近准备分享内容的时候又翻起这本书，
这次完整地读完了一遍。感觉应该是目前微服务领域最好的一本书了。另一本《Building Microservices》也不错，不过内容还是稍微单薄了一些。
这本书为我们提供了宏观上俯瞰微服务整个生态的大图，比如：
![avatar](https://mmbiz.qpic.cn/mmbiz_png/Lq0XA1b7xbfjQPLSUGPRibSTP5TgePh37SyLrwTQUY5XQ0I0vAtGibZbaEWAEJvP7ibkFxUYAckmEsyD7M8FYj8ug/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)
当然，18 年的时候，service mesh 之类的东西还没有太火，所以后来在网站上有个更新的版本：
![avatar](https://mmbiz.qpic.cn/mmbiz_png/Lq0XA1b7xbfjQPLSUGPRibSTP5TgePh37XRI5CG2TDC1iaOtkujcxHI3AL82lu3tzjwc64FPZMoVkIjtZRyHrPRA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

下面简单写一写我对这本书的总结，里面 saga 和测试的部分我就先省略了。
