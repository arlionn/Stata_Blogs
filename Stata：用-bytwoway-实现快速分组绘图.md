> 作者：卢家锐 | 连玉君 | ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


&emsp;


### 1. 分组绘图的用途
一般我们在检验交互（调节）作用的时候，一般选择纳入交乘项或者分组进行系数差异性检定判断，然而还有第三种更加直观的方法就是通过分组绘图的方法，有的时候你可以在还没有进行实证前通过分组绘图的方法就可以直接征服论文评审，说明这确实是存在调节作用的。下面举几个分组绘图在国内外期刊中应用的例子：

> 范例 1 ：Ashiq Ali, Lee-Seok Hwang, Mark A. Trombley. Arbitrage Risk and the Book-to-market Anomaly [J]. Journal of Financial Economics, 2003, 69(2):355-373.  
&emsp;
> **解读：** 横轴表示投资组合建立后的时间(月度)，纵轴表示相应投资组合经规模调整后的累计回报率。可以非常明显的看出，波动率大的 (G1) 投资组合的累计回报率明显高于波动率小的 (G2) 投资组合。
![](http://upload-images.jianshu.io/upload_images/8580278-1b7193ff4fe07de0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

&emsp;


 > 范例 2 ：刘柏, 郭书妍. 董事会人力资本及其异质性与公司绩效[J]. 管理科学, 2017, 30(3):23-34.
&emsp;
>![](http://upload-images.jianshu.io/upload_images/8580278-841fc1153c4f0687.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

&emsp;


> 范例 3 ：汝毅, 郭晨曦, 吕萍. 高管股权激励、约束机制与对外直接投资速率[J]. 财经研究, 2016, 42(3):4-15.
&emsp;
![](http://upload-images.jianshu.io/upload_images/8580278-123e0578cb750722.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


&emsp;

> 范例 4 ：李绍龙, 龙立荣, 贺伟. 高管团队薪酬差异与企业绩效关系研究:行业特征的跨层调节作用[J]. 南开管理评论, 2012, 15(4):55-65.
&emsp;
>![](http://upload-images.jianshu.io/upload_images/8580278-075a645498b56f38.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

&emsp;

> 范例 5 ：李维安, 孙林. 同乡关系在晋升中会起作用吗?——基于省属国有企业负责人的实证检验[J]. 财经研究, 2017, 43(1):17-28.
&emsp;
![](http://upload-images.jianshu.io/upload_images/8580278-71de205c3a382153.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


&emsp;

### 2. bytwoway 命令和官方命令 twoway 的区别

- 当分组变量只有 1 个并且分组类别为 2 ，两种命令在绘图几乎没有区别，但是 `bytwoway` 命令的语法更为简洁，不需要根据分组变量类别每一组都重复，一个 `by( )` 选项就解决了。下面举例说明：

**用 `twoway` 命令根据分组变量 foreign 绘图**
```stata
sysuse auto.dta, clear
set scheme m2color  // 设定绘图模板为「彩色模板」
twoway (line price weight if foreign==1, sort) ///
       (line price weight if foreign==0, sort)
```
效果如下：
>![](http://upload-images.jianshu.io/upload_images/8580278-03bfd0c5f40df38b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**用 `bytwoway` 命令根据分组变量 foreign 绘图**
```stata
sysuse auto.dta, clear
collapse (mean) price, by(weight foreign)
bytwoway (line price weight), by(foreign) aes(color lpattern)
```
效果如下：
>![](http://upload-images.jianshu.io/upload_images/8580278-d04103211007d912.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**特别提示：** 当分组变量大于 1 个或者分组类别为 2 时，官方命令 `twoway` 不再适用，而只能用 `bytwoway`。

### 3. bytwoway命令的具体操作

`bytwoway` 是一个在实现快速分组绘图上相当便捷的命令，可以适用于对数据进行任意分组的情形，不同组别的颜色和类型可以通过选择项 `aesthetics` 来设置。
 
#### 3.1 `bytwoway` 命令安装

```stata
net install bytwoway, ///
    from(https://github.com/matthieugomez/stata-bytwoway/raw/master)
```

#### 3.2 `bytwoway` 命令举例

> 范例1：根据单个变量分组绘图

- 根据分组变量 race 绘图
```stata
sysuse nlsw88.dta, clear
collapse (mean) wage, by(grade race)
bytwoway (line wage grade),   ///
         by(race) aes(color lpattern)
```

>![](http://upload-images.jianshu.io/upload_images/8580278-d952311341dea00d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 根据分组变量 race 绘图，改变了分组线条的类型
```stata
bytwoway (scatter wage grade, connect(l)), ///
         by(race) aes(color msymbol)
```
>![](http://upload-images.jianshu.io/upload_images/8580278-1b9a7e57ae9d0223.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 根据分组变量 race 绘图，改变了分组线条的颜色类型
```stata
bytwoway line wage grade, by(race) aes(color) ///
   colors("248 118 109" "0 186 56"  "97 156 255")
```
>![](http://upload-images.jianshu.io/upload_images/8580278-00e49895f61e9d65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**说明：**  
在上述命令中，我们使用了 `palette` 等用于改变分组线条的颜色的选项，这些选项需要安装 `colorscheme` 命令包才可以使用。安装命令如下：
```stata
net install colorscheme, /// 
from(https://github.com/matthieugomez/stata-colorscheme/raw/master/)
```

&emsp;

> 范例2：多变量分组绘图

- 根据分组变量 smsa 、race 绘图
```stata
sysuse nlsw88.dta, clear
collapse (mean) wage, by(grade smsa race)
bytwoway line wage grade, by(smsa race)
```
>![](http://upload-images.jianshu.io/upload_images/8580278-13d15e028247cdca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 根据分组变量 smsa 、race 绘图，改变了分组线条的颜色深浅
```stata
bytwoway line wage grade, by(smsa race)  ///
         aes(color) palette(GnBu)
```
>![](http://upload-images.jianshu.io/upload_images/8580278-c6962c107e620bc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
&emsp;

&emsp;


>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)
>
>![连玉君Stata现场班报名中](https://upload-images.jianshu.io/upload_images/7692714-78fa5fece25aa2fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-58cb1227decc15f7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")


