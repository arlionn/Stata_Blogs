> Stata连享会 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



> Source: [mostly-harmless-replication - 05 Fixed Effects, DD and Panel Data](https://github.com/vikjam/mostly-harmless-replication/blob/master/05%20Fixed%20Effects%2C%20DD%20and%20Panel%20Data/05%20Fixed%20Effects%2C%20DD%20and%20Panel%20Data.md)

----

# 05 Fixed Effects, DD and Panel Data
## 5.2 Differences-in-differences

### Figure 5-2-4  [dofile](https://github.com/vikjam/mostly-harmless-replication/blob/master/05%20Fixed%20Effects%2C%20DD%20and%20Panel%20Data/Figure%205-2-4.do)

Completed in [Stata](https://github.com/vikjam/mostly-harmless-replication/blob/master/05%20Fixed%20Effects%2C%20DD%20and%20Panel%20Data/Figure%205-2-4.do).

![Figure 5-2-4 in Stata](https://upload-images.jianshu.io/upload_images/7692714-2c2995cac8ebbac0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)






### Stata dofiles
```stata
clear all
set more off
eststo clear
capture version 14

/* Stata code for Figure 5.2.4 */

/* Download the data and unzip it */
shell curl -o outsourcingatwill_table7.zip "http://economics.mit.edu/~dautor/outsourcingatwill_table7.zip"
unzipfile outsourcingatwill_table7.zip

/*-------------*/
/* Import data */
/*-------------*/
use "table7/autor-jole-2003.dta", clear

/* Log total employment: from BLS employment & earnings */
gen lnemp = log(annemp)

/* Non-business-service sector employment from CBP */
gen nonemp  = stateemp - svcemp
gen lnnon   = log(nonemp)
gen svcfrac = svcemp / nonemp

/* Total business services employment from CBP */
gen bizemp = svcemp + peremp
gen lnbiz  = log(bizemp)

/* Time trends */
gen t  = year - 78 // Linear time trend
gen t2 = t^2       // Quadratic time trend

/* Restrict sample */
keep if inrange(year, 79, 95) & state != 98

/* Generate more aggregate demographics */
gen clp     = clg + gtc
gen a1624   = m1619 + m2024 + f1619 + f2024
gen a2554   = m2554 + f2554
gen a55up   = m5564 + m65up + f5564 + f65up
gen fem     = f1619 + f2024 + f2554 + f5564 + f65up
gen white   = rs_wm + rs_wf
gen black   = rs_bm + rs_bf
gen other   = rs_om + rs_of
gen married = marfem + marmale

/* Modify union variable */
replace unmem = . if inlist(year, 79, 81) // Don't interpolate 1979, 1981
replace unmem = unmem * 100               // Rescale into percentage

/* Diff-in-diff regression */
reg lnths lnemp admico_2 admico_1 admico0 admico1 admico2 admico3 mico4 admppa_2 admppa_1   ///
    admppa0 admppa1 admppa2 admppa3 mppa4 admgfa_2 admgfa_1 admgfa0 admgfa1 admgfa2 admgfa3 ///
    mgfa4 i.year i.state i.state#c.t, cluster(state)

coefplot, keep(admico_2 admico_1 admico0 admico1 admico2 admico3 mico4)                     ///
          coeflabels(admico_2 = "2 yr prior"                                                ///
                     admico_1 = "1 yr prior"                                                ///
                     admico0  = "Yr of adopt"                                               ///
                     admico1  = "1 yr after"                                                ///
                     admico2  = "2 yr after"                                                ///
                     admico3  = "3 yr after"                                                ///
                     mico4    = "4+ yr after")                                              ///
          vertical                                                                          ///
          yline(0)                                                                          ///
          ytitle("Log points")                                                              ///
          xtitle("Time passage relative to year of adoption of implied contract exception") ///
          addplot(line @b @at)                                                              ///
          ciopts(recast(rcap))                                                              ///
          rescale(100)                                                                      ///
          scheme(s1mono)
graph export "Figures/Figure 5-2-4.png", replace

/* End of script */
```

>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-a46a75047303c4b7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
