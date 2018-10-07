
> 作者：华晨 (The University of Manchester) | ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

> **编者注：**在处理纵横变换数据时，Stata 官方提供的 `reshape` 命令十分便捷，但运算速度相对较慢。此时，可以使用外部命令 `sreshape` 或 `fastreshape` 来转换大型数据，速度可以提升 5-30 倍。本文介绍的 `gather` 和 `spread` 命令则可以处理上述命令都无法很好解决了的一类问题。

"扁担长，板凳宽，板凳没有扁担长，扁担没有板凳宽。"长长宽宽绕不清。一些 Stata 的初学者也绕不清长宽数据的转换。应该是`reshape long`呢？还是`reshape wide`呢？`i`又是什么？`j`又是什么？……

头昏眼花了对不对！的确，Stata 官方的 `reshape` 命令在使用的时候会有不便。比如, `reshape long` 会要求被转置的变量名要有相同前缀，诸如此类。那有没有更 (jian) 人 (dan) 性 (cu) 化 (bao) 的方法呢？当然有啦，不然这篇推文的意义何在。

在介绍更简单的长宽转换命令前，初学者需要明白什么叫长宽转换。简单地说：
**原来横着的行，你却想把它竖起来，这就是“宽变长”；**
**原来竖着的列，你却想把它横过来，那就是“长变宽”。**

### 神奇的  `gather` 和 `spread` 命令

说了半天，是什么神奇的命令呢？灯灯灯等！是 `gather` 和 `spread` 。`gather` 负责宽变长，`spread` 负责长变宽。要感受它们的神奇，你首先得安装它们，执行以下命令：
```
ssc install tidy
```
接下来就直接来看例子吧。

---
首先我们调入一个 Stata 自带的发达国家教育经费占 GDP 比重的数据。它包含了 3 个变量和 10 个观察值。第一列是国家名称，第二列是公共部门教育花销占 GDP 的比重，第三列是私有部门教育花费占 GDP 的比重。
```
sysuse educ99gdp.dta, clear
```
![](http://upload-images.jianshu.io/upload_images/9797551-93cecf14d79c611b.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我们想把这个宽数据变长，也就是让每一个国家的公共部门教育花费占比和私有部门教育花费占比以不同观察值来呈现，通俗地讲就是各处一行。那么，整个数据集将扩充成 20 个观察值。执行以下命令：
```
gather public private
```
对于一个国家（观测值）来说，public 和 private 各部门的取值本来是横着的，我们想让它们竖起来，就在`gather`后面指定想要竖起来的变量名就可以了。

执行完后数据集变成什么样子了呢？
![](http://upload-images.jianshu.io/upload_images/9797551-92a010ce781eebd5.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

的确变成了一个含有20个观察值的数据。第一列仍然是国家名称，如预期一般，每个国家出现了两次。第二列是自动生成的变量 variable ，它其实是将我们指定的原先横着的变量给竖起来了，并将原来的变量名作为新变量的取值来标识这一行。第三列也是自动生成的变量 value，它就是教育花费占比的取值。

是不是很简 (cu) 单 (bao) ？

当然我们还可以自定义 variable 和 value 两个变量的名称，使得新数据的变量名有更清晰的含义。执行以下代码：
```
sysuse educ99gdp.dta, clear
gather public private, variable(sector) value(GDP)
```
我们得到如下数据
![](http://upload-images.jianshu.io/upload_images/9797551-ffb7ed4fd1986608.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注意到，两个新生成的变量名称改变成我们自定义的名称了。

有人要说了，想变回去怎么办呢？长变宽？ 执行以下命令：
```
spread sector gdp
```
![](http://upload-images.jianshu.io/upload_images/9797551-eed79982adc09145.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
又变回了最初宽数据的形态。是不是很 easy？

这里需要注意的是`spread`命令中指定的变量名的顺序。 其实，就我的理解，长宽转换的本质是一个类别变量的取值在二维表里如何呈现的问题：如果将类别变量的各个类别取值放在变量名里，则是一个宽数据；如果将类别变量的各个取值放在同一个变量里，则是一个长数据。

`spread`后面指定的第一个变量名一定是那个类别变量。建议这个类别变量使用字符型变量（红色的）。如果是数值型变量但是加了变量取值标签（蓝色的），生成的新变量的名称含义就不是那么直接了，有兴趣的胖友可以自己试一下。

最后，还在 `reshape`上挣扎的胖友们，还不赶紧试试？


### 样例代码

```stata
cls
cap ssc install tidy

*- Wide to long
   sysuse educ99gdp.dta, clear
   gather public private

*- Wide to long with renaming new variables
   sysuse educ99gdp.dta, clear
   gather public private, variable(sector) value(gdp)

   
*- Long to wide
   
   * Create the long data set first
     sysuse educ99gdp.dta, clear
     gather public private, variable(sector) value(gdp)
   
   * Reshape long to wide
     spread  sector gdp
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

- Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-487cca57dccbc571.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
