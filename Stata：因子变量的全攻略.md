![](https://upload-images.jianshu.io/upload_images/8108279-2bc66f2d02807f09.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

---
作者：连玉君 | 杨柳 ( [知乎](https://zhuanlan.zhihu.com/arlion) | [简书](https://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) )



注：该文已发表: 连玉君, 杨柳.《郑州航空工业管理学院学报》, 2018, 36(2): 90-103.  [【 -点击下载-】](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2018&filename=ZZHK201802009&v=MTc2ODBGckNVUkxLZlllZHJGaTdtVUx6UFB6ZkRaYkc0SDluTXJZOUZiWVI4ZVgxTHV4WVM3RGgxVDNxVHJXTTE=)  || [本文PDF版本2](https://gitee.com/arlionn/jianshu/raw/master/PDF%E4%B8%8B%E8%BD%BD/Stata_Factor_variable.pdf)


Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


>**事倍功半 vs. 事半功倍**

当需要控制公司个体效应、面对成千上万家公司的数据资料时，你会如何处理？或许你会首先想到对每家公司生成虚拟变量，但是这种做法的**工作量实在是太大了！**那么，有没有**事半功倍**的方法呢？下面，小编就带你学习Stata软件中`因子变量`的使用方法。

### 1. 问题背景
实证分析中，我们经常需要在模型中加入反映类别的**虚拟变量**，以便控制不可观测的**组间差异**。而在另一些分析中，为了刻画**调节效应**，尚需在模型中加入变量的**交乘项**或**平方项**。传统的做法是，预先生成虚拟变量或交乘项，进而将它们加入模型。然而，当虚拟变量或交乘项的数目较多时，上述方法就显得尤为不便，不但浪费计算机内存，也会严重降低我们的工作效率。

在Stata中，我们可以使用`因子变量（Factor Variable）`简化操作步骤、快捷地在回归模型中加入**虚拟变量**、**交乘项**、**平方项**或**高次项**。更为重要的是，由于引入交乘项或平方项后，解释变量对被解释变量的边际影响不再是常数，而是某个变量（调节变量）的函数，在有些模型设定下，这种关系可能是非线性的。此时，若使用`因子变量`，并配合Stata中的`margins`和`marginsplot`命令，可以非常便捷、直观地分析关键变量的**边际效应**。

### 2. 什么是因子变量
`因子变量（Factor Variable）`是对现有变量的延伸，是从类别变量中生成虚拟变量、设定类别变量之间的交乘项、类别变量与连续型变量之间的交乘项或连续变量之间的交乘项（或多项式）。在Stata中的大多数回归命令和回归后的估计命令中都可以使用这些`因子变量`^[1]^。

`因子变量`的五种**运算符**及其含义如下**表1**所示：
>**表1 因子变量的运算符及含义**
>![表1 因子变量的运算符及含义.png](https://upload-images.jianshu.io/upload_images/8108279-f07d884552d4489e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以研究**妇女工资的决定因素**为例，使用Stata软件自带的数据文件 `nlsw88.dta`。该数据包含了1988年采集的2246个美国妇女的资料，包括：小时工资`wage`、每周工作时数`hours`、种族`race`、职业`occupation`、年龄`age`、是否大学毕业`collgrad`、当前职业的工作年限`tenure`、是否结婚`married`、是否居住在南部地区`south`、合计工作年限`ttl_exp`等变量。其中，小时工资`wage`、每周工作时数`hours`、年龄`age`、当前职业的工作年限`tenure`、合计工作年限`ttl_exp`为连续型变量；种族`race`为类别变量（1代表白种人`white`，2代表黑种人`black`，3代表其他人种`other`）、职业`occupation`为类别变量（13个职业类别）；是否大学毕业`collgrad`、是否结婚`married`、是否居住在南部地区`south`为虚拟变量。

在这份数据中有一个表示种族的类别变量`race`，取值为`1、2、3`，分别对应`白人`、`黑人`和`其他人种`。假设我们想在模型中加入一个反映种族的虚拟变量`black`，当某个妇女是黑人时，`black`取值为`1`，否则为`0`。则传统的做法如下^[3]^：

```stata
. sysuse "nlsw88", clear
. gen black=1
. replace black=0 if race!=2
. reg wage black
```
若延续这一思路，但使用因子变量来生成`black`变量，则命令为：

```stata
. gen black = 2.race
```

只需要一条命令，而且含义非常明确。这里`2.race`本质上是一个条件判断语句：判断某一行观察值中的`race`变量取值是否为`2`，若是，则返回`1`到变量`black`中，否则返回`0`。

然而，在多数情况下，我们的目的只是希望得到虚拟变量`black`的估计系数，而不希望生成或存储这个变量^[4]^。Stata中的`因子变量`语法完全注意到了这个问题。使用`因子变量`的标准做法如下：

```stata
. sysuse "nlsw88.dta", clear
. reg wage 2.race
```

注意，我们无需预先生成`black`变量，而是直接在回归模型中加入了`2.race`因子变量。有些读者注意到，`race`变量有三个取值，因此，我们可以在模型中放入两个虚拟变量，此时可以书写如下命令：

```stata
. reg wage i.race
```

回归结果如下所示：

```stata
(部分回归结果省略)
------------------------------------------------------------------------------
        wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
        race |
      black  |     -1.238      0.276    -4.48   0.000       -1.781      -0.696
      other  |      0.468      1.133     0.41   0.680       -1.754       2.690
             |
       _cons |      8.083      0.142    57.06   0.000        7.805       8.361
------------------------------------------------------------------------------
```

从上述结果中可以看到，回归模型中加入了`black`虚拟变量和`other`虚拟变量，分别对应`race`变量的第二个和第三个类别，而第一个类别`white`被Stata默认作为基准组，目的在于防止完全共线性。`black`变量的系数值为-1.238，表示黑人的平均工资比白人低1.238个单位，并在统计上显著；`other`变量的系数值为0.468，表示其他人种的平均工资比白人高0.468个单位，但是在统计上不显著。

在实证分析中，有时会根据研究内容的需要改变基准组的设定，此时可使用`ib.`或`b.`的前缀，具体写法如下**表2**中所描述：

>**表2 基准组的运算符及含义**
>![表2 基准组的运算符及含义.png](https://upload-images.jianshu.io/upload_images/8108279-85d422d10eaa2f77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


在实证分析中，变量的**交乘项**或**高次项**往往是重要的解释变量。以研究**妇女工资的决定因素**为例，若我们想在模型中加入黑人每周工作时数的变量，则传统做法是预先生成一个虚拟变量 `black` 代表是否黑人，再生成一个新变量 `black_x_hours` 表示黑人与每周工作时数的交乘项，然后再将这个新变量放入回归模型中。Stata命令为：

```stata
. gen black=1
. replace black=0 if race!=2
. gen black_x_hours = black * hours
. reg wage black_x_hours
```

若在Stata中使用`因子变量`实现上述过程，则命令十分简洁：

```stata
. reg wage 2.race#c.hours
```

下面，我们介绍如何在Stata中使用`因子变量`表示变量的**交乘项**或**高次项**。我们以研究**妇女工资的决定因素**为例进行说明。

**（1） 两个类别变量的交乘项：**
在回归模型中加入种族`race`和职业类别`occupation`的**交乘项**，Stata命令为：

```stata
. reg wage i.race#i.occupation
```

若在回归模型中既要放入种族`race`和职业类别`occupation`的**虚拟变量**，又需要同时放入这两个变量的**交乘项**，则在回归命令中使用`i.race##i.occupation`，相应的命令为：

```stata
. reg wage i.race##i.occupation
```

**（2） 类别变量与连续变量的交乘项：**
在回归模型中加入种族`race`和每周工作时数`hours`的交乘项，Stata命令为：

```stata
. reg wage i.race#c.hours
```

需要注意的是，在上例中，由于我们把`hours`变量视为**连续变量**，因此，需要在其前面加上`c.`符号以便告知Stata该变量是**连续变量**。

**（3） 连续变量与连续变量的交乘项（高次项）：**
在回归模型中加入年龄`age`变量，以及其**平方项**，Stata命令如下：

```stata
. reg wage c.age##c.age
```

上述命令中`c.age`表示年龄`age`变量被当成**连续型变量**。如果我们在Stata命令中使用`i.age`，则年龄`age`变量被当成**类别变量**处理，此时，类别的个数为年龄`age`变量中不同取值的个数。

### 3. 常用回归模型的因子变量表述

>**范例1：邹氏检验**

由于不同组别之间可能会存在**差异（截距项或斜率项存在差异）**，因此，我们需要检验这些差异在统计上是否显著。这时，我们可以使用**邹氏检验**。以研究**妇女工资的影响因素**为例，我们可以使用`chowtest`命令检验工会成员与非工会成员两个样本组中工资影响因素是否存在差异（或称之为存在结构变化），Stata命令如下：

```stata
. global xx "hours age tenure ttl_exp married"
. chowtest wage $xx, group(union) detail
```

事实上，我们也可以使用`因子变量`的语法，在回归模型中加入**分组变量**与**其他控制变量**的**交乘项**，然后再**联合检验**分组变量的系数以及所有交乘项的系数是否都等于0。此时，即使我们不使用`chowtest`命令，也可以轻松实现**邹氏检验**，Stata命令如下：

```stata
. global xx "hours age tenure ttl_exp married"
. reg wage $xx i.union i.union#c.($xx)
. testparm i.union i.union#c.($xx)
```

>**范例2：双向固定效应模型**

在实证分析过程中，经常需要在模型中加入反映**年度、公司或行业特征**的**虚拟变量**。当虚拟变量的数目众多时，采用手动输入变量的方式会非常耗时。例如，下述**模型(1)**是文献中广泛应用的**双向固定效应模型**：
$${{y}_{it}}={{\alpha }_{i}}+{{\lambda }_{t}}+{{x}_{it}}^{'}\beta +{{\varepsilon }_{it}}  \quad (1)$$
上式等价于
$${{y}_{it}}=\sum\limits_{i=1}^{N}{{{D}_{i}}\cdot }\ {{\alpha }_{i}}+\sum\limits_{t=2}^{T}{{{W}_{t}}\cdot }\ {{\lambda }_{t}}+{{x}_{it}}^{'}\beta +{{\varepsilon }_{it}}   \quad (2)$$
其中，$D_i$ 是对应于每家公司的虚拟变量，对于公司 $i$，$D_i=1$，否则 $D_i=0$。对于一个有 $N=1000$ 家公司的面板数据而言，共有 1000 个虚拟变量。$W_t$  是 $T-1$ 个年度虚拟变量^[5]^， 定义方式与 $D_i$ 相似。

若使用因子变量，则语法很简单^[6]^：

```stata
. reg y x1 x2 x3 i.id i.year
```
其中，`i.id` 表示 `N-1` 个公司虚拟变量（Stata 会默认将一家公司作为基准组），即(1)式中的 ${{\alpha}_{i}}$。

当然，我们也可以用 `xtreg` 命令自带的 `fe` 选项来控制个体效应 ，同时使用 `因子变量` 来加入年度虚拟变量 ，命令如下：

```stata
. xtreg y x1 x2 x3 i.year, fe
```

>**范例3：DID模型**

在实证分析中，常常需要分析**政策实施后带来的效果**。这时，我们就需要采集**实验组**与**控制组**两组样本（**实验组**的样本代表政策实施前后的情况，**控制组**的样本代表不实施政策的情况），再把这两组样本合并为一份数据后进行回归分析。在建模时，我们需要设定一个是否为实验组或控制组的处理虚拟变量  *Treat*  放入模型，还需要设定一个反映政策实施时间前与实施后的时间虚拟变量 *Time*  放入模型，此外，还需要在模型中加入 *Treat*  变量与 *Time*  变量的交乘项，该交乘项的系数估计值 ${{a}_{3}}$ 就是政策实施后带来的效果，即实验组在政策实施后与假设未实施政策的差异。回归模型如下式(3)所示：
$$y={{a}_{0}}+{{a}_{1}} {Treat}+{{a}_{2}} {Time}+{{a}_{3}} {Treat}\times {Time}+{{x}^{'}}\beta +\varepsilon  \quad  (3)$$

在 Stata 中的命令写法如下：

```stata
. reg y Treat Time Treat#Time x1 x2 x3
*-或者写为
. reg y Treat##Time x1 x2 x3
```

在多期 DID 分析中，我们常常需要加入**年度虚拟变量**与**处理变量**的**交乘项**来检验**共同趋势假设(common trend)**以及**政策效果**。例如，在 [Acemoglu and Angrist(2001)](http://www.jstor.org/stable/10.1086/322836) 文中，作者采集了1988年至1997年的人口调查数据，将样本分为残疾人与非残疾人，使用多期DID模型研究了1992年美国对残疾人工作保护法案（ADA）的实施效果。文中模型设定如下(pp.925)：

$${{y}_{it}}={{a}_{0}} + {{x}_{it}}^{'}\cdot {\gamma} +  {{p}_{it}}^{'}\cdot {\eta} +  {{x}_{it}^{'}}\cdot {{p}_{it}}\cdot {\beta}+ {{d}_{it}^{'}}\cdot \delta+ {{d}_{it}^{'}}\cdot {{p}_{it}}\cdot {{\alpha}_{t}}+{{\varepsilon}_{it}}   \quad (4)$$

其中，$i$ 为残疾人个体；$t$ 为年份；${{y}_{it}}$ 为工作周数或周平均工资；${{x}_{it}^{'}}$ 为一系列控制变量，${{p}_{it}}$ 为时间趋势项；${{d}_{it}}$  为是否残疾，其系数为 $\delta$； $\alpha_t$ 为随年份变化的实施 ADA 与未实施 ADA 对残疾人工作保护效应的系数。当 $t\ge1992$  年时，$\alpha_t$ 量化了实施ADA之后对残疾人工作保护的效果（使用非残疾人作为控制组）；当 $t<1992$  年时，$\alpha_t$  检验了实施 ADA 之前的各年份中残疾人和非残疾人的工作周数或周平均工资是否在统计上具有显著差异，相当于同时进行了DID模型的 **“共同趋势假设”** 的检验，若 $\alpha_t$ 系数在1992年之前不显著，表明存在**“共同趋势”**。Stata 命令如下^[7]^：

```stata
. use "ABA_JPE2001.dta", clear
. global controls "i.age_G i.edu_G i.race_G i.region"
. reg wkswork1 i.year##($controls) i.disabled##i.year
```
Stata回归结果如下：

```stata
(部分回归结果省略)
-----------------------------------------------------------------------------------
         wkswork1 |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
------------------+----------------------------------------------------------------
------------------|----------------------部分回归结果省略----------------------------
                  |
    disabled#year |
     Disabl#1988  |  -.6873656   .7197987    -0.95   0.340    -2.098154    .7234227
     Disabl#1989  |  -.5385104   .7056609    -0.76   0.445    -1.921589    .8445682
     Disabl#1990  |  -2.289956   .6945586    -3.30   0.001    -3.651274   -.9286372
     Disabl#1991  |  -2.158817   .6916205    -3.12   0.002    -3.514376    -.803257
     Disabl#1992  |  -1.386539   .6873483    -2.02   0.044    -2.733725   -.0393526
     Disabl#1993  |  -2.743325   .6953003    -3.95   0.000    -4.106097   -1.380553
     Disabl#1994  |  -3.917873   .7198205    -5.44   0.000    -5.328704   -2.507042
     Disabl#1995  |  -3.702671    .748066    -4.95   0.000    -5.168863    -2.23648
     Disabl#1996  |  -4.472484   .7373381    -6.07   0.000    -5.917649   -3.027319
                  |
            _cons |   41.05231   .4546278    90.30   0.000     40.16125    41.94337
-----------------------------------------------------------------------------------
```

>**范例4：超越对数生产函数**

很多文献使用超越对数生产函数估计生产效率，模型中会包含投入要素 *K* 和 *L* 的高阶项，包括平方项和二者的交乘项^[8]^。例如在 [Kumbhakar(1989)](https://www.tandfonline.com/doi/abs/10.1080/07350015.1989.10509734) 中，作者通过在模型中加入二次项来捕捉要素的交互影响和潜在的非线性关系，模型如下式(5)所示。

$${ln{y}}={\alpha} + {\sum_{k=1}^{k}} {{\beta} _{k}} \cdot {ln{x}_{k}} + {\frac {1}{2}} {\sum_{k=1}^{k}} {\sum_{m=1}^{m}} {{\gamma} _{km}}\cdot {ln{x}_{k}} \cdot {ln{x}_{m}}  \quad  (5)$$


此时，使用`因子变量`会让Stata中的命令变得异常简洁，如下所示：

```stata
. webuse frontier1.dta, clear  
. global y "lnoutput"            // y
. global x "lnlaborlncapital"    // x1 x2
*-超越对数生产函数
. sfcross $y c.($x)##c.($x)      // Eq.(5)
```

### 4. 边际效应分析和图形化呈现

一些实证研究的文献中常常加入**变量的交乘项**以反映可能存在的**调节效应**，例如 [Faulkender and Wang(2006)](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1540-6261.2006.00894.x) 、[戴魁早和刘友金(2016)](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2016&filename=JJYJ201607007&v=MzA1MzN5M2hVYi9LTHlmU1pMRzRIOWZNcUk5Rlk0UjhlWDFMdXhZUzdEaDFUM3FUcldNMUZyQ1VSTEtmWk9kbkY=) 与 [张苏和高扬(2012)](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFD2012&filename=GLSJ201204017&v=MTk0MDZZUzdEaDFUM3FUcldNMUZyQ1VSTEtmWk9kbkZ5M2hVYnJNSWlIWVpMRzRIOVBNcTQ5RVk0UjhlWDFMdXg=) ^[9]^，此时最重要的是分析  *x*  对  *y*  的**边际效应**。

$${y} = {{\alpha} _{1}} + {x}{{\beta}_{1}} + {z}{{\beta} _{2}} + ({x}\cdot {z}){{\beta}_{3}} + {\varepsilon}  \quad  (6)$$

$x$  对 $y$  的边际影响为：

$$\frac{\partial {y}}{\partial {x}} = {{\beta}_{1}} + {z} {{\beta} _{3}}$$

显然，*x*  对 *y*  的边际影响取决于 *z* 的取值。这种边际效应分析若是采用手动计算会非常繁琐，而后续的图形呈现则更为复杂。然而，若是借助`因子变量`，并配合Stata中的`margins`和`marginsplot`命令，对于包含**交乘项**的模型中的**边际效应**的分析和**图形化**展示都变得异常轻松。

下面，我们以**研究妇女工资的决定因素**为例进行说明。使用Stata软件的自带数据`nlsw88.dta（1988年美国妇女小时工资）`，以`wage（妇女的小时工资）`作为被解释变量，以`race（种族类别）`、`collgrad（是否大学毕业）`、`race与collgrad的交乘项`作为解释变量建立线性回归模型，Stata中的命令如下：

```stata
. sysuse "nlsw88.dta", clear
. reg wage i.race##collgrad
```

回归结果如下所示：

```stata
(部分回归结果省略)
-------------------------------------------------------------------------------------
               wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
--------------------+----------------------------------------------------------------
               race |
             black  |     -1.442      0.297    -4.85   0.000       -2.026      -0.859
             other  |     -0.380      1.348    -0.28   0.778       -3.024       2.263
                    |
           collgrad |
      college grad  |      2.981      0.312     9.54   0.000        2.368       3.593
                    |
      race#collgrad |
black#college grad  |      2.502      0.676     3.70   0.000        1.177       3.827
other#college grad  |      1.678      2.297     0.73   0.465       -2.826       6.182
                    |
              _cons |      7.318      0.158    46.25   0.000        7.008       7.629
-------------------------------------------------------------------------------------
```

我们使用`margins`命令来计算`种族(race)与是否大学毕业(collgrad)的交乘项的各个类别`的`妇女小时工资(wage)`的**预测边际值**，Stata命令和结果如下所示：

```stata
. margins i.race#collgrad
```

计算结果如下所示：

```stata
Adjusted predictions                            Number of obs     =      2,246
Model VCE    : OLS
Expression   : Linear prediction, predict()
-----------------------------------------------------------------------------------------
                        |            Delta-method
                        |     Margin   Std. Err.      t    P>|t|     [95% Conf. Interval]
------------------------+----------------------------------------------------------------
          race#collgrad |
white#not college grad  |   7.318251   .1582176    46.25   0.000     7.007983     7.62852
    white#college grad  |   10.29895   .2693243    38.24   0.000     9.770797     10.8271
black#not college grad  |   5.875918   .2519298    23.32   0.000     5.381878    6.369958
    black#college grad  |   11.35861    .543853    20.89   0.000      10.2921    12.42512
other#not college grad  |   6.938195   1.338677     5.18   0.000     4.313019    9.563371
    other#college grad  |   11.59678   1.839835     6.30   0.000     7.988818    15.20474
-----------------------------------------------------------------------------------------
```

使用`marginsplot`命令将计算结果用图的形式表示，Stata命令和结果如下所示：

```stata
. marginsplot
```

>**图1 种族与是否大学毕业交乘项各类别的妇女小时工资的预测边际值**
>![图1 种族与是否大学毕业交乘项各类别的妇女小时工资的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-9857a40264ffa305.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图1中可以直观的看到，不论种族类别，大学毕业的妇女的平均工资高于非大学毕业的妇女，该结果符合我们的一般认知；另外，我们从图中还发现一个有趣的结果，非大学毕业的白人妇女的平均工资高于非大学毕业的黑人妇女，而大学毕业的白人妇女的平均工资低于大学毕业的黑人妇女。

下面，我们使用`margins`命令附加`atmeans`选项来计算**当其他变量取均值**时**不同种族类别**的妇女小时工资的**预测边际值**，Stata命令和结果如下所示：

```stata
. margins i.race, atmeans
*-或者写为
. margins race, atmeans
```

计算结果如下所示：

```stata
Adjusted predictions                            Number of obs     =      2,246
Model VCE    : OLS

Expression   : Linear prediction, predict()
at           : 1.race          =    .7288513 (mean)
               2.race          =    .2595726 (mean)
               3.race          =    .0115761 (mean)
               0.collgrad      =    .7631345 (mean)
               1.collgrad      =    .2368655 (mean)
------------------------------------------------------------------------------
             |            Delta-method
             |     Margin   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
        race |
      white  |   8.024276    .136558    58.76   0.000     7.756482    8.292069
      black  |   7.174578    .231424    31.00   0.000      6.72075    7.628406
      other  |   8.041653   1.110659     7.24   0.000     5.863625    10.21968
------------------------------------------------------------------------------
```

使用`marginsplot`命令将计算结果用图的形式表示，Stata命令和结果如下所示：

```stata
. marginsplot
```

>**图2 不同种族类别的妇女小时工资的预测边际值**
>![图2 不同种族类别的妇女小时工资的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-015fd7be593e903e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图2中可以直观的看到，白人与其他种族的妇女的平均工资高于黑人妇女。

两个**连续变量**的**交乘项**对被解释变量的边际效应也可以使用`margins`命令来计算。我们仍以**研究妇女工资的决定因素**为例进行说明。在回归模型中加入`tenure（当前职业的工作年限）及其平方项`，并将`hours（每周工作时数）`、`age（妇女年龄）`、`married（是否结婚）`、`south（是否居住在南部地区）`、`race（种族类别）`作为控制变量，Stata命令如下所示：

```stata
. global xx "hours age married south i.race"
. reg wage c.tenure##c.tenure $xx
```

回归结果如下：

```stata
(部分回归结果省略)
-----------------------------------------------------------------------------------
             wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
------------------+----------------------------------------------------------------
           tenure |   .3015859   .0704148     4.28   0.000        .1635    .4396718
                  |
c.tenure#c.tenure |  -.0076366   .0037758    -2.02   0.043    -.0150411   -.0002321
                  |
            hours |   .0749604   .0115722     6.48   0.000     .0522669    .0976539
              age |  -.0850009   .0388991    -2.19   0.029    -.1612832   -.0087185
          married |  -.4619864   .2544385    -1.82   0.070     -.960949    .0369761
            south |  -1.283751   .2493374    -5.15   0.000     -1.77271   -.7947914
                  |
             race |
           black  |  -1.221256   .2870854    -4.25   0.000    -1.784241   -.6582719
           other  |   .3129332   1.098312     0.28   0.776    -1.840893     2.46676
                  |
            _cons |   8.183759   1.632092     5.01   0.000     4.983171    11.38435
-----------------------------------------------------------------------------------
```

由于在模型中加入了 *tenure* 的平方项，因此，*tenure* 对 *wage* 的边际效应将会受到 *tenure* 取值的影响。上述Stata命令对应的模型设定如下：

$${wage}={\alpha} + {tenure}{{\beta} _{1}} + ({tenure}\cdot {tenure}){{\beta} _{2}} + {{x}_{3}}{{\beta} _{3}} +\cdots  + {{x}_{8}}{{\beta} _{8}} + {\varepsilon}  \quad  (7)$$

*tenure* 对 *wage* 的边际影响为：

$$\frac{\partial {wage}}{\partial {tenure}} = {{\beta} _{1}} + 2\cdot {tenure}\cdot {{\beta} _{2}}$$

显然，当 *tenure* 取值不同时，*tenure* 对 *wage* 的边际效应是不相同的。因此，需要先使用描述性统计分析的命令（如sum *tenure*）查看 *tenure* 取值的范围，然后再计算当 *tenure* 取不同的值所对应的边际效应，Stata命令如下：

```stata
. preserve
. keep if e(sample)
. sum tenure
. restore
. margins, dydx(tenure) at(tenure=(0 1(3)25 25.9))
```

计算结果如下所示：

```stata
Average marginal effects                        Number of obs     =      2,227
Model VCE    : OLS

Expression   : Linear prediction, predict()
dy/dx w.r.t. : tenure
1._at        : tenure          =           0
2._at        : tenure          =           1
... (output omitted) ...
11._at       : tenure          =        25.9
------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
tenure       |
         _at |
          1  |   .3015859   .0704148     4.28   0.000        .1635    .4396718
          2  |   .2863127   .0632764     4.52   0.000     .1622256    .4103998
... (output omitted) ...
         11  |  -.0939893   .1304534    -0.72   0.471    -.3498129    .1618343
------------------------------------------------------------------------------
```

使用`marginsplot`命令将计算结果用图的形式表示，Stata命令和结果如下所示：

```stata
. marginsplot, xlabel(,format(%3.1f) angle(60))
```

>**图3 工作年限对妇女工资的平均边际效应**
>![图3 工作年限对妇女工资的平均边际效应.png](https://upload-images.jianshu.io/upload_images/8108279-28bd35c7fded9e59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Stata中的多数命令都支持`margins`和`marginsplot`命令。因此，即使对于非线性模型，如logit, tobit等，我们仍然可以借助这两个命令很方便地分析边际效应。

### 5. 输出回归结果时的问题及解决办法

以研究**妇女工资的决定因素**为例。`wage（妇女的小时工资）`作为回归模型的被解释变量，`race（种族类别）`、`collgrad（是否大学毕业）`、`race与collgrad的交乘项`作为解释变量，并将回归结果输出到Excel中，Stata命令和结果如下：

```stata
. sysuse "nlsw88.dta", clear
. reg wage i.race##collgrad
. est store R1
. esttab R1 using D:/Table_factor_1.csv, nogap replace
```
>**表3 1988年美国妇女工资模型结果**
>![表3 1988年美国妇女工资模型结果.png](https://upload-images.jianshu.io/upload_images/8108279-baedbef6dacbf11a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们发现在表3中有很多变量的系数值为 0，并缺失 t 统计量。造成这种情况的原因有二：一方面是由于有些虚拟变量作为了基准组，例如：`1.race`，`0.collgrad`与`1.race#0.collgrad`，Stata默认将它们作为基准组，所以就缺失这些基准组的估计系数值和标准误；另一方面是由于有些交乘项中的其中一个因子变量是基准组，其它变量与这个作为基准组的因子变量交乘后的交乘项就被忽略了，所以其估计系数值和标准误就会缺失，例如：`1.race#1.collgrad`，`2.race#0.collgrad`与`3.race#0.collgrad`。此时，可以使用`esttab`命令的`drop()`选项来屏蔽这些系数的显示，还可以使用`nobase`和`noomit`的选项，Stata命令和结果如下所示：

```stata
*-输出结果（不显示基准组和忽略组的系数，使用drop选项）
. esttab R1 using D:/Table_factor_2.csv, nogap      ///
	     drop(1.race 0.collgrad 1.race#0.collgrad   ///
         1.race#1.collgrad 2.race#0.collgrad        ///
         3.race#0.collgrad) replace
*-输出结果（不显示基准组和忽略组的系数，使用nobase与noomit选项）
. esttab R1 using D:/Table_factor_3.csv, nogap nobase noomit replace
```

>**表4 1988年美国妇女工资模型结果（删除缺失估计系数值的变量）**
>![表4 1988年美国妇女工资模型结果（删除缺失估计系数值的变量）.png](https://upload-images.jianshu.io/upload_images/8108279-03123cabcd9d53e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 6. 小结

本文介绍了Stata中`因子变量`产生**虚拟变量**与**交乘项**的使用方法，以常用经典回归模型为例，提供了它们的Stata命令，并进一步提供了`因子变量`与`margins`与`marginsplot`命令相配合分析**边际效应**的示例。在Stata中的大多数命令中都可以使用`因子变量`的表述方法。该方法可以使Stata命令更加简洁，并能够大幅度提高实证分析的效率，但需要注意分析使用`因子变量`表述方法后得到的**模型设定结构**。

---
### 参考文献：
**Acemoglu, D., J. D. Angrist**, 2001, Consequences of Employment Protection? The Case of the Americans with Disabilities Act, Journal of Political Economy, 109 (5): 915-957.            
**Altunbas, Y., M.-H. Liu, P. Molyneux, R. Seth**, 2000, Efficiency and Risk in Japanese Banking, Journal of Banking & Finance, 24 (10): 1605-1628.           
**Faulkender, M., R. Wang**, 2006, Corporate Financial Policy and the Value of Cash, Journal of Finance, 61 (4): 1957-1990.               
**Kumbhakar, S. C.**, 1989, Estimation of Technical Efficiency Using Flexible Functional Form and Panel Data, Journal of Business and Economic Statistics, 7 (2): 253-258.              
**Wang, E. C.**, 2007, R&D Efficiency and Economic Performance: A Cross-Country Analysis Using the Stochastic Frontier Approach, Journal of Policy Modeling, 29 (2): 345-360.              
**戴魁早, 刘友金**, 2016, 要素市场扭曲与创新效率——对中国高技术产业发展的经验分析, 经济研究, (7): 72-86.              
**王德祥, 李建军**, 2009, 我国税收征管效率及其影响因素——基于随机前沿分析(SFA)技术的实证研究, 数量经济技术经济研究, (4): 152-160.              
**张苏, 高扬**, 2012, 大学生学习行为与国家竞争力关联关系的实证研究, 管理世界, (4): 175-176.              

-----
**注释：**
**[1]** 详情参阅Stata帮助文件`help fvvarlist`。              
**[2]** 详情参见Stata帮助文件`help varlist`。              
**[3]** 为了便于说明，后续多数回归命令中都省略了控制变量。              
**[4]** 事实上，只要你的数据中存储了`race`变量，我们只需要保存好`dofile`文件，就无需生成`black`这个中间变量。              
**[5]** 之所以加入*T* - 1 个年度虚拟变量，是为了防止完全共线性。              
**[6]** 此处设定`i.year`会自动加入 *T* 个年度虚拟变量，但Stata会自动删除一个，以防完全共线性。              
**[7]** 文中实证分析所用的原始数据和相关程序可以从作者主页上下载：http://economics.mit.edu/faculty/acemoglu/data/aa2001。
**[8]** 例如，[Altunbas, Liu, Molyneux and Seth(2000)](https://www.sciencedirect.com/science/article/pii/S0378426699000953) 使用超越对数成本函数估算了日本银行的效率和风险；[Wang(2007)](http://www.sciencedirect.com/science/article/pii/S0161893807000026) 则使用超越对数生产函数研究30个国家R&D效率；[王德祥和李建军(2009)](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFD2009&filename=SLJY200904015&v=MDk2NTN5M2hXNzdCTmlIQmQ3RzRIdGpNcTQ5RVlZUjhlWDFMdXhZUzdEaDFUM3FUcldNMUZyQ1VSTEtmWk9kbkY=) 基于超越对数生产函数估算了我国的税收流失率。              
**[9]** [Faulkender and Wang(2006)](https://onlinelibrary.wiley.com/doi/full/10.1111/j.1540-6261.2006.00894.x) 检验了由公司融资约束对现金持有边际市场价值的影响。[戴魁早和刘友金(2016)](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFDLAST2016&filename=JJYJ201607007&v=MzA1MzN5M2hVYi9LTHlmU1pMRzRIOWZNcUk5Rlk0UjhlWDFMdXhZUzdEaDFUM3FUcldNMUZyQ1VSTEtmWk9kbkY=) 研究发现了要素市场扭曲对创新效率的影响存在着企业差异，企业规模在规避要素市场扭曲对创新效率的抑制效应中具有积极作用。[张苏和高扬(2012)](http://kns.cnki.net/KCMS/detail/detail.aspx?dbcode=CJFQ&dbname=CJFD2012&filename=GLSJ201204017&v=MTk0MDZZUzdEaDFUM3FUcldNMUZyQ1VSTEtmWk9kbkZ5M2hVYnJNSWlIWVpMRzRIOVBNcTQ5RVk0UjhlWDFMdXg=) 的实证研究发现学生来源于城市或农村地区对国家竞争力的影响作用受到每周上网时间的影响，如果上网时间在每周8小时以下，城市大学生的学习行为落入“增进国家竞争力导向”上高效率区域而不是低效率区域的概率比农村大学生要高，否则要低。              

---
>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-c75dabfe9f11794e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
