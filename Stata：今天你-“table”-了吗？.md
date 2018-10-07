> 作者：朱磊 | 连玉君 | Stata连享会 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))
> &emsp;
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



---
> 毫不夸张的讲， `table` 命令是 stata 中最基础的命令之一。说它基础并不是说这条命令不重要，或是用的少，恰恰相反，它就像空气和水一样，在我们的研究中不可或缺，但却十分容易被忽视。
>
>曾今身边有小伙伴跟我说，他写文章从不 `table` ，更不 `winsor` ，其他的什么 `twoway` 啥的更是想都别想，他一般是在文献里看中了哪个酷炫的模型直接扣之，多扣几篇再加以整合，变量的处理皆为“前人经验”。这样做出的实证结果往往还相当令人满意，很少需要调整，最后论文同样发了一箩筐。因此，他对我等每日因为一个离群值而苦思冥想甚是不解，认为是在浪费生命。
>
>实际上，做学问的套路因人而异，就像擂台上的拳手打法各不相同。这种先有研究方法，再倒逼研究目的的做法，尽管看似投机取巧，但只要能达到最终目的，有时我们也不必苛求。那么，问题来了，如果我们想做一个“老实人”，有了研究目的和研究对象后，再选择研究方法，该怎么做呢？问的好！那你必然是离不开这条 `table` 命令了。

 ## 1. 引言   

 `table` 实际上相当常用，所以会常用，是因为它能在研究的最初就将各个变量观察值的特征简洁的展现出来，包括频数、均值和总额等，从而帮助我们选择研究方法和构建合适的模型，在一定程度上避免出现 “走弯路” 的现象。

