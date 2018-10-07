> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



### 导言
多数情况下，我们在 Stata 命令窗口中输入命令，敲回车，结果基本上瞬间生成。然而，在有些情况下，一个程序经常需要花费几十分钟 (比如，做面板门槛模型，或需要 Bootstrap)，甚至几十个小时才能完成。此时，若能预先估算完成所有程序所花费的时间，对于我们合理安排日程会非常有帮助。   

 今天我们介绍两种方法：一个是 Stata 官方命令 `timer`，另一个是外部命令 `benchmark`，二者各有优劣。

### 程序计时器 timer

类似于我们在操场上跑步时所使用的秒表，每次摁下秒表，开始计时；再次摁下时则会呈现跑一圈所花费的时间。

下面我们通过一个简单的例子来说明该命令的使用方法：
```stata
timer clear 1
timer on 1
  sysuse "auto.dta", clear
  reg price wei len, vce(bootstrap, reps(1000))
timer off 1
timer list 1
```
结果如下：
```stata
. timer clear 1
. timer on 1
.   sysuse "auto.dta", clear
(1978 Automobile Data)
.   reg price wei len, vce(bootstrap, reps(1000))
(running regress on estimation sample)

Bootstrap replications (1000)
----+--- 1 ---+--- 2 ---+--- 3 ---+--- 4 ---+--- 5 
..................................................    50
..................................................   100
            .......................
..................................................   950
..................................................  1000

Linear regression                               Number of obs     =         74
                                                Replications      =      1,000
                                                Wald chi2(2)      =      34.30
                                                Prob > chi2       =     0.0000
                                                R-squared         =     0.3476
                                                Adj R-squared     =     0.3292
                                                Root MSE          =  2415.7351
------------------------------------------------------------------------------
             |   Observed   Bootstrap                         Normal-based
       price |      Coef.   Std. Err.      z    P>|z|     [95% Conf. Interval]
-------------+----------------------------------------------------------------
      weight |      4.699      1.728     2.72   0.007        1.313       8.085
      length |    -97.960     56.432    -1.74   0.083     -208.564      12.644
       _cons |  10386.541   5670.915     1.83   0.067     -728.249   21501.330
------------------------------------------------------------------------------

. timer off 1
. timer list 1
   1:      3.92 /        1 =       3.9180
```
- 关键点是说明：
  - `timer clear 1`：用于复位第一个秒表的时间至 0；
  - `timer on 1`：用于启动第一个秒表，
  - `timer close 1`：用于关闭第一个秒表，
  - `timer list 1`：用于列示第一个秒的耗时
- 在本例中，我们进行了一百次 Bootstrap 抽样，用于估算稳健型标准误，整个过程耗时 13.9 秒。
- 因此，如果把抽样的次数增加为5000次耗时大约是 4×5=20秒 (实测结果是  18.77 秒)。你也可以据此推断当变量增加、样本数增加等情形下，大致的耗时。

### 外部命令 benchmark 

#### Stata 范例
```stata
. benchmark, reps(10) disp:    ///
      qui reg price wei len, vce(bootstrap, reps(1000))
1: 3.373 seconds
2: 3.225 seconds
3: 3.247 seconds
4: 3.172 seconds
5: 3.207 seconds
6: 3.398 seconds
7: 3.265 seconds
8: 3.292 seconds
9: 3.208 seconds
10: 3.375 seconds
Average over 10 runs: 3.277 seconds
```
该命令的语法相对比较简洁，而且可以一次性帮我们进行十轮测试，并进而帮我们计算出平均耗时为 3.277 秒。据此我们推断，进行5000次抽样的时间，就会更加准确一些，约为 3.277*5 = 16.385 秒。

实测结果如下，与我们推算的结果非常接近：
```stata
. benchmark, reps(10) disp:    \\\
     qui reg price wei len, vce(bootstrap, reps(5000))
1: 16.12 seconds
2: 16.12 seconds
3: 16.18 seconds
4: 15.82 seconds
5: 16.07 seconds
6: 15.99 seconds
7: 16.08 seconds
8: 16.19 seconds
9: 16.07 seconds
10: 15.51 seconds
Average over 10 runs: 16.02 seconds
```
#### benchmark 的下载及安装
在命令窗口中输入如下命令，即可下载：
```stata
net install benchmark,   ///
from(https://raw.githubusercontent.com/mcaceresb/stata-benchmark/master/)
```
下载完成后输入如下命令，可查看详细的帮助文件：
```stata
help benchmark
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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-a37019d4682e0d37.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
