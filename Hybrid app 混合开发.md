Hybrid app 混合开发

​	网页和原生的结合

1.原生主导

​	原生开发为主，web为辅，像微信，JD

​	原生为主，做主要的框架，其中的某些模块使用h5来编写，原生会提供一个类似于妹纸浏览器的东西来访问这个h5页面 webview

混合开发的一些工具：

​	ponegap

​	Cordova

​	React Native（非真正的混合开发，实际上是转化了原生的应用）

​	Dclound

​	Electron

2.h5主导

​	直接开发网页，原生提供js接口，让网页去调用，完了之后，用原声的工具进行打包

Native app 

​	IOS:Objective  C/ Swift

​	Android: java

​	缺点： 开发周期长，开发不灵活需要维护多套代码

​	优点： 用体验很好，因为调用原生，更流畅

Web app

​	优点： 开发快，足够灵活，只需要维护一套代码(html/css/javascript)

​	缺点： 早期没办法直接调用原生的接口，比如点位之类的，借助于h5的功能

PWA 渐进式增强web应用可以有部分原生的调用能力，很多api还得基于https