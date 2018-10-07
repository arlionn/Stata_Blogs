> 作者：顾小龙，连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) | [github](http://github.com/StataChina))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


![What a lovely Stata boy !](http://upload-images.jianshu.io/upload_images/7692714-2a87e7e8d5da5fb3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


> **导言：** 本文汇集了 Stata 相关的经典书籍、教材、论坛和重现论文所需的 Stata 数据和程序(dofiles)。

---
---
---
> Stata 寒假班  **报名中……**
连玉君主讲，`2018年1月13日-21日`，北京）   
[Stata**初级班**](http://www.peixun.net/view/307.html)&ensp;｜&ensp;[Stata**高级班**](http://www.peixun.net/view/308.html)&ensp; | &ensp;[Stata**全程班**](http://www.peixun.net/view/1135.html) 
---
---
---


# 寻求帮助与网络资源  
- 有多种途径可以获得 Stata 的帮助，主要的途径有三个：手册、Stata 自带帮助和网络帮助。
- 对于多数人而言，Stata 手册总有些可望不可及的感觉，但事实上 Stata 帮助中的英文表述都极其简洁明了，范例都很经典，解读也都比较到位，基本上可以挪用到论文的解释中去。
- 平时用的最多的还是 `help` 和 `findit` 命令：前者属于**精确查询**，后者则是**模糊查询**。`help` 的作用在于帮助我们了解一个命令的选项功能和具体用法；而 `findit` 则用于搜索关键词，帮我快速找到解决特定问题或估计某类模型的命令。
- 网上有大量的 Stata 公开课程和学者们公布的 Stata 程序和数据，在本文中都做了细致的筛选和梳理。
  
## 获取帮助的命令  
- `help`  
显示出Stata所有帮助内容的目录结构。  
如果输入具体的命令，则只显示该命令的帮助，如  
```stata  
help summarize  
```
- 也可以通过菜单式的点选方式获得帮助: `Help>>stata command…`在弹出的对话框中输入：summarize，然后回车，得到与whelp summarize同样的结果。  
使用帮助的小窍门：先看命令描述（Description）部分，然后直接看帮助文件后面的命令示例（Examples），将命令示例复制到命令窗口，执行，看看执行结果，体会命令的用法。 

- 网络帮助可以采用`findit`或者`search`命令获得，如：  
```stata
findit ddid
search ddid, net  
```  
- 这两条命令等价，均为寻找多期政策冲击的双重差分命令ddid。由于ddid不是Stata内置命令，所以需要通过这两个命令搜索并下载安装后才能使用。

### 提问前请认真阅读如下资料
- [Stata官网-FAQ](http://www.stata.com/support/faqs/) 提问前请先看看这个
- [Baum 教授的建议](http://fmwww.bc.edu/GStat/docs/GSAfaq.html#sstata)

### Stata 官网资源  
- [Stata官网](http://www.stata.com)  
- [Stata官网-资源链接](http://www.stata.com/links/resources.html) 各种Stata资源链接汇总
- [Stata Blogs](https://blog.stata.com/) Stata官方博客，多为 Stata 公司程序员撰写，偏难
- [Stata出版社](http://www.stata-press.com) 了解 Stata 新书信息
- [Stata Journal](http://www.stata-journal.com/), [SJ中文](http://bbs.pinggu.org/thread-3158333-1-1.html) Stata 最新命令推送和详细范例 

### Stata 论坛
- [Stata list](http://www.statalist.com) Stata官方论坛，有 Stata 公司资深程序员坐镇
- [Stack Overflow](https://stackoverflow.com), 问答质量非常高
- 经管之家(原人大论坛)：[Stata专区](http://bbs.pinggu.org/forum-67-1.html) | [连玉君老师Stata-VIP专区](http://bbs.pinggu.org/forum-114-1.html) | [Stata常见问题](http://bbs.pinggu.org/thread-272681-1-1.html)

### 主要 Stata 在线课程
- [UCLA Stata 在线课程](https://stats.idre.ucla.edu/stata/) 
- [Princeton Stata 在线课程 (Princeton University - Stata Tutorial )](http://www.princeton.edu/~otorres/Stata/)
- [伦敦经济学院 LSE Stata Tutorials](http://www.lse.ac.uk/Methodology/Software-tutorials/Stata-tutorials) 在线视频，提供 dofiles
  
### Stata 书籍
- [Books about Stata](https://www.stata.com/bookstore/books-on-stata/)
- [Stata连享会：Stata教科书资源汇总和**下载**](https://gitee.com/Stata002/LuJiaRui/blob/master/StataBooks/Stata%E6%95%99%E7%A7%91%E4%B9%A6%E8%8C%83%E4%BE%8B%E9%93%BE%E6%8E%A5(Jiarui-%E8%B4%9F%E8%B4%A3).md)
- [Stata examples and datasets](https://www.stata.com/links/examples-and-datasets/), 教科书 Stata 范例和数据
  

### 数据 (Data)  

> 在 Yahoo 中搜索关键词 `stata research data repository`

- [Harvard Dataverse](https://dataverse.harvard.edu/) 学者提供的用于重现论文结果的数据和程序
- [Berkeley University - Health Statistics & Data](http://guides.lib.berkeley.edu/publichealth/healthstatistics/rawdata)   
- [Internet Data Sources for Social Scientists](https://www.ciser.cornell.edu/ASPs/datasource.asp)
- [Open Data Sets by topic - Milne Library Data Collections](http://libguides.geneseo.edu/data)
- [乱七八糟 - 1001 Datasets and Data repositories](https://dreamtolearn.com/ryan/1001_datasets)



---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-b0fc03521cb35eec.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")


>## 关于我们
- 【**Stata 连享会**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