先来看看文献中出现的表格长啥样：
> 范例1：徐鸿翔, 张文彬. 国家重点生态功能区转移支付的生态保护效应研究——基于陕西省数据的实证研究[J]. 中国人口·资源环境, 2017(11): 141-148.
>
> ![image](http://upload-images.jianshu.io/upload_images/7692714-7dd9b78cf05d3464.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "QQ截图20180409044237.png")
> 
这是《中国人口·资源环境》上的一篇文献中的表格，完全可以通过 `table` 命令生成后转置表格得到。我们再来看一篇：
>范例2： 赵西亮. 教育、户籍转换与城乡教育收益率差异[J]. 经济研究, 2017(12): 164-178.
>
>![image](http://upload-images.jianshu.io/upload_images/7692714-a06d91f99683b0e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "QQ截图20180409044327.png")

这是《经济研究》上的一篇文献，与表1有些不同，但同样也可以采用 `table` 命令实现。

原来这些牛×哄哄的期刊论文中，也能用我们小小的 `table` 来完成部分内容。此时的你是不是已经按捺不住内心的激动，迫切想知道 `table` 命令到底如何使用了呢？你一定是！

## 2. table 命令简介

`table` 命令主要是用来做列表统计，尤其对于类别变量的统计，使用的更加普遍。`table` 是 stata 的官方命令，无需另行下载。命令格式大致表示为：

```stata
table rowvar colvar supercolvar if in weight, options
```

其中， if, in, weight 为可选项，用的少。 options 中可替换内容较多。一般用得多的有 contents(), format(), center, row, colum 等选项。

* contents() 选项表示的是表格内所填数据为何。统计量名称在前，变量名称在后。要注意的是，当需要呈现多个统计量时，这两个名称需要成对在括号内出现多次。**若不加此选项，stata默认变量统计量为频数。**

* format() 选项表示数据呈现格式。每个符号啥意思不知道没关系，背下来吧: format(%9.2f).

* center 选项表示表内数据居中。

* row 选项表示行末增加 total 统计量。
* colum 选项表示列末增加 total 统计量。

`table` 命令后紧跟的 rowvar, colvar 和 supercolvar 分别表示行变量、列变量和超列变量（你也可以叫别的...），这三个变量可以填一个，也可以填多个。根据填入的变量的个数，可以把 table 表格划分为一维列表、二维列表和三维列表。

### 2.1 一维列表

所谓一维列表就是 table 后面只加了 rowvar 一个变量。以stata官方数据库为例，一维列表使用范例如下：

```stata
sysuse auto, clear
table foreign,   ///
   contents(mean price sd price min price max price n price) ///
   format(%9.2f) center row
```
输出结果如下：
```stata
------------------------------------------------------------------
 Car type | mean(price) sd(price)  min(price) max(price) N(price) 
----------+-------------------------------------------------------
 Domestic |    6072.42    3097.10    3291.00   15906.00        52 
  Foreign |    6384.68    2621.92    3748.00   12990.00        22 
    Total |    6165.26    2949.50    3291.00   15906.00        74 
------------------------------------------------------------------  
```
表内汽车价格为国产车和进口车在各个统计量下的统计值。

### 2.2 二维列表

二维列表是指在 table 后面加入了 rowvar 和 colvar 两个变量。列表不仅在行方向上有变量，在列方向上也有变量控制。二维列表范例如下：

```stata
sysuse auto, clear
table foreign rep78,  ///
    c(mean price sd price min price max price n price) ///
	format(%9.2f) center row col
```
输出结果如下：
```stata
----------------------------------------------------------------------
          |                     Repair Record 1978                    
 Car type |     1         2         3         4         5       Total 
----------+-----------------------------------------------------------
 Domestic |  4564.50   5967.63   6607.07   5881.56   4204.50   6179.25
          |   522.55   3579.36   3661.27   1592.02    311.83   3188.97
          |  4195.00   3667.00   3291.00   3829.00   3984.00   3291.00
          |  4934.00  14500.00  15906.00   8814.00   4425.00  15906.00
          |        2         8        27         9         2        48
          | 
  Foreign |                      4828.67   6261.44   6292.67   6070.14
          |                      1285.61   1896.09   2765.63   2220.98
          |                      3895.00   3995.00   3748.00   3748.00
          |                      6295.00   9735.00  11995.00  11995.00
          |                            3         9         9        21
          | 
    Total |  4564.50   5967.63   6429.23   6071.50   5913.00   6146.04
          |   522.55   3579.36   3525.14   1709.61   2615.76   2912.44
          |  4195.00   3667.00   3291.00   3829.00   3748.00   3291.00
          |  4934.00  14500.00  15906.00   9735.00  11995.00  15906.00
          |        2         8        30        18        11        69
----------------------------------------------------------------------
```

列表中的统计值可解释如：1978 年维修过 1 次的国产车均价为 4564.50美元。需要注意的是，当统计量不止一个时，stata 命令中的统计量书写顺序与列表中呈现的各列顺序要一一对应起来，以免出现错误。

### 2.3 三维列表
三维列表是在二维列表的基础上，进一步加入了 supercolvar 变量。这时的列表已经比较复杂，要注意对行和列各变量含义的把握，构造出符合自身研究需要的列表。三维列表范例提供如下：

```stata
sysuse "nlsw88.dta", clear
table race married coll, ///
      c(mean wage) format(%9.2f) 
```

```stata
------------------------------------------------
          |     college graduate and married    
          | - not college  -    - college grad -
     race |  single  married     single  married
----------+-------------------------------------
    white |    7.93     7.06      11.61     9.70
    black |    5.95     5.79      10.52    12.25
    other |    6.47     7.14      11.71    11.54
------------------------------------------------
```
列表中呈现的是不同种族 (race)、不同学历 (coll_grad) 和不同婚姻状况(married) 下，妇女的平均工资。

### 2.3 四维列表
可以进一步在上述命令中增加 `by()` 选项，以便呈现四维表格：
```stata
table race married coll, by(union) ///
      c(mean wage) format(%9.2f) 
```
输出结果为：
```stata
------------------------------------------------
union     |     college graduate and married    
worker    | - not college  -    - college grad -
and race  |  single  married     single  married
----------+-------------------------------------
nonunion  |
    white |    7.10     6.62      10.74     9.85
    black |    5.34     5.32      10.10     8.81
    other |    7.25     6.74      15.15    15.16
----------+-------------------------------------
union     |
    white |    8.23     7.56      11.70     9.89
    black |    7.95     7.90      10.78    10.68
    other |    5.29     8.49                7.92
------------------------------------------------
```



## 3. 列表输出
实际上，故事到此还并未结束。毕竟我们使用 `table` 命令做出列表的最终目的是运用于我们的论文中。因此，我们还需要将列表从 stata 窗口导出到 word 中。

要实现这一目的，当然，我们可以采用复制粘贴的办法，粘贴到 word 中后再逐步调整到我们需要的样子。然而，为了保证实证分析工作的可重复性，我么还是尽量使用命令来输出表格。

有两个外部命令可以完成这一工作：`logout` 和 `asdoc`。

二者都是外部命令，因此都需要使用 `ssc install` 命令下载安装。以 `asdoc` 为例，我们可以通过 `ssc install asdoc` 或者 `findit asdoc` 来自行下载。

虽然 `asdoc` 在语法上更为简洁，也支持多张表依序放置在一个 Word 文档中，但就本文所列举的实例而言，使用 `logout` 输出结果更为稳定。

下面首先以 **2.3** 小节中介绍的三维列表为例，展示 `asdoc` 的方便快捷 —— 我们只需在制表命令前添加 `asdoc` 命令作为前缀即可：
```stata
asdoc table race married coll, ///
      c(mean wage) format(%9.2f) 
```
如下是输出的 word 表格，显然，表头部分的信息并未得到妥善处理：
![asdoc 输出的三维表格](https://upload-images.jianshu.io/upload_images/7692714-9270968a32ae57ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

事实上，由于本例中表头信息较为复杂，`logout` 也无法很好地处理表头信息：
```stata
logout, save(Table03) word fix(8) replace: ///
table race married coll, ///
      c(mean wage) format(%9.2f) 
```
![logout 输出的三维表格](https://upload-images.jianshu.io/upload_images/7692714-1bd069f9b46c9d77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在这轮对比中，`asdoc` 似乎小胜一筹。然后，对于 **2.2** 小节中介绍的二维表格而言，`asdoc` 的输出结果则惨不忍睹，而 `logout` 则可以通过调整用于控制拆分敏感度的选项 `fix(#)` 得到较为满意的输出效果。
```stata
asdoc ///	
table foreign rep78,  ///
    c(mean price sd price min price max price n price) ///
	format(%9.2f) center row col	
```
![asdoc 输出的二维表格](https://upload-images.jianshu.io/upload_images/7692714-8239a2df5a23cb84.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```stata
logout, save(Table02) word fix(3) dec(2) replace: ///
table foreign rep78,  ///
    c(mean price sd price min price max price n price) ///
	format(%9.2f) center row col
```
![logout 输出的二维表格](https://upload-images.jianshu.io/upload_images/7692714-493cd4ec7fc74e7d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

此时，我们在 `logout` 命令中将选项 `fix()` 里的数值设定为 3，同时设定了选项 `dec(2)` 以保证所有统计量都保留小数点后两位有效数字。当然，此时也有瑕疵：表中第五行 (每个组中的观察值个数) 被遗漏了。

## 4. 其他相关命令

最后，再给大家列举几个与 `table` 命令相关的命令,感兴趣的话自行 `help` 。这些命令各自有自己的特色，但在部分功能上也有所重叠，总之，总有一款适合你。

* `tabulate` 命令：会报告变量所占比重和累积比重，但只能用于一维和二维列表，多维列表的情况无法显示。
* `summarize` 命令：主要用于一维列表的相关统计量的计算。
* `tabstat` 命令：适用范围更加广泛，是生成论文第一张表：变量描述性统计的主要命令。
* `fsum` 命令：快速生成论文中的第一张表，功能比 `tabstat` 更强大，尤其善于处理类别变量的列表统计。



>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)




---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-1aa27dd428ae4ba1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
















































