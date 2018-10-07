> 作者：刘杨 | 连玉君  | ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


我一直认为：

> 没有耳朵不好的听众，只有嘴巴不好的讲者！
>
> 没有思维迟钝的学生，只有逻辑混乱的老师！
> 
> 所以，不要怪别人笨，那是因为我们没有讲清楚，至少是没有用心讲！

### 1. 回归结果的图形呈现

大家在做完一篇实证研究的论文之后是否有这样的困惑：就是如何将估计系数结果能够简洁、直观的呈现出来？

在学术会议交流论文、或者在毕业论文答辩时，一个常见的做法就是直接将回归表格粘贴出来，然而弊端也是显而易见的，那就是页面中信息太多，导致想要突出的估计结果和显著性判定被淹没其中。

无疑，这是对老师和同行耐心力和细致力的一份考验，需要他们瞪大眼睛来看、竖起耳朵来听，稍有打盹，翻页过去的结果可能就忘记了，只能坐在那里安静的怀疑人生......  

那么，如果能够用图形的方式呈现估计系数及其显著性，并且不同估计系数间的相互关系也能够一目了然，岂不美哉？！

很幸运，`Stata` 提供了这样的外部命令来完成这一工作。我们今天的推文就来给大家简单的介绍一下这两个外部命令 `coefplot` 和 `arrowplot`。

### 2. 目的

- 图形化展示回归结果，尤其是分组回归的系数差异。
- 适用于课题报告、毕业答辩等展示。也适用于呈现论文中的复杂结果。


### 3. coefplot 命令 

- `coefplot` 可以非常便捷地对结果进行图形化展示。
  - 参见 Stata Journal 14(4):708--737。
  - 作者提供了非常完整的介绍和Stata 范例，参见：   
