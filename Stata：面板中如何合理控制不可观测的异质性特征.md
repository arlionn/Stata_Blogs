> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



> **提要：** 实证研究中，经常需要控制那些不可观测的异质性特征，比如行业层面个体差异或行业层面的冲击 (通常会在模型中加入一组行业虚拟变量)。本文探讨了文献中普遍使用的两种方法，一种是**组内去心(demean)**，就是把模型中的解释变量和被解释变量都减掉它所在行业的平均值后再进行回归；另一种是在模型中加入被解释变量的行业均值作为控制变量。我们的研究表明，上述两种方法得到的这个估计结果都是非一致的，从而扭曲我们的统计推断。**相对而言，固定效应估计量 (Fixed Effects Estimator) 的结果是一致的，应该在实证分析中使用。**我们还进一步的解释了当传统的计算方法不可行时，我们该如何高计算效率。

> **Abstract:** Controlling for unobserved heterogeneity (or “common errors”), such as industry-specific shocks, is a fundamental challenge in empirical research. This paper discusses the limitations of two approaches widely used in corporate finance and asset pricing research: demeaning the dependent variable with respect to the group (e.g., “industry-adjusting”) and adding the mean of the group’s dependent variable as a control. We show that these methods produce inconsistent estimates and can distort inference. In contrast, the fixed effects estimator is consistent and should be used instead. We also explain how to estimate the fixed effects model when traditional methods are computationally infeasible. 
(JEL G12, G2, G3, C01, C13)

