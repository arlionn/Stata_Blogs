
> 作者：谢作翰 |  连玉君 | ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn)) 

> **编者按：** 从本期开始，Stata 连享会将推出「Stata小白系列」推文，介绍数据导入、命令语法等 Stata 入门知识，以帮助各位尽快掌握 Stata 的基本操作。




##### 三  数据拆分与合并 

- 数据拆分

  1. 数据横向拆分—— `keep`& `drop`
  2.  数据纵向拆分—— `keep`& `drop`
  3.  一步到位保存数据子集—— `savesome`
- 数据合并
  1. 数据的横向合并—— `merge`
  2. 数据的纵向合并—— `append`
  3. 对多个 `csv`文件纵向合并—— `csvconvert`

##### 四  长宽数据转换—— `reshape`命令
 - 宽数据转为长数据
- 长数据转为宽数据


> 数据文件下载：链接: https://pan.baidu.com/s/1qXRh9EG  密码: 5ltw


###  数据拆分

如果把内存中的数据集视为一个矩阵，则我们对数据的基本操作包括两类：删除**列**或删除**行**。对于 Stata 而言，命令的主要操作对象是变量 (矩阵的**列**)，而有些时候我们也需要删除特定的观察值 (矩阵的**行**)。这些操作都可以配合使用 `drop` 和 `keep` 命令来实现。

#### 删除\保留变量：数据的横向拆分
原始数据有时包含过多的变量，但在实际应用中可能根据需要将原始数据拆分为不同的数据表，这时就要实现数据的横向拆分。数据的横向拆分用到的两个命令为 `drop` 和 `keep`。

> Stata 范例：

```stata
sysuse "auto.dta", clear
drop weight length
save "d:\data\auto_sim01.dta", replace
```
我们调入数据后，使用 `drop` 命令删除了两个变量，进而把处理后的数据文件另存到 `D:\data` 文件夹中。若该文件夹下已经存在一个名称为 `auto_sim01.dta` 则自动覆盖之，这是 `replace` 选项的作用。

按照上述思路，也就不难理解如下命令的含义了：
```stata
sysuse "auto.dta", clear
keep  make price mpg rep78 foreign
save "d:\data\auto_sim02.dta", replace
```
> **特别说明：** 一般而言，除非数据文件非常大导致每次调入耗时较长，我们很少另存数据文件。我们会将主要的修改动作都保留于 **dofile** 中。每次分析时，只需修改 **dofile** 或执行 **dofile** 中的特定语句即可实现对子样本的处理。毕竟，**dofile** 本质上是文本文件，文件大小通常不过几十 k。
 
#### 删除\保留观察值：数据的纵向拆分

原始数据有时包含过多的样本观测值，但在实际应用中可能根据需要将其按某种特征拆分为不同的数据表，这是就要实现数据的纵向拆分。此时，仍然可以使用 `drop` 和 `keep` 命令。

例如将 `auto.dta` 数据文件拆分为两个数据文件：`auto_domestic.dta` 和 `auto_foreign.dta`，操作如下：
```stata
sysuse "auto.dta", clear
keep if foreign==0
save auto_domestic.dta, replace 

sysuse "auto.dta", clear
keep= if foreign==1
save auto_foreign.dta, replace
```

#### 一步到位保存数据子集： `savesome` 命令

通过上述范例可以看出，使用 Stata 自带的 `keep` 和 `drop` 命令可以快捷地删除变量或观察值，但在命令写法上较为繁琐。我们可以使用外部命令 `savesome` 来提高效率 （该命令是 Stata 官方命令 `save` 的扩展版）：
- 安装程序文件：
```stata
ssc install savesome, replace
```
在命令窗口中输入 `help savesome` 可以查看该命令的帮助文件，其语法格式如下：
```
savesome [varlist] [if exp] [in range] using filename [, old save_options]
```
其中，
- `using filename` 表示要保存的子集文件名及文件路径；
- `old` 是较为重要的选项，可以将数据保存为较低版本的文件格式，如 stata15 用户可以将数据另存为在 stata14 或以下版本中的打开的数据文件。类似的命令还有 Stata 官方命令 `saveold`。

> Stata 范例 1：

对于前文提到的例子，我们可以使用 `savesome` 来实现相同的功能：
```stata
sysuse "auto.dta", clear
keep  make price mpg rep78 foreign
save "d:\data\auto_sim02.dta", replace
```
等价于
```stata
sysuse "auto.dta", clear
savesome make price mpg rep78 foreign ///
     using "d:\data\auto_sim02.dta", replace
```
**评论：** 凭心而论，笔者更喜欢前一种方式。虽然多写了一行命令，但思路清晰，也便于记忆。

> Stata 范例 2：

```stata
sysuse "auto.dta", clear
keep if foreign==0
save auto_domestic.dta, replace 
```
等价于：
```stata
sysuse "auto.dta", clear
savesome if foreign==0 using auto_domestic.dta, replace 
```



