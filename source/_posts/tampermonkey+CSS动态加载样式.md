---
title: tampermonkey+CSS动态加载样式
time:  13-12
date:  2022-04-17 13:12:36
categories:
- 浏览器
- tampermonkey

tags: 
- tampermonkey
---

 tampermonkey+CSS动态加载样式

 tampermonkey+js不会，

在别人的基础上修改些CSS样式

<!-- more -->

参考[【JavaScript+CSS+tampermonkey】动态加载样式——还你一个干净无广告的CSDN博客](https://blog.csdn.net/pangji0417/article/details/94029462?ops_request_misc=&request_id=&biz_id=102&utm_term=tampermonkey%20%E6%8F%92%E5%85%A5%E4%B8%80%E6%AE%B5css&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-94029462.142%5Ev9%5Econtrol,157%5Ev4%5Econtrol&spm=1018.2226.3001.4187)

幸亏一下就找到了这篇文章文章，所以以后一些东西记到博客上，方便找。

```js
(function () {
  	/* 这里编写css代码 */在下面加入这段代码
    var style = document.createElement("style");
    style.type = "text/css";
    /* 这里编写css代码 */
    var text = document.createTextNode(".video-info-v1 .video-title { color: #27ff00;}");
    style.appendChild(text);
    var head = document.getElementsByTagName("head")[0];
    head.appendChild(style);
```