### Source:
- Gormley, T. A., D. A. Matsa, 2014, Common errors: How to (and not to) control for unobserved heterogeneity, *Review of Financial Studies*, 27 (2): 617-661.  [PDF原文](https://watermark.silverchair.com/hht047.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAbIwggGuBgkqhkiG9w0BBwagggGfMIIBmwIBADCCAZQGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMQKQpVI9L_9lGZ-gsAgEQgIIBZSoFd6ccPdKSLI_hYX_14XZrltxmw2UNd97fAn-ffhA9VvOPhH6ytXRd_72PiZjY034iYCEegRu4Qc38bzZGwr74O55SKgV40dUgtcVNrce1y1ereIQQ5CCO_bTJXrv2aVIdB6vBfRTuV0GkuTwMPFOCWAOT4g0NLFTibgd1gx7cR7oc6Jdc0kDQBjIqxRUmYUGrnN7deqqOYXLDG3XODqYN5rTt3JlGpDdotq4ldvJjaGnshTd1N2q0tFTxFKjzmXZYIOuss0BxVU-3wLZgXMjKwH_3zQuhqXvrN8ThvvbrrYYJKKH1kM1GFD2sQ1N7MJpGa4NAr3Ot5jZm1RQ0RpdC-ouIEINVda4-c88_yKo7FlI04ujgEJhtgPgK-UjeNFOD7dqbDca8K1QxVMt8FkdZbM8J5cqCbepSh5ltU8W8Xovt8cO7DWwn3CV1iD9SYwNI2-89txcFBwKbR8Cs47NNjRbQdA),  [Stata 实现过程](http://www.kellogg.northwestern.edu/faculty/matsa/htm/fe.htm)


### Stata: Implementing Fixed Effects Estimation

Our purpose in writing paper [[ Gormley, T. A., D. A. Matsa, 2014, Common errors: How to (and not to) control for unobserved heterogeneity, Review of Financial Studies, 27 (2): 617-661. ]](http://ssrn.com/abstract=2023868) | ( [下载 PDF-1](https://watermark.silverchair.com/hht047.pdf?token=AQECAHi208BE49Ooan9kkhW_Ercy7Dm3ZL_9Cf3qfKAc485ysgAAAbIwggGuBgkqhkiG9w0BBwagggGfMIIBmwIBADCCAZQGCSqGSIb3DQEHATAeBglghkgBZQMEAS4wEQQMQKQpVI9L_9lGZ-gsAgEQgIIBZSoFd6ccPdKSLI_hYX_14XZrltxmw2UNd97fAn-ffhA9VvOPhH6ytXRd_72PiZjY034iYCEegRu4Qc38bzZGwr74O55SKgV40dUgtcVNrce1y1ereIQQ5CCO_bTJXrv2aVIdB6vBfRTuV0GkuTwMPFOCWAOT4g0NLFTibgd1gx7cR7oc6Jdc0kDQBjIqxRUmYUGrnN7deqqOYXLDG3XODqYN5rTt3JlGpDdotq4ldvJjaGnshTd1N2q0tFTxFKjzmXZYIOuss0BxVU-3wLZgXMjKwH_3zQuhqXvrN8ThvvbrrYYJKKH1kM1GFD2sQ1N7MJpGa4NAr3Ot5jZm1RQ0RpdC-ouIEINVda4-c88_yKo7FlI04ujgEJhtgPgK-UjeNFOD7dqbDca8K1QxVMt8FkdZbM8J5cqCbepSh5ltU8W8Xovt8cO7DWwn3CV1iD9SYwNI2-89txcFBwKbR8Cs47NNjRbQdA) | [下载 PDF-2](http://portal.idc.ac.il/en/main/research/caesareacenter/annualsummit/documents/gormley-matsa.pdf) ) was to examine the econometrics underlying the ad hoc estimation methods commonly used to account for unobserved heterogeneity in the finance literature.  Our investigation found that these methods should not be used because they typically provide inconsistent estimates.  For example, *Adj*Y estimation transforms only the dependent variable and does not remove problematic correlations from the independent variables.  Fixed effects (FE) estimation, on the other hand, is consistent and should be used in place of these other estimators.  But it is not always obvious how to implement fixed effects.

This website provides examples and corresponding code to illustrate how to implement fixed effects in these cases.  We also provide suggestions on how to overcome computational hurdles that arise when estimating models with multiple high-dimensional fixed effects.  The code we provide is for Stata and SAS.  If you want to suggest ways to handle these issues in other languages, we are happy to post links. 

If you use this information or code, please cite Gormley and Matsa (*RFS *2014).  Our paper, which provides deeper analysis of these ideas, is available [here](http://ssrn.com/abstract=2023868).  Lecture slides used by Gormley to teach these methods to PhD students are available [here](http://www.kellogg.northwestern.edu/faculty/matsa/htm/gormley_matsa_lecture_notes.ppt). 

### Examples of how to run a FE estimation in place of *Adj*Y or *Avg*E

A FE estimator correctly transforms both the dependent and independent variables and should be used in place of *Adj*Y and *Avg*E estimators.  Commands for implementing the FE estimator in Stata are in bold and the variable names, which the user must specify, are in italics.  Here are two examples: (1) industry-adjusting and (2) characteristically-adjusting stock returns. 

#### Example #1 – Industry-adjusting

Industry-adjusting, an *Adj*Y estimator, can take many forms.  A common form is to demean the dependent variable with respect to industry mean (or median) before estimating the model with OLS.  However, this estimate is inconsistent whenever there are within-industry correlations among independent variables.  Instead, a researcher should estimate a model with industry FE.  Any of the following four sets of estimation commands can be used:
```stata
*-Stata
 . reg dependent_variable independent_variables i.industry
 . areg dependent_variable independent_variables, a(industry)
 . xtset industry
 . xtreg dependent_variable independent_variables, fe
````
**Note #1:** Unless you are interested in the individual group means, `areg`, `xtreg` are typically preferable, because of shorter computation times.
**Note #2:** While these various methods yield identical coefficients, the standard errors may differ when Stata’s `cluster` option is used.    When clustering, `areg` reports cluster-robust standard errors that reduce the degrees of freedom by the number of fixed effects swept away in the within-group transformation; `xtreg` reports smaller cluster-robust standard errors because it does not make such an adjustment.  `xtreg`’s approach of not adjusting the degrees of freedom is appropriate when the fixed effects swept away by the within-group transformation are nested within clusters (meaning all the observations for any given group are in the same cluster), as is commonly the case (e.g., firm fixed effects are nested within firm, industry, or state clusters). See Wooldridge (2010, Chapter 20).

XTREG-clustered standard errors can be recovered from AREG as follows:

1. Run the AREG command *without* clustering
2. Then, construct two variables using the following code:
```stata
. gen df_areg = e(N) – e(rank) – e(df_a)
. gen df_xtreg = e(N) – e(rank)
```
3. Run the `areg` command again *with* clustering
4. Multiply the reported cluster-robust standard errors by `sqrt(df_areg / df_xtreg)`

If the desired industry-adjusting is on a yearly basis, then instead of using the mean or median of observations in the same industry-year to adjust the dependent variable, estimate a model with *industry×year* fixed effects:
```stata
. reg dependent_variable independent_variables i.industry#i.year
. egen industry_year = group(industry year)
. areg dependent_variable independent_variables, a(industry_year)
. egen industry_year = group(industry year)
. xtset industry_year
. xtreg dependent_variable independent_variables, fe
```

If you are interested in combining industry-year FE with another fixed effect, like firm FE, then absorb the fixed effect of highest dimension and control for the other(s) using indicator variables:
```stata
. areg dependent_variable independent_variables i.industry#i.year, a(firm)
. xtset firm
. xtreg dependent_variable independent_variables i.industry#i.year, fe
```



**Note:** The above specification may be computationally difficult to estimate if the number of industry-year indicator variables is large.  To resolve this, please see the discussion below about Stata programs that can be used to estimate models with multiple high-dimensional FE.

#### Example #2 – Characteristically-adjusted stock returns

Although there are many ways to construct characteristically-adjusted stock returns, the basic idea is the same.  Before analyzing stock returns, you first construct a set of benchmark portfolios based on various firm characteristics, and then “characteristically-adjust” the individual stock returns by subtracting the equal- or value-weighted average return of their corresponding benchmark portfolio for each period.  For example, construct 25 size and value portfolios each period by first dividing stocks into quintiles based on their size and then further subdividing them into quintiles based on their market-to-book ratios.  A firm’s size and market-to-book ratio in a given period then determines which benchmark portfolio is used to adjust the firm’s stock return in that period.  After constructing these “characteristically-adjusted” returns, you then further sorts the stocks based on an independent variable of interest to determine whether stock returns vary across this independent variable.  Such analyses typically sort stocks into quintiles based on the independent variable and then compare returns across the top and bottom quintiles. 

This method is equivalent to *Adj*Y in that it only transforms the dependent variable (stock returns), and it doesn’t account for correlations of the independent variable within groups (i.e., portfolios).  To avoid potential biases that might occur because of such correlations, one should instead estimate a model with fixed effects for each of the portfolio-periods and indicators for each quintile of the independent variable, excluding an indicator for the bottom quintile. The resulting estimates indicate how the average stock return across each quintile differs from the average stock return for the bottom quintile:
```stata
. reg stock_return  ind_var_quintile2-ind_var_quintile5  i.benchmark_portfolio#i.period
  
. egen portfolio_period = group(benchmark_portfolio period)
. areg stock_return  ind_var_quintile2-ind_var_quintile5, a(portfolio_period)
 
. egen portfolio_period = group(benchmark_portfolio period)
. xtset portfolio_period
. xtreg stock_return  ind_var_quintile2-ind_var_quintile5, fe
```

### Stata programs that can be used to estimate models with multiple high-dimensional FE

Estimating fixed effects models with multiple sources of unobserved heterogeneity can be computationally difficult when there are a high number of FE that need to be estimated.  As discussed in our paper, only one FE can typically be removed by transforming the data.  The other fixed effects need to be estimated directly, which can cause computational problems.  For example, to estimate a regression on Compustat data spanning 1970-2008 with both firm and 4-digit SIC industry-year fixed effects, Stata’s `xtreg` command requires nearly 40 gigabytes of RAM. 

#### User-written commands in Stata

As noted in our paper, there are memory-saving and iteration techniques that can be used to avoid these limitations.  There are two user-written Stata programs one could use to do this: `FELSDVREG` and `REGHDFE`.  Both programs are capable of handling two high-dimensional FE and are available from the Statistical Software Components (SSC) archive.  To download either program, simply type the following command once in Stata (replacing *program_name* with FELSDVREG or REGHDFE):
```stata
. ssc install felsdvreg, replace
. ssc install reghdfe, replace
```
This command will load everything associated with programs, including the help files.

Both commands can be used to estimate models with two high-dimensional fixed effects.  For example, if one wanted to estimate a model with firm and industry-year fixed effects (as in example #1 above), the commands could be used as follows:
```stata
. egen industry_year = group(industry year)
. felsdvregdependent_variable independent_variables,  ///
    ivar(firm) jvar(industry_year) xb(xb) peff(peff)  ///
    feff(feff) res(res) mover(mover) mnum(mnum)       ///
    pobs(pobs) group(group)
 
. egen industry_year = group(industry year)
. reghdfe dependent_variable independent_variables, a(firm industry_year)
```
Refer to the help files for more details on how to use these commands.  Please address any questions you might have about these programs directly to their respective authors.  Our personal experience is that `REGHDFE` often executes much more quickly than `FELSDVREG`, but run time will depend on the specific application and data structure.  `REGHDFE` is also capable of estimating models with more than two high-dimensional fixed effects, and it correctly estimates the cluster-robust errors.  Such a command is necessary, for example, if you want to estimate a model with firm, state-year, and industry-year fixed effects as done in [our JFE paper on managers’ preference to “play it safe”. ](http://ssrn.com/abstract=2465632) 

**Note:** If you use `FELSDVREG` or `REG2HDFE` (an older version of `REGHDFE`), an adjustment to the standard errors may be necessary.These programs report cluster-robust errors that reduce the degrees of freedom by the number of fixed effects swept away in the within-group transformation.  This is the same adjustment applied by the `AREG` command.  To recover the cluster-robust standard errors one would get using the `XTREG` command, which does not reduce the degrees of freedom by the number of fixed effects swept away in the within-group transformation, you can apply the following adjustments:

·  For FELSDVREG, use the `noadji` option built into the command

·  For REG2HDFE, multiply the reported standard errors by `sqrt([e(N) - e(df_r)] / [e(N) - [e(df_r) - (*G<sub>1 </sub>*- 1)]])`, 

where *G<sub>1</sub>* is the number of distinct values taken on by the main panel variable.  While the values e(N) and e(df_r) are created and saved into memory by the `REG2HDFE` command itself, you’ll need to calculate *G<sub>1</sub>* separately.  For example, for a panel of firms, *G<sub>1</sub>* is the number of unique firms in the estimation sample.  You can use Stata’s `DISTINCT` command to calculate this number.  Here is example code for a firm-level regression with two independent variables, both firm and industry-year fixed effects, and standard errors clustered at the firm level:
```stata
. egen industry_year = group(industry year)
. reg2hdfe dependent_variable ind_variable1 ind_variable2,  ///
        id1(firm) id2 (industry_year) cluster(firm)
. matrix varTemp = e(V)
. qui distinct firm if ind_variable1 != . & ind_variable2 != . & industry_year != .
. disp “SE ind_variable1: “ sqrt(varTemp[1,1]) * sqrt((e(N)-e(df_r))/(e(N)-(e(df_r)-(r(ndistinct)-1))))
. disp “SE ind_variable2: “ sqrt(varTemp[2,2]) * sqrt((e(N)-e(df_r))/(e(N)-(e(df_r)-(r(ndistinct)-1))))
```

As discussed above in the context of `AREG` vs. `XTREG`, this adjustment is only applied when the panel variable is nested within clusters.  If you are ever unsure which standard errors are correct in a particular application, reporting the higher standard error is prudent.

If you find errors or corrections, please e-mail us at gormley@wustl.edu and dmatsa@kellogg.northwestern.edu.

Todd A. Gormley and David A. Matsa


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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-b4c7337dd38c9209.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

