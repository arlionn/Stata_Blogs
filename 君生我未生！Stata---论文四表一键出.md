
> 作者： 吴水亭&emsp; | &emsp;Stata连享会

&emsp;

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


----
>>>君生我未生，我生君已老；
>>>恨不生同时，日日与君好。
>>>我离君天涯，君隔我海角；
>>>化蝶去寻花，夜夜栖芳草。

---
> 每次折腾论文表格时，总是痛恨自己手慢，更怨骂万恶的 Word 排版。
>
> 然而，偶拾`esttab`、`outreg2`，尤其是最近发布的`sum2docx`，`reg2docx`，不禁想起上面这首小诗。
>
>于是，庸俗地感慨：凄凄三键毁，但因识君晚！**Ctrl** &bull; **C** &bull; **V**


&emsp;
### 1. 写在前面

**描述性统计、相关系数矩阵、组间均值差异检验和实证结果**是实证论文中最常见、同时也是最重要的四张基本表格。

Stata 官方命令 `est table` 仅能输出回归结果表格，效果差强人意，与多数期刊的格式要求相去甚远。

外部命令则主要使用 `logout` 命令输出**描述性统计、相关系数矩阵，用 `ttable2` 或 `ttable3` 输出组间均值检验表格，`esttab` 或 `outreg2` 输出回归结果。对于 Stata 15 以前的用户而言，这些外部命令已经基本上可以满足我们的需求。缺点是，每张表格需要单独存放于一个 Word 或 Excel 文档，事后尚需手动合并之。

若想锦上添花，在 Stata 15 版本下，我们可以使用今天介绍的四个命令，将论文中的所有表格自动汇总到一个 Word 文档中，大幅提高表格处理效率。
 
### 2. 简要说明

那么，**描述性统计、相关系数矩阵、组建均值差异检验和回归结果**四张常用表格如何快速输出?我们可以使用四个热门的外部命令： **sum2docx**, **corr2docx**, **t2docx** 和 **reg2docx**：
- `help sum2docx`  // 将**描述性统计量表**直接输出到一个 docx 文件中；
- `help corr2docx` // 将**相关系数矩阵**直接输入到一个 docx 文件中；
- `help t2docx` // 将**分组均值t检验**结果导出到一个 docx 文件中；
- `help reg2docx` // 将**回归结果**导出至 docx 文件中，用法类似于 esttab。


### 3. 下载及安装
- 在 Stata 命令窗口中输入 `ssc hot` 命令，会显示出最热门的 10 个外部命令。
`ssc hot`

   如图：

