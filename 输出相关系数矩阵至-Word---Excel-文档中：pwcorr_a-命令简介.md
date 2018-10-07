> 作者：王若溪 | 连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) | [github](http://github.com/StataChina))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

### 功能介绍

- Stata 官方命令 `pwcorr` 可用于计算一组变量中两两变量的相关系数，同时还可以对相关系数的显著性进行检验。使用 `pwcorr varlist，sig star(0.01)` 指令还可以将特定的显著性水平（如 0.01）的相关系数进行标注。缺点是仅能对指定的显著水平列印一个 `*`。  

> ![使用stata自带的 auto.dta 文件，需要标注的显著性为0.01](http://upload-images.jianshu.io/upload_images/8666264-d42d276eeb60c3e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 改进命令 `pwcorr_a` 可以分别对 1%，5%，10% 的显著水平列印 `***` ，`**` 和 `*`， 便于输出到论文中。  

> ![ pwcorr_a 可以分别对 1%，5%，10% 的显著水平列印* * * ，* * 和 *](http://upload-images.jianshu.io/upload_images/8666264-2703de685ebd50c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 使用方法

- **Step 1：**   
  进入[【码云•Stata连享会】](https://gitee.com/arlionn) 项目 [【`pwcorr_a`】](https://gitee.com/arlionn/pwcorr_a)，单击 **“克隆/下载”**，出现对话框后点击 **“下载zip”**，下载该项目下的所有文件。

- **Step 2：**  
  将解压后的文件中的 `pwcorr_a.ado` 和 `pwcorr_a` 文件复制到 Stata 文件夹中，`..\stata15\ado\plus\p` 或 `..Stata\ado\base\p`   
  - （视各自电脑情况而定，Stata 的 ado 文件夹中没有`plus`文件夹可以自行建立一个，或直接放在 `base` 文件夹的 `p` 文件夹中）  

![选择 pwcorr_a.ado 和 pwcorr_a](http://upload-images.jianshu.io/upload_images/8666264-e0631a77a04bc000.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![复制到 stata 相关文件夹中，如 Stata\ado\base\p](http://upload-images.jianshu.io/upload_images/8666264-3b2bd301d0940de5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **Step 3：** 在 Stata 命令框中输入 `help pwcorr_a` 指令，了解使用方法。例如：
![输入pwcorr_a varlist指令即可使用](http://upload-images.jianshu.io/upload_images/8666264-2703de685ebd50c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 输出到 Excel 或 Word 文档

可以使用 `logout` 命令将基于 `pwcorr_a` 得到的相关系数矩阵输出到 Excel  或 Word 文档中。命令如下： 
```stata
*-----表2：相关系数矩阵-------
sysuse auto, clear
local v "price wei len mpg" //填入变量名
local s "Table2_corr" //存储的文件名(或路径\文件名)
logout, save("`s'") excel replace: ///
        pwcorr_a `v', format(%6.2f) //star(0.05)
```
点击屏幕上的蓝色链接，可以直接打开生成的 Excel 表格：
> ![pwcorr_a 命令输出的 Excel 文档](http://upload-images.jianshu.io/upload_images/7692714-8a6bea57ebe141be.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")

**说明：**如需输出 Word 表格，只需将上述 `logout ....` 那一行中的 `excel` 选项修改为 `word` 即可。

 
 
-----
#### 补充：Stata 中的 `correlate` 命令和 `pwcorr` 命令有何区别？

- 简言之，如果样本中所有变量都没有缺失值，则 `corr` 和 `pwcorr` 输出的结果相同。
- 否则， `corr` 只针对所有变量都没有缺失值的子样本进行计算；
- 而 `pwcorr` 则针对每一对变量进行计算，因此，每组变量在计算相关系数时使用的样本数可能是不同的。
 

 
 
-----
#### 相关阅读

- Stata 用户发布了一个 -corsp- 命令 (安装命令：`ssc install corsp, replace`)，可以同时输出 Spearman (斯皮尔曼) 和 Pearson (皮尔森) 相关系数矩阵，但只能标注 p-value, 无法标注星号。如下程序可以弥补这一缺憾： [Pearson/Spearman Correlation Matrix using -mata-](https://stackoverflow.com/questions/25211192/combined-pearson-spearman-rank-correlation-matrix-with-significance-stars-in-sta)
- 对于 Stata 15 用户而言，可以使用外部命令 `corr2docx` 快捷地将相关系数矩阵输出到 Word 文档中。我们将在另一篇推文中进行详细介绍。
- [pwcorr_a: stata 相关分析的改进命令](http://bbs.pinggu.org/thread-32761-1-1.html)
- [码云 pwcorr_a](https://gitee.com/arlionn/pwcorr_a/blob/master/README.md)



>#### 关于我们
- 【**Stata 连享会**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

> ### 往期精彩推文

- [Stata小程序: 提取简书文章列表](http://www.jianshu.com/p/76f18c9b96ad)
- [在 Markdown 中使用 HTML 特殊符号](http://www.jianshu.com/p/e65e1e36056b)
- [数据分析修炼历程：你在哪一站？](http://www.jianshu.com/p/7382f9f57605)
- [Stata帮助和网络资源汇总(持续更新中)](http://www.jianshu.com/p/c723bb0dbf98)
- [协整：醉汉牵着一条狗](http://www.jianshu.com/p/08e194e14b7a)
- [在 Markdown 中快速插入文字连接](http://www.jianshu.com/p/ff3b1fa07a97)
- [Stata dofile 转换 PDF 制作讲义方法](http://www.jianshu.com/p/b119033d8b93)
- [Github使用方法及Stata资源](http://www.jianshu.com/p/d2ee76b0f74c)
- [码云：我把常用小软件都放这儿了](http://www.jianshu.com/p/fbb6bdeb9df5)



---
> ![欢迎加入 Stata 连享会](http://upload-images.jianshu.io/upload_images/7692714-f653a01065333210.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
