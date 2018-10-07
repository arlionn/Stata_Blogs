> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) | [github](http://github.com/StataChina))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



**目的：**把 Stata 里的 dofile 打印成 PDF 文档，制作成讲义，方便打印和阅读。

**工具：** 电脑中安装 PDF 虚拟打印机，如 Abode Professional 版本就自带 PDF 虚拟打印机

**步骤：**用 Adobe PDF 虚拟打印机打印 dofile，随后调整 PDF 文档页面，保证适当的页面边距。详情介绍如下：
- **Step 1：打开 Stata  `dofile`** 。可以用鼠标点击 **open** 菜单，也可以用命令，如 `doedit  A1_intro.do`；
- **Step 2：打印 `A1_intro.do` 文档。** 方法为：点击 dofile 窗口第二排第四个按钮 (打印机图标，快捷键为：`Ctrl+P`)。详见`图 1`。特别注意：**打印机名称**选择`Adobe PDF`。若有需要可以进一步单击【属性】按钮设置页面大小、打印颜色等。输出的 PDF 文件为 `A1_intro.pdf`。
- **Step 3： 更改 PDF  文件页面大小。**第二步输出的 PDF 文档页面边距都很小 (如`图 2` 所示)，如果直接打印成纸质讲义无法装订。我们可以自行增加文档边距。(1) 使用 Adobe Professional 打开 `A1_intro.pdf`，依次点击【文档】→【剪裁页面(P)】(快捷键为 `Shift+Ctrl+T`)，弹出界面如`图 3` 所示。注意：(1) 【页面大小】选择`ISOB4` 比较合适；(2)【页面范围】选择`所有页面`。


>![图1：选择 Adobe PDF 虚拟打印机](http://upload-images.jianshu.io/upload_images/7692714-ec8c4f495c2dc5f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![图2：使用 Adobe PDF 虚拟打印机输出的 PDF 文档](http://upload-images.jianshu.io/upload_images/7692714-385c683551c126f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![图3：更改页面大小](http://upload-images.jianshu.io/upload_images/7692714-c81124189fffa32a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 几点说明：

- **编辑 dofile 时的注意事项。** 我们注意到，在`图 2` 中，右侧有一条绿色的竖线，它对应 Stata dofile 的第 74 列 (具体数值决定于你的 dofile 编辑器中的字号设置)。如果你在 dofile 中输入的单行内容超过 74 列，打印时，PDF 虚拟打印机会自动帮你换行，这会影响输出效果。因此，在编写 dofile 时，单行的文字长度不要超过 74 个字符 (包括空格)。

- **同时处理多个 dofile。** 如果有多个 dofile 需要做上述处理，可以先进行第 1-2 步，然后再把这些 PDF 文件合并到成一个 PDF 文档，最后对其进行第 3 步的处理。   
合并多个 PDF 文档的步骤为：依次点击**【文件】→【合并(M)】→【合并到单个PDF(M)】**（参见`图 4`）。

>![图4：合并多个 PDF 文档到单个 PDF 文件](http://upload-images.jianshu.io/upload_images/7692714-d8618263b0caa734.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-9f280d5bcb8d0937.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")


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