[coefplot主页](http://repec.sowi.unibe.ch/stata/coefplot/index.html) 
&rarr; | [Getting started](http://repec.sowi.unibe.ch/stata/coefplot/getting-started.html) | [Examples](http://repec.sowi.unibe.ch/stata/coefplot/index.html#) | [Help file](http://repec.sowi.unibe.ch/stata/coefplot/help-file.html) 
  - 安装：此命令为外部命令，可以使用 `ssc install coefplot` 进行安装。
  - 该命令可以将回归模型不同解释变量的估计系数绘制在一张图形中，默认设定还会添加估计系数的置信区间，方便判别显著性。当然，该命令具有很强大的扩展性，可以呈现多种类型的图示结果。

- **Note:** 由于上述主页已经对 `coefplot` 命令做了非常详细的解释，我们的推文中就不再细致介绍，只是呈现一些常用的效果。

#### 3.1 coefplot 命令的基本语法

> `coefplot [name] [, opition]`

其中，`name` 为储存于 stata 内存中的回归结果名称（使用 `est store` 命令），如果此处缺省，则默认是 stata 内存中现存的回归结果。
- Stata 范例如下：
```stata
 . sysuse auto, clear 
 . reg price mpg length turn
 . coefplot, drop(_cons) 
```
**说明：** 
- 本例中，我们省略了 `name` 填项，则绘制的是刚刚完成回归的结果。
- `drop` 选项表示不绘制常数项的估计系数，也可以将 `_cons` 替换为其它变量。
输出效果如下：

>![图1：coefplot 基本绘图](http://upload-images.jianshu.io/upload_images/7692714-121ac6c854b77cf3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
##### 3.2 拓展：分组回归后绘图
实证分析中还经常划分两个或两个以上的子样本，进行分组回归，进而对比两组的回归系数。此时，使用 `coefplot` 可以非常直观地进行组建系数的对比。
- Stata 范例如下：

```stata
. reg price mpg length turn if foreign==0
. est store Domestic 

. reg price mpg length turn if foreign==1
. est store Foreign 

. coefplot Domestic Foreign, drop(_cons) xline(0) 
  *-Note: xline表示在x轴0处做出辅助线，便于判断显著性
 
. coefplot Domestic Foreign,  ///
    drop(_cons)               ///
    xline(0, lp(dash) lc(black*0.3)) 
  *-Note: 如果想要调换 X 轴和 Y 轴，可以添加 vertical 选项, 图略
```

> ![图2：分组呈现系数估计值](http://upload-images.jianshu.io/upload_images/7692714-9e9f9534ab0e6d3e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "图2")



##### 3.3 拓展：对每一组回归系数进行个性化设定
 
`coefplot`命令可以对不同的回归进行图形的单独设定，基本语法如下:   

>  `coefplot (name [, plotopts]) (name [, plotopts]) ... [, globalopts]`     

其中：
- `plotopts` 为设定单个组别图形特征的选项
- `globalopts` 为设定图形整体风格的选项

**Stata范例** 如下：

```stata
coefplot    ///
  (Domestic, label("国产汽车") offset(0.05)  pstyle(p3)) ///
  (Foreign , label("进口汽车") offset(-0.05) pstyle(p4)) ///
   , drop(_cons) xline(0, lp(dash) lc(black*0.3)   
```
说明：
- `offset(#)` 选项中的用于设定标记两组系数的横线的间距与默认值的倍数。默认 1 个单位的长度，则 offset(0.05) 表示间距为默认值的 5%。该选项也可以由全局 option 的 nooffsets 代替
- `pstyle` 选项则用于设定线条的风格和色彩，可以在 p1-p15 之间任意选择。

> ![图3：对每组的系数图形进行个性化设定](http://upload-images.jianshu.io/upload_images/7692714-5d226cb39ee8d0de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 3.4 拓展：不同的估计模型下估计系数的比较 
- 语法如下：

>`coefplot (namelist [, plotopts]) ... `

不同估计模型用括号来隔开。例如，我们将多元回归的每个解释变量进行单变量回归，并将估计系数与多元回归的系数比较。

- Stata范例如下：

 ```stata
 . reg price mpg length turn
 . est store multi

 . foreach var in mpg length turn {
     qui reg price `var'
  	 est store  `var'
   }

 . coefplot        ///
     (mpg \length \turn, label("单变量回归"))  ///
	 (multi, label("多元回归"))                ///
	 , drop(_cons) ///
	 xline(0, lp(dash) lc(black*0.3)) 
   graph export "图4：多元回归系数和单变量回归系数比较.png", replace
  *-Note: 在 namelist 中用 \ 分隔存储的回归结果名称
 ```
>![图4：多元回归系数和单变量回归系数比较](http://upload-images.jianshu.io/upload_images/7692714-2ec0762c1be6cd6c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 3.5 拓展：不同被解释变量下估计系数的图形比较

`coefplot`命令也可以对更换被解释变量后，估计系数的相互关系进行比较，这实质上是不同图形的合并。基本语法如下:  

> coefplot plotlist [, subgropts] || plotlist [, subgropts] || ... [, globalopts]

其中，
- `plotlist` 是一个子回归存储的名称
- `subgropts` 是对应的 option 选项。

- Stata范例如下：

```stata
reg price mpg length turn if foreign==0
est store Domestic

reg price mpg length turn if foreign==1
est store Foreign

reg weight mpg length turn if foreign==0
est store Domestic_w  //更换了被解释变量

reg weight mpg length turn if foreign==1
est store Foreign_w   //更换了被解释变量

#d ;
coefplot 
  (Domestic, label("国产汽车"))  
  (Foreign , label("进口汽车")), bylabel(Price) || 
  (Domestic_w) (Foreign_w), bylabel(Weight) ||
  , 
  drop(_cons) byopts(xrescale) 
  xline(0, lp(dash) lc(black*0.3))
  ;
#d cr
graph export "图5：不同被解释变量回归的比较.png", replace
```

>![图5：不同被解释变量回归的比较](http://upload-images.jianshu.io/upload_images/7692714-6640c3e8514b27b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
   
###  4. arrowplot 命令 

顾名思义，这个命令是用来画箭头的！

**安装**：`help arrowplot` 此命令为外部命令，可以使用 `ssc install arrowplot` 下载安装。

**用途**： 
- `arrowplot` 命令可以通过 `groupvar` 选项对样本进行分组，并绘制分组后的散点图来描述组间趋势。并在每一个散点上绘制箭头线，来描述每一组的组内趋势（回归后斜率）。
- 具体应用该图形的实证研究可以参见  “Betsey Stevenson & Justin Wolfers, 2008.  "Economic Growth and Subjective Well-Being: Reassessing the Easterlin Paradox”。文中给出了描述幸福感的“Stevenson-Wolfers happiness graphs”，就是利用该命令呈现的。

> ![图6：arrowplot - Stevenson-Wolfers happiness graphs](http://upload-images.jianshu.io/upload_images/7692714-82b471ddd38c8188.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 4.1 基本语法 

> `arrowplot yvar xvar [if] [in] [weight], groupvar(varname) [options]`

- `yvar`、`xvar` 用来定义被解释变量和解释变量，同时也指定了坐标轴
- `groupvar`用来制定分组变量。
 
- Stata范例如下：

```stata
. sysuse "nlsw88.dta", clear
. decode occupation, gen(occu_str) maxlength(6)
. arrowplot wage hours, groupvar(occu_str)
. graph export "图7：arrowplot 基本绘图.png", replace
```
>![图7：arrowplot 基本绘图](http://upload-images.jianshu.io/upload_images/7692714-e0d9b6d5c81948de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 4.2 拓展：加入控制变量的箭头图

如果想要在回归过程中加入其它控制变量，可以加入 `controls(varlist)` 选项，Stata 范例如下
```stata
. arrowplot wage hours, groupvar(occu_str) ///
           control(age collgrad)
. graph export "图8：arrowplot 回归中加入控制变量.png", replace
``` 

>![图8：arrowplot 回归中加入控制变量](http://upload-images.jianshu.io/upload_images/7692714-1cb0b99eceea4b8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 4.3 拓展：添加分组信息，规定箭头长度 

-Stata范例如下

```stata
. arrowplot wage hours, groupvar(occu_str) ///
      groupname(occupation) line(2)
. graph export "图9：arrowplot 添加分组信息，规定箭头长度.png", replace
```
>![图9：arrowplot 添加分组信息，规定箭头长度](http://upload-images.jianshu.io/upload_images/7692714-13abee1805b16f3f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 4.4 更为丰富的设定

我们可以在 `arrowplot` 命令中加入诸多 Stata 一般图形所支持的选项，以便对图形进行更为灵活的设定。

- Stata范例如下

```stata
sysuse "nlsw88.dta", clear
decode occupation, gen(occu_str) maxlength(6)
#d ;
 arrowplot wage hours,   
   groupvar(occu_str) 
   cont(age collgrad) 
   groupname(occupation) 
   line(2) 
   title("工作时数与小时工资关系之行业特征") 
   subtitle("nlsw88.dta")
   xtitle("工作时间(每周工作小时数)")  
   xscale(titlegap(2))
   ytitle("小时工资") 
   scheme(s1mono) ;
#d cr
. graph export "图10：arrowplot 加入一般化图形选项.png", replace
```

- 为图形添加标题和子标题，并设定X轴与Y轴说明，并改为中文投稿常用的黑白模式
>![图10：arrowplot 中文投稿模式图形](http://upload-images.jianshu.io/upload_images/7692714-0e02798e01e110ff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 附：文中使用的 Stata dofiles

```stata

cd "D:\stata15\ado\personal\Jianshu\coefplot"
cap mkdir Fig
cd Fig 

*-下载 coefplot 命令

   ssc install coefplot
   
   help coefplot
   
 
*-图1
 . sysuse auto, clear 
 . reg price mpg length turn
 . coefplot, drop(_cons) 
 . graph export "图1：coefplot 基本绘图.png", replace
	 
 
*-图2
  sysuse auto, clear
. reg price mpg length turn if foreign==0
. est store Domestic 

. reg price mpg length turn if foreign==1
. est store Foreign 

. coefplot Domestic Foreign,  ///
    drop(_cons)      ///
    xline(0, lp(dash) lc(black*0.3)) 
. graph export "图2：分组呈现系数估计值.png", replace
	
	
*-图3	
. coefplot    ///
  (Domestic, label("国产汽车") offset(0.05)  pstyle(p3)) ///
  (Foreign , label("进口汽车") offset(-0.05) pstyle(p4)) ///
   , drop(_cons) xline(0, lp(dash) lc(black*0.3))   	
. graph export "图3：对每组的系数图形进行个性化设定.png", replace   


*-图4
. reg price mpg length turn
. est store multi

  foreach var in mpg length turn {
    qui reg price `var'
 	 est store  `var'
  }

. coefplot        ///
    (mpg \length \turn, label("单变量回归"))  ///
	 (multi, label("多元回归"))                ///
	 , drop(_cons) ///
	 xline(0, lp(dash) lc(black*0.3)) 
  graph export "图4：多元回归系数和单变量回归系数比较.png", replace
 *-Note: 在 namelist 中用 \ 分隔存储的回归结果名称


*-图5
reg price mpg length turn if foreign==0
est store Domestic

reg price mpg length turn if foreign==1
est store Foreign

reg weight mpg length turn if foreign==0
est store Domestic_w  //更换了被解释变量

reg weight mpg length turn if foreign==1
est store Foreign_w   //更换了被解释变量

#d ;
coefplot 
  (Domestic, label("国产汽车"))  
  (Foreign , label("进口汽车")), bylabel(Price) || 
  (Domestic_w) (Foreign_w), bylabel(Weight) ||
  , 
  drop(_cons) byopts(xrescale) 
  xline(0, lp(dash) lc(black*0.3))
  ;
#d cr
graph export "图5：不同被解释变量回归的比较.png", replace


*-图7
. sysuse "nlsw88.dta", clear
. decode occupation, gen(occu_str) maxlength(6)
. arrowplot wage hours, groupvar(occu_str)
. graph export "图7：arrowplot 基本绘图.png", replace


*-图8
. arrowplot wage hours, groupvar(occu_str) ///
           control(age collgrad)
. graph export "图8：arrowplot 回归中加入控制变量.png", replace


*-图9
. arrowplot wage hours, groupvar(occu_str) ///
      groupname(occupation) line(2)
. graph export "图9：arrowplot 添加分组信息，规定箭头长度.png", replace


*-图10
sysuse "nlsw88.dta", clear
decode occupation, gen(occu_str) maxlength(6)
#d ;
 arrowplot wage hours,   
   groupvar(occu_str) 
   cont(age collgrad) 
   groupname(occupation) 
   line(2) 
   title("工作时数与小时工资关系之行业特征") 
   subtitle("nlsw88.dta")
   xtitle("工作时间(每周工作小时数)")  
   xscale(titlegap(2))
   ytitle("小时工资") 
   scheme(s1mono) ;
#d cr
. graph export "图10：arrowplot 加入一般化图形选项.png", replace
```


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
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-18b4c8f1c1001c52.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")