###  数据合并

#### 数据的横向合并

数据的横向合并是横向拆分的逆操作，但是其要比拆分复杂。对于时间序列资料而言，要保证同一时点的两个变量的观察值对接到同一行；而对于界面个体资料而言，要保证同一个人的年龄数据与该人的收入数据在同一行。而对于面板数据资料，则需数据中有两个变量能够唯一标示每一行观察值，以保证 A 数据文件中的 “张三疯-2016” 与 B 数据文件中的 “张三疯-2016” 处于同一行。合并所使用的命令语句为 `merge `，具体语句如下所示：

```
merge [varlist] using filename [filename ...] [, options]
``` 

 `merge `为合并的命令语句， `[varlist] `代表合并进去的新变量， `using filename`指的是所要与原文件合并的文件路径， `options`包含较多的功能，表 2.11 显示了其具体内容。
 ![](http://upload-images.jianshu.io/upload_images/1844375-a34f88966686e4a8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*举例*：利用横向拆分实验中生成的数据文件 `waterinput` 和 `wateroutput`实现数据的横向合并，匹配变量为 `year`，生成新的数据文件命名为 `waternew`。这个操作的命令为：
```
use c:\data\wateroutput, clear
sort year
save c:\data\wateroutput, replace
use c:\data\waterinput, clear
sort year
merge year using c:\data\wateroutput
save c:\data\waternew, replace
```
在以上命令语句中，第一个命令语句实现了 `wateroutput`数据文件的打开，第二个命令语句将文件按年份变量进行排序，第三个命令语句保存了排序之后的数据文件，第四个命令语句实现了 `waterinput`数据文件的打开，第五个命令语句将此数据按年份变量进行排序，第六个命令语句按年份变量将 `wateroutput`文件合并到 `waterinput`文件中，第七个命令语句保存合并之后的数据文件。
2. **数据的纵向合并**

数据的纵向合并为数据纵向拆分的逆操作，使用的主要命令为 `append`命令，具体语句如下：

```
 append using filename [, options] 
```

在这个命令语句中， `append`是进行纵向合并的命令语句， `using filename`是进行纵向合并的文件路径， `[, options]`的内容与 `merge`相似，但更为简化。

例如，利用纵向拆分实验中生成的数据文 `domesticauto`和 `foreignauto`实现数据的纵向合并，生成的数据文件命名为 `usaautonew`。这个操作的命令为：
```
use c:\data\domesticauto, clear
append using c:\data\foreignauto
save c:\data\usaautonew, replace
```
在以上命令中，第一个命令语句打开了原始数据文件，第二个命令将 `foreignauto`文件合并到 `domesticauto`文件中，第三个命令语句存储了合并后的数据文件。
3. **一次合并多个csv文件—— `csvconvert`**
 `csvconvert` 命令用于将多个 `csv`格式文件合并为一个 `.dta`格式文件，比较适合处理具有时间周期性特点的变量。
命令：
``` 
 csvconvert input_directory , replace [options] 
```
该命令有三个参数：

| 参数名        | 功能         | 
| :------------- |:-------------:| 
| output_file(file_name)   |设置输出文件名 | 
|  output_dir(output_directory)  |设置输出路径   | 
| input_file(.csv file list) | 选择要合并的文件   |  

命令下载：
```
help csvconvert
``` 
以命令配套的四个世界银行文件为例，首先看文件结构 
```
dir C:\Uers\Administrator\Desktop\stata学习文件\worldbank\*.csv
```
![](http://upload-images.jianshu.io/upload_images/1844375-e5cf9482ce01b6b2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![wb2010.csv](http://upload-images.jianshu.io/upload_images/1844375-f7bbe40ed5d92e78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

使用 `csvconvert`命令将四个年度数据纵向合并
```  
csvconvert  C:\Users\Administrator\Desktop\stata学习文件\worldbank, replace
```

![默认情况下合并后的文件名为output.dta ](http://upload-images.jianshu.io/upload_images/1844375-af672ac5039dd20f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


要合并的四个子文件必须在同一个文件夹中，默认情况下， `csvconvert`命令会合并文件夹中所有文件 ，可以用 `note` 命令核对所合并的文件情况 .用 `input_file`参数选择所有合并的文件名，在此，我们只选择2008年度和2009年度的数据
```
csvconvert C:\Users\Administrator\Desktop\stata学习文件\worldbank
, replace input_file(wb2008.csv wb2009.csv)
```

> 本文中所用数据文件下载地址：

数据文件：链接: [https://pan.baidu.com/s/1qXRh9EG](https://pan.baidu.com/s/1qXRh9EG)  密码: 5ltw


>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 特别说明
文中包含的链接在微信中无法生效。请点击本文底部左下角的`【阅读原文】`，转入本文`【简书版】`。


---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-8a75eac8a9b0a525.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")


