> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
>  
>   [ ---- Stata连享会 - 往期精彩推文 ----](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



>作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
>&emsp;&emsp;[ …… Stata连享会 - 往期精彩推文 ……](https://www.jianshu.com/p/de82fdc2c18a)
  
## 1. 问题背景
在 [伍德里奇先生的问题：PSM 分析中的配对——小蝌蚪找妈妈](https://www.jianshu.com/p/6c65bb4a8aff) 一文中，我们介绍了几种配对方法。今天，又遇到了一个有点相似但又不完全相同的问题。最终看来，处理方法却迥异。

我们手头有如下数据：
```stata
     +----------------+
     | year   g0    y |
     |----------------|
  1. | 2016    0   10 |
     |----------------|
  2. | 2016    1   20 |
  3. | 2016    1   30 |
  4. | 2016    1   40 |
     |----------------|
  5. | 2017    0   50 |
  6. | 2017    0   60 |
  7. | 2017    0   70 |
     |----------------|
  8. | 2017    1   80 |
  9. | 2017    1   90 |
     +----------------+
```
想得到如下结果（即生成 ym 和 ym_peer 两个新变量）：
```stata
     +-------------------------------+
     | year   g0    y   ym   ym_peer |
     |-------------------------------|
  1. | 2016    0   10   10        30 |
     |-------------------------------|
  2. | 2016    1   20   30        10 |
  3. | 2016    1   30   30        10 |
  4. | 2016    1   40   30        10 |
     |-------------------------------|
  5. | 2017    0   50   60        85 |
  6. | 2017    0   60   60        85 |
  7. | 2017    0   70   60        85 |
     |-------------------------------|
  8. | 2017    1   80   85        60 |
  9. | 2017    1   90   85        60 |
     +-------------------------------+
```
我们需要完成的工作图示如下：
![image.png](https://upload-images.jianshu.io/upload_images/7692714-d388ff8f929354a6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 2. 处理思路
- `ym` 变量：只需借助 `egen` 命令的 **mean()** 函数即可实现；
- `ym_peer` 稍微复杂一些，可以借助虚拟变量 (哑变量) 的 **开关** 功能，以及 `egen` 命令的 **max()** 函数实现数据的扩充。

### 3. 代码及思路展示
```stata
clear
input ///
    year   g0    y  
    2016    0   10  
    2016    1   20  
    2016    1   30  
    2016    1   40  
    2017    0   50  
    2017    0   60  
    2017    0   70  
    2017    1   80  
    2017    1   90  
end

*- ym
bysort year g0: egen ym = mean(y)  
*- ym_peer (一部分)
gen g1 = 1-g0
gen ymxg0 = ym*g0
bysort year: egen ymxg0full = max(ymxg0)
gen ymp0 = ymxg0full*(1-g0)
```
![Stata处理思路图示](https://upload-images.jianshu.io/upload_images/7692714-06e3cb54d0873286.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4. 完整代码
```stata
clear
input ///
    year   g0    y  
    2016    0   10  
    2016    1   20  
    2016    1   30  
    2016    1   40  
    2017    0   50  
    2017    0   60  
    2017    0   70  
    2017    1   80  
    2017    1   90  
end

list year g0 y, sepby(year g0)

bysort year g0: egen ym = mean(y)

gen g1 = 1-g0
gen ymxg0 = ym*g0
bysort year: egen ymxg0full = max(ymxg0)
gen ymp0 = ymxg0full*(1-g0)

gen ymxg1 = ym*g1
bysort year: egen ymxg1full = max(ymxg1)
gen ymp1 = ymxg1full*(1-g1)

gen ym_peer = ymp0 + ymp1

keep y year g0 ym ym_peer
list, sepby(year g0) noobs
```
结果如下：
```stata
. list, sepby(year g0) noobs

  +-------------------------------+
  | year   g0    y   ym   ym_peer |
  |-------------------------------|
  | 2016    0   10   10        30 |
  |-------------------------------|
  | 2016    1   20   30        10 |
  | 2016    1   30   30        10 |
  | 2016    1   40   30        10 |
  |-------------------------------|
  | 2017    0   50   60        85 |
  | 2017    0   60   60        85 |
  | 2017    0   70   60        85 |
  |-------------------------------|
  | 2017    1   80   85        60 |
  | 2017    1   90   85        60 |
  +-------------------------------+
```

>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-d1581b8a4e1ef6c3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")





