> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

----
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
---




## 1. 简介
- 强大的 `egen` 和 `egenmore` 命令都只能生成新变量，然而，在很多情况下，我们需要替换已有变量。此时就可以采用 `ereplace` 命令。
- `egenmisc` 提供了几个新函数：`nacorr`, `nacov`， `navar` 可以计算全样本或分组子样本中变量的相关系数、协方差和方差。`pick` 可以挑选出符合条件的观察值。上述命令均支持 `by(varlist)` 选项，这很重要。

## 2. 安装方法
```stata
ssc install ereplace, replace
ssc install egenmisc, replace
```

## 3. 说明文档
- egen 命令详解  [egen (PDF)](https://www.stata.com/manuals14/degen.pdf)

- [egenmore 命令简介和补充范例](http://jearl.faculty.arizona.edu/sites/jearl.faculty.arizona.edu/files/By%2C%20System%20Variables%2C%20Return%20Codes%2C%20Egenmore%2C%20Year%202%20CR.pdf) (功能异常强大)

- [egenmisc 命令的 Github 项目主页](https://github.com/matthieugomez/stata-egenmisc)


## 4. Stata 范例

### 4.1 ereplace 命令
```stata

help ereplace

  sysuse "nlsw88.dta", clear
  
  egen sd_wage = sd(wage)
  list wage sd_wage in 1/10, clean
  ereplace sd_wage = sd(wage), by(industry)
  list wage sd_wage industry in 1/10, clean
```
结果
```stata
.   sysuse "nlsw88.dta", clear
(NLSW, 1988 extract)

.   
.   egen sd_wage = sd(wage)

.   list wage sd_wage in 1/10, clean

           wage     sd_wage  
  1.   11.73913   5.7555229  
  2.   6.400963   5.7555229  
  3.   5.016723   5.7555229  
  4.   9.033813   5.7555229  
  5.   8.083731   5.7555229  
  6.    4.62963   5.7555229  
  7.   10.49114   5.7555229  
  8.   17.20612   5.7555229  
  9.   13.08374   5.7555229  
 10.   7.745568   5.7555229  

.   ereplace sd_wage = sd(wage), by(industry)
(2,246 real changes made)

.   list wage sd_wage industry in 1/10, clean

           wage     sd_wage                 industry  
  1.   11.73913   6.1277447   Transport/Comm/Utility  
  2.   6.400963   5.3684145            Manufacturing  
  3.   5.016723   5.3684145            Manufacturing  
  4.   9.033813   5.1140115    Professional Services  
  5.   8.083731   5.3684145            Manufacturing  
  6.    4.62963   5.1140115    Professional Services  
  7.   10.49114   6.1277447   Transport/Comm/Utility  
  8.   17.20612   5.1140115    Professional Services  
  9.   13.08374   5.1140115    Professional Services  
 10.   7.745568   5.1140115    Professional Services
```

### 4.2 egenmisc 命令

```stata
sysuse  "nlsw88.dta", clear
egen temp1 = fastxtile(hours) if married == 0, by(race) nq(10)
egen temp2 = xtile(hours) if married == 0, by(race) nq(10)
assert temp1 == temp2
drop temp*

egen temp1 = sd(wage) if married == 0, by(race) 
egen temp2 = fastsd(wage) if married == 0, by(race) 
assert float(temp1) == float(temp2)
drop temp*

sort race
egen temp1 = corr(hours wage) if married == 0, by(race) 
egen temp2 = fastcorr(hours wage) if married == 0, by(race) 
assert temp1 == temp2 if !missing(hours) & !missing(wage)
drop temp*
```




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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-3624e5d3649127b2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
