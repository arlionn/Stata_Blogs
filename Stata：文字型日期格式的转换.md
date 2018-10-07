> 作者：Stata连享会 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



### 问题背景
导入数据后，日期变量显示为红色，是文字变量。我们需要将其转换成黑色，即数值变量。以下是待处理数据。
![待处理的文字型日期变量](https://upload-images.jianshu.io/upload_images/7692714-71eb3f33ebe009ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 处理方法 1：Stata官方命令 date() 函数
- **两步：**
   - 第一步，使用 `date()` 函数将文字变量转化为日期变量；
   - 第二步，使用 `format` 命令定义日期显示格式；
```stata
  gen date_back1 = date(date_str, "MDY")
  format date_back1 %td //定义日期显示格式
```
- **解释：** 完成转换后的 `date_back1` 变量中存储的日期数值并不是你预期的 `01/29/1960` 或  `29jan1960` 样式，而是数值 **28**。事实上，1960 年 1 月 29 日距离 1960 年 1 月 1 日刚好是 28 天。这就是 Stata 中标记日期的规则：以 **1960 年 1 月 1 日** 为基准日！
```stata
  gen date_back1 = date(date_str, "MDY")
  list date* in 1/5, clean abb(20)
```
![date()转换后的数据](https://upload-images.jianshu.io/upload_images/7692714-8ae59ed938077b1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
因此，我们只需设定其显示格式，即可让数值 **28** 显示为常规的日期格式：
```stata
  format date_back1 %td //定义日期显示格式
  list date* in 1/5, clean abb(20)
```
![定义显示格式后的日期变量](https://upload-images.jianshu.io/upload_images/7692714-2db3916696b58888.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 处理方法 2：外部命令 todatetime  

- **安装**
```stata
net install todatetime,  ///
    from(https://raw.githubusercontent.com/mcaceresb/stata-todatetime/master/)
```

- Stata 范例：
```stata
todatetime datestr, gen(date_back2) datefmt(MDY)
format dateback2 %td
```

![两种方法对比](https://upload-images.jianshu.io/upload_images/7692714-b9a91db1d8698c90.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 附：原始 dofile
```stata
*--------------------------- 
*-伪造一份文字型的日期变量
*---------------------------
  sysuse gnp96, clear
  tostring date, format(%td_NN/DD/CCYY) gen(date_str) force
  list date date_str in 1/5, clean
  

*----------
*-开始转换
*----------

*-Stata官方命令 date() 函数
  gen date_back1 = date(date_str, "MDY")
  list date* in 1/5, clean abb(20)
  
  format date_back1 %td //定义日期显示格式
  list date* in 1/5, clean abb(20)
  
  
*-外部命令：todatetime  

*-install
net install todatetime,  ///
    from(https://raw.githubusercontent.com/mcaceresb/stata-todatetime/master/)

*-Stata 范例：
todatetime date_str, gen(date_back2) datefmt(MDY)
list date* in 1/5

format date_back2 %td
list date* in 1/5
```


>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [CSDN-Stata连享会](https://blog.csdn.net/arlionn) 、[简书-Stata连享会](http://www.jianshu.com/u/69a30474ef33) 和 [知乎-连玉君Stata专栏](https://www.zhihu.com/people/arlionn)。可以在上述网站中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- [……Stata 连享会精彩推文……](https://blog.csdn.net/arlionn/article/details/82746992)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-99e0f080f41af585.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

