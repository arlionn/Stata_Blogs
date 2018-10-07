
----
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---

Stata Blog 更新了一条新的博客，介绍了一个将 Stata 表格导入 Excel 或 Word 快捷命令，效果不错！

> Source： [Export tabulation results to Excel—Update](https://blog.stata.com/2018/06/07/export-tabulation-results-to-excel-update/)


---
[Home](https://blog.stata.com/ "Go to homepage") > [Data Management](https://blog.stata.com/category/data-management/) > Export tabulation results to Excel—Update

## Export tabulation results to Excel—Update

7 June 2018  [Kevin Crow, Senior Software Developer](https://blog.stata.com/author/kcrow/ "Posts by Kevin Crow, Senior Software Developer")  || [Go to comments](https://blog.stata.com/2018/06/07/export-tabulation-results-to-excel-update/#disqus_thread)

[Tweet](http://twitter.com/share?text=Export+tabulation+results+to+Excel---Update)

It’s summer time, which means we have interns working at StataCorp again. Our newest intern, Chris Hassell, was tasked with updating my community-contributed command [**tab2xl**](https://blog.stata.com/2013/09/25/export-tables-to-excel/) with most of the suggestions that blog readers left in the comments. Chris updated **tab2xl** and wrote **tab2docx**, which writes a tabulation table to a Word file using the **putdocx** command.

To install or update your **tab2xl** command, type

```stata
net install http://www.stata.com/users/kcrow/tab2xl, replace
```

To install the new **tab2docx** command, type

```stata
net install http://www.stata.com/users/kcrow/tab2docx
```

**tab2xl** now allows *weight*s, **if**, **in**, formatting of the cells, and two-way tabulations. Once installed, you can type

```stata
. sysuse auto, clear
(1978 Automobile Data)
. tab2xl rep78 foreign in 1/50 [fweight=mpg] using testfile, col(1) row(1)
file testfile.xlsx saved
```

to produce

[![](http://upload-images.jianshu.io/upload_images/7692714-f5e34911cf490e7b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://blog.stata.com/wp-content/uploads/2018/06/tab2xl-1.png) 

To write the table to a Word document, you must first open a **.docx** file using the command **putdocx begin**, type your **tab2docx** command to append the table to your file, and then save the document using **putdocx save** *filename*. For example, typing

```stata
. sysuse auto, clear
(1978 Automobile Data)
. putdocx begin
. tab2docx rep78 in 1/50 [fweight=mpg]
. putdocx save testfile.docx
```

will produce

[![](http://upload-images.jianshu.io/upload_images/7692714-ef2368a807fc6a81.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://blog.stata.com/wp-content/uploads/2018/06/tab2docx.png) 

Chris did an excellent job updating **tab2xl** and coding **tab2docx**, making it easier for you to create tables for inclusion in a Word file.


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-ba0b3951f3e8fc5d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
