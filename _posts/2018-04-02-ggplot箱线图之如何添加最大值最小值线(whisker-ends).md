---
title: ggplot箱线图之如何添加最大值最小值线(whisker-ends)
date: 2018-04-02 23:55:30 +0800
categories: 
- 生信/软件
tags: 
- 技术 
- R
- 画图
---

`ggplot`作为R语言画图的瑞士军刀，相比于基础的R包，语法更加易于理解和掌握，不需要掌握很多的命令就能画出整洁美观的图表。比方说用`ggplot`来画箱线图，首先我们可以这样操作：

<!--more-->

```
library(ggplot2)
library(RColorBrewer)
p <- ggplot(mpg, aes(class, hwy))
p + geom_boxplot(aes(colour=class),width=0.5)+
  theme(panel.background = element_blank(),axis.ticks=element_blank(),legend.position="none",axis.line = element_line(colour="black",arrow=arrow(length = unit(0.20, "npc"))))
```
得到下图
![ggplot的箱线图](http://upload-images.jianshu.io/upload_images/166439-901d0d1857bd5dc5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是在基础R包绘制的箱线图和大部分的箱线图中是这样的
![基础R包的箱线图](http://upload-images.jianshu.io/upload_images/166439-fa581c058200cfa6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
相比较于基础R包的箱线图，`ggplot`的箱线图缺少了最大值和最小值的两条须线（whisker-ends），于是我谷歌后找到了一个[网页](https://stackoverflow.com/questions/12993545/how-to-put-whisker-ends-on-ggplot2-boxplot),参照网页中的操作，我在我的代码中加入了`stat_boxplot`,变成
```
library(ggplot2)
library(RColorBrewer)
p <- ggplot(mpg, aes(class, hwy))
p + geom_boxplot(aes(colour=class),width=0.5)+
  stat_boxplot(geom = "errorbar",width=0.15,aes(color=class))+
  theme(panel.background = element_blank(),axis.ticks=element_blank(),legend.position="none",axis.line = element_line(colour="black",arrow=arrow(length = unit(0.20, "npc"))))
```
![加上须线后的box图](http://upload-images.jianshu.io/upload_images/166439-6994ae7c13f9c28e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
但是这样有一个问题，加上去的须线会挡住我们的box，显得图很乱，后来比照了前面的[网页](https://stackoverflow.com/questions/12993545/how-to-put-whisker-ends-on-ggplot2-boxplot)和我的代码，发现他的`stat_boxplot`在`geom_boxplot`前面，后来我想，之所以我的图会变成这样是因为`ggplot`是以图层的方式进行绘图，即后一个图层会覆盖前一个图层，因此我的代买里面先`geom_boxplot`后`stat_boxplot`导致前面的box被后面的线覆盖掉，如果不想出现这种情况，那就只需要将这两个命令调整一下顺序,修改代码如下：
```
library(ggplot2)
library(RColorBrewer)
p <- ggplot(mpg, aes(class, hwy))
p + stat_boxplot(geom = "errorbar",width=0.15,aes(color=class))+
  geom_boxplot(aes(colour=class),width=0.5)+
  theme(panel.background = element_blank(),axis.ticks=element_blank(),legend.position="none",axis.line = element_line(colour="black",arrow=arrow(length = unit(0.20, "npc"))))
```
![最终修改完的](http://upload-images.jianshu.io/upload_images/166439-e9f8c9a060ab9a3c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最终总结：记住`ggplot`是基于图层的绘图系统。