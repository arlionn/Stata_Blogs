

> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
> &emsp;&emsp;
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/Stata_training/blob/master/README.md) || [精彩推文2](https://github.com/arlionn/Stata/blob/master/README.md)

---
注：该文已发表。    
 连玉君, 廖俊平, 2017, 如何检验分组回归后的组间系数差异?, 郑州航空工业管理学院学报 35, 97-109\. [PDF 原文下载](https://gitee.com/arlionn/jianshu/raw/master/PDF%E4%B8%8B%E8%BD%BD/%E8%BF%9E%E7%8E%89%E5%90%9B-2017-%E5%A6%82%E4%BD%95%E6%A3%80%E9%AA%8C%E5%88%86%E7%BB%84%E5%9B%9E%E5%BD%92%E5%90%8E%E7%9A%84%E7%BB%84%E9%97%B4%E7%B3%BB%E6%95%B0%E5%B7%AE%E5%BC%82.pdf)  || [PDF-万方](https://gitee.com/arlionn/Stata/attach_files/download?i=136991&u=http%3A%2F%2Ffiles.git.oschina.net%2Fgroup1%2FM00%2F03%2FAC%2FPaAvDFr71kCAThGsAAft58PjYKc432.pdf%3Ftoken%3D96a722e84d95bcebad7e18b6c1acfca5%26ts%3D1526453824%26attname%3D%25E8%25BF%259E%25E7%258E%2589%25E5%2590%259B_%25E5%25BB%2596%25E4%25BF%258A%25E5%25B9%25B3_2017_%25E5%25A6%2582%25E4%25BD%2595%25E6%25A3%2580%25E9%25AA%258C%25E5%2588%2586%25E7%25BB%2584%25E5%259B%259E%25E5%25BD%2592%25E5%2590%258E%25E7%259A%2584%25E7%25BB%2584%25E9%2597%25B4%25E7%25B3%25BB%25E6%2595%25B0%25E5%25B7%25AE%25E5%25BC%2582.pdf)
---

## 1. 问题背景

实证分析中，经常需要对比分析两个子样本组的系数是否存在差异。 例如，在公司金融领域，研究薪酬激励是否有助于提升业绩时，模型设定为：

$$ROE_{it} = a_{i} + Salary_{it-1}\cdot\beta + Controls_{it-1}\cdot \gamma + u_{it}$$

关注的重点是系数 $\beta$。 我们经常把样本分成**"国有企业(SOE)”**和**“民营企业(PRI)”**两个样本组，继而比较$\beta_{SOE}$与$\beta_{PRI}$是否存在差异。通常认为，民营企业的薪酬激励更有效果，即 $\beta_{SOE}<\beta_{PRI}$。

如果两个样本组中的模型设定是相同的，则两组之间的系数大小是可以比较的，而且这种比较在多数实证分析中都是非常必要的。

### 1.1 两个文献中的例子

>**例1：** Cleary, S., 1999, The relationship between firm investment and financial status, Journal of Finance, 54 (2): 673-692. Tabel IV
>![](https://upload-images.jianshu.io/upload_images/7692714-aade9d37767668c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1000/format/webp)

> **例2：** 连玉君, 彭方平, 苏治, 2010, 融资约束与流动性管理行为, 金融研究, (10): 158-171. 表2.
>![连玉君, 彭方平, 苏治, 2010, 融资约束与流动性管理行为, 金融研究, (10): 158-171. 表2.](https://upload-images.jianshu.io/upload_images/7692714-94d4450106d253d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600/format/webp)

### 1.2 请问：2 > 1 吗？

下面使用 Stata 软件自带的数据集 **nlsw88.dta** 来对此问题进行初步说明。

这份数据包含了 1988 年采集的 2246 个妇女的资料，核心变量包括：小时工资 `wage`，每周工作时数 `hours`， 种族 `race` 等变量。

我们想研究的是妇女的工资决定因素。

最为关注的是白人和黑人（相当于把原始数据分成了两个样本组：白人组和黑人组）的工资决定因素是否存在差异。

分析的重点集中于工龄（`ttl_exp`）和婚姻状况 (`married`) 这两个变量的系数在两组之间是否存在显著差异。

下面是分组执行 OLS 回归的命令和结果：
```Stata
. sysuse "nlsw88.dta", clear
. gen agesq = age*age
*-分组虚拟变量
. drop if race==3
. gen black = 2.race
. tab black 
*-删除缺漏值 
. global xx "ttl_exp married south hours tenure age* i.industry"
. reg wage $xx i.race
. keep if e(sample)   
*-分组回归
. global xx "ttl_exp married south hours tenure age* i.industry"
. reg wage $xx if black==0 
. est store White
. reg wage $xx if black==1 
. est store Black
 *-结果对比
. local m "White Black"
. esttab `m', mtitle(`m') b(%6.3f) nogap drop(*.industry) ///
	 s(N r2_a) star(* 0.1 ** 0.05 *** 0.01) 
```

结果：
> Table1: 白人组和黑人组工资影响因素差异对比
```Stata
------------Table 1-------------------
                (1)             (2)   
              White           Black   
--------------------------------------
ttl_exp       0.251***     0.269***
             (6.47)          (4.77)   
married      -0.737**      0.091   
            (-2.31)          (0.23)   
... (output ommited) ...
--------------------------------------
N          1615.000         572.000   
r2_a          0.112           0.165   
--------------------------------------
t statistics in parentheses
* p<0.1, ** p<0.05, *** p<0.01
```

可以看到，`ttl_exp` 变量在 `[white 组]` 和 `[black 组]` 的系数分别为 0.251 和 0.269， 二者都在 1% 水平上显著异于零。

**问题在于：** 我们能说 0.269 比 0.251 大吗？

从统计意义上来看，答案显然没有那么明确（小学五年级的小朋友会觉得这根本不是个问题！）。

相对而言，若把注意力放在 `married` 这个变量上，或许更容易判断二者的差异是否显著。因为，`_b[married]_white` (白人组的 married 估计系数) 为 `-0.737**`，而 `_b[married]_black` 为 `0.091` —— 前者在 5% 水平上显著为负，而后者不显著。

即便如此，我们仍然无法直接作出结论：$\beta_{married}^{White} < \beta_{married}^{Black}$，因为二者的置信区间尚有重叠：

```Stata
----------------------------------------
             White         Black        
----------------------------------------
 ttl_exp 
---------
   beta     0.251***    0.269***  
  95% CI  [0.17, 0.33]   [0.16, 0.38]   
----------------------------------------
 married 
---------
   beta     -0.737**      0.091      
  95% CI  [-1.36, -0.11]  [-0.69, 0.87] 
----------------------------------------
```
我们也可以使用 `coefplot` 命令更为直观地呈现上述关系。下图中蓝色方框部分的系数是两组估计系数置信区间的重叠区域。由于它的存在，我们无法确信 $\beta_{married}^{White} < \beta_{married}^{Black}$。
```stata
. global xline "xline(0,lp(dash) lc(red*0.5))"
. coefplot White Black, keep(ttl_exp married) ///
           nolabels $xline ciopt(recast(rcap))
. graph export "Fig01.png", replace
```
>![组间系数及置信区间](https://upload-images.jianshu.io/upload_images/7692714-12424632e3e49ef7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 2. 组间系数差异的检验方法

下面我们介绍三种检验组间系数差异的方法：
>- **方法 1：** 引入交叉项（Chow 检验）
>- **方法 2：** 基于似无相关模型的检验方法 (suest)
>- **方法 3：** 费舍尔组合检验（Permutation test）

---
### 2.1 方法 1: 引入交叉项

#### A. 基本思想

引入交乘项来检验某个或某几个变量的系数是否存在组间差异，只需在普通线性回归中加入交乘项即可。这是文献中最常用的方法，执行起来也最简单。

下面以检验 `ttl_exp` 在两组之间的系数是否存在显著差异为例进行说明。引入一个虚拟变量 $D_i$，若某个妇女是黑人，则 $D_i = 1$，否则 $D_i = 0$。在如下命令中，black 变量即为这里的 $D_i$ 。模型设定为：

$$
(1) \quad  Wage_{i} = \alpha + \gamma\cdot D_i + \beta\cdot ttl\_exp_i + \delta(D_i \times ttl\_exp_i) + Controls_i \cdot \gamma + \epsilon_{i}
$$

这是最基本的包含虚拟变量，以及虚拟变量与一个连续变量交乘项的情形。

显然，对于白人组而言，$D_i = 0$，则 (1) 式可以写为：

$$
(1a)  \quad  Wage_{i} = \alpha + \beta\cdot ttl\_exp_i + Controls_i \cdot \gamma + \epsilon_{i}
$$

对于黑人组，$D_i=1$，(1) 式可以写为：

$$
(1b)  \quad  Wage_{i} = (\alpha + \gamma) + (\beta+\delta)\cdot ttl\_exp_i + Controls_i \cdot \gamma + \epsilon_{i}
$$

由此可见，在 (1) 式中，参数 $\gamma$ 和 $\delta$ 分别反映了黑人组相对于白人组的截距和斜率差异。我们关注的是参数 $\delta$，它反映了 `ttl_exp` 这个变量在两个样本组中的系数差异。

因此，检验 `ttl_exp` 在两组之间的系数是否存在显著差异就转变为检验 $H_0 : \delta = 0$。

#### B. Stata 实现

对于上述妇女工资决定因素的例子而言，通过引入交叉项来检验组间系数差异的命令如下：
```Stata 
dropvars ttl_x_black marr_x_black
global xx "ttl_exp married south hours tenure age* i.industry" //Controls
gen ttl_x_black = ttl_exp*black  //交乘项
reg wage black ttl_x_black $xx   //全样本回归+交乘项
```

为节省篇幅，仅列出最关键的结果如下：

```Stata 
. reg wage black ttl_x_black $xx   //全样本回归+交乘
 Number of obs =  2,187
 R-squared     = 0.1379
 Adj R-squared = 0.1299
--------------------------------------------------
        wage |      Coef.      SE     t    P>|t|  
-------------+------------------------------------
       black |     -0.819    0.802  -1.02  0.307  
 ttl_x_black |     -0.018    0.059  -0.31  0.756  
     ttl_exp |      0.254    0.035   7.23  0.000  
     married |     -0.465    0.253  -1.84  0.066  
... (output omitted) ...
--------------------------------------------------
```

交乘项 `[ttl_x_black]` 的系数为 `-.01818`, 对应的 p-value 为 `0.756`，表明 `[ttl_exp]` 的系数在两组之间并不存在显著差异。

我们也可以不事先生成交乘项，而直接采用 Stata 的因子变量表达式(参见 [Stata因子变量的全攻略](https://www.jianshu.com/p/16b08797e591))，得到完全相同的结果：
```Stata 
reg wage i.black ttl_exp i.black#c.ttl_exp $xx
```

或采用如下更为简洁的方式 (详情参见 `help fvvarlist`)：
```Stata 
reg wage i.black##c.ttl_exp $xx 
```

然而，需要特别强调的是，在上述检验过程中，我们无意识中施加了一个非常严格的假设条件：只允许变量 `[ttl_exp]` 的系数在两组之间存在差异，而其他控制变量(如 `married`, `south`, `hours` 等) 的系数则不随组别发生变化。

这显然是一个非常严格的假设。因为，从 -Table 1- 的结果来看, `married`， `south`, `hours` 等变量在两组之间的差异都比较明显。

为此，我们放松上述假设，允许 `married`，`south`, `hours` 等变量在两组之间的系数存在差异：

```Stata 
. reg wage i.black i.black#(c.ttl_exp i.married i.south c.hours) $xx    //Model 2
```

在这种相对灵活的设定下，`[ttl_exp]` 的系数为 **-0.016147** ，相应的 **p-value=0.787**，依然不显著。

当然，我们也可以采用更为灵活的方式：允许所有的变量在两组之间都存在系数差异（注意：所有离散变量前都要加 `i.` 前缀，否则将被视为连续变量进行处理（对于取值为0/1的虚拟变量，可以省略前缀 `i.`）；连续变量则需加 `c.` 前缀）：

```Stata 
. global xx "c.ttl_exp married south c.hours c.tenure c.(age*) i.industry"
. reg wage i.black##($xx)                                               //Model 3
```

这其实就是大名鼎鼎的 Chow test （邹检验），可以用 `chowtest` 命令快捷地完成。

#### C. 引入交叉项法的假设条件

通过引入交叉项来检验组间系数差异虽然操作上非常简洁，但需要注意这一方法背后隐含的假设条件（为了便于说明，重新将 (1) 式列出）：

$$(1) \quad Wage_{i} = \alpha + \gamma\cdot D_i + \beta\cdot ttl\_exp_i + \delta(D_i \times ttl\_exp_i) + Controls_i \cdot \gamma + \epsilon_{i}$$

>-   **A1:** 所有控制变量的系数在两组之间无差异，即 $\gamma^{White} = \gamma^{Black} $;
>- **A2:** 两组的干扰项具有相同的分布（因为估计时是将两组样本混合在一起进行估计的），即 $ \epsilon^{Wight} \sim N(0, \sigma^2)$ , 且 $\epsilon^{Black} \sim N(0, \sigma^2) $，换言之，$\sigma^2_{White} =\sigma^2_{Black}$ 。

因此，当其它变量的系数在两组之间也存在明显差异（A1 不满足），或存在异方差（A2 不满足）时，上述检验方法得到的结果都存在问题。

#### D. 解决办法
  -  对于 **A1**，实际操作过程中，可以通过引入更多的交乘项来放松 A1，如上文提到的 Model 2 或 Model 3。
  -  对于 **A2**， 则可以在上述回归分析过程中加入 `vce(robust)` 选项，以便允许干扰项存在异方差；或加入 `vce(cluster varname)` 以便得到聚类调整后的稳健型标准误。当然，也可以在模型中允许二维聚类标准误，此时可以使用 `vce2way` 等命令。

最后需要说明的是，虽然在上述范例中，我们是以基于截面数据的 OLS 回归为例的，但这一方法也适用于其他命令，如针对面板数据的 `xtreg`, 针对离散数据的 `logit`, `probit` 等。

---
### 2.2 方法 2: SUEST (基于似无相关模型 SUR 的检验)

#### A. 基本思想

顾名思义，所谓的似无相关模型（seemingly unrelated regression）其实就是表面上看起来没有关系，但实质上有关系的两个模型。这听起来有点匪夷所思。这种“实质上”的关系其实是假设白人组和黑人组的干扰项彼此相关。为了表述方便，将白人和黑人组的模型简写如下：

$$
(2a) \quad y_{1i} = x_{1i}\,\beta_{1} + \epsilon_{1i}, \quad i=1,2,\cdots, N_1, 白人组
$$

$$
(2b) \quad y_{2j} = x_{2j}\beta_{2} + \epsilon_{2j} , \quad j=1,2,\cdots, N_2, 黑人组
$$

若假设 $corr( \epsilon_{1}, \epsilon_{2})=0$，则我们可以分别对白人组和黑人组进行 OLS 估计。然而，虽然白人和黑人种族不同，但所处的社会和法律环境，面临的劳动法规都有诸多相似之处，使得二者的干扰项可能相关，即 $corr( \epsilon_{1}, \epsilon_{2})\neq 0$ 。此时，对两个样本组执行联合估计（GLS）会更有效率（详见 Greene (2012, Econometric analysis, 7th ed, 292–304)）。

执行完 SUR 估计后，我们就可以对两组之间的系数差异进行检验了。

从上面的原理介绍，可以看出，基于 SUR 估计进行组间系数差异检验时，假设条件比第一种方法要宽松一些：
- 其一，在估计过程中，并未预先限定白人组和黑人组各个变量的系数一定要相同，因此在 (2) 式中，我们分别用 $\beta_1$ 和 $\beta_2$ 表示白人组和黑人组各个变量的系数向量；
- 其二，两个组的干扰项可以有不同的分布，即 $\epsilon_1 \sim N(0, \sigma_1^2)$ , $\epsilon_2 \sim N(0, \sigma_2^2)$ ，且允许二者的干扰项相关，$corr( \epsilon_{1}, \epsilon_{2})\neq 0$。

#### B. Stata 实现方法

我们可以采用两种方法来执行似无相关检验：一是使用 Stata 的官方命令 `suest`；二是使用外部命令 `bdiff`。后者语法较为简洁。

##### B1. 基于 `suest` 命令的检验

在 Stata 中执行上述检验的步骤为：
- **Step 1：** 分别针对白人组和黑人组进行估计（不限于 OLS 估计，可以执行 Logit, Tobit 等估计），存储估计结果；
- **Step 2：** 使用 `suest` 命令执行 SUR 估计；
- **Step 3：** 使用 `test` 命令检验组间系数差异。

范例如下：
```Stata
*-Step1: 分别针对两个样本组执行估计
  reg wage $xx if black==0 
  est store w  //white
  reg wage $xx if black==1 
  est store b  //black
*-Step 2: SUR
  suest w b
*-Step 3: 检验系数差异
  test [w_mean]ttl_exp = [b_mean]ttl_exp 
  test [w_mean]married = [b_mean]married  
  test [w_mean]south = [b_mean]south
```

Step 2 的结果如下（为便于阅读，部分变量的系数未呈现）：

```Stata 
.  suest w b

Simultaneous results for w, b
                             Number of obs  = 2,187
---------------------------------------------------
            |             Robus
            |    Coef.   Std. Err.      z    P>|z| 
------------+--------------------------------------
w_mean         
    ttl_exp |    0.251      0.036     6.92   0.000 
    married |   -0.737      0.349    -2.11   0.035 
      south |   -0.813      0.289    -2.81   0.005 
                ... (output omitted) ..
------------+--------------------------------------
b_mean         
    ttl_exp |    0.269      0.051     5.25   0.000 
    married |    0.091      0.405     0.23   0.822 
      south |   -2.041      0.425    -4.80   0.000 
                ... (output omitted) ..
------------------+--------------------------------
```

对上述命令和结果的简要解释如下：
- 白人组和黑人组的估计结果分别存储于 `w` 和 `b` 两个临时性文件中；
- 执行 `suest w b` 命令时，白人组和黑人组的被视为两个方程，即文的 (2a) 和 (2b) 式。Stata 会自动将两个方程对应的样本联合起来，采用 GLS 执行似无相关估计（SUR）；
- 由于 SUR 属于多方程模型，因此需要指定每个方程的名称，在下面呈现的回归结果中，`[w_mean]` 和 `[b_mean]` 分别是白人组和黑人组各自对应的方程名称。因此，`[w_mean]ttl_exp` 表示白人组方程中 `ttl_exp` 变量的系数，而 `[b_mean]ttl_exp` 则表示黑人组中 `ttl_exp` 变量的系数。 

执行组间系数差异检验的结果如下（Step 3）：

```Stata 
. *-Step 3: 检验系数差异

. test [w_mean]ttl_exp = [b_mean]ttl_exp 
  (1)  [w_mean]ttl_exp - [b_mean]ttl_exp = 0
           chi2(  1) =    0.08
         Prob > chi2 =    0.7735

. test [w_mean]married = [b_mean]married  
  (2)  [w_mean]married - [b_mean]married = 0
           chi2(  1) =    2.40
         Prob > chi2 =    0.1213

. test [w_mean]south = [b_mean]south      
  (3)  [w_mean]south - [b_mean]south = 0
           chi2(  1) =    5.70
         Prob > chi2 =    0.0169
```

此时，`ttl_exp` 在两组之间的系数差异仍然不显著，这与采用第一种方法得到的结论是一致的。在我们测试的三个变量中，只有 `south` 的系数在两组之间存在显著差异，对应的 p-value 为 0.0169。

##### B2. 基于 `bdiff` 命令的检验

上述过程可以使用我编写的 `bdiff` 命令非常快捷的加以实现，结果的输出方式也更为清晰（在 Stata 命令窗口中输入 `ssc install bdiff, replace` 可以下载最新版命令包，进而输入 `help bdiff` 查看帮助文件）：

```Stata
preserve
  drop if industry==2  // 白人组中没有处于 Mining (industry=2) 的观察值
  tab industry, gen(d)  //手动生成行业虚拟变量
  local dumind "d2 d3 d4 d5 d6 d7 d8 d9 d10 d11" //行业虚拟变量
  global xx "c.ttl_exp married south c.hours c.tenure c.age c.agesq `dumind'"  
  bdiff, group(black) model(reg wage $xx) surtest
restore
```

结果如下：
```Stata
* -SUR- Test of Group (black 0 v.s 1) coeficients difference

   Variables |      b0-b1    Chi2     p-value
-------------+-------------------------------
     ttl_exp |     -0.017    0.07       0.788
     married |     -0.814    2.32       0.128
       south |      1.238    5.80       0.016
       hours |      0.014    0.28       0.597
      tenure |      0.030    0.32       0.571
         age |     -1.027    0.23       0.629
       agesq |      0.015    0.31       0.578
          d2 |     -2.732    1.63       0.202
          d3 |     -1.355    1.45       0.228
          d4 |     -2.708    2.23       0.135
          d5 |     -1.227    1.00       0.317
          d6 |      0.087    0.00       0.950
          d7 |     -0.534    0.07       0.785
          d8 |     -1.316    1.26       0.261
          d9 |      0.346    0.06       0.807
         d10 |     -1.105    0.94       0.333
         d11 |     -1.689    1.81       0.179
       _cons |     18.770    0.20       0.652
---------------------------------------------
```

**几点说明：**
- 使用 `suest` 时，允许两个样本组的解释变量个数不同。但由于一些技术上的问题尚未解决(很快可以解决掉)，`bdiff` 命令要求两个样本组中的解释变量个数相同。在上例中，白人组在 Mining 行业的观察值个数为零（输入`tab industry black` 可以查看），导致我们加入行业虚拟变量时，白人组只有 10 个行业虚拟变量，而黑人组则有 11 个行业虚拟变量。为此，在上述命令中，我使用 `drop if industry==2` 命令删除了 Mining 行业的观察值。
- 目前，`bdiff` 还不能很好地支持因子变量的写法 (`help fvvarlist`)，因此上例中的行业虚拟变量不能通配符方式写成 `d*`，而必须写成原始模样： `d2 d3 d4 d5 d6 d7 d8 d9 d10 d11`。

#### C. 面板数据的处理方法

- `suest` 不支持 `xtreg` 命令，因此无法直接将该方法直接应用于面板数据模型，如 FE 或 RE。此时，可以预先手动去除个体效应，继而对变换后的数据执行 OLS 估计，步骤如下：
  - **step 1：**  对于固定效应模型而言，可以使用 `center` 或 `xtdata` 命令去除个体效应；对于随机效应模型而言，可以使用 `xtdata` 命令去除个体效应。
  - **step 2：** 按照截面数据的方法对处理后的数据进行分组估计，并执行 `suest` 估计和组间系数检验。

- 举个例子：

```Stata 
*-SUEST test for panel data
  *-数据概况
    webuse "nlswork", clear
    xtset idcode year
    xtdes
  *-对核心变量执行组内去心：去除个体效应
	help center   //外部命令, 下载命令为 ssc install center, replace
	local y "ln_wage"
	local x "hours tenure ttl_exp south"
	bysort id: center `y', prefix(cy_)   //组内去心
	bysort id: center `x', prefix(cx_) 	
  *-分组回归分析	
	reg cy_* cx_* i.year if collgrad==0  // 非大学组
	est store Yes
	reg cy_* cx_* i.year if collgrad==1  //   大学组
	est store No
  *-列示分组估计结果	
	esttab Yes No, nogap mtitle(Yes_Coll No_Coll) ///
		   star(* 0.1 ** 0.05 *** 0.01) s(r2 N)		
  *-似无相关估计	
	suest Yes No
  *-组间差异检验	
    test [Yes_mean]cx_ttl_exp = [No_mean]cx_ttl_exp 
	test [Yes_mean]cx_hours = [No_mean]cx_hours 
```

#### D. 小结

- 相对于方法1（引入交乘项），基于 SUR 的方法更为灵活一些。在上例中，白人组和黑人组的被解释变量相同 （均为 wage），此时方法 1 和方法 2 都能用。有些情况下，两个组中的被解释变量不同，此时方法 1 不再适用，而方法 2 则可以。
- 对于面板数据而言，可以预先使用 `center` 或 `xtdata` 命令去除个体效应，变换后的数据可以视为截面数据，使用 `regress` 命令进行估计即可。
- 为了便于呈现结果，可以使用`estadd` 命令将上述检验结果(chi2 值或 p值) 加入内存，进而使用 `esttab` 命令列示出来。可以参考 `help bdiff` 中的类似范例。

---
### 2.3 方法 3：费舍尔组合检验 （Fisher's Permutation test)

#### A. 基本思想

$$(3a) \qquad y = x\,\beta_{1} + \epsilon_{1}, \quad 白人组，样本数为\  n_1$$ 

$$(3b) \qquad y = x\,\beta_{2} + \epsilon_{1}, \quad 黑人组，样本数为\  n_2$$ 

将二者的系数差异定义为 $d=\beta_1 - \beta_2$，检验的原假设为：$H_0: d=0$ 。

我们仍然关注 `ttl_exp` 变量在两组之间的系数差异。以 -Table 1- 中的结果为例，可以看到，`ttl_exp` 变量在 [white 组] 和 [black 组] 的系数估计值分别为 0.251 ($\hat{\beta}_1$ ) 和 0.269 ($\hat{\beta}_2$)，因此，实际观察到的系数差异为 $\hat{d}_0 = \hat{\beta}_1 - \hat{\beta}_2=0.251-0.269=-0.018$ 。

这里， $d$ 是一个统计量，若能知道其分布特征，便可通过分析 $\hat{d}_0$ 在 $d$ 的分布中的相对位置来判断我们实际观察到 $\hat{d}_0 =-0.018$ 的概率。若概率很小，则表明 $H_0: d=0$ 是小概率事件，此时拒绝原假设，反之则无法拒绝原假设。

例如，若假设 $d$ 服从标准正态分布，即 $d \sim N(0, 1)$，则基于实际观察到的 $\hat{d}_0 =-0.018$ ，我们很容易得出结论：无法拒绝原假设，即两组之间的 `ttl_exp` 的系数不存在显著差异。p-value 很容易计算 (当然，也可以查表得到)：

```Stata 
. dis normal(-0.018)    //单尾检验
.49281943
```

然而，我们并不知道 $d$ 的分布特征。此时，可以对现有样本进行重新抽样，以得到经验样本 (empirical sample)，进而利用经验样本构造出组间系数差异统计量 $d$ 的经验分布 (empirical distribution)，从而最终得到经验 p 值 (empirical p-value)。

下面先通过一个小例子说明 **“经验 p 值”** 和 **“经验分布”** 的概念，进而介绍使用组合检验获得 “经验 p 值” 的流程。

#### B. 经验 p 值 (empirical p-value)

在这个小例子中，我们先随机生成一个服从标准正态分布的随机数 $d$，共有 10000 个观察值。这些观察值是通过模拟产生的。如果这些观察值构成的样本是通过从原始样本（原始样本是从母体中一次随机抽样，称为 “抽样样本，sample”）中二次抽样得到的，则称为 “经验样本 (empirical sample)”。

然后，我们数一下在这 10000 个随机数中，有多个是大于$-0.018$ (我们实际观察到的数值)，命令为 `count if d<-0.018`。一共有 4963 个观察值大于 $-0.018$ ，由此可得，经验 p 值 = 4963/10000 = 0.4963。这与采用 `normal()` 函数得到的结果 (0.4928) 非常接近。

```Stata 
. preserve
 . clear
 . set obs 10000
 . set seed 1357
 . gen d = rnormal()  // d~N(0,1) 服从标准正态分布的随机数
 . sum d, detail
 . count if d<-0.018
 . dis "Empirical p-value = "  4963/10000
. restore 
```

#### C. 经验样本 (empirical sample)

上例中，我们假设 $d$ 服从标准正态分布，从而可以通过 Monte Carlo 模拟的方式产生 10000 个观察值，这事实上是构造了一个经验样本。但多数情况下，我们并不知道 $d$ 的分布特征，此时无法使用 Monte Carlo 模拟。然而，若假设抽样样本 (sample) 是从母体 (population) 中随机抽取的，则可以通过抽样样本中二次抽样得到经验样本 (empirical sample)，这些经验样本也可以视为对母体的随机抽样。

若抽样过程中为无放回抽样 (sampling with no replacement)，则相应的检验方法称为 “组合检验（permutation test）”；若抽样过程中为有放回抽样 (也称为可重复抽样，sampling with replacement)，则对应的检验方法称为 “基于 Bootstrap 的检验”。

#### D. 费舍尔组合检验的步骤

若 $H_0$ 是正确的，则对于任何一个妇女而言（不论她是白人还是黑人），其 $x$ 对 $y$ 的边际影响都是相同的。因此，我们可以将白人组和黑人组的观察值混合起来，从中随机抽取 $n_1$ 个观察值，并将其视为"白人组"，剩下的 $n_2$ 个观察值可以视为“黑人组”。
- **Step 0:** 分别针对白人组和黑人组估计模型 (3a) 和 (3b)，得到系数估计值    $\hat{\beta}_1$ 和 $\hat{\beta}_2$，以及二者的系数差异 $\hat{d}_0 = \hat{\beta}_1 - \hat{\beta}_2$；
- **Step 1:** 将白人组和黑人组的样本混合起来，得到 $n_1+n_2$ 个观察值构成的样本 $S$；
- **Step 2:** 获得经验样本 —— 从 $S$ 中随机抽取 (无放回) $n_1$ 个观察值，将其视为“白人组”(记为 $Sw$)，剩下的 $n_2$ 观察值可以视为“黑人组” (记为 Sb)；
- **Step 3:** 分别针对经验样本 $Sw$ 和 $Sb$，估计模型 (3a) 和 (3b)，得到 $\hat{\beta}_1^{S_1}$ 和 $\hat{\beta}_2^{S1}$ （上标 $^{S_1}$ 表示利用第一笔经验样本得到的估计值），以及二者的差异 $d^{S_1}=\hat{\beta}_1^{S_1}-\hat{\beta}_2^{S_1}$ ；
- **Step 4:** 获得统计量 $d$ 的经验分布 —— 将 Step 2 和 Step 3 重复执行 $K$ 次 (如 $K=1000$)，则可以得到 $\{d^{S_1}, \ d^{S_2}, \cdots, d^{S_{1000}} \}$ ，亦可简记为 $d^{S_j} \, (j=1,2, \cdots, K)$；
- **Step 5:** 计算经验 p 值：$\hat{p}= \frac{\#\{d^{S_j}>\hat{d}_0\}}{K}$
其中， ${\#\{d^{S_j}>\hat{d}_0\}}$ 表示 **Step 4** 中得到的 $K$ 个 $d^{S_j}$ 中大于我们实际观测到的 $\hat{d}_0$ 的个数。若 $\hat{p}<0.05$ ，则可以在 5% 水平上拒绝原假设，表明两组的系数差异是显著的。

需要说明的是，由于 $d^{S_j}$ 的分布未必是对称的，因此，$\hat{p}=0.03$ 与 $\hat{p}=0.97$ 都可以视为在 5% 水平上拒绝原假设的证据。因为，前者意味着 $\hat{d}_0$ 在 1000 个 $d^{S_j}$ 中属于非常大的数值，而后者意味着它是非常小的数值。无论如何，在原假设 $H_0$ 下观察到 $\hat{d}_0$ 都是小概率事件，也就意味着原假设是不合理的。

此外，该方法的并不局限于普通的线性回归模型(`regress` 命令)，可以应用于各种模型的估计命令，如 `xtreg`, `xtabond`, `logit`, `ivregress' 等。

#### E. Stata 实现

上述过程可以使用连玉君编写的 `bdiff` 命令来实现。在命令窗口中输入 `ssc install bdiff, replace` 可以自动安装该命令。帮助文件中提供了多个范例。

**例1：不考虑行业虚拟变量**

```Stata 
* 数据处理
. sysuse "nlsw88.dta", clear
. gen agesq = age*age
. drop if race==3
. gen black = 2.race 
. global xx "ttl_exp married south hours tenure age agesq"
*-检验
. bdiff, group(black) model(reg wage $xx) reps(1000) detail
```
相关说明如下：
- 选项 `group()` 中填写用于区分组别的类别变量（若有多个组，可以预先删除不参与比较的组，类似于上面的 `drop if race==3` 命令）；
- 选项 `model()` 用于设定回归模型，即上面提到的模型 (3a) 或 (3b)；
- 选项 `reps(#)` 用于设定抽样次数，即上文提到的 ***K*** ，通常设定为 1000-5000 次即可；
- 附加 `detail` 选项，可以进一步列表呈现两组的实际估计系数 $\hat{\beta}_1$ 和 
$\hat{\beta}_2$ ；

上述过程大约用时 13 秒，结果如下：

```Stata 
- Permutaion (1000 times)- Test of Group (black 0 v.s 1) coeficients difference

   Variables |      b0-b1    Freq     p-value
-------------+-------------------------------
     ttl_exp |      0.007     490       0.490
     married |     -0.824     920       0.080
       south |      1.411       5       0.005
       hours |      0.010     344       0.344
      tenure |     -0.006     512       0.488
         age |     -1.579     751       0.249
       agesq |      0.022     218       0.218
       _cons |     28.051     267       0.267
---------------------------------------------
```

可以看到，ttl_exp 的经验 p 值为 0.49，表明白人和黑人组的 ttl_exp 系数不存在显著差异；married 变量的 p 值为 0.08，我们可以在 10% 水平上拒绝原假设。细心的读者会发现，该变量对应的 Freq = 920，为什么？（答案在上面 Step 5 处）。

**例1：考虑行业虚拟变量** 

若需在模型中加入虚拟变量，处理过程会稍微复杂一些。需要手动生成行业虚拟变量，并保证两个样本组中参与回归的行业虚拟变量个数相同。此外，书写命令时，不能使用通配符。（后续版本的 `bdiff` 命令会使用 `fvunab` 命令解决这些 bugs）。

```Stata 
*-数据预处理(可以忽略)
  sysuse "nlsw88.dta", clear
  gen agesq = age*age
*-分组虚拟变量
  drop if race==3
  gen black = 2.race 
*-删除缺漏值 
  global xx "ttl_exp married south hours tenure age* i.industry"
  qui reg wage $xx i.race
  keep if e(sample) 
*-生成行业虚拟变量
  drop if industry==2  // 白人组中没有处于 Mining (industry=2) 的观察值
  tab industry, gen(d)  //手动生成行业虚拟变量
  local dumind "d2 d3 d4 d5 d6 d7 d8 d9 d10 d11" //行业虚拟变量
  global xx "c.ttl_exp married south c.hours c.tenure c.age c.agesq `dumind'"
*-permutation test
  bdiff, group(black) model(reg wage $xx) reps(1000) detail
```

#### F. 面板数据

若原始数据为面板数据，通常会采用 `xtreg`, `xtabond` 等考虑个体效应的方法进行估计。抽样过程必须考虑面板数据的特征。在执行 `bdiff` 命令之前，只需设定 `xtset id year`，声明数据为面板数据格式，则抽样时便会以 id (公司或省份代码) 为单位，以保持 id 内部的时序特征。

```Stata 
*-Panel Data (sample by cluster(id))
*-数据预处理
. webuse "nlswork.dta", clear
. xtset id year                  //声明为面板数据，否则视为截面数据
. gen agesq = age*age
. drop if race==3
. gen black = 2.race
*-检验
. global x "ttl_exp hours tenure south age agesq"
. local m "xtreg ln_wage $x, fe" //模型设定
. bdiff, group(black) model(`m') reps(1000) bs first detail 
```

耗时 608 秒方可完成，结果如下：
```Stata 
-Bootstrap (1000 times)- Test of Group (black 0 v.s 1) coeficients difference

   Variables |      b0-b1    Freq     p-value
-------------+-------------------------------
     ttl_exp |      0.011      47       0.047
       hours |      0.003      58       0.058
      tenure |      0.004     116       0.116
       south |      0.093      14       0.014
         age |     -0.022     984       0.016
       agesq |      0.000      45       0.045
       _cons |      0.302      22       0.022
---------------------------------------------
Ho: b0(ttl_exp) = b1(ttl_exp)
  Observed difference =  0.011
    Empirical p-value =  0.047
```

**解释和说明：**
- `bdiff` 命令中设定 `bs` 选项，则随机抽样为可重复抽样 (bootstrap)；
- `first` 选项便于将组间系数差异检验结果保存在内存中，方便后续使用 `esttab` 合并到回归结果表格中。具体使用方法参见 `help bdiff`。
- 由于抽样过程具有随机性，因此每次检验的结果都有微小差异。在投稿之前，可以附加 `seed()` 选项，以保证检验结果的可复制性。
- 其他选项和使用方法参阅 `help bdiff` 的帮助文件。

#### G. 延伸阅读

关于这一方法的更为一般化的介绍参见：Efron, B., R. Tibshirani. An introduction to the bootstrap[M]. New York: Chapmann & Hall, 1993 (Section 15.2, pp.202), [-PDF-](http://www.doc88.com/p-0028679297137.html).

如下论文使用了这一方法检验了 “投资-现金流敏感性” 分析中的组间系数差异：

- Cleary, S., 1999, The relationship between firm investment and financial status, Journal of Finance, 54 (2): 673-692. (pp.684-685) [-PDF-](http://affordableessays.net/samples/1472587243220The%20Relationship%20between%20Firm%20Investment%20and%20Financial%20Status.pdf)

- 连玉君, 彭方平, 苏治, 2010, 融资约束与流动性管理行为, 金融研究, (10): 158-171\. （pp.164）[-PDF-](https://www.ixueshu.com/document/d8c7ecad345d6d59318947a18e7f9386.html#pdfpreview)

### H. 其它基于 Permutation test 的检验

- `mtest` 命令基于组合检验的思想来检验两个样本组是否具有相同的分布。
   - Tebaldi P, Bonetti M, Pagano M. M statistic commands: Interpoint distance distribution analysis. Stata Journal, 2011, 11(2):271-289. [- PDF -](http://www.Stata-journal.com/sjpdf.html?articlenum=st0228)
- `tsrtest`, `mwtest` 命令用于检验两个样本组的均值，中位数，方差等多个维度的差异。
   - Kaiser J, Lacy M G. A general-purpose method for two-group randomization tests[J]. Stata Journal, 2009, 9(1):70-85. [- PDF -](http://www.Stata-journal.com/sjpdf.html?articlenum=st0158)
- 上述命令都可以使用 `findit` 命令搜索到，或直接用 `ssc install` 命令下载安装。

## 3. 小结

- **方法1（加入交乘项）** 在多数模型中都可以使用，但要注意其背后的假设条件是比较严格的。若在混合回归中，只引入你关心的那个变量（`ttl_exp`）与分组变量 (`black`) 的交乘项（`ttl_exp*black`），则相当于假设其他控制变量在两组之间的不存在系数差异。相对保守的处理方法是：在混合估计时，引入所有变量与分组变量的交乘项，同时附加 `vce(robust)` 选项，以克服异方差的影响。
- **方法2（基于 SUR 模型的检验）** 执行起来比较方便，假设条件也比较宽松：允许两组中所有变量的系数都存在差异，也允许两组的干扰项具有不同的分布，且彼此相关。局限在于，有些命令无法使用 `suest` 执行联合估计，如几乎所有针对面板数据的命令都不支持（`xtreg`，`xtabond` 等）。 
- **方法3（组合检验）** 是三种方法中假设条件最为宽松的，只要求原始样本是从母体中随机抽取的（看似简单，但很难检验，只能靠嘴说了），而对于两个样本组中干扰项的分布，以及衡量组间系数差异的统计量 $d={\beta}_1 - {\beta}_2$ 的分布也未做任何限制。事实上，在获取 **经验 p 值** 的过程中，我们采用的是“就地取材”、“管中窥豹”的思路，并未假设 $d$ 的分布函数；另一个好处在于可以适用于各种命令，如 `regress`，`xtreg`，`logit`, `ivregress' 等，而 `suest` 则只能应用于部分命令。

> 方法无优劣。无论选择哪种方法，都要预先审视一下是否符合这些检验方法的假设条件。



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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-3418f07dc002b17e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")





