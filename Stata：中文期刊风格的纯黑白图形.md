>作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
>
>…… Stata连享会 - [ 精彩推文1 ](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md) ……

  

> **致谢：** 在第二届 Stata 用户大会 (广东-顺德，2018/8/19-20) 上，上海财经大学的丁剑平老师提出了这个问题，促使我写了这个推文。特此致谢！


### 导言
Stata 输出的图形带有其标志性的浅蓝色底纹，识别度很高。但有些时候杂志社要求提交的图片不能有任何底纹颜色，要纯黑白图片。此时，可以通过设定绘图模板(**scheme**) 来改变图形的整体风格。


### Stata 官方模板
官方提供的默认绘图模板是 `s2color`，输出效果如下：

```stata
. sysuse "auto.dta", clear  
. twoway scatter mpg weight      
```
![Stata默认彩色模板 s2color 输出效果](https://upload-images.jianshu.io/upload_images/7692714-2d56813eb5a42d05.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Stata 官方提供的黑白模板有两个：`s1mono` 和 `s2mono`，前者的输出效果已经能满足多数中文期刊的要求：
![Stata 提供的黑白模板 s1mono 输出效果](https://upload-images.jianshu.io/upload_images/7692714-624a0a9cdc820518.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Stata 用户提供的模板
网上有很多用户提供的绘图模板，经测试，比较符合多数中文期刊要求的有如下几个，其中尤以 `tufte` 模板风格最佳：
```stata

. twoway scatter mpg weight, scheme(tufte) 
```
![tufte.png](https://upload-images.jianshu.io/upload_images/7692714-1822f0c1a534a13f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

其他几个模板的效果也不错：

![外部模板 burd 的输出效果](https://upload-images.jianshu.io/upload_images/7692714-7077782099c23795.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![外部模板 leand1 的输出效果](https://upload-images.jianshu.io/upload_images/7692714-4b0ea6b53a20a5f5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![外部模板 leand2 的输出效果](https://upload-images.jianshu.io/upload_images/7692714-392ad96893d33bbb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 下载外部模板
- burd 模板
```stata
. ssc install scheme-burd, replace  //burd 模板
```
- lean1, lean2 模板
```stata
. net install gr0002_3.pkg   //纯黑白风格, lean1, lean2 模板
```
- tufte 模板
```stata
. ssc install scheme_tufte, replace //  tufte 模板
```
### 如何查找更多的绘图模板
可以使用 `findit` 命令搜索，根据需要点击下载即可：
```stata
. findit scheme
```
![](https://upload-images.jianshu.io/upload_images/7692714-30ce67bde21622c5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进而点击如下链接以便下载相关文档：

![](https://upload-images.jianshu.io/upload_images/7692714-ac0fc7981cb28572.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当然，也可以直接输入如下命令下载相关文档：
```stata
. ssc install scheme-burd, replace
```

----
### 附：本文完整 dofile
```stata

*----------------------------------
*- Stata：中文期刊风格的纯黑白图形 
*----------------------------------

*  连玉君，2018/8/22 0:00

  
  cd "D:\stata15\ado\personal\Jianshu\scheme_black"
  
  net set other "D:\stata15\ado\personal\Jianshu\scheme_black"

*-下载模板文件

. ssc install scheme-burd, replace
. net install gr0002_3.pkg   //纯黑白风格, lean1, lean2 模板
. ssc install scheme_tufte, replace
. net install gr0070.pkg     //plotplain and plottig
 
*-Stata 范例：
 
. sysuse "auto.dta", clear

*-s2color (Stata 默认模板)

. set scheme s2color 
. twoway scatter mpg weight 


*-tufte

. twoway scatter mpg weight, scheme(tufte) 


*-plotplain
. twoway scatter mpg weight, scheme(plotplain) 

*-plottig
. twoway scatter mpg weight, scheme(plottig) 


*-burd

. twoway scatter mpg weight, scheme(burd)  //黑白背景，彩色前景，极简


*-lean1, lean2

. twoway scatter mpg weight, scheme(lean1) //黑白，不带网格线

. twoway scatter mpg weight, scheme(lean2) //黑白,带网格线,无外框



*--------------
*-输出图形文档

*-s2color
. set scheme s2color 
. twoway scatter mpg weight 
  graph export s2color.png, replace 

*-s1mono
  local t title("scheme(s1mono)")
. twoway scatter mpg weight, scheme(s1mono)  `t'
  graph export s1mono.png, replace   
  
*-tufte
  local t title("scheme(tufte)")
. twoway scatter mpg weight, scheme(tufte)  `t'
  graph export tufte.png, replace 

*-burd
  local t title("scheme(burd)")
. twoway scatter mpg weight, scheme(burd) `t' //黑白背景，彩色前景，极简
  graph export burd.png, replace 

  
*-lean1, lean2

  local t title("scheme(lean1)")
. twoway scatter mpg weight, scheme(lean1) `t' //黑白，不带网格线
  graph export lean1.png, replace 
  
  local t title("scheme(lean2)")
. twoway scatter mpg weight, scheme(lean2) `t' //黑白,带网格线,无外框
  graph export lean2.png, replace 
```


>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
>…… Stata连享会 - [ 精彩推文1 ](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md) ……


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-d41f3e97a109d211.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")









