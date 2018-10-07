> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


#### 此求和非彼求和
实证分析中，进场需要进行加总计算。Stata 中的 `generate` 命令以及更为强大的 `egen` 命令都提供了 `sum()` 函数。然而，需要特别注意的是，二者的功能有很大的差异。先看看如下范例：
```stata
clear
input x
      1
      2
      3
      4
end

 gen sx_gen  = sum(x)
egen sx_egen = sum(x)

list , clean noobs
```
结果如下：
```stata
. list , clean noobs
    x   sx_gen   sx_egen  
    1        1        10  
    2        3        10  
    3        6        10  
    4       10        10  
```
可见，`gen` 提供的 `sum()` 函数执行的是**累积加总**，而 `egen` 提供的 `sum()` 函数则进行**整体加总**。

#### 扩展应用：分组求和
计算各个年度的销售总额 (**sx_egen**)，以及每家公司当年的市场份额 (**sale_per**)：
```stata
clear
input id     year    sale  
      601    2011    0.1
      602    2011    0.2
      601    2012    0.3
      602    2012    0.4
	  603    2012    0.5
end

bysort year:  gen sx_gen  = sum(sale)
bysort year: egen sx_egen = sum(sale)

gen sale_per = sale/sx_egen*100 //市场份额

format sx* sale* %3.1f
list, noobs sepby(year)
```
结果如下：
```stata
. list, noobs sepby(year)

  +-------------------------------------------------+
  |  id   year   sale   sx_gen   sx_egen   sale_per |
  |-------------------------------------------------|
  | 601   2011    0.1      0.1       0.3       33.3 |
  | 602   2011    0.2      0.3       0.3       66.7 |
  |-------------------------------------------------|
  | 601   2012    0.3      0.3       1.2       25.0 |
  | 602   2012    0.4      0.7       1.2       33.3 |
  | 603   2012    0.5      1.2       1.2       41.7 |
  +-------------------------------------------------+
```


>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-4cb5befef4ab08c8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")







