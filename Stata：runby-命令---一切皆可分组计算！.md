> 作者：黄河泉 | 连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    

  
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


> **背景：** 在 Stata 提供了一个高效实用的副指令 —— `byable`，可以让我快捷地计算分组统计量，如各行业的均值、标准差等。例如，`by industry: egen invest_mean = mean(invest)`，可以快速计算出每个行业的平均投资支出。然而，并不是所有的 `generate` 或 `egen` 命令提供的函数都支持 `byable` 副指令。此时，我们如何计算分组统计量呢？一个粗暴的方法就是使用 `forvalues` 或 `foreach` 等循环语句。其实，还有更为简洁的方法 —— 使用外部命令 `runby` !

## 1. runby 的运行原理
其本质仍然是使用  `forvalues` 或 `foreach` 等循环语句执行分组计算。但便利之处在于我们无需自行书写完整的循环语句，只需提供核心计算公式即可。因此，我们只需使用 `program define` 语句定义一个简单的小程序，然后内嵌到 `runby` 语句之中即可。

## 2. 使用过程释义
- **目的：** 对每家公司的投资支出进行标准化。公式为 `std_x = [x - mean(x)]/sd(x)`。
- **难点：** 虽然可以使用 `egen` 命令提供的 `std()` 函数实现标准化转换，但却不支持 `byable`。
- **解决方法：**
- - **Step 1：** 定义一个小程序，用于执行标准化转换：
```stata
program define one_std    
  egen invest_std = std(invest)
end
```
**程序的调用方法：** (1) 如果这个程序只是偶尔用一下，可以在 dofile 中撰写上述程序，选中后按快捷键 `Ctrl+R`，将该程序读入 Stata 内存，随后就可以像使用一般的 Stata 命令那样使用 `one_std` 命令了。(2) 如果这个程序在日后会经常使用，则可以将其单独存放在一个 dofile 中，保存为 “**one_std.ado**” (注意：后缀是 **.ado**，文件的与程序名称同名)，将其保存到 `D:\stata15\ado\personal\myado` 文件夹下(如果没有，可以执行创建)，进而执行 `adopath + D:\stata15\ado\personal\myado`，告知 Stata：我在这里还存放了一些可执行的自编程序！设定完后，我们自行定义的 **one_std** 程序就是一个能够被 Stata 识别的合法程序了。
- - **Step 2：** 运行 `runby` 命令，执行分组计算：
`runby` 是外部命令，可以执行如下命令安装之：
```stata
ssc install runby, replace
```
然后，就可以愉快滴进行分组计算了：
```stata
runby one_std, by(company)
```
## 3. 完整Stata 范例
```stata
*-定义程序
capture program drop one_std
program define one_std    
  egen invest_std = std(invest)
end
*-Note：选中上述程序，按快捷键 Ctrl+R 将其读入内存

*-调入数据
. webuse "grunfeld.dta", clear
*-分组计算
. runby one_std, by(company)

*-列示结果
. list company year invest* if year<1938, sepby(company)
  +--------------------------------------+
  | company   year   invest   invest_std |
  |--------------------------------------|
  |       1   1935    317.6   -.93812598 |
  |       1   1936    391.8   -.69844231 |
  |       1   1937    410.6   -.63771376 |
  |--------------------------------------|
  |       2   1935    209.9    -1.599489 |
  |       2   1936    355.3   -.43999411 |
  |       2   1937    469.9    .47388569 |
  |--------------------------------------|
  |       3   1935     33.1   -1.4241168 |
  |       3   1936       45   -1.1791827 |
  |       3   1937     77.2    -.5164199 |
  |--------------------------------------|
  |       4   1935    40.29   -1.0727421 |
  |       4   1936    72.76   -.31277531 |
  |       4   1937    66.26   -.46490908 |
  |--------------------------------------|
  |       5   1935    39.68   -1.4586008 |
  |       5   1936    50.73   -.73004219 |
  |       5   1937    74.24    .82004045 |
  |--------------------------------------|
  |       6   1935    20.36   -1.0029697 |
  |       6   1936    25.98   -.84215578 |
  |       6   1937    25.94   -.84330034 |
  |--------------------------------------|
  |       7   1935    24.43   -1.2647902 |
  |       7   1936    23.21   -1.3313998 |
  |       7   1937    32.78   -.80889688 |
  |--------------------------------------|
  |       8   1935    12.93   -1.5678286 |
  |       8   1936     25.9   -.88913306 |
  |       8   1937    35.05    -.4103309 |
  |--------------------------------------|
  |       9   1935    26.63   -1.0253897 |
  |       9   1936    23.39   -1.2431145 |
  |       9   1937    30.65   -.75524966 |
  |--------------------------------------|
  |      10   1935     2.54   -.31681649 |
  |      10   1936        2   -.63101462 |
  |      10   1937     2.19   -.52046338 |
  +--------------------------------------+
```
### 验证程序
```stata
webuse grunfeld, clear

// default/manual
bys company: egen invest_mean0 = mean(invest)
bys company: egen invest_sd0 = sd(invest) 
gen invest_std0 = (invest-invest_mean0)/invest_sd0

// runby 
capture program drop one_std
program define one_std   
  egen invest_mean1 = mean(invest)   
  egen invest_sd1 = sd(invest)  
  gen invest_std1 = (invest-invest_mean1)/invest_sd1
  egen invest_std2 = std(invest)
  exit
end
runby one_std, by(company)
```

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
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-1f69e00e9b180ee6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


