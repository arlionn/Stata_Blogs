
>译者：谢作翰 | 连玉君 |  ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))   
> &emsp;
> 原文链接：[Princeton Stata 在线课程 (Princeton University - Stata Tutorial )](https://link.jianshu.com/?t=http%3A%2F%2Fwww.princeton.edu%2F%7Eotorres%2FStata%2F)
> &emsp;
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



### 专题链接
- [普林斯顿Stata教程 - Stata做图](http://www.jianshu.com/p/069bb26ad842)
- [普林斯顿Stata教程 - Stata数据管理](http://www.jianshu.com/p/f0aa1d0bcf79)
- [普林斯顿Stata教程 - Stata编程](http://www.jianshu.com/p/916ddc3948d6)

## 目录
- 2.1 散点图
  - 2.1.1简单的散点图
  - 2.1.2 拟合线
  - 2.1.3 点标签
  - 2.1.4 标题，图例和说明
  - 2.1.5 轴标尺和标签
- 2.2 线图
  - 2.2.1 简单的线图
  - 2.2.2 标题和图例
  - 2.2.3 线条样式
  - 2.2.4 标度选项
  - 2.2.5 图形方案
- 2.3 其他图形
  - 2.3.1条形图
  - 2.3.2 箱线图
  - 2.3.3 核密度估计
- 2.4 图形管理

---


Stata拥有出色的图形功能，可通过`graph`命令，`help graph`了解详情。统计中最常见的图表是显示点或线的双坐标轴X-Y图。这可以通过子命令`twoway`实现。`twoway`命令中又含42个子命令及绘图类型，其中最重要的是`scatter`和`line`。我们将对`scatter`和`line`着重介绍，并简要介绍其他绘图类型。

Stata 10引入了一个图形编辑器，可用于交互式地修改图形。然而，我不会提倡这种做法，因为它与记录和确保研究中所有步骤可重复的目标相冲突。

本节中的所有图表（除非另有说明）都使用带蓝色标题和白色背景的自定义方案，我将在第 2.2.5 节对方案进行讨论。文末附有原文链接。

## 2.1 散点图

在本节中，我将使用前文使用过的有关生育率下降的`effort`数据集进行图表说明。读取数据：
```stata
infile str14 country setting effort change  ///
using http://data.princeton.edu/wws509/datasets/effort.raw, clear 
```
为了激发你的兴趣，先展示我们将在本节中完成的作品：
![本节成果](https://upload-images.jianshu.io/upload_images/1844375-26bfa20dac31ff17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.1.1 简单的散点图

可以使用以下命令生成生育率变化（change）与社会环境（setting）关系的简单散点图：
```stata
graph twoway scatter change setting
``` 
请注意，首先指定的变量是在Y轴。如果变量有定义标签，则坐标轴显示变量标签名，若无定义则显示变量名。如果这是唯一的图，该命令可以缩写为`twoway scatter`，或者`scatter`。现在我们将添加一些东西。

![简单散点图](https://upload-images.jianshu.io/upload_images/1844375-607195bce5bd64f1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.1.2 拟合线

**拟合线和散点图**  
假设我们想显示拟合的回归线。在某些软件包中，你需要先进行回归，计算拟合线，然后对其进行绘制。Stata中可以使用`lfit`绘图类型一步完成所有操作（还有一个二次拟合绘图类型`qfit`）。通过将每个子图封闭在括号内，可以将它与散点图结合使用（也可以使用两条竖线来分隔它们）。
```stata
graph twoway (scatter setting effort)  (lfit setting effort)
```
![拟合线和散点图](https://upload-images.jianshu.io/upload_images/1844375-66f126a25716e4e2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**置信区间带**   
现在假设我们想在回归线上显示置信区间。Stata可以通过`lfitci`来实现这一点，该绘图类型将置信区域绘制为灰色带。（还有一个`qfitci`类型用于二次拟合的频带。）因为置信带会遮蔽一些点，所以我们先绘制该区域再绘制点
```stata
graph twoway (lfitci setting effort)   (scatter setting effort)
``` 
![显示置信区间带](https://upload-images.jianshu.io/upload_images/1844375-7c329416d9bc9f36.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

请注意，该命令不会标记 y 轴，而是使用图例。你可以使用 `ytitle()` 选项为y轴指定标签，并隐藏图例 `legend(off)`：
```stata
twoway ///
  (lfitci setting effort) ///
  (scatter setting effort), ///
   ytitle("Fertility Decline") legend(off)
```       
 ![指定 Y 轴隐藏图例](https://upload-images.jianshu.io/upload_images/1844375-b8f53061e1d1358a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.1.3 点标签

有很多选项可以让你对点标签进行控制，包括它们的形状和颜色，参见`help marker_options`。使用`mlabel(varname)`选项也可以用变量的值标记点。在下一步中，我们将国名添加到图中：
```stata
twoway (lfitci change setting) 
       (scatter change setting, mlabel(country)) 
```
![点标签](https://upload-images.jianshu.io/upload_images/1844375-9c95db1147c6615d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

标签中的一个小问题是哥斯达黎加和特立尼达多巴哥（以及巴拿马和尼加拉瓜）相互重叠。我们可以使用12小时时钟指定标签相对于标记的位置来解决这个问题（12是上方，3是右方，6是下方，9是在标记的左边）。
我们创建一个变量，将默认设置的位置保持为3点，然后将哥斯达黎加移动到9点，特立尼达多巴哥移动到11点以上的位置（我们也可以将尼加拉瓜和巴拿马上移位，到2点方向）：
```stata
gen pos=3
replace pos = 11 if country == "TrinidadTobago"
replace pos = 9 if country == "CostaRica"
replace pos = 2 if country == "Panama" | country == "Nicaragua"
```
生成此版本图形的命令如下
```stata
graph twoway ///
  (lfitci change setting)  ///
  (scatter change setting, ///
   mlabel(country) mlabv(pos) ) 
```
![去重叠](https://upload-images.jianshu.io/upload_images/1844375-1ecf52fc466bcefa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.1.4 标题\图例和说明

有些选项适用于所有双向图形，包括标题，标签和图例等。Stata图表的`title()` 和 `subtitle()` 通常在顶部，`legend()`，`note()` 和 `caption()` 通常在底部，更多信息键入 `help title_options`。通常你只需了解标题即可。Stata 11 允许图形中的文本包括粗体，斜体，希腊字母，数学符号和字体选择。Stata 14 引入了 Unicode，大大扩展了可以完成的工作。`help graph text` 以了解更多信息。

我们对图表的最后调整是添加一个图例来指定线性拟合和 95％ 置信区间。我们使用 `order(2 "linear fit" 1 "95% CI")` 命令，图例的选项按照该顺序标记第二个和第一个项目。我们还使用`ring(0)`将图例移动到绘图区域内，并使用 `pos(5)` 将图例框放置在5点钟位置附近。完整命令就是：
```stata
#d ;
. twoway 
 (lfitci change setting) 
 (scatter change setting, mlabel(country) mlabv(pos)) 
  , title("Fertility Decline by Social Setting") 
    ytitle("Fertility Decline") 
    legend(ring(0) pos(5) order(2 "linear fit" 1 "95% CI")) ;
#d cr
. graph export fig31.png, width(500) replace                     
```

结果就是本节开始处显示的图形。

### 2.1.5 轴标尺和标签

有一些选项可以控制轴的缩放比例和范围，包括`xscale()`和`yscale()`。可以是算术，对数值等。更多信息`help axis_scale_options`。其它选项控制主要和次要记号和标签，如`xlabel()`,` xtick()` and `mtick()`，同样地，对于y轴，见`help axis_label_options`。

## 2.2 线图

下面将使用美国预期寿命数据集来说明线图，这是数据Stata附带的数据集之一（试试`sysuse dir`看看还有什么可用的）。
```stata
. sysuse uslifeexp ,clear
(U.S. life expectancy, 1900-1999)
```
我们的目标是绘制20世纪美国白人和黑人男性的预期寿命。为了激发你的兴趣，将先向你展示最终成果，然后我们将一点一点地构建图表。

![美国预期寿命](https://upload-images.jianshu.io/upload_images/1844375-248ef7957291c261.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2.2.1 简单的线图

最简单的图形是所有参数使用默认值：
```stata
twoway line le_wmale le_bmale year
``` 
![默认参数做图](https://upload-images.jianshu.io/upload_images/1844375-c8a26a59ce43b401.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果这就是我们所要的图形，可以将命令缩写为`twoway line`，或者`line`（只适用于散点图和线图）。

**线图允许我们指定多个 “y” 变量**，顺序为y1，y2，...，ym，x。本例中，我们指定了两个，分别对应白人男性和黑人男性的预期寿命。我们也可以使用两条线图：
```stata
twoway (line le_wmale year) (line le_bmale year)
```

### 2.2.2 标题和图例

默认图形很好，但图例似乎太罗嗦，接下来我们要将大部分信息转移到标题中，只保留肤色信息：
```stata
graph twoway line le_wmale le_bmale year,  ///
  title("U.S. Life Expectancy")  ///
  subtitle("Males") ///
  legend( order(1 "white" 2 "black") )
```

在这里，我使用了三个选项：`title`，`subtitle`和`legend`。`legend`选项有许多子选项; 此处用`order`列出关键点（即**1**和**2**）及其标签，说明第一条线代表白人，第二条线代表黑人。要省略关键点，只需将其从列表中移除即可。其他的图例选项，请参阅`help legend_option`。
![图例信息转移](https://upload-images.jianshu.io/upload_images/1844375-0657575187728e0f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下面我希望在画图区域内移动图例来改善空间，比如说在5点钟左右的位置有空余空间。如前所述，我们可以通过使用`ring(0)` 将图例移动到绘图区域内，并通过`pos(5)`将其置于5点钟位置附近。因为这些都是图例子选项，所以都在`legend()`命令括号中输入：
```stata
graph twoway line le_wmale le_bmale year,  ///
   title("U.S. Life Expectancy") ///
   subtitle("Males")  ///
   legend( order(1 "white" 2 "black") ring(0) pos(5) )
```
![](https://upload-images.jianshu.io/upload_images/1844375-4ab2b361dc04dc71.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.2.3 线条样式
我不知道你感觉如何，但我自己很难区分图画中的默认线条。Stata中有不同的方式控制线条样式。`clstyle()`选项可以让你使用已命名的不同风格，比如`foreground`，`grid`，`yxline`，或是根据线1~15使用样式命名的`p1-p15`，详情请参阅`help linestyle`。如果您想根据方案选择合适样式元素，这非常有用。

您也可以指定样式的三个成分从而确定风格：线条样式，宽度和颜色：
- 线条样式由 `clpattern()` 选项指定。最常见的模式是 `solid`，`dash` 和`dot`, 查看 `help linepatternstyle` 获取更多信息。
- 线宽由 `clwidth()` 指定，可用的选项包括 `thin`，`medium` 和 `thick`，详情参见 `help linewidthstyle`。
- 颜色由 `clcolor()` 指定，使用颜色名称（如 `red`，`white` 和 `blue` ）或 RGB 值确定颜色，请参阅 `help colorstyle`。

我们将白人指定为蓝色，黑人指定为红色：
```stata
graph twoway  ///
   line le_wmale le_bmale year, clcolor(blue red), ///
   title("U.S. Life Expectancy") ///
   subtitle("Males")  ///
   legend( order(1 "white" 2 "black") ///
           ring(0) pos(5))
``` 
![指定线条颜色](https://upload-images.jianshu.io/upload_images/1844375-9007fe7839010fb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

请注意，这里 `clcolor()` 是线图的一个选项，所以我将括号放在`line`命令的周围并把 `clcolor()` 插入那里。

### 2.2.4 标度选项
由上图我们可以看出，预期寿命的提升速度在20世纪下半叶有所减缓。使用对数刻度可以更直观的理解，需要注意的是对数刻度中直线表示恒定的改善幅度。这由`help axis_options`可以很容易完成。尤其是`yscale()`，它可以让你选择算数（`arithmetic`），对数（`log`）或倒置刻度（`reversed`）。其中倒置刻度是指y轴是从最大的值开始的，最小值反而在最上方。还有一个子选项`range()`可以控制绘图范围。在这里，我将y范围指定为25到80，以便将曲线稍微向上移动：
![对数刻度](https://upload-images.jianshu.io/upload_images/1844375-5a40ce984f2a4597.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
```stata
#d ;
. twoway 
  (line le_wmale le_bmale year, clcolor(blue red)), 
   title("U.S. Life Expectancy") 
   subtitle("Males") 
   yscale(log range(25 80))
   legend(order(1 "white" 2 "black") 
          ring(0) pos(5)) ;
#d cr
```
### 2.2.5  图形方案
Stata使用方案来控制图的外观，参见`help scheme`。您可以设置默认方案并在所有图形中应用`set scheme_name`。您也可以使用不同的方案对所作的最后一个图形重新展示，选出效果最好的方案`graph display, scheme(scheme_name）`。

使用`graph query, schemes`查看可用方案类型列表。`s2color`方案适用于屏幕图表，`s1manual`是Stata手册中的风格。`economist`是经济学人杂志使用的风格。我们可以获得本节开头所示的图形使用的是`economist`风格。
```stata
 graph display, scheme(economist)
 graph export fig32.png, width(500) replace
```
## 2.3 其他图形

### 2.3.1条形图
条形图可用于绘制分类变量的频数分布，或绘制由分类变量定义的组内连续变量的描述性统计。我们将使用Stata附带的城市温度数据集为例说明。

![城市温度数据集](https://upload-images.jianshu.io/upload_images/1844375-a31092e2f942ab9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果我只是键入`graph bar, over(region)`我将获得区域变量的频数分布。让我们来展示一月和七月的平均气温的区域分布。要做到这一点，我可以指定`(mean) tempjan (mean) tempjuly`，但由于默认统计是平均值，我们可以简写如下。我认为默认图例太长，所以也指定了一个自定义图例。

我使用`over()`这样所以区域出现在同一个图表中;`by()`则相反，每个区域都会产生一个单独的坐标轴。`bargap()`选项则控制同一个组中不同统计的小节之间的间隔; 在这里我放了一个小空间。`gap()`(此处未使用)选项控制不同组别的空间。我还将颜色填充强度设置为70％，我认为这看起来更好。
```stata
. sysuse citytemp, clear
. #d ;
. graph bar tempjan tempjul, 
     over(region) bargap(10) intensity(70) 
     title(Mean Temperature) 
	 legend(order(1 "January" 2 "July")) ;
. #d cr
. graph export bar.png, width(500) replace
```
![平均气温地区分布图](https://upload-images.jianshu.io/upload_images/1844375-ab92b03606e46adc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

显然，1月份东北部和北部中部地区比南部和西部冷得多。七月份的变化较少，但南部的气温较高。

### 2.3.2 箱线图
使用箱线图可以快速获得变量分布的特征，箱线图是取值范围为1~3分位数的箱子，将中位数用横线显示，并且在盒子上下方增加了“wiskers”，定义为距离中值不超过四分位数间距的1.5倍的最高和最低值。在wiskers上下方的点用圆圈表示为异常值。

让我们画一个地区1月份的温度箱形图。我将使用`over(region)`选项,并用`sort(1)`选项控制排列顺序——按照第一个变量`tempjan`中位数大小排列。我还通过设定RGB值将颜色设置为蓝色：
```stata
. #d ;
. graph box tempjan, 
   over(region, sort(1)) 
   box(1, color("51 102 204")) 
   title(Box Plots of January Temperature by Region);
. #d cr
. graph export boxplot.png, width(500) replace
```
![箱线图](https://upload-images.jianshu.io/upload_images/1844375-88117b6169db3b8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们看到，1月份的气温在东北部和北部中部地区较低，变化较小，相当一部分城市气温异常偏冷。

## 2.3.3 核密度估计

对变量分布更详细的展示需要用到平滑直方图，可以使用`kdensity`命令使用核密度平滑器计算平滑直方图。

让我们使用默认设置对每个区域的1月温度进行单独的核密度估计，并保存结果。
```stata
 forvalues i=1/4 {
    capture drop x`i' d`i'
    kdensity tempjan if region== `i', generate(x`i'  d`i')
  }
 gen zero = 0
```

接下来我们做出核密度估计图。由于密度图重叠，我使用Stata 15中引入的不透明选项使它们透明度达到50％。在这种情况下，我使用颜色名称后面跟着一个%符号和不透明度。我也简化了图例，匹配密度的顺序，并把它放在图示的右上角。
```stata
. #d ;
. twoway 
     rarea d1 zero x1, color("blue%50")
  || rarea d2 zero x2, color("purple%50")  
  || rarea d3 zero x3, color("orange%50")  
  || rarea d4 zero x4, color("red%50"),     
     title(January Temperatures by Region) 
     ytitle("Smoothed density")
     legend(ring(0) pos(2) col(1) 
	        order(2 "NC" 1 "NE" 3 "S" 4 "W")) ;   
. #d cr
. graph export kernel.png, width(500) replace
```
![](https://upload-images.jianshu.io/upload_images/1844375-25fa4333d26aec94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个图示使我们清楚地看到了1月份气温的区域差异，东北部和北部中心地区的气候分布更冷，更窄，南部和西部的气候相当相似。

## 2.4 图形管理

Stata默认在内存中保存您绘制的最后一个图形，并将其称为“Graph”。如果你在在创建图形时使用`name()`为图形单独命名，在内存中可以保留多个图形。这对于组合图形很有用，`help graph combine`了解更多。请注意，即使您保存了数据，保存在内存中的图表也会在您退出Stata时消失，除非您保存图形本身。

要使用Stata自己的格式将当前图形保存到磁盘上，输入`graph save filename`。该命令有两个选项`replace`和`asis`，如果该文件已存在，则需要使用`replace`选项替代原有图形，而`asis`选项会冻结图形（包括其当前风格），然后将其保存。默认情况下，将图形保存为可在未来可编辑的实时格式。以Stata格式保存图形后，可以使用`graph use filename`命令从磁盘加载它。（`graph save`和`graph use`类似于`save`和`use`）存储在内存中的任何图形可以使用`graph display [name]`显示。`help graph_manipulation`了解更多信息。

如果您打算将图表合并到另一个文档中，您可能需要将其保存为更便携的格式。Stata的命令`graph export filename`可以使用各种矢量或光栅格式导出图形，通常由文件扩展名指定。您还可以使用`graph print`打印图形，或使用Windows剪贴板将其复制并粘贴到文档中。



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

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-304824e4b09652a7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")



[教程原文](http://data.princeton.edu/stata/graphics.html)


> 原文链接：[Princeton Stata 在线课程 (Princeton University - Stata Tutorial )](https://link.jianshu.com/?t=http%3A%2F%2Fwww.princeton.edu%2F%7Eotorres%2FStata%2F)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)









