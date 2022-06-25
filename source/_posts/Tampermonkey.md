---
title: Tampermonkey

time:  20-11

date:  2022-04-17 20:11:23

categories:
- 浏览器
- Tampermonkey

tags: 
- Tampermonkey
---

**注意**

[帐号被盗？卡被盗刷？几行代码演示油猴的恐怖！油猴脚本才是实至名归的超级马槽！](https://www.bilibili.com/video/BV1ZY4y1e7WW?spm_id_from=333.880.my_history.page.click)

油猴得小心使用

<!-- more -->

1. [AC-baidu-重定向优化百度搜狗谷歌必应搜索_favicon_双列](https://greasyfork.org/zh-CN/scripts/14178-ac-baidu-重定向优化百度搜狗谷歌必应搜索-favicon-双列)

**自定义设置**

{% spoiler "" %}

```CSS
/**计数器的颜色样式*/
div .AC-CounterT{
  background: #000000;
}

#wrapper #rs, #wrapper #content_left .result, #wrapper #content_left .result-op, #wrapper #content_left>.c-container {
    background-color: #c9c9c9;
}
#wrapper #content_left>.result[tpl='soft'] .op-soft-title, #wrapper #content_left>.result h3, #wrapper #content_left>.result-op h3, #wrapper #content_left>.c-container h3{
  background-color:#000000;
}
/**百度样式区域**/
body[baidu] {
  position: relative;
   background-color: #161616;
  #wrapper #content_left>.c-container[tpl='soft'] .op-soft-title a,
#wrapper #content_left>.c-container h3 a {
    color: #3476d2;;
    font-weight: bold;
}
#wrapper #content_left>.c-container[tpl='soft'] .op-soft-title a em,
#wrapper #content_left .result h3 a em,
#wrapper #content_left .result-op h3 a em {
    color: #c9c9c9;
    font-weight: bold;
}
#container.sam_newgrid #content_left .result-op,#container.sam_newgrid #content_left .result{margin-bottom:20px;wight:300px;height: 180px;}
  .c-container{
    border-radius: 5px;
    background-color: #c9c9c9; 
    h3{
      background-color: #c9c9c9; 
    }
  }
}
body[google] {
  background-color: #161616;
  #s_lg_img_new{
    display:none !important;
  }
  #rso .g, .vk_c {
    height: 180px;
    background-color: #c9c9c9;
 }
   
  #rso .g:hover {
    background-color: #c9c9c9 !important;
  }
}
```

{% endspoiler %}

**百度**

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650202759600.png)

**谷歌**

![img](https://wangguanjingji.oss-cn-beijing.aliyuncs.com/picture/1650202716288.png)

1. 

Tampermonkey