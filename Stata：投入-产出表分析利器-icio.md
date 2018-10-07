----
![](http://image100.360doc.com/DownloadImg/2016/09/2901/81166789_6)
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


![](http://image100.360doc.com/DownloadImg/2016/09/2901/81166789_6)
---



help `icio`  // Analysis of Inter-Country Input-Output tables

### 版本和发布时间
Version 1.1.5 - 16jan2018, beta release

### 命令简介
`icio` allows the analysis of trade in value-added and countries' participation in GVCs, exploiting the Inter-Country Input-Output tables, such as TIVA and WIOD.

It computes countries' value-added in foreign or domestic final demand, also at the sectoral level,      the Koopman, Wang and Wei (2014) aggregate decomposition of exports and the Borin and Mancini (2017)
      bilateral and bilateral-sectoral export decomposition.

A variety of outputs can be obtained using the `output()` option.       Of special note is that the command is flexible enough to be used with user-provided ICIO tables.

It also allows to work with user-defined groups of countries, that is by considering a particular      decomposition or output for the "South Europe" or the "Euro area" group of countries (see the option
      `groups()` for more details on the groups specification).

In order to facilitate its usability, a Stata dialog is included in the package.

### 下载和安装

```stata
ssc install icio, replace
```

### 参考文献
- Borin, A., and M. Mancini (2017).  Follow the value-added: tracking bilateral relations in Global         Value Chains.  MPRA Working Paper, No. 82692.

- Koopman, R., Z. Wang and S. Wei (2014).  Tracing Value-Added and Double Counting in Gross Exports.        American Economic Review, 104(2), 459-94.

- Timmer, M. P., Dietzenbacher, E., Los, B., Stehrer, R. and de Vries, G. J. (2015).  An Illustrated        User Guide to the World Input-Output Database: the Case of Global Automotive Production.  Review        of International Economics, 23, 575–605.



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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-d570411da9ac7b77.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
