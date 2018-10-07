> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



### 1. 问题背景：
- (1) 在 **idata** 文件夹下存放了 3500 个个股日交易资料的 **.dta** 数据文件。这些文件采用公司代码命名，因此，并不连续（参见 `图 1`）；
- (2) **任务**：(a) 部分文件中的 `turnover` 变量是文字型变量，需要使用 `destring` 命令将其转换为数值变量；(b) 将这 3500 个数据文件纵向合并起来 (append)；
- (3) 使用 `forvalues` 语句无法奏效，因为文件名对应的数字不连续；

 ![图1：待处理的 3500 个文件 (部分显示)](https://upload-images.jianshu.io/upload_images/7692714-bb7a4b61a7d8867b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2. 解决思路：
- (1) 使用 `foreach` 语句对无规则的文件名进行循环处理；
- (2a) **难点：**如何提取文件名到一个暂元或变量中？
- (2b) **对策：**使用 `dir` 命令以及暂元的高级功能，将 `dir` 查询到的文件名存放到一个暂元 `s`中，随后在循环语句中从 `s` 暂元中提取文件名；
- (3) **关键命令：** 提取特定文件夹下的所有文件的名称到一个暂元 `s` 中，可以使用如下语句(详情参见 `help macro`，以及 `help extended_fcn`)：
```stata
  local s: dir "D:\data\idata'"  files "*.dta",  respectcase
```
  - **命令含义：** 
>![图2：暂元高级功能释义](https://upload-images.jianshu.io/upload_images/7692714-6e94313c0c92537f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

执行如下语句可以在屏幕上呈现暂元 `s` 中存放的内容，如 **图 3** 所示：
```stata
. local s: dir "D:\data\idata'"  files "*.dta",  respectcase
. dis `"`s'"'
```
>![图3：暂元 s 中存放的内容](https://upload-images.jianshu.io/upload_images/7692714-c5305368c4c94ad7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3. 采用循环语句将 turnover 变量转换为数值变量

```stata
global d "D:\data\idata"
cd "$d"
local s: dir "`d'"  files "*.dta",  respectcase
foreach i  in `s' {
  use "`i'", clear
  dis in red "ID = `i'"  //可以不要，只是为了标示公司代码
  destring turnover, replace force  
  qui save "`i'", replace 
  dis "." _c     //屏幕打点，纯属娱乐
}
```

### 4. 合并 3500 个数据文件

#### 4.1 方法1：使用 foreach 语句
使用 `append` 命令可以合并纵向合并文件中的 3500 个文件，但有一个棘手的问题：使用 `append ` 命令时，必须先将第一份数据调入内存，然后将剩余的 3499 份文件追加到这份文件的尾部。

然而，我们的数据文件名称是没有特定规则的，这就需要采用一种自动提取文件名的机制。此时，可以使用 `gettoken` 或 `tokenize` 命令对暂元 `s` 进行切割，其包含的第一个单词就是第一个文件的名称。这里以 `gettoken` 为例进行说明。

##### gettoken 的用法
```stata
  local a "Jack Rose Make Love"
  gettoken s1 s2: a, parse(" ")
  dis "`s1'"
  dis "`s2'"
```
结果如下：
```stata
.   dis "`s1'"
Jack

.   dis "`s2'"
 Rose Make Love
```
可见，`gettoken` 的作用是将暂元 `a` 的内容按照我们的要求以空格 (`parse(" ")` 的作用) 为切割点，分成两部分，第一空格之前的部分存入暂元 `s1`，剩余部分存入暂元 `s2`。

##### 合并数据的命令
```stata
* 4.1 方法1：使用 foreach 语句
  local s: dir "$d"  files "*.dta",  respectcase
  gettoken f1 frest: s, parse(" ")
  dis "`f1'"        //第一个文件的名称
  dis `"`frest'"'   //其他文件的名称
  
  use "`f1'", clear
  foreach i of local frest {
    append using `i'
  }  
  save "D:\data\idata_all01.dta", replace //保存合并后的数据
```

#### 4.2 方法2：使用外部命令 openall 
若使用外部命令 `openall`，则上述繁杂的处理过程会简化成一条极其简洁的语句：
```stata
  cd "D:\data\idata"  // 将要合并该文件夹下的所有文件
  openall *           // 纵向合并当前工作路径下的所有文件
  save "D:\data\idata_all02.dta", replace //保存合并后的数据
```
可以使用如下命令安装外部命令 `openall`：
```stata
ssc install openall, replace
```
详细语法格式参见：`help openall`。

> 有兴趣的话，你可以对比一下上述两份最终文件是否有差异。


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

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-878bf27d6e7ba94b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
