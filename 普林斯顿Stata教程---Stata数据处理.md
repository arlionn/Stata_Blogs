>译者：谢作翰 | 连玉君 |  ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))   
> &emsp;
> 原文链接：[Princeton Stata 在线课程 (Princeton University - Stata Tutorial )](https://link.jianshu.com/?t=http%3A%2F%2Fwww.princeton.edu%2F%7Eotorres%2FStata%2F)
> &emsp;
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


## 专题链接

- [普林斯顿Stata教程 - Stata做图](http://www.jianshu.com/p/069bb26ad842)
- [普林斯顿Stata教程 - Stata数据管理](http://www.jianshu.com/p/f0aa1d0bcf79)
- [普林斯顿Stata教程 - Stata编程](http://www.jianshu.com/p/916ddc3948d6)

## 目录 

**1.1 数据读取**
  - 1.1.1 自由格式数据
  - 1.1.2 固定格式数据   
**1.2 数据文档**
  - 1.2.1 数据标签与注释
  - 1.2.2 变量标签和注释
  - 1.2.3 值标签
  - 1.2.4 多语言标签

**1.3 创建新变量**
  - 1.3.1 生成和替换
  - 1.3.2 运算符，表达式及函数
**1.4 变量重编码**

## 1.1 数据读取

在本节中，我们将讨论如何读取**原始**数据文件。如果您的数据来自其他统计软件包（如SAS或SPSS），请考虑使用诸如Stat/Transfer
（[www.stattransfer.com](http://www.stattransfer.com/)）或DBMSCopy（[www.dataflux.com](http://www.dataflux.com/Product-Services/Products/dbms.asp)）之类的工具。Stata可以使用`fdause`命令来读取SAS文件`help fdause`。Stata还可以导入和导出Excel电子表格，输入`help import excel`以了解更多信息，并且可以从关系数据库读取数据，输入`help odbc`简介。

### 1.1.1 自由格式数据

如果数据是自由格式——**变量由空格，逗号或制表符分隔**，则可以使用`infile`命令。有关自由格式文件的示例，请参阅[http://data.princeton.edu/wws509/datasets](http://data.princeton.edu/wws509/datasets)上提供的计划生育工作数据（请阅读说明并单击`effort.raw`）。这实质上是一个包含四列的文本文件，其中一列带有国家名称，另一列带有数字变量，由空格分隔。我们可以使用该命令将数据读入Stata
```stata
infile str14 country setting effort change ///
using http://data.princeton.edu/wws509/datasets/effort.raw
```
该 `infile`命令后面跟着变量的名称。由于国家名称是一个字符串而不是数字变量，因此我们在名称前加上`str14`，它将变量的类型设置为最多14个字符的字符串。所有其他变量都是数字。

该`using`后面跟着文件的名称，该文件可以是计算机，本地网络或互联网上的文件。在这个例子中，我们直接从互联网上读取文件。更多信息`help infile1`。
还可以选择`webuse`命令读取该数据库：
```stata
webuse set http://data.princeton.edu/wws509/datasets
webuse effort
```
首先将默认网址设置为普林斯顿数据库，然后直接用`webuse`命令读取相关文件。`webuse` 在stata小白系列中有更多介绍。
可用list查看所读入数据：
```stata
. list in 1/3
     ┌─────────────────────────────────────┐
     │ country   setting   effort   change │
     ├─────────────────────────────────────┤
  1. │ Bolivia        46        0        1 │
  2. │  Brazil        74        0       10 │
  3. │   Chile        89       16       29 │
     └─────────────────────────────────────┘
```

### 1.1.2 固定格式数据

调查数据通常采用固定格式，每个案例有一个或多个记录，每个记录中的每个变量都处于固定位置。

读取固定格式数据的最简单方法是使用该`infix`命令指定每个变量所在的列。正如它发生的那样，努力数据整齐排列在列中，所以我们可以阅读它们如下：
```stata
infix str country 4-17 setting 23-24 effort 31-32 change 40-41 using 
     http://data.princeton.edu/wws509/datasets/effort.raw, clear
```
这表示`country`要从第4-17列读取名称， `setting`从第23-24 列读取名称。`str`指定该`country`是一个字符串变量，但不必指定宽度，因为宽度从列数限定中可以看出。

如果有大量的变量，应该考虑在一个单独的文件上输入名字和位置，这个文件又被称为**字典**，然后可以用`infix`命令中调用字典。下面尝试将以下字典内容输入到名为effort.dct的文件中：
```stata
infix dictionary using http://data.princeton.edu/wws509/datasets/effort.raw {
  str country  4-17
      setting 23-24
      effort  31-32
      change  40-41
}
```
字典只接受*注释，但必须出现在第一行之后。保存此文件后，可以使用以下命令读取数据：
```stata
infix using effort.dct, clear
```
请注意，您现在“使用”字典，它反过来“使用”数据文件。您可以使用表单指定它作为infix命令的选项，而不是在字典中指定数据文件的名称。infix using dictionaryfile, using(datafile).第一个'using'指定字典，第二个'using'是指定数据文件的选项。如果要使用一个字典来读取以相同格式存储的多个数据文件，这一点尤其有用。更多信息，请参阅help infix。如果您的观测值跨越多个记录或线条，infix只要所有观测记录的记录数量相同（不一定全部相同），仍然可以使用它们来读取它们。欲了解更多信息，请参阅help infix。

`infile`命令也可以用于固定格式的数据和字典。这是一个非常强大的命令，它提供了许多不适用的选项`infix`; 例如它可以让你在字典中定义变量标签，但是语法有点复杂。看`help infile2`。

## 1.2 数据文档

在将数据读入Stata之后，准备一些文档很重要。在本节中，我们将看到如何创建数据集，变量和值标签，以及如何为数据或变量创建注释。

### 1.2.1 数据标签与注释
Stata允许您使用`label data `命令标记您的数据集，然后标记最多80个字符（Stata SE中为244）。您还可以使用`notes`命令，然后使用冒号和文本添加最多约64K字符的注释：
```stata
label data "Family Planning Effort Data"
. notes:  Source P.W. Mauldin and B. Berelson (1978). 
   Conditions of fertility decline in developing countries, 1965-75. 
   Studies in Family Planning, 9:89-147
```
数据用户可以键入`notes`以查看您的注释。仔细记录您的数据总是会带来回报。

### 1.2.2 变量标签和注释

您可以（也应该）使用`label variable` 命令来标记变量。命令后跟变量名称和标签（引号包围，最多80k字符）。使用`infile`命令，您可以将这些标签添加到字典中。否则，你应该准备一个带有所有标签的do文件。以下是如何为我们的数据集中的三个变量定义标签：
```stata
label variable setting "Social Setting"
label variable effort  "Family Planning Effort"
label variable change  "Fertility Change"
```
Stata还允许您使用该命令将注释添加到特定变量`notes varname: text`。请注意，该命令后面跟着一个变量名，然后是一个冒号：
```stata
. notes change: Percent decline in the crude birth rate (CBR) 
  the number of births per thousand population between 1965 and 1975.
```
键入`describe`，然后`notes`检查我们到目前为止的工作。

### 1.2.3 值标签

您还可以标记分类变量的值。我们的数据集没有任何分类变量，但我们创建一个。我们将复制`effort`变量，然后将其分为三类，0-4,5-14和15+，它们分别代表弱，中等和强壮三个程度（前两行中使用的`generate`和`recode`在下一节介绍，我们还展示了如何用一个命令完成所有这些步骤）：
```stata
 generate effortg = effort 
 recode effortg 0/4=1 5/14=2 15/max=3
 (effortg: 20 changes made)
 label define effortg 1 "Weak" 2 "Moderate" 3 "Strong", replace
 label values effortg effortg
 label variable effortg "Family Planning Effort (Grouped)"
```
Stata采用两步法来定义标签。首先定义一个标签集，使用`label define`命令将整数代码与标签（最多80k）相关联。然后，使用`label values`命令将该组标签与变量相关联。通常，标签集和变量使用相同的名称，就像我们在示例中所做的那样。

这种方法的一个优点是可以为多个变量使用同一组标签。规范的例子是label define yesno 1 "yes" 0 "no"，它可以与数据集中的所有0-1变量相关联，使用每个变量的形式命令`label values variablename yesno`。定义标签时，如果标签是单个单词，则可以省略引号，但为了清晰起见，我更愿意使用它们。

可以使用`add`或者`modify`选项修改标签集，使用`label dir`（仅列出名称）或`label list`（列出名称和标签）列出标签集，并使用`label save`将它们保存到一个do文件。输入`help label`以了解更多信息。您也可以**使用不同语言的标签**，如下所述。

### 1.2.4 多语言标签

一个Stata文件可以用多种语言存储标签，并且您可以从一组到另一组自由移动。我将通过为我们的数据集创建西班牙语标签来说明。遵循Stata建议，我们将使用ISO标准的双字母语言代码，`en`代表英文，`es`代表西班牙语。

首先我们使用`label language`用来重命名当前语言为`en`，并创建一个新的语言集`es`：
```stata
 label language en, rename
(language default renamed en)
 label language es, new
(language es now current language)
```
西班牙语标签定义不会覆盖相应的英文标签，而是并行存在。值标签命名时需小心些，不能直接将标签集取名`effortg`.因为`effortg`仅表示变量和标签之间的关联。你需要定义一个新的标签集; 我们在此取名`ffortg_es`，结合旧名称和新语言代码，然后将其与变量`effortg`相关联：
```stata
label define effortg_es 1 "Débil" 2 "Moderado" 3 "Fuerte"
label values effortg effortg_es
```
您可能想要尝试命令`describe`现在。可以尝试用表格输出：
```stata
table effortg
```
接下来，我们将语言改回英文并再次运行表格：
```stata
label language en
table effortg
```
更多信息，请键入 `help label_language`.

## 1.3 创建新变量

Stata创建新变量最重要的命令是`generate`/`replace`和`recode`，他们经常一起使用。

### 1.3.1 生成和替换

`generate`命令使用可以结合常量，变量，函数，算术和逻辑运算符的表达式创建新变量.
```stata
gen settingsq = setting^2.
```
如果你打算在回归中使用这个项，而且知道线性和二次项是高度相关的。那么在平方之前将变量中心化可能是个好主意。这里我们运行`summarize`，并使用`quietly`来抑制输出,从存储结果中检索均值r(mean)：
```stata
quietly summarize setting
gen settingcsq = (setting - r(mean))^2
```
请注意，我为此变量使用了不同的名称。Stata不会让你用`generate`来覆盖现有的变量。如果你真的想替换旧变量的值使用`replace`。您也可以使用`drop var_names`从数据集中删除一个或多个变量。

### 1.3.2 运算符、表达式及函数

下表显示了您可以在表达式中使用的标准算术，逻辑和关系运算符：

![运算符及表达式](https://upload-images.jianshu.io/upload_images/1844375-6a15d6b8c423bdab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Stata有大量的函数，这里有一些常用的数学函数，输入`help mathfun`可以查看完整列表：

![函数](https://upload-images.jianshu.io/upload_images/1844375-576da8ad82f12f12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当参数是数据集中的变量时，这些函数会自动应用于所有观察值。

Stata还具有生成随机数的功能（在模拟中很有用），即`uniform()`。它还有一套广泛的函数来计算概率分布（p值所需的）和它们的反函数（临界值所需的），请参阅`help density functions`以获取更多信息。
还有一些专门的函数用于处理字符串，请参阅`help string functions`，处理日期函数，请参阅`help date functions`。

## 1.4 变量重编码

`recode`命令作用是将数字变量转化为类别变量。例如，假设一项生育率调查中对年龄在15岁至49岁的女性进行单身年龄分析.您想以5年为一个区间对样本分组。可以使用命令：
```stata
gen age5 = int((age-15)/5)+1 if !missing(age)
```
但这只适用于间隔规则的情况。也可以其实用如下方法：
```stata
recode age  ///
  (15/19=1) (20/24=2) (25/29=3) (30/34=4) ///
  (35/39=5) (40/44=6) (45/49=7), gen(age5)
```
括号中的每个表达式都是一个重新编码规则，由值的列表或范围组成，后跟等号和新值。使用斜线指定的范围包括两个边界，因此15/19是15到19，其也可以被指定为15 16 17 18 19或甚至15 16 17/19。您可以使用min参考最小值并max参考最大值，如在min/19和中44/max。当规则的形式为range = value时，括号可以省略，但它们通常有助于使命令更具可读性。

值被分配到它们落在的第一个类别。从未分配给某个类别的值将保持原样。您可以使用`else`（或*）作为最后一个子句来引用尚未分配的任何值。或者，您可以使用`missing`和`nonmissing`引用未分配的缺失值和非缺失值; 这些必须是最后两个语句，不能与其他语句相结合。

在我们的例子中，我们还使用了`gen()`选项生成一个新的变量`age5`，在这种情况下，新变量默认替换现有变量的值。我强烈建议您在重新编码之前制作原始变量副本。
您也可以在重编码时指定值标签。选项`label(label_name)`允许您为创建的标签分配一个名称（默认与变量名称相同）。下面是一个示例，显示如何在一步进行重编码和做值标签。（上文中需使用四个命令）。
```stata
recode effort  ///
  (0/4=1 Weak) (5/14=2 Moderate) (15/max=3 Strong), ///
  generate(efffortg) label(effortg)
```

对原始和重新编码的变量进行交叉制表以检查转换是否按预期工作通常是一个好主意。



>### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


>### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-6605266d12e82534.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")



[原文](http://data.princeton.edu/stata/dataManagement.html)


> 原文链接：[Princeton Stata 在线课程 (Princeton University - Stata Tutorial )](https://link.jianshu.com/?t=http%3A%2F%2Fwww.princeton.edu%2F%7Eotorres%2FStata%2F)









