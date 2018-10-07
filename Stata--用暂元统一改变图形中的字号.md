> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

> 在绘制一些复杂图形时，我们希望统一放大或缩小文字的字号。由于 Stata 图形中不同元素的默认字号不同 (例如，标题和坐标刻度的字号就存在很大差异)，若逐个设定会导致图形中的字体字号不协调。
>
>此时，使用相对字号会非常方便，基本用法就是使用诸如 **size(\*0.8)** 的形式，使字号缩小为默认字号的 80%。
> 
> 为了能批量修改图形中所有元素的字号，使用暂元 (`local`) 会非常方便。

### Stata 范例
第 **6** 行的命令是关键，我们通过一个暂元 `size` 来统一设定**标题**、**坐标刻度**等元素的字号。这里使用的是相对字号：`"size(*0.8)"`，其含义是，所有字号都设定为默认字号的 0.8 倍。显然，如果设定 `"size(*1.2)"`，便是将默认字号放大 1.2 倍。
```stata
1	. sysuse "nlsw88.dta", clear
2	
3	. replace industry = 2000 + industry
4	. local x1 "wage hours"
5	. local x2 "industry"
6	. local size "size(*0.8)" // 字体缩小程度
7	
8	#d ;
9	. graph bar `x1', stack 
10	   over(`x2', label(angle(60) lab`size')) 
11	   legend( label(1 "July") label(2 "January") `size')
12	   ytitle("Degrees Fahrenheit", `size')
13	   ylabel(, lab`size')
14	   title("Average July and January temperatures", `size')
15	   subtitle("by regions of the United States", `size')
16	   note("Source: U.S. Census Bureau, U.S. Dept. of Commerce") ;
17	#d cr
18    . graph export "Fig01.png", replace 
```
- 输出效果 ：**size(\*0.8)**
> ![字号为默认字号的 80%](https://upload-images.jianshu.io/upload_images/7692714-7829be3fa2a514b8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 输出效果 ：**size(\*1.0)**
>![默认字号](https://upload-images.jianshu.io/upload_images/7692714-06c52ab47913a7d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 相关链接：[Stata：让图片透明——你不要掩盖我的光芒](https://www.jianshu.com/p/e061f73c2a4f)

---
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [CSDN-Stata连享会](https://blog.csdn.net/arlionn) 、[简书-Stata连享会](http://www.jianshu.com/u/69a30474ef33) 和 [知乎-连玉君Stata专栏](https://www.zhihu.com/people/arlionn)。可以在上述网站中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
- [Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
- Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-882df304c100d8ab.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")