![图 1 : 最热门的 10 个外部命令](http://upload-images.jianshu.io/upload_images/8601921-8c755dcf491e0236.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 下载只需在  Stata 命令窗口执行  `ssc install **2docx, replace`  即可。
附加 replace 可以保证下载最新版并自动覆盖旧文件。

以下载` sum2docx ` 为例，其命令为：
```stata
. ssc install sum2docx, replace
```
![图 2 :下载外部命令](http://upload-images.jianshu.io/upload_images/7692714-b11dfbbfa2fd8c12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")

### 4. 输出四张常用基本表格实操

#### 4.1 输出基本统计量： `sum2docx` 命令

##### 4.1.1 语法结构

```stata
sum2docx varlist [if] [in] using filename, [options]
```
其中， **varlist** 指数值型变量列表， **filename** 指的是输出的文件名，该命令的 **options** 非常丰富，可以根据需要选择。

##### 4.1.2 范例

```stata
sysuse auto,clear
sum2docx price-foreign using      ///
        1.docx, append obs        ///
        mean(%9.2f) sd min(%9.0g) ///
        median(%9.0g) max(%9.0g)  ///
        title("表 1: 描述性统计")
shellout 1.docx
```

**结果如图：**
 
![图 3 ：基本统计量](http://upload-images.jianshu.io/upload_images/7692714-b1ab41be2fef12d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 4.2 输出相关系数矩阵：`corr2docx` 命令

##### 4.2.1 语法结构
```stata
corr2docx varlist [if] [in] using filename, [options]
```
其中， varlist 指数值型变量列表， filename 指的是输出的文件名。

##### 4.2.2 范例

```stata
sysuse auto,clear
corr2docx price-foreign using ///
        2.docx, star(* 0.05) ///
        fmt(%4.2f) ///
        title("表 2：相关系数矩阵")
shellout 2.docx

```
想指定显示特定的显著性水平并加标记，想设置小数点位数可以加上 `fmt` 选项。

**结果如图：**
 
![图4：相关系数矩阵](http://upload-images.jianshu.io/upload_images/7692714-d0c463091ea0be01.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")



#### 4.3 组间均值差异检验：`t2docx` 命令

##### 4.3.1 语法结构

```stata
corr2docx varlist [if] [in] using filename [, options]
```
其中， varlist 指数值型变量列表， filename 指的是输出的文件名
##### 4.3.2 范例
```stata
sysuse auto, clear
t2docx price weight length mpg ///
        using 3.docx,replace   ///
        by(foreign)            ///
        title("表 3： t 检验")
shellout 3.docx
```
当然也可以改变小数点位数，加星星啥的，和上面一样一样的。

  **结果如图：**

![图5：组间均值 t 检验](http://upload-images.jianshu.io/upload_images/7692714-d2141e3e463771f4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")


#### 4.4 回归结果：`reg2docx` 命令

如果我们想把几个回归结果何在一张表里，该如何处理？

##### 4.4.1 说明
`reg2docx` 命令可以将回归结果保存到 .docx 文件中，用法类似于 `esttab`。先逐项回归后再汇总至一个文件中。

##### 4.4.2 范例
```stata
*-调入数据
  sysuse "auto.dta", clear
*-比如先做两个线性回归
  reg price mpg weight length
  est store m1
  reg price mpg weight length foreign
  est store m2
*-然后再做一个 Probit 回归
  probit foreign price weight length
  est store m3
*-输出结果至 Word 文档
  reg2docx m1 m2 m3 using result.docx,         ///
        ar2(%9.2f) b(%9.3f) t(%7.2f) r2(%9.3f) ///
        title("表4: 回归结果") replace
*-查看 Word 文档
  shellout result.docx
```

  **结果如图：**

![图 6 :回归结果陈列](http://upload-images.jianshu.io/upload_images/7692714-a4d567b32e8a966f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 5. 将上述四张表输出至一个 Word 文档中


#### 5.1 基本思路
- (1) 用` putdocx `命令生成一个空白 Word 文档 - **[My_Table.docx]**，进而使用` putdocx  text `等命令设定文档属性;
- (2) 用` sum2docx `生成`「表 1」`, 并使用` sum2docx `命令的` append ` 选项将这张表追加到 **[My_Table.docx]** 文档尾部;
- (3) 按第二步的方法, 依次使用   `corr2docx`, `t2docx`, `reg2docx` 命令添加后续表格.

#### 5.2 写作建议

- (1) 写论文**正文**时，新建**一个 Word 文档**, 命名为: `[My_Paper.docx]`
在需要插入表格的地方写上`[---------Table # Here--------]`，其中，**#** 表示表格编号；
- (2) Stata 自动生成的 **表格** 则存放于 **另一个 Word 文档**： `[My_Table.docx]`，
里面存放 `[Table 1]`, `[Table 2]`, .....

![图 7 ：论文结构设计](http://upload-images.jianshu.io/upload_images/7692714-39394931c157c204.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")

#### 5.3 Stata 范例

我们需要把上述四个基本表格汇总至一份  word 文档里。

```stata
clear all
set more off

putdocx begin                     //新建 Word 文档
putdocx paragraph, halign(center) //段落居中

*-定义字体、大小等基本设置
putdocx text ("附：文中待插入表格"), ///
        font("华为楷体",16,black) bold linebreak  

*-保存名为 My_Table.docx 的 Word 文档        
putdocx save "My_Table.docx", replace

*-调入数据
sysuse "auto.dta", clear


*-----Table 1-----
sum2docx price-length using "My_Table.docx", append ///
         obs mean(%9.2f) sd min(%9.0g) median(%9.0g) max(%9.0g) ///
         title("表 1: 描述性统计")
*-Note: 选项 append 的作用是将这张新表追加到 "My_Table.docx" 尾部, 下同.


*-----Table 2-----
putdocx begin
putdocx pagebreak
putdocx save "My_Table.docx", append

corr2docx price-length using "My_Table.docx", append ///
          star(* 0.05) fmt(%4.2f) ///
          title("表 2：相关系数矩阵")


*-----Table 3-----
putdocx begin
putdocx pagebreak
putdocx save "My_Table.docx", append

t2docx price-length using "My_Table.docx", append ///
       by(foreign) title("表 3：组间均值差异 t 检验")


*-----Table 4-----
putdocx begin
putdocx pagebreak
putdocx save "My_Table.docx", append

reg price mpg weight length
est store m1
reg price mpg weight length foreign
est store m2
probit foreign price weight length
est store m3
reg2docx m1 m2 m3 using "My_Table.docx", append ///
         r2(%9.3f) ar2(%9.2f) b(%9.3f) t(%7.2f) ///
         title("表4: 回归结果")

shellout "My_Table.docx"  //大功告成！打开生成的 Word 文档
```

  **结果如图：**

![image](http://upload-images.jianshu.io/upload_images/7692714-c962d755b2c312f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")

![图 8 ：汇总展示  ](http://upload-images.jianshu.io/upload_images/7692714-ebc751f35d27bc87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "屏幕截图.png")

> ### 本文相关资料下载链接
为了方便各位复制文中的代码和命令，我们将本文涉及的 Stata 命令汇总到了一个 **Do-file** 中，各位可以到 [『Stata连享会&bull;码云』](https://gitee.com/arlionn)  &rarr; [『快速输出论文表格』](https://gitee.com/arlionn/reg2docx) 项目中下载。
- 推文中使用的 Do-files：[---点击下载---](https://gitee.com/arlionn/reg2docx/blob/master/reg2docx-Wu.do)
- 推文原始 Markdown 文档：[---点击下载---](https://gitee.com/arlionn/reg2docx/blob/master/reg2docx-Wu.md)


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
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会 (公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-7a9f35b57cfb18be.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")




