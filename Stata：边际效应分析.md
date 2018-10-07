> 作者：连玉君 | 杨柳 ( [知乎](https://zhuanlan.zhihu.com/arlion) | [简书](https://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) )

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


## 1. 边际效应简介
### 1.1 引言

- 考研复试结束后，你和闺蜜决定去成都旅游。当你和闺蜜正在品尝当地最有名气的麻辣火锅时，你们感觉心情非常愉快。此时，麻辣火锅将有助于有一个好心情，它对心情的边际效应是正值；当你们吃到一半时，手机上收到了一条消息，是考研复试的排名结果，打开消息后，发现你们俩都榜上有名。你们看到被录取的消息后万分高兴，吃的更high了，又多点了一些菜，并决定吃完后再去KTV庆祝下。此时，考研成功的结果极大的增加了吃火锅对心情的边际效应值。

- 研究生入学后，你和闺蜜都十分努力学习，认真的完成导师布置的课题任务并将课题研究内容整理成小论文投到了一个C刊上，但遭到拒稿。此时，努力学习对科研成果的边际效应是负值。不过你们没有就此停止努力，继续请教导师、按照意见认真修改并写成英文投到了一个SSCI期刊上，结果被录用了。此时，随着努力程度的增加，它对科研成果的边际效应变为正值。

- 总结上面的例子，我们发现吃麻辣火锅（x1）对心情（y）的边际效应受到其他变量（x2：考研成功）的调节作用，使得该边际效应值增加了。一开始，努力学习（x3）对科研成果的边际效应为负，但随着努力程度的增加（x3值的增加），它对科研成果的边际效应变为正值。


### 1.2 边际效应分析的必要性

- 虽然 `回归结果表格` 中的变量的系数估计值反映了该变量对被解释变量影响作用的大小，并且一直是学者们交流回归模型结果的重要方式，但是，当回归模型中包含 `类别变量`、`交乘项` 或者 `回归模型为非线性`（诸如 `Logit`, `Probit` 等非线性模型）时，对系数估计值的解释就非常具有挑战性。这时，就需要计算变量的 `边际效应` 或者计算 `预测边际值`， 以探求 `自变量变化` 对 `因变量变化` 的 `影响作用` 或分析比较不同情况时的因变量预测边际值的大小。

- 下面，我们就一起来学习如何在stata中计算边际效应并绘制图形。

### 1.3 边际效应的定义
- 所谓 `边际效应` 是从已有拟合模型结果中计算出来的统计量，该数值表示 `自变量的变化` 对 `因变量的变化` 的 `影响作用` 的 `大小`。

- 在对模型结果进行分析时，可以计算 `连续变量取某一个值` 时，`连续变量` 对 `因变量` 的边际效应，也可以计算 `连续变量平均值处` 的边际效应，或者还可以计算  `其他变量取均值` 时 `连续变量` 对 `因变量` 的平均边际效应。


## 2. 计算边际效应命令(margins)与绘图命令(marginsplot) 

### 2.1 margins 命令 

- `margins` 命令的语法如下所示：
```stata
margins [marginlist] [if] [in] [weight] [, response_options options]
```

- 命令含义：
![图1 margins命令解释.png](https://upload-images.jianshu.io/upload_images/8108279-6e880b03b6ad8bd2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


Note: 有关如何使用因子变量的介绍请参见 [往期推文：stata中因子变量的使用方法](https://zhuanlan.zhihu.com/p/34673263)。

### 2.2  marginsplot 绘图命令
- 使用 `marginsplot` 命令可以将之前刚刚计算的边际效应的结果以图的形式展示出来。语法如下：
    ```stata
    marginsplot [, options]
    ```

- 命令含义：

![图2 marginsplot命令解释.png](https://upload-images.jianshu.io/upload_images/8108279-92626e8cfce0d016.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 3. margins与marginsplot命令举例

### 3.1 基础案例

- 在美国的种族文化中，不可否认白人与黑人之间的差异性。例如，存在着白人与黑人在行业类别与工资方面差异的现象。于是，我们想检验种族是否为工资的显著影响因素，还想了解当行业类别相当时，不同种族的工资的平均水平分别是多少，它们之间的差别有多少。

- 接下来，我们使用 `stata` 的自带数据 `nlsw88.dta (1988年美国妇女小时工资)` ，以 `wage (妇女的小时工资)` 作为被解释变量、以  `industry (行业类别)`、 `race (种族类别)` 作为解释变量建立线性回归模型。

- `race` 变量为 `类别变量`，它包括三个类别，分别为 `white`、`black`、`other`。可以使用 `因子变量` 的语法格式，在变量前面加上前缀 `i.` 生成虚拟变量 `(i.race)`，基准组为第一个类别 `white`。`industry` 变量也同样使用 `因子变量` 的语法格式生成虚拟变量 `(i.industry)`，基准组为第一个类别 `Ag/Forestry/Fisheries`。 `stata` 中的回归命令和结果如下所示：

```stata
.   sysuse "nlsw88.dta", clear
(NLSW, 1988 extract)

.   reg wage i.race i.industry

      Source |       SS           df       MS      Number of obs   =     2,232
-------------+----------------------------------   F(13, 2218)     =     13.00
       Model |  5246.90865        13  403.608358   Prob > F        =    0.0000
    Residual |  68870.3701     2,218  31.0506628   R-squared       =    0.0708
-------------+----------------------------------   Adj R-squared   =    0.0653
       Total |  74117.2788     2,231  33.2215503   Root MSE        =    5.5723

------------------------------------------------------------------------------------------
                    wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------------------+----------------------------------------------------------------
                    race |
                  black  |  -1.099771   .2743495    -4.01   0.000     -1.63778   -.5617626
                  other  |   .1317467   1.103937     0.12   0.905    -2.033111    2.296604
                         |
                industry |
                 Mining  |   9.469702   3.097312     3.06   0.002     3.395767    15.54364
           Construction  |   1.832193   1.702718     1.08   0.282    -1.506895     5.17128
          Manufacturing  |   2.021802   1.382963     1.46   0.144    -.6902358    4.733841
 Transport/Comm/Utility  |   5.891929   1.473775     4.00   0.000     3.001807    8.782052
 Wholesale/Retail Trade  |   .4639784    1.38559     0.33   0.738     -2.25321    3.181167
Finance/Ins/Real Estate  |    4.10511   1.410372     2.91   0.004     1.339321    6.870898
    Business/Repair Svc  |   1.888596   1.479264     1.28   0.202     -1.01229    4.789483
      Personal Services  |  -.9699527   1.466554    -0.66   0.508    -3.845914    1.906009
  Entertainment/Rec Svc  |   1.038595   1.911355     0.54   0.587    -2.709638    4.786828
  Professional Services  |   2.252467   1.365435     1.65   0.099    -.4251976    4.930132
  Public Administration  |   3.602952   1.415632     2.55   0.011     .8268485    6.379055
                         |
                   _cons |   5.879891   1.353025     4.35   0.000     3.226563    8.533219
------------------------------------------------------------------------------------------
```

- 回归结果显示：妇女种族为 `black` 的系数值为 `-1.099` 并在1%的水平上显著；妇女种族为 `other` 的系数值为 `0.131` 但在统计上不显著。上述结果表明：当控制各行业类别变量时，黑人妇女的小时工资比白人妇女的小时工资低 `1.099`个单位。我们还想进一步了解当控制行业类别变量时，各个种族类别（`white`, `black`, `other`）的妇女的小时工资的平均水平是多少。于是，我们使用 `margins` 命令附加 `atmeans` 的选项就可以计算当 `其他变量取均值时`，`不同种族的妇女` 的小时工资的 `预测边际值`。`stata` 命令和结果如下所示：
```stata
.   margins i.race, atmeans    //前缀 i. 可省略不写

Adjusted predictions                            Number of obs     =      2,232
Model VCE    : OLS

Expression   : Linear prediction, predict()
at           : 1.race          =    .7289427 (mean)
               2.race          =    .2594086 (mean)
               3.race          =    .0116487 (mean)
               1.industry      =    .0076165 (mean)
               2.industry      =    .0017921 (mean)
               3.industry      =    .0129928 (mean)
               4.industry      =    .1644265 (mean)
               5.industry      =    .0403226 (mean)
               6.industry      =    .1491935 (mean)
               7.industry      =    .0860215 (mean)
               8.industry      =    .0385305 (mean)
               9.industry      =    .0434588 (mean)
               10.industry     =    .0076165 (mean)
               11.industry     =    .3691756 (mean)
               12.industry     =     .078853 (mean)

------------------------------------------------------------------------------
             |            Delta-method
             |     Margin   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
        race |
      white  |   8.067219   .1387997    58.12   0.000     7.795028     8.33941
      black  |   6.967447   .2345358    29.71   0.000     6.507515     7.42738
      other  |   8.198965   1.094967     7.49   0.000     6.051697    10.34623
------------------------------------------------------------------------------
```
- 计算结果表明：当行业类别变量取均值时，白人妇女的小时工资的预测边际值为 `8.067`、黑人妇女的小时工资的预测边际值为 `6.967`、其他种族的妇女的小时工资的预测边际值为 `8.198`。

- 我们还想将边际效应的计算结果用图的形式表示。使用 `marginsplot` 命令就可以很方便的实现这个想法。`stata` 命令如下所示：
```stata
marginsplot
```
- `marginsplot` 命令的输出图片如下所示：  
![图3 各种族类别的妇女小时工资的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-66bcbe15bf258050.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3.2 交乘项案例

- 下面，继续以研究妇女工资的影响因素为例对计算交乘项的边际效应的使用方法进行说明。

#### 3.2.1 类别变量与类别变量交乘
- 从3.1节案例的回归结果中，我们已知道种族 `race` 是妇女工资 `wage` 的影响因素之一。但是，除此之外，还有诸多因素会影响妇女工资，例如是否大学毕业 `collgrad`。因此，我们想探究是否大学毕业 `collgrad` 能否调节种族 `race` 对妇女工资 `wage` 的影响作用。于是，拟在回归模型中加入这两个变量的 `交乘项` 来检验是否存在调节效应。

- 使用 `stata` 的自带数据 `nlsw88.dta (1988年美国妇女小时工资)`，以 `wage (妇女的小时工资)` 作为被解释变量、以 `industry (行业类别)`、`collgrad (是否大学毕业)` 、 `race (种族类别)` 、 `race (种族类别)` 与 `collgrad (是否大学毕业)` 的 `交乘项` 建立线性回归模型。

- 使用 `因子变量` 的语法格式，`collgrad##i.race` 表示在模型中既包括 `collgrad` 与 `race` 变量，还包括 `collgrad` 与 `race` 变量的 `交乘项`。`stata` 中的回归命令和结果如下所示：

```stata
.    reg wage i.industry collgrad##i.race

      Source |       SS           df       MS      Number of obs   =     2,232
-------------+----------------------------------   F(16, 2215)     =     23.20
       Model |  10639.3304        16  664.958149   Prob > F        =    0.0000
    Residual |  63477.9484     2,215   28.658216   R-squared       =    0.1435
-------------+----------------------------------   Adj R-squared   =    0.1374
       Total |  74117.2788     2,231  33.2215503   Root MSE        =    5.3533

------------------------------------------------------------------------------------------
                    wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------------------+----------------------------------------------------------------
                industry |
                 Mining  |   10.14131   2.976152     3.41   0.001     4.304972    15.97765
           Construction  |   2.198272   1.636227     1.34   0.179    -1.010428    5.406972
          Manufacturing  |   2.419311   1.329182     1.82   0.069     -.187261    5.025884
 Transport/Comm/Utility  |    6.12037    1.41599     4.32   0.000     3.343563    8.897177
 Wholesale/Retail Trade  |   .7578957   1.331357     0.57   0.569    -1.852943    3.368734
Finance/Ins/Real Estate  |   4.258568   1.355005     3.14   0.002     1.601355    6.915781
    Business/Repair Svc  |   1.894149   1.421387     1.33   0.183    -.8932415    4.681539
      Personal Services  |  -.3136217   1.410269    -0.22   0.824     -3.07921    2.451967
  Entertainment/Rec Svc  |   .7577804   1.837152     0.41   0.680     -2.84494      4.3605
  Professional Services  |   1.399826   1.313806     1.07   0.287    -1.176593    3.976246
  Public Administration  |   3.367397   1.360314     2.48   0.013     .6997726    6.035021
                         |
                collgrad |
           college grad  |   3.346222   .3197178    10.47   0.000     2.719244      3.9732
                         |
                    race |
                  black  |  -1.279729   .2951157    -4.34   0.000    -1.858461   -.7009966
                  other  |  -.3266847    1.31086    -0.25   0.803    -2.897328    2.243959
                         |
           collgrad#race |
     college grad#black  |   2.098549   .6590111     3.18   0.001     .8062049    3.390893
     college grad#other  |   .7141623     2.2373     0.32   0.750    -3.673262    5.101587
                         |
                   _cons |    5.20828   1.301126     4.00   0.000     2.656726    7.759834
------------------------------------------------------------------------------------------
```

- 回归结果显示：大学毕业与黑人的交乘项  `collgrad#black` 的系数显著为正，而黑人 `black` 的系数显著为负，表明大学毕业 `collgrad` 对黑人 `black` 与 妇女的小时工资 `wage` 之间的影响关系具有调节作用。因此，我们想进一步了解 `大学毕业(collgrad)` 与 `种族(race)` 交乘项的 `各个类别` 对 `妇女的小时工资 (wage)` 的边际效应分别是多少。于是，我们使用 `margins` 附加 `atmeans` 的选项就可以计算当 `其他变量取均值时`，`collgrad 与race交乘项的各个类别` 的小时工资的 `预测边际值`。`stata` 命令和结果如下所示：
```stata
.    margins collgrad#i.race, atmeans

Adjusted predictions                            Number of obs     =      2,232
Model VCE    : OLS

Expression   : Linear prediction, predict()
at           : 1.industry      =    .0076165 (mean)
               2.industry      =    .0017921 (mean)
               3.industry      =    .0129928 (mean)
               4.industry      =    .1644265 (mean)
               5.industry      =    .0403226 (mean)
               6.industry      =    .1491935 (mean)
               7.industry      =    .0860215 (mean)
               8.industry      =    .0385305 (mean)
               9.industry      =    .0434588 (mean)
               10.industry     =    .0076165 (mean)
               11.industry     =    .3691756 (mean)
               12.industry     =     .078853 (mean)
               0.collgrad      =    .7629928 (mean)
               1.collgrad      =    .2370072 (mean)
               1.race          =    .7289427 (mean)
               2.race          =    .2594086 (mean)
               3.race          =    .0116487 (mean)

-----------------------------------------------------------------------------------------
                        |            Delta-method
                        |     Margin   Std. Err.      t    P>|t|     [95% Conf. Interval]
------------------------+----------------------------------------------------------------
          collgrad#race |
not college grad#white  |   7.226442    .156204    46.26   0.000      6.92012    7.532763
not college grad#black  |   5.946713   .2504188    23.75   0.000     5.455632    6.437793
not college grad#other  |   6.899757   1.300669     5.30   0.000     4.349098    9.450416
    college grad#white  |   10.57266    .273137    38.71   0.000     10.03703    11.10829
    college grad#black  |   11.39148   .5335905    21.35   0.000     10.34509    12.43787
    college grad#other  |   10.96014   1.789176     6.13   0.000     7.451503    14.46878
-----------------------------------------------------------------------------------------
```
- 计算结果表明：当其他变量处于均值水平时，当妇女没有大学毕业时，白人妇女的小时工资的预测边际值最大，为 `7.226`，而当妇女为大学毕业时，黑人妇女的小时工资的预测边际值最大，为 `11.391`。

- 我们使用 `marginsplot` 命令将计算结果用图的形式表示。`stata` 命令如下所示：
```stata
. marginsplot
```
- 输出图片如下所示：  
![图4 collgrad与race交乘项各个类别的妇女小时工资的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-9e23dedc445fb25c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 此外，我们还可以进一步计算在 `不同种族类别下(race)`，`大学毕业(collgrad=1)` 与 `非大学毕业(collgrad=0)` 的 `妇女小时工资 (wage)` 的预测边际值的差值是多少。于是，我们使用 `margins` 附加 `dydx`、`at` 与 `atmeans` 的选项来实现 。`stata` 命令和结果如下所示：

```stata
.    margins, dydx(collgrad) at(race=(1 2 3)) atmeans

Conditional marginal effects                    Number of obs     =      2,232
Model VCE    : OLS

Expression   : Linear prediction, predict()
dy/dx w.r.t. : 1.collgrad

1._at        : 1.industry      =    .0076165 (mean)
               2.industry      =    .0017921 (mean)
               3.industry      =    .0129928 (mean)
               4.industry      =    .1644265 (mean)
               5.industry      =    .0403226 (mean)
               6.industry      =    .1491935 (mean)
               7.industry      =    .0860215 (mean)
               8.industry      =    .0385305 (mean)
               9.industry      =    .0434588 (mean)
               10.industry     =    .0076165 (mean)
               11.industry     =    .3691756 (mean)
               12.industry     =     .078853 (mean)
               0.collgrad      =    .7629928 (mean)
               1.collgrad      =    .2370072 (mean)
               race            =           1

2._at        : 1.industry      =    .0076165 (mean)
               2.industry      =    .0017921 (mean)
               3.industry      =    .0129928 (mean)
               4.industry      =    .1644265 (mean)
               5.industry      =    .0403226 (mean)
               6.industry      =    .1491935 (mean)
               7.industry      =    .0860215 (mean)
               8.industry      =    .0385305 (mean)
               9.industry      =    .0434588 (mean)
               10.industry     =    .0076165 (mean)
               11.industry     =    .3691756 (mean)
               12.industry     =     .078853 (mean)
               0.collgrad      =    .7629928 (mean)
               1.collgrad      =    .2370072 (mean)
               race            =           2

3._at        : 1.industry      =    .0076165 (mean)
               2.industry      =    .0017921 (mean)
               3.industry      =    .0129928 (mean)
               4.industry      =    .1644265 (mean)
               5.industry      =    .0403226 (mean)
               6.industry      =    .1491935 (mean)
               7.industry      =    .0860215 (mean)
               8.industry      =    .0385305 (mean)
               9.industry      =    .0434588 (mean)
               10.industry     =    .0076165 (mean)
               11.industry     =    .3691756 (mean)
               12.industry     =     .078853 (mean)
               0.collgrad      =    .7629928 (mean)
               1.collgrad      =    .2370072 (mean)
               race            =           3

------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
0.collgrad   |  (base outcome)
-------------+----------------------------------------------------------------
1.collgrad   |
         _at |
          1  |   3.346222   .3197178    10.47   0.000     2.719244      3.9732
          2  |   5.444771   .5929386     9.18   0.000     4.281997    6.607545
          3  |   4.060384   2.212164     1.84   0.067    -.2777476    8.398516
------------------------------------------------------------------------------
Note: dy/dx for factor levels is the discrete change from the base level.
```
- 计算结果显示：`大学毕业(collgrad=1)` 黑人妇女的小时工资的预测边际值与 `非大学毕业(collgrad=0)` 黑人妇女的小时工资的预测边际值的差值是最大的，为 `5.444`，结果表明，上大学将在很大程度上提高黑人妇女工资。

- 同样的，我们使用 `marginsplot` 命令将计算结果用图的形式表示。`stata` 命令如下所示：
```stata
. marginsplot
```
- 输出图片如下所示：  
![图5 不同种族的大学毕业与非大学毕业的妇女小时工资的预测边际值的差别.png](https://upload-images.jianshu.io/upload_images/8108279-24a913fc1dca3385.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 3.2.2 类别变量与连续型变量交乘

- 影响妇女工资的因素较多，下面我们就来检验诸如 `hours (每周工作时间)` 与 `union (是否工会成员)` 这两个变量的 `交乘项` 是否会对妇女工资产生影响，其中 `hours` 为连续型变量， `union` 为类别变量。

- 使用 `stata` 的自带数据 `nlsw88.dta (1988年美国妇女小时工资)`，以 `wage (妇女的小时工资)` 作为被解释变量、以  `industry (行业类别)` 、 `union (是否工会成员)`、 `hours (每周工作小时数)`、`union (是否工会成员)` 与  `hours (每周工作小时数)` 的 `交乘项` 建立线性回归模型。

- 使用 `因子变量` 的语法格式，`i.union##c.hours` 表示在模型中既包括 `union` 与 `hours` 变量，还包括 `union` 与 `hours` 变量的 `交乘项`。`stata` 中的回归命令和结果如下所示：

```stata
.    reg wage i.industry c.hours##i.union

      Source |       SS           df       MS      Number of obs   =     1,864
-------------+----------------------------------   F(14, 1849)     =     19.48
       Model |  4165.55214        14  297.539439   Prob > F        =    0.0000
    Residual |  28235.7439     1,849  15.2708188   R-squared       =    0.1286
-------------+----------------------------------   Adj R-squared   =    0.1220
       Total |   32401.296     1,863      17.392   Root MSE        =    3.9078

------------------------------------------------------------------------------------------
                    wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------------------+----------------------------------------------------------------
                industry |
                 Mining  |   1.965311    2.98501     0.66   0.510    -3.889033    7.819655
           Construction  |   2.944549   1.427687     2.06   0.039     .1445001    5.744598
          Manufacturing  |   .8876583   1.149997     0.77   0.440    -1.367771    3.143088
 Transport/Comm/Utility  |    4.66455   1.206553     3.87   0.000     2.298201    7.030899
 Wholesale/Retail Trade  |  -.2176107    1.15411    -0.19   0.850    -2.481107    2.045886
Finance/Ins/Real Estate  |   2.714369   1.171926     2.32   0.021     .4159324    5.012805
    Business/Repair Svc  |   1.261646   1.238032     1.02   0.308    -1.166442    3.689734
      Personal Services  |  -1.602596   1.231961    -1.30   0.193    -4.018775    .8135844
  Entertainment/Rec Svc  |   1.454197   1.537525     0.95   0.344    -1.561271    4.469666
  Professional Services  |   1.454095   1.137893     1.28   0.201    -.7775962    3.685785
  Public Administration  |   2.658987   1.171222     2.27   0.023     .3619308    4.956043
                         |
                   hours |   .0565515   .0104406     5.42   0.000     .0360748    .0770282
                         |
                   union |
                  union  |   3.761049   .8990725     4.18   0.000     1.997745    5.524353
                         |
           union#c.hours |
                  union  |  -.0747591   .0226682    -3.30   0.001    -.1192171   -.0303012
                         |
                   _cons |   3.864411   1.193668     3.24   0.001     1.523332     6.20549
------------------------------------------------------------------------------------------
```
- 回归结果显示 `hours(每周工作小时数)` 的系数值显著为正，为 `0.056`，`union(是否工会成员)` 的系数值显著为正，为 `3.761`，而 `工会成员的每周工作小时(union#c.hours)` 的系数值显著为负，为 `-0.074`，表明  `hours (每周工作小时数)` 对 `wage (妇女的小时工资)` 的边际效应会受到 `union (是否工会成员)` 的影响；`union (是否工会成员)` 对 `wage (妇女的小时工资)` 的边际效应也会受到 `hours (每天工作小时数)` 的影响。

- 我们使用 `margins` 命令附加 `dydx` 选项与 `at` 选项来计算当妇女为工会成员或非工会成员时，`hours` 对 `wage` 的平均边际效应分别为多少。`stata` 中的命令和结果如下所示：
```stata
.    margins, dydx(hour) at(union=(0 1))

Average marginal effects                        Number of obs     =      1,864
Model VCE    : OLS

Expression   : Linear prediction, predict()
dy/dx w.r.t. : hours

1._at        : union           =           0
2._at        : union           =           1

------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
hours        |
         _at |
          1  |   .0565515   .0104406     5.42   0.000     .0360748    .0770282
          2  |  -.0182076    .020211    -0.90   0.368    -.0578463    .0214312
------------------------------------------------------------------------------
```
- 计算结果表明：当妇女为工会成员时，每周工作小时数增加 `1` 个单位，则小时工资下降`0.018`个单位，但在统计上不显著；当妇女为非工会成员时，每周工作小时增加 `1` 个单位，则小时工资将显著增加 `0.056` 个单位。

- 使用 `marginsplot` 命令将计算结果用图的形式表示。`stata` 命令如下所示：
```stata
. marginsplot
```
- 输出图片如下所示：  
![图6 是否工会成员的工作小时数对妇女小时工资的平均边际效应.png](https://upload-images.jianshu.io/upload_images/8108279-0881b24d0df7c691.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 我们还可以计算当妇女每周工作小时数不同时，`union` 对 `wage` 的边际效应分别为多少。可以使用 `margins` 命令附加 `dydx` 选项与 `at` 选项。在计算之前，我们需要事先知道 `hours` 变量的取值范围，可使用 `sum`命令查看。 `stata` 中的命令和结果如下所示：
```stata
.    keep if e(sample)
(382 observations deleted)
 
.    sum hours

    Variable |        Obs        Mean    Std. Dev.       Min        Max
-------------+---------------------------------------------------------
       hours |      1,864    37.62071    9.959845          1         80

.    margins, dydx(union) at(hours=(1(5)80))

Average marginal effects                        Number of obs     =      1,864
Model VCE    : OLS

Expression   : Linear prediction, predict()
dy/dx w.r.t. : 1.union

1._at        : hours           =           1
2._at        : hours           =           6
3._at        : hours           =          11
4._at        : hours           =          16
5._at        : hours           =          21
6._at        : hours           =          26
7._at        : hours           =          31
8._at        : hours           =          36
9._at        : hours           =          41
10._at       : hours           =          46
11._at       : hours           =          51
12._at       : hours           =          56
13._at       : hours           =          61
14._at       : hours           =          66
15._at       : hours           =          71
16._at       : hours           =          76

------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
0.union      |  (base outcome)
-------------+----------------------------------------------------------------
1.union      |
         _at |
          1  |    3.68629   .8771081     4.20   0.000     1.966064    5.406516
          2  |   3.312494   .7678999     4.31   0.000     1.806452    4.818536
          3  |   2.938699   .6600865     4.45   0.000     1.644105    4.233292
          4  |   2.564903   .5544822     4.63   0.000     1.477426     3.65238
          5  |   2.191107   .4526359     4.84   0.000     1.303376    3.078838
          6  |   1.817312   .3577712     5.08   0.000     1.115634     2.51899
          7  |   1.443516   .2771526     5.21   0.000     .8999512    1.987081
          8  |    1.06972   .2265375     4.72   0.000     .6254243    1.514017
          9  |   .6959248   .2269741     3.07   0.002     .2507724    1.141077
         10  |   .3221292   .2782222     1.16   0.247    -.2235335    .8677918
         11  |  -.0516665   .3591522    -0.14   0.886    -.7560529    .6527199
         12  |  -.4254621   .4541644    -0.94   0.349    -1.316191    .4652668
         13  |  -.7992578   .5560869    -1.44   0.151    -1.889882    .2913665
         14  |  -1.173053   .6617344    -1.77   0.076    -2.470879    .1247717
         15  |  -1.546849   .7695742    -2.01   0.045    -3.056175   -.0375234
         16  |  -1.920645   .8787996    -2.19   0.029    -3.644189   -.1971008
------------------------------------------------------------------------------
Note: dy/dx for factor levels is the discrete change from the base level.
```

- 计算结果表明：相对于非工会成员，随着 `hours` 取值的增加，工会成员对妇女工资的边际效应逐渐减小；当 `hours大于等于51小时`，工会成员的妇女工资的预测边际值比非工会成员的低。

- 为了更直观的显示结果，我们使用 `marginsplot` 命令进行绘图。`stata` 命令如下所示：
```stata
. marginsplot
```
- 输出图片如下所示：  
![图7 工会成员与非工会成员的工作小时对妇女小时工资的边际效应的差别.png](https://upload-images.jianshu.io/upload_images/8108279-a36d8d3de3fcfeb4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 3.2.3 连续型变量与连续型变量交乘

- 在实证研究中常常会分析两个连续型变量的交乘项的影响作用和变量的调节作用。例如，当车辆重量 `weight` 与每加仑汽油行驶的距离 `mpg` 增加时，汽车价格 `price` 会有所增加。现在，我们想进一步了解每加仑汽油行驶距离 `mpg` 能否调节车辆重量 `weight` 与汽车价格 `price` 之间的影响关系。于是，拟在回归模型中加入这两个 `连续变量` 的 `交乘项`，然后再计算当每加仑汽油行驶距离 `mpg` 取不同的数值时，车辆重量 `weight` 对汽车价格 `price` 的边际效应。

- 使用 `stata` 的自带数据 `auto.dta (1978年美国汽车数据)`，以 `price (汽车价格)` 作为被解释变量、以 `foreign (是否进口车)`、 `mpg (每加仑汽油能够行驶的英里数)`、`weight (汽车重量)`、 `mpg (每加仑汽油能够行驶的英里数)` 与  `weight (汽车重量)` 的 `交乘项` 作为解释变量建立线性回归模型。

- 使用 `因子变量` 的语法格式，`c.mpg##c.weight` 表示在模型中既包括 `mpg` 与 `weight` 变量，还包括 `mpg` 与 `weight` 变量的 `交乘项`。`stata` 中的回归命令和结果如下所示：

```stata
.    sysuse "auto.dta", clear
(1978 Automobile Data)

.    reg price foreign c.mpg##c.weight

      Source |       SS           df       MS      Number of obs   =        74
-------------+----------------------------------   F(4, 69)        =     18.96
       Model |   332566402         4  83141600.6   Prob > F        =    0.0000
    Residual |   302498994        69  4384043.39   R-squared       =    0.5237
-------------+----------------------------------   Adj R-squared   =    0.4961
       Total |   635065396        73  8699525.97   Root MSE        =    2093.8

--------------------------------------------------------------------------------
         price |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
---------------+----------------------------------------------------------------
       foreign |   3369.814   691.4218     4.87   0.000     1990.466    4749.163
           mpg |   292.8295   162.2982     1.80   0.076    -30.94655    616.6056
        weight |   5.382755   1.198909     4.49   0.000     2.990997    7.774512
               |
c.mpg#c.weight |  -.1189117   .0636245    -1.87   0.066    -.2458392    .0080157
               |
         _cons |  -10105.04   4023.204    -2.51   0.014    -18131.11   -2078.967
--------------------------------------------------------------------------------
```

- 回归结果显示 `mpg` 与 `weight` 的系数值显著为正，而 `c.mpg#c.weight` 的系数值显著为负 `-0.118`，表明 `mpg (每加仑汽油能够行驶的英里数)` 能够调节 `weight (汽车重量)` 对 `汽车价格 (price)` 的边际效应。

- 下面，我们计算当 `mpg (每加仑汽油能够行驶的英里数)` 取不同数值时，`weight (汽车重量)` 对 `price (汽车价格)` 的边际效应。使用 `margins` 命令附加 `dydx` 选项与 `at` 选项来计算。首先，需要知道 `mpg (每加仑汽油能够行驶的英里数)` 的取值范围，因此，先使用 `sum` 命令查看该变量的基本统计量，再使用 `margins` 命令附加 `dydx` 选项与 `at` 选项。`stata` 中的命令和结果如下所示：

```stata
.    keep if e(sample)
(0 observations deleted)

.    sum mpg  //查看 mpg 的基本统计量

    Variable |        Obs        Mean    Std. Dev.       Min        Max
-------------+---------------------------------------------------------
         mpg |         74     21.2973    5.785503         12         41

.    margins, dydx(weight) at(mpg=(12(2)41))

Average marginal effects                        Number of obs     =         74
Model VCE    : OLS

Expression   : Linear prediction, predict()
dy/dx w.r.t. : weight

1._at        : mpg             =          12
2._at        : mpg             =          14
3._at        : mpg             =          16
4._at        : mpg             =          18
5._at        : mpg             =          20
6._at        : mpg             =          22
7._at        : mpg             =          24
8._at        : mpg             =          26
9._at        : mpg             =          28
10._at       : mpg             =          30
11._at       : mpg             =          32
12._at       : mpg             =          34
13._at       : mpg             =          36
14._at       : mpg             =          38
15._at       : mpg             =          40

------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
weight       |
         _at |
          1  |   3.955814   .6732094     5.88   0.000     2.612798     5.29883
          2  |    3.71799   .6344514     5.86   0.000     2.452294    4.983686
          3  |   3.480167   .6198636     5.61   0.000     2.243573    4.716761
          4  |   3.242343   .6311243     5.14   0.000     1.983285    4.501402
          5  |    3.00452   .6669255     4.51   0.000      1.67404       4.335
          6  |   2.766696   .7236338     3.82   0.000     1.323086    4.210306
          7  |   2.528873   .7967979     3.17   0.002     .9393044    4.118441
          8  |   2.291049   .8823336     2.60   0.011      .530842    4.051257
          9  |   2.053226   .9769968     2.10   0.039     .1041705    4.002281
         10  |   1.815402   1.078387     1.68   0.097    -.3359202    3.966725
         11  |   1.577579   1.184777     1.33   0.187    -.7859873    3.941145
         12  |   1.339755   1.294937     1.03   0.304    -1.243573    3.923084
         13  |   1.101932   1.407981     0.78   0.437    -1.706913    3.910777
         14  |   .8641083   1.523268     0.57   0.572    -2.174727    3.902944
         15  |   .6262849   1.640324     0.38   0.704    -2.646072    3.898641
------------------------------------------------------------------------------
```
- 为了使结果更加直观的显示出来，可使用 `marginsplot` 命令进行绘图。`stata` 命令和结果如下所示：

```stata
marginsplot
```
- 输出图片如下：  
![图8 mpg取值不同时，weight对price的平均边际效应.png](https://upload-images.jianshu.io/upload_images/8108279-d7d482f720171a55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 计算结果表明：当 `mpg` 小于28时，随着 `mpg` 增加，`weight` 对 `price` 的边际效应逐渐减小且在统计上显著；当 `mpg` 大于28时，`weight` 对 `price` 的边际效应逐渐减小在统计上不显著。


### 3.3 非线性模型案例 (Logit Model)
- 由于在非线性模型的回归结果中，例如 `Logit Model`，自变量的系数值不能直接代表该变量对因变量的边际效应值，因此，我们需要借助 `margins` 命令来计算边际效应。

- 使用 `stata` 的自带数据 `auto.dta (1978年美国汽车数据)`，以 `foreign (是否进口车)` 作为被解释变量、以 `mpg (每加仑汽油能够行驶的英里数)`、`weight (汽车重量)` 作为解释变量建立Logit回归模型。`stata` 中的回归命令和结果如下所示：

```stata
.    sysuse "auto.dta", clear
(1978 Automobile Data)

.    logit foreign mpg weight

Iteration 0:   log likelihood =  -45.03321  
Iteration 1:   log likelihood = -29.238536  
Iteration 2:   log likelihood = -27.244139  
Iteration 3:   log likelihood = -27.175277  
Iteration 4:   log likelihood = -27.175156  
Iteration 5:   log likelihood = -27.175156  

Logistic regression                             Number of obs     =         74
                                                LR chi2(2)        =      35.72
                                                Prob > chi2       =     0.0000
Log likelihood = -27.175156                     Pseudo R2         =     0.3966

------------------------------------------------------------------------------
     foreign |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
         mpg |  -.1685869   .0919175    -1.83   0.067    -.3487418     .011568
      weight |  -.0039067   .0010116    -3.86   0.000    -.0058894    -.001924
       _cons |   13.70837   4.518709     3.03   0.002     4.851859    22.56487
------------------------------------------------------------------------------
```
- 回归结果显示 `mpg` 与 `weight` 的系数值显著为负，表明当车辆的 `mpg` 与 `weight` 增加时，该车辆是进口车的概率减小。但从这两个变量的系数值无法直接看出 `mpg` 与 `weight` 对 `车辆是进口车的概率` 的边际效应。于是，我们可以使用 `margins` 命令附加 `dydx` 选项来进行计算。`stata` 中的命令和结果如下所示：

```stata
. margins, dydx(mpg)

Average marginal effects                        Number of obs     =         74
Model VCE    : OIM

Expression   : Pr(foreign), predict()
dy/dx w.r.t. : mpg

------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      z    P>|z|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
         mpg |  -.0197187   .0096987    -2.03   0.042    -.0387277   -.0007096
------------------------------------------------------------------------------

. margins, dydx(weight)

Average marginal effects                        Number of obs     =         74
Model VCE    : OIM

Expression   : Pr(foreign), predict()
dy/dx w.r.t. : weight

------------------------------------------------------------------------------
             |            Delta-method
             |      dy/dx   Std. Err.      z    P>|z|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
      weight |  -.0004569   .0000571    -8.01   0.000    -.0005688   -.0003451
------------------------------------------------------------------------------
```
- 计算结果显示：当 `mpg` 增加 `1` 个单位时，车辆为进口车的概率减少 `1.97%`；当 `weight` 增加 `1` 个单位时，车辆为进口车的概率减少 `0.04%`。

## 4. marginscontplot命令及举例
### 4.1 命令简介 
- `marginscontplot` 命令可以计算当 `连续变量` 取值不同、其他变量取均值时，被解释变量的预测边际值并绘制图形（同样适用于分类变量）。该命令可适用于绝大部分的回归命令，诸如 `regress` 、`logit`、 `probit` 、`poisson`、 `glm`、`stcox`、 `streg`、 `xtreg` 等。

- `marginscontplot` （可简写为 `mcp`）命令将 `margins` 命令的计算功能与 `marginsplot` 命令的绘图功能整合在一起，并且能够识别 `连续变量` 的取值范围，无须在计算之前使用 `sum` 命令确定 `该连续变量的取值范围` 。因此，`marginscontplot` 命令使用起来更加便捷。

- 更为方便的是，当回归模型中的 `连续变量` 进行了线性或非线性的数值转换后，`marginscontplot` 命令可以在图形的坐标轴上显示连续变量的 `原始取值`。

### 4.2 语法

- `marginscontplot` 命令的语法如下所示：

```stata
{marginscontplot|mcp} xvar1 [(xvar1a [xvar1b ...])] [xvar2 [(xvar2a [xvar2b ...])]] [if] [in] [, options]
```

- 命令含义：
- 当分析一个变量 (`xvar1`) 时，由 `at1()` 选项和 `var1()` 选项来确定；当分析两个变量时 (`xvar1` 与  `xvar2`)，还需加入 `var2()` 与 `at2()` 选项。

![图9 marginscontplot命令解释.png](https://upload-images.jianshu.io/upload_images/8108279-a642adeab7d6289d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 4.3 安装
- 由于 `marginscontplot` 命令是由 Patrick Royston 编写的外部命令 [(Patrick Royston, 2013)](https://www.stata-journal.com/article.html?article=gr0056)，因此使用前需要安装该命令。
- 首先在命令窗口中搜索 `marginscontplot`， 点击搜索结果中的安装包链接进行安装后即可使用。

### 4.4 使用案例

#### 4.4.1  单个变量

- 下面，仍以研究妇女工资的决定因素为例进行说明。在3.2.2节中，我们使用了 `stata` 的自带数据 `nlsw88.dta (1988年美国妇女小时工资)`，以 `wage (妇女的小时工资)` 作为被解释变量、以 `industry (行业类别)`、`union (是否工会成员)`、 `hours (每周工作小时数)`、`union (是否工会成员)` 与  `hours (每周工作小时数)` 的 `交乘项` 建立线性回归模型。`stata` 中的回归命令和结果见3.2.2节所述。

- 现在，我们使用 `marginscontplot` 命令来计算当其他变量取均值时， `hours` 取不同值时， `wage` 的预测边际值，附加95%的置信区间并呈现图形。此时，无需提前使用 `sum` 命令查看 `hours` 变量的取值范围，也无需在计算完预测边际值之后使用 `marginsplot` 命令绘图，直接在 `stata` 中使用 `marginscontplot` 命令即可完成计算和绘图，命令和结果如下所示：

```stata
.    sysuse "nlsw88.dta", clear
(NLSW, 1988 extract)

.    reg wage i.industry c.hours##i.union

      Source |       SS           df       MS      Number of obs   =     1,864
-------------+----------------------------------   F(14, 1849)     =     19.48
       Model |  4165.55214        14  297.539439   Prob > F        =    0.0000
    Residual |  28235.7439     1,849  15.2708188   R-squared       =    0.1286
-------------+----------------------------------   Adj R-squared   =    0.1220
       Total |   32401.296     1,863      17.392   Root MSE        =    3.9078

------------------------------------------------------------------------------------------
                    wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------------------+----------------------------------------------------------------
                industry |
                 Mining  |   1.965311    2.98501     0.66   0.510    -3.889033    7.819655
           Construction  |   2.944549   1.427687     2.06   0.039     .1445001    5.744598
          Manufacturing  |   .8876583   1.149997     0.77   0.440    -1.367771    3.143088
 Transport/Comm/Utility  |    4.66455   1.206553     3.87   0.000     2.298201    7.030899
 Wholesale/Retail Trade  |  -.2176107    1.15411    -0.19   0.850    -2.481107    2.045886
Finance/Ins/Real Estate  |   2.714369   1.171926     2.32   0.021     .4159324    5.012805
    Business/Repair Svc  |   1.261646   1.238032     1.02   0.308    -1.166442    3.689734
      Personal Services  |  -1.602596   1.231961    -1.30   0.193    -4.018775    .8135844
  Entertainment/Rec Svc  |   1.454197   1.537525     0.95   0.344    -1.561271    4.469666
  Professional Services  |   1.454095   1.137893     1.28   0.201    -.7775962    3.685785
  Public Administration  |   2.658987   1.171222     2.27   0.023     .3619308    4.956043
                         |
                   hours |   .0565515   .0104406     5.42   0.000     .0360748    .0770282
                         |
                   union |
                  union  |   3.761049   .8990725     4.18   0.000     1.997745    5.524353
                         |
           union#c.hours |
                  union  |  -.0747591   .0226682    -3.30   0.001    -.1192171   -.0303012
                         |
                   _cons |   3.864411   1.193668     3.24   0.001     1.523332     6.20549
------------------------------------------------------------------------------------------

.    marginscontplot hours, ci 
```

![图10 当其他变量取均值，hours取不同值时，wage的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-6d44841fe2b73c04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 我们还可以将 `hours` 变量的取值范围均匀的分为若干个区间，计算在各区间节点上的 `wage` 的预测边际值。例如，计算 `hours` 取4个不同数值时 `wage` 的预测边际值，并且这4个数值均匀分布于 `hours` 变量的取值范围中，`stata` 中的命令和结果如下所示：

```stata
.    marginscontplot hours, ci var1(4)
```

![图11 当其他变量取均值，hours取值为4个数值时（均匀分布于hours变量的取值范围）wage的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-abdebdfc76e9124c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 此外，我们还可以指定计算 `hours` 变量的取值范围与间隔。例如，指定 `hours` 变量的取值范围为10至40，间隔为5，`stata` 中的命令和结果如下所示：

```stata
.    marginscontplot hours, ci at1(10(5)40)
```

![图12 当其他变量取均值，hours取值为10至50之间时，wage的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-d91ec74dae01f699.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 4.4.2  两个变量

在3.2.2节案例中，由于加入了 `union` 与 `hours` 的交乘项并且该系数在统计上显著，由此，我们还想分别计算当 `hours` 取值不同时，工会成员与非工会成员的 `wage` 的预测边际值。因此，需要在 `marginscontplot` 命令中加入两个变量，`stata` 中的命令和结果如下所示：

```stata
.    marginscontplot hours union, ci at1(1(5)80)
*-或写为
.    marginscontplot hours union, ci at1(1(5)80) at2(0 1)
```

![图13 当其他变量取均值，hours取不同值时，工会与非工会成员的wage的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-78c0305404225206.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


#### 4.4.3  对变量的数值转换

- 假设妇女工资 `wage` 与每周工作小时数 `hours` 变量取对数之后（记为 `lnhours`）存在线性关系。现需要计算 `lnhours` 变量对 `wage` 的影响作用并在图形的坐标轴上显示 `hours` 变量的原始取值。于是，我们以 `wage` 作为被解释变量，以 `lnhours` 作为解释变量，并以 `industry(行业类别)`、 `collgrad (是否大学毕业)` 、 `union (是否工会成员)` 作为控制变量，建立线性回归模型。`stata` 中的命令和结果如下所示：

```stata
.    gen lnhours = ln(hours)
(4 missing values generated)

.    reg wage i.industry collgrad union lnhours

      Source |       SS           df       MS      Number of obs   =     1,864
-------------+----------------------------------   F(14, 1849)     =     43.37
       Model |  8010.08706        14  572.149076   Prob > F        =    0.0000
    Residual |   24391.209     1,849  13.1915679   R-squared       =    0.2472
-------------+----------------------------------   Adj R-squared   =    0.2415
       Total |   32401.296     1,863      17.392   Root MSE        =     3.632

------------------------------------------------------------------------------------------
                    wage |      Coef.   Std. Err.      t    P>|t|     [95% Conf. Interval]
-------------------------+----------------------------------------------------------------
                industry |
                 Mining  |   2.610535   2.774693     0.94   0.347    -2.831325    8.052395
           Construction  |   2.840079   1.326651     2.14   0.032     .2381882    5.441971
          Manufacturing  |   1.231468   1.069077     1.15   0.250     -.865257    3.328193
 Transport/Comm/Utility  |   4.943764   1.121633     4.41   0.000     2.743963    7.143564
 Wholesale/Retail Trade  |  -.1333967    1.07259    -0.12   0.901    -2.237011    1.970218
Finance/Ins/Real Estate  |   2.806101   1.089317     2.58   0.010     .6696813    4.942521
    Business/Repair Svc  |   .9528829   1.150232     0.83   0.408    -1.303006    3.208772
      Personal Services  |  -1.255364   1.144976    -1.10   0.273    -3.500946    .9902172
  Entertainment/Rec Svc  |   1.157725   1.428893     0.81   0.418    -1.644688    3.960138
  Professional Services  |   .4944097   1.058875     0.47   0.641    -1.582307    2.571126
  Public Administration  |   2.404413   1.088813     2.21   0.027     .2689805    4.539845
                         |
                collgrad |   3.701502   .2091694    17.70   0.000     3.291269    4.111735
                   union |   .6846933   .2044257     3.35   0.001     .2837639    1.085623
                 lnhours |   .4712976   .2119332     2.22   0.026     .0556441    .8869511
                   _cons |   3.742689   1.285111     2.91   0.004     1.222268     6.26311
------------------------------------------------------------------------------------------
```

- 现在使用 `marginscontplot` 命令计算当其他变量取均值、对均匀分布于 `hours` 变量的取值范围中的20个值取对数时，`wage` 的预测边际值。此时，需要在命令中加入原始变量 `hours` 与变量取对数后 `lnhours` 的对应关系，`stata` 中的命令和结果如下所示：

```stata
.    keep if e(sample)
(382 observations deleted)

.    sum hours

    Variable |        Obs        Mean    Std. Dev.       Min        Max
-------------+---------------------------------------------------------
       hours |      1,864    37.62071    9.959845          1         80

.    range h r(min) r(max) 20
(1,844 missing values generated)

.    gen lnh = ln(h)
(1,844 missing values generated)

.    marginscontplot hours(lnhours), ci var1(h(lnh))
```
![图14 对hours变量取对数后wage的预测边际值.png](https://upload-images.jianshu.io/upload_images/8108279-ed37c8467e2e163f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 5. 相关阅读资料

- [Marginal means, adjusted predictions, and marginal effects](https://www.stata.com/features/overview/marginal-analysis/)
- [The Stata Blog » margins](https://blog.stata.com/tag/margins/)
- [The Stata Blog » marginal effects](https://blog.stata.com/tag/marginal-effects/)
- [Margins plots](https://www.stata.com/features/overview/margins-plots/#)
- [Trouble at the margins](http://www.drbanderson.com/2017/07/30/trouble-at-the-margins/)
- [In the spotlight: Estimating, graphing, and interpreting interactions using margins](https://www.stata.com/stata-news/news31-3/spotlight/)
- [In the spotlight: Visualizing continuous-by-continuous interactions with margins and twoway contour](https://www.stata.com/stata-news/news32-1/spotlight/)
- [Effects of nonlinear models with interactions of discrete and continuous variables: Estimating, graphing, and interpreting](https://blog.stata.com/2016/07/12/effects-for-nonlinear-models-with-interactions-of-discrete-and-continuous-variables-estimating-graphing-and-interpreting/)
- [Ben Jann, 2013, PPT, Predictive Margins and Marginal Effects in Stata](https://www.stata.com/meeting/germany13/abstracts/materials/de13_jann.pdf)


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

> [……Stata 连享会精彩推文……](https://blog.csdn.net/arlionn/article/details/82746992)
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-95f9bf00c48aa4f1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
