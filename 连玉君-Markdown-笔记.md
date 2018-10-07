> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
>♩ 好脑瓜不如烂笔头！  
♩ 儿时父母如此教导我们，博士时导师亦是如此要求。  
♩ 手写时代里，**一支好钢笔+一个好本子**，才会有写东西的欲望；  
♩ 键盘时代里，**一个机械键盘+一个好的书写工具**才能真正愿意打字；  
♩ 只听得键盘的清脆敲击，不用去碰那只**老鼠**，思绪方才能连贯。
>
> **Markdown** 就是这个东西——**一个好的书写工具**！
> 
> 至于为什么，可以自行百度或简书一下。
>

----
对于 Stata 用户而言，我们可以用 `markdown`, `markdoc`, `markstat` 等命令直接写 dofile，并将文章正文和图形表格等一并输出为`网页`，`Word` 或 `PDF` 等格式，即所谓的**一种输入，多种输出**。更为重要的是，可以实现**可重复写作**，即论文中的数据、模型发生变化后，只需修改完 Stata 代码后，使用上述命令更新输出结果即可。你不用担心**老板的反反复复了**。

> 目录
[1. Markdown 基础](#1-markdown-基础)     
 [1.1 标题](#11-标题)     
 [1.2 列表](#12-列表)     
 [1.3 文字效果和分隔线](#13-文字效果和分隔线)     
 [1.4 链接、图片、注释](#14-链接-图片-注释)[1.4.1 文本链接](#141-文本链接)     
  [1.4.2 插入图片](#142-插入图片)     
  [1.4.3 脚注 / 尾注 / 参考文献](#143-脚注-尾注-参考文献)     
  [1.4.5 缩写名词/专有名词/索引条目](#145-缩写名词专有名词索引条目)     
 [1.5 文字高亮和代码块](#15-文字高亮和代码块)     
 [1.6 特殊字符](#16-特殊字符)     
 [1.7 表格](#17-表格)     
 [1.8 目录](#18-目录)     
[2. Markdown 进阶功能](#2-markdown-进阶功能)     
 [2.1 数学公式](#21-数学公式)     
 [2.2 流程图、甘特图等](#22-流程图-甘特图等)     
 [2.3 编辑器](#23-编辑器)          
 [2.4 Markdown Here 插件](#24-markdown-here-插件)          
 [2.5 HTML 与 Markdown 的相互转换](#25-html-与-markdown-的相互转换)     
 [2.6 Markdown 与 Word 快捷转换：Writage](#26-markdown-与-word-快捷转换writage)     
 [2.7 有道云笔记 Markdown 编辑器快捷键](#27-有道云笔记-markdown-编辑器快捷键)     
[参考文献 (相关资料)](#参考文献-相关资料)     
---

## 1. Markdown 基础

- 快速入门资料
  - [为什么应该用 Markdown](http://jolestar.com/markdown-tools/)
  - [有道 Markdown 指南](http://note.youdao.com/iyoudao/?p=2411&vendor=unsilent14 "有道云笔记 Markdown 使用指南")
  - [极简 Markdown 指南](https://www.binarization.com/archive/2016/markdown-guide/#help)
  - [Markdown 语法](https://www.binarization.com/archive/2016/markdown-syntax/) 
   - 语法-预览对照范例：[Markdown在线练习页面](http://commonmark.cn/dingus/)， [ 范例1](https://www.zybuluo.com/mdeditor#872604), [范例2](http://pandao.github.io/editor.md/)

[Markdown](http://daringfireball.net/projects/markdown/) 于 2004 年发布，包含一套纯文本格式化语法以及由其创建者 John Gruber 发布的 Perl 工具，该工具用于将符合 Markdown 语法的纯文本文档转化为对应的 HTML。多年来，Markdown 语法被逐步采纳，现在使用它的有 [GitHub](https://github.com/arlionn/)、Reddit、Stack Exchange (https://stackexchange.com/)、[码云](https://gitee.com/arlionn) 等 等，所以我们可以认为 Markdown 已经被整个软件社区所采用。Markdown 的成功与其简洁性紧密相关，Gruber 当初的设计决定了 Markdown 今天的成功。

> 快速上手：[十分钟 Markdown 教程](http://commonmark.cn/help/) → [在线写 Markdown，实时呈现](http://commonmark.cn/dingus/?text=&smart=1)。


### 1.1 标题

- 只要一段文字前加 `#` 号即可定义标题
- `#` 号的个数代表标题的级数，共有六级标题
- 书写时，建议在 `#` 号后加一个空格，这是最标准的 Markdown 语法。

```
# 一级标题

## 二级标题

### 三级标题
```

### 1.2 列表

Markdown 可以支持++有序列表++、++无序列表++、++记事列表++。

- **无序列表**
  - 无序列表使用星号 (`* `)、加号(`+`)或是减号(`-`) 作为列表标记 
  - 三者均可，其后需要加一个空格，如下两种写法等价：

```R
- 红色     
- 绿色
- 蓝色

* 红色     
* 绿色
* 蓝色
```
**Note**: 三者可以混用，但会导致行间距不一致。

- **有序列表**
有序列表则使用数字接着一个英文句点：
```
1. 打开冰箱
2. 塞入大象
3. 关上冰箱
```
- **说明：**
  - 其一，将文字前的数字序号从`1. 2. 3. ` 修改为 `1. 1. 1.` 并不影响最终的显示效果；
  - 其二，若希望有序列表从`8.` 开始编号，则第一行改写为 `8. 打开冰箱` 即可。

- **列表的缩进和混排**  
若需缩进，只需在 `-` 前加两个空格或四个空格 (敲一个 `Tab` 键相当于四个空格)。
例如：
```
- 一级列表
  - 二级列表
    - 三级列表
      1. 四级列表
      1. 三级列表
```
输出效果为：
- 一级列表
  - 二级列表
    - 三级列表
      1. 四级列表
      1. 三级列表
      
- **记事列表**
  - 用 `- [x]` 开头的文本表示已完成的记事
  - 用 `- [ ]` 开头的表示待办记事
  - 上述标记后要加一个空格。如：
```
- [x] 已完成记事
- [ ] 待办记事
  - [ ] 吃啥？
    - [ ] 去吃个 subway 吧？
```
> 输出效果为：
- [x] 已完成记事
- [ ] 待办记事
  - [ ] 吃啥？
    - [ ] 去吃个 subway 吧？


- **嵌套区块引用**   
区块引用可以嵌套（例如：引用内的引用），只要根据层次加上不同数量的 `>`：
```
> This is the first level of quoting.
>
> > This is nested blockquote.sdf
>
> Back to the first level.
```
**输出效果为：**

> This is the first level of quoting.
>
> > This is nested blockquote.sdf
>
> Back to the first level.


### 1.3 文字效果和分隔线

- 文字效果的基本语法
```
*斜体*
**粗体**
++下划线++
~~删除线~~
==文本高亮==
**** (分隔线1)
---- (分隔线2)
```
- 分隔线
  - 在一行中用三个以上的星号 (`*`)、减号 (`-`) 或是下划线 (`_`) 来建立一个分隔线
  - 需要独占一行，中间可以有空格，但行内不能包含其他字符
  - 如下三种写法等价：
```
***
* * *
---
```

- **范例**
    ```
    ---
    我用`Stata`已经有**十五年**了，
    它的++功能++很==庞大==。
    目前还没有~~发现~~明显的*缺点*。
    * * *
    ```
**输出效果：**  
我用`Stata`已经有**十五年**了，它的++功能++很==庞大==。目前还没有~~发现~~明显的*缺点*。
---

- **换行**：Markdown 中使用`两个空格+回车`实现换行。


### 1.4 链接、图片、注释

#### 1.4.1 文本链接
Markdown 支持两种形式的文本链接语法：++行内式++和++参考式++。

- **行内式**： `[文本](链接地址)`   
  也可以采用这种格式：`[文本](链接地址 "可选说明文字")`。
好处是当鼠标停留在`文本`上时，可以显示`可选文字说明`的内容。
  > **范例：**
    
  ```R
  学习 [Stata](http:\\www.stata.com) 过程中遇到问题，
  可以查看 [老连](http://www.lingnan.sysu.edu.cn/lnshizi/faculty_vch.asp?name=lianyj 
  "连玉君主页") 
  的 [知乎专栏](https://zhuanlan.zhihu.com/arlion "连玉君 Stata 知乎专栏")。
    ```
  > 学习 [Stata](http:\\www.stata.com) 过程中遇到问题，
  可以查看 [老连](http://www.lingnan.sysu.edu.cn/lnshizi/faculty_vch.asp?name=lianyj 
  "连玉君主页") 
  的 [知乎专栏](https://zhuanlan.zhihu.com/arlion "连玉君 Stata 知乎专栏")。

- **参考式：** 
  > `[文本][标签]`
  >
  > `[标签]: 链接地址 "可选文字说明"`

  - Npte: 第一行定义与第二行定义之间要有一个空行；
  - 参考式比较适合定义那些将在文中多次出现的文本链接；
  - 使用参考式的最大好处是代码的可读性比较高。
  - **范例：**
  ```
  学习 [Stata][1] 时遇到问题，
  可以查看 [老连][2]
  的 [知乎专栏][Lzh]。
  [老连][2] 用 [Stata][1] 很多年了，人很好。  
  
  [1]: http:\\www.stata.com
  [2]: http://www.lingnan.sysu.edu.cn/lnshizi/faculty_vch.asp?name=lianyj "连玉君主页" 
  [Lzh]: https://zhuanlan.zhihu.com/arlion "连玉君 Stata 知乎专栏"
    ```
    
  > 学习 [Stata][1] 时遇到问题，
  可以查看 [老连][2]
  的 [知乎专栏][Lzh]。
  [老连][2] 用 [Stata][1] 很多年了，人很好。  
  
  [1]: http:\\www.stata.com
  [2]: http://www.lingnan.sysu.edu.cn/lnshizi/faculty_vch.asp?name=lianyj "连玉君主页" 
  [Lzh]: https://zhuanlan.zhihu.com/arlion "连玉君 Stata 知乎专栏"  

#### 1.4.2 插入图片
插入图片的方法与文字链接非常相似，也分为行内式和参考式两种类型。这里仅介绍行内式。
- 语法格式： `[图片上传失败...(image-349243-1537717802394)]`   
    或   `[图片上传失败...(image-c6a554-1537717802394)]`

范例：
  ```R
  这是老连讲课的样子
  ![](http://dwz.cn/lianyj45 "连玉君授课中")
  ```
  > 这是老连讲课的样子
  >![](http://upload-images.jianshu.io/upload_images/7692714-96e301647511e46b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "连玉君授课中")
    
 **说明：**  
  - 插入图片的语法和链接的语法很像，只是前面多了一个 `!`。将鼠标移动至图片区域内，光标下方会显示`连玉君授课中`字样。
  - 本地图片没有 `http://` 开头的链接，有人建议先通过新浪微博、[极简图床](http://jiantuku.com/#/)、七牛图床([攻略](https://zhuanlan.zhihu.com/p/20380531?refer=cnfeat))等生成图片链接网址。其实，[简书](http://www.jianshu.com/u/69a30474ef33)、[码云](https://gitee.com/arlionn) 都自带图床功能，直接将图片粘贴在编辑器中即可生成图片链接。参见 [用简书作 Markdown 图床](http://www.jianshu.com/p/67934ea29945)。
  - 若用新浪微博做图床，则操作为：在新浪微博发布一个包含待上传图片的微博（选择`仅自己可看`），然后`右击`图片，选择`复制图形地址(O)`即可。 
  - 我目前使用的方法是，在 [简书](http://www.jianshu.com/) 中新建文章，然后把图片拖入编辑框，图片链接会自动生成。
  - 目前，Markdown 还无法指定图片的宽高，如果需要，可以使用普通的 <img> 标签，亦可参考 [Markdown 图片尺寸设定](https://www.zhihu.com/question/23378396 "在 Markdown 中定义图片大小或比例(知乎)")。
  - 许多 MarkDown 编辑器中直接按原图大小显示图片，造成版面凌乱。如何让图片像简书中那样大小一致居中显示呢？使用该命令`[图片上传失败...(image-eb9661-1537717628356)]`设置图片大小，再用`<div align=center></div>`命令包裹达到居中效果，如：
  ```
  <div align=center>
  ![Stata连享会二维码](http://t.cn/R0aNoWh)
  </div>
  ```
  - **Note:** 经测试，**简书**和**有道云笔记**都不支持上述设定方式，但在网易邮箱和微信公众平台使用 Markdown here 插件时，都可以凑效。


#### 1.4.3 脚注 / 尾注 / 参考文献

可以采用 **文本^[#]^** 模式添加尾注。这个尾注也可以用作学术论文的参考文献。

- 定义方法：
    
  ```
  Stata[^1] 可以很方便地估计各类面板数据[^xt]模型，连玉君(2008)[^连2008] 应用了动态面板模型。
  
  [^1]: 一个很流行的数据处理和统计分析软件。
  [^xt] 也称为追踪数据。
  [^连2008]: 连玉君, 苏治, 2008, 现金持有动态调整机制研究. 《世界经济》，第 10 期。
  ```
  Stata[^1] 可以很方便地估计各类面板数据[^xt]模型，连玉君(2008)[^连2008] 应用了动态面板模型。
  [^1]: 一个很流行的数据处理和统计分析软件。
  [^xt]: 也称为追踪数据。
  [^连2008]: 连玉君, 苏治, 2008, 现金持有动态调整机制研究. 《世界经济》，第 10 期。
    
- 说明：
  - 正文中的添加尾注标签：`文字内容[^label]`。
  - 定义尾注内容：`[^label]: 标签的具体内容` (冒号要采用英文半角录入)。
  - `label` 可以是数字、英文字母、汉字，或者三者的组合。
  - `[^label]: 标签的具体内容` 可以放在任何位置。

#### 1.4.5 缩写名词/专有名词/索引条目

- 定义方式
    
  `*[缩写名词]: 名词解释文字`
  - 范例：
  
  ```markdown
  在 Stata 中，千万别把 GMM 当成 郭美美。
  *[GMM]: Generalized Mothod of Moment (广义矩估计方法)
  *[郭美美]: 一个网红美女
  ```
  
  > 在 Stata 中，千万别把 GMM 当成 郭美美。
*[GMM]: Generalized Mothod of Moment (广义矩估计方法)
*[郭美美]: 一个网红美女
    
- 说明
  - 正文中引用缩写名词 (如上例中的 **GMM** 和 **郭美美**) 时，前必须有一个空格；
  - 引用缩写名词时不能附加其他效果(如斜体、粗体等);
  - 一次定义，全文有效：一旦在文中任何一处定义了缩写名词， 则无论在定义之前还是之后出现的缩写名词都会显示其释义。我通常会把所有缩写名词都定义在 `[toc]` 语句下方，便于查看和修改。

### 1.5 文字高亮和代码块

- **文本高亮**。需要高亮的文本，用 `` ` `` 包围之。如，输入 `` `stata` ``，显示为 `stata`。
- **引用代码块**。可以使用两组 `` ``` `` (独占一行) 包围这段代码，或将代码缩进四个空格 (直接敲入 `Tab` 键即可产生四个空格)。
  * 范例
  ````markdown
  ```stata
  sysuse auto, clear 
  regress mpg price wei leng, robust
  ```
  ````
  **输出效果：**
  ```stata
  sysuse auto, clear 
  regress mpg price wei leng, robust
  ```

  **Notes**:   
  - 上述代码引用效果是通过用 `` ````markdown `` (首)
  和 `` ```` `` (尾) 包围来实现的。二者都是独占一行。
  -  `` ``` `` 后面的 `stata` 表示此段代码为 `stata`  代码，Markdown 会自动使用 stata 代码颜色渲染。根据需要，可以替换成 `R`, `python`, `java` 等。平时也可以不写。

- **代码段中的反引号。** 如果要在代码区段内插入反引号 `` ` ``，可以用多个反引号来开启和结束代码区段。
  - 范例：
  ```
  你可以在正文中呈现 `` ` ``，`` `stata` ``，
  以及 `` ```stata `` 的原貌。
  ```
  > 你可以在正文中呈现 `` ` ``，`` `stata` ``，以及 `` ```stata `` 的原貌。
    
- **列表中包含引用。** 如果要在列表中放入引用，那 `>` 就需要缩进 4 个空格或 1 个制表符：

  ```
  * A list item with a blockquote:
      > This is a blockquote
      > inside a list item.
  ```
  
  * A list item with a blockquote:
      > This is a blockquote
      > inside a list item.
    
- **列表中放入代码块。** 如果要在列表中放代码区块的话，该区块就需要缩进两次，也就是 8 个空格或是 2 个制表符：
  ````
  - 呈现相关系数矩阵  
        ```
        pwcorr y x1 x2 x3
        ```
  ````
  
  - 呈现相关系数矩阵  
        ```
        pwcorr y x1 x2 x3
        ```


### 1.6 特殊字符
Markdown 中可以使用 HTML 标记来呈现一些特殊字符。参见 [++特殊字符列表++](http://www.jb51.net/onlineread/htmlchar.htm)，以及[++上标和下标++](http://www.jianshu.com/p/80ac23666a98)。

- 转义符：在如下字符前加反斜杠 `\` 可以呈现其原貌：

    ```
    \   反斜线
    `   反引号
    *   星号
    _   底线
    {}  花括号
    []  方括号
    ()  括弧
    .   英文句点
    !   惊叹号
    上角文字: 19^th^
    下角文字: H~2~O
    ```

- **范例：**
    ```markdown
    \#陌上花开\#，可缓缓归\`矣\` !  
    (我+你)\-她 = Sad^2^ + H~2~O
    ```
    > \#陌上花开\#，可缓缓归\`矣\` !  
    > (我\-你)+她 = Happiness^3^ + Sad^2^ + H~2~O

- **段首空两格**
  - 在 Markdown 里直接打空格无法实现段首缩进。但由于 Markdown 支持 HTML 语法，可以用 ` `，` ` 等 [HTML 标记符](http://blog.csdn.net/xcg8818/article/details/75006210) 产生空格效果。
  - 范例：
    ```R
    普通空格| |    
    半角空格| |
    全角空格| |
      让我们另起一段吧。(此处：两个空格+回车)  
    其实，在网页中看文字，段首空两行反而难受。
    ```
    > 普通空格| |  
    > 半角空格| |  
    > 全角空格| |  
    >   让我们另起一段吧。  
    > 其实，在网页中看文字，段首空两行反而难受。

  - 另一种[方法](http://www.zhihu.com/question/21420126)：切换到全角模式下（中文输入法中按 `shift + space`）输入两个空格即可，宽度恰好是两个汉字。不过，此法在有些编辑器(如，有道云笔记的 Markdown) 中不适用。 


### 1.7 表格

具体使用方式请看示例。
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
**语法说明：**
- `----:` 为右对齐
- `:----` 为左对齐
- `:---:` 为居中对齐
- `-----` 为使用默认居中对齐

**Notes:**
  - 输入代码时，无需对齐，上述呈现方式只是为了美观；
  - 若想快速生成表格，可以到 [TablesGenerator.com](http://www.tablesgenerator.com/markdown_tables) 在线生成，或者用小软件 [exceltk0.0.4.7z](www.jianshu.com/p/4705a1dc8e5a)。若是经常有大量表格需要生成，可以用 [Typora](http://www.jianshu.com/p/a70c16bc543b) 编辑器。
  - Markdown 不支持单元格合并，复杂表格只能使用 HTML 语句，参见 [Markdown表格之合并单元格效果 ](http://blog.csdn.net/loongshawn/article/details/72829090?ref=myread)。
  
### 1.8 目录

自动生成的目录有两类：顶端目录和侧边目录。
对于有道云笔记中的 Markdown 文档而言，可以在需要插入目录的地方输入 `[toc]` 即可自动生成目录。

- 目录的层级与你所定义的标题层级一致。
- 有些编辑器不支持 `[toc]` 目录功能，如【简书】。
> 相关链接
- [MarkDown 自动生成目录](http://www.jianshu.com/p/1b29172d4f7e) 


## 2. Markdown 进阶功能

### 2.1 数学公式

**基本语法：**
- 行内公式：`$行内公式$`
- 行间公式：`$$行间公式$$`  

**例如：**  
```
$$
y_i = \beta_0 + x_i\beta_1 + \epsilon_i
$$
其中，$\epsilon_i$ 表示干扰项，$ i=1, 2, \cdots, N $。
```
**输出效果为：** 
$$
y_i = \beta_0 + x_i\beta_1 + \epsilon_i
$$
其中，$\epsilon_i$ 表示干扰项，$ i=1, 2, \cdots, N $。

- **特别说明：**
  - 上面介绍的是标准的 Markdown 语法中数学公式的写法。**有道云笔记**的语法比较特殊：
    - 行间公式要用 `` ```math `` 和 `` ``` `` 包围；
    - 行内公式则用 `` `$ `` 和 `` $` `` 包围。


- 相关链接
  - [简书支持 LaTeX 数学公式了！](https://www.jianshu.com/p/d08cb0472af1)
  - [常用 LaTeX 数学公式写法](http://www.mohu.org/info/symbols/symbols.htm)
  - [Markdown LaTeX 完美教程](http://xiang.leanote.com/post/introduction-to-mathjax-and-latex-expression)
  - [如何在 Markdown 中添加数学公式？](http://www.cnblogs.com/peaceWang/p/Markdown-tian-jia-Latex-shu-xue-gong-shi.html) 
  - [如何在简书中编辑数学公式 - 简书](https://www.jianshu.com/p/cf8922042d0c) 这篇非常适合简书作者，但编辑过程仍然比较复杂



### 2.2 流程图、甘特图等

[StataChina-github](https://github.com/StataChina/mermaidjs.github.io) 提供非常详细的语法说明和范例。


### 2.3 编辑器
- 目前我主要在如下三个地方使用 Markdown：[**有道云笔记**](http://note.youdao.com/)、[**简书•Stata连享会**](http://www.jianshu.com/u/69a30474ef33) 和 [码云](https://gitee.com/arlionn)。
  - **有道云笔记**对 Markdown 的支持基本完整，数学公式的引用格式与一般的 Markdown 编辑器略有差异，缺点是无法直接插入本地图片；
  - **简书**不支持直接录入数学公式，但可以通过简单地拖拽来插入本地图片，本身自带图床功能。因此，可以把简书作为有道的图床。
  - **码云**也自带图床，支持直接在编辑器中粘贴图片，但无法输入数学公式。其 Markdown 语法使用了 [通用标注 (CommonMark)](http://www.commonmark.cn/w/) 标准，参见 [用 10 分钟学会 Markdown](http://commonmark.cn/help/) 和 [码云帮助文件之 Markdown](http://git.mydoc.io/?t=223172)。

- 此外，在 [**github**](https://github.com/StataChina "Stata连享会github") 上也可以在线写 Markdown。
- 微信公众平台不支持 Markdown 格式，但可以通过下面将要介绍的 Markdown Here 插件来实现。
- [7 款优秀 Markdown 编辑工具推荐](https://sspai.com/post/27792)


### 2.4 Markdown Here 插件
使用 Markdown Here (MDH) 插件，可以大幅提高 Markdown 的输出效率和效果。除了在微信公众平台、知乎等网站可以用这个插件快速的做好牛逼闪闪的排版效果以外，在发网页邮件的时候，用这个插件也可以让自己一键编辑出高端大气上档次的专业风范邮件排版，给自己的职业形象加分。  

**例：**[在知乎使用 Markdown](https://zhuanlan.zhihu.com/p/23501187) 

**思路**: `Chrome 浏览器 + Markdown Here 插件`, 用后者将 Markdown 代码渲染为 html 格式代码。

**流程:** 

- 下载一个叫 Markdown Here 的插件并安装
- 在 chrome 上打开知乎编辑器使用 
- Markdown 格式进行写作
- 写作完毕，点击浏览器右上角 Markdown here 插件图标进行渲染，发布文章

**相关链接：**
- [使用 Markdown 写知乎专栏的终极解决方案](https://zhuanlan.zhihu.com/p/26983517)
- [在知乎使用 Markdown](https://zhuanlan.zhihu.com/p/23501187) 
- [将 Markdown 文章搬运到微信公众号](http://blog.csdn.net/dyc87112/article/details/77417572)
- [简书+MDH](http://www.jianshu.com/p/a28ccdf88c0f) | [微信公众号+MDH 1](http://www.jianshu.com/p/4bbe8b439dad) | [微信公众号+MDH 2](http://www.jianshu.com/p/e766a7ea063a)


### 2.5 HTML 与 Markdown 的相互转换

有时看到别人用 Markdown 写的文章不错，想要弄到自己的文章中，但又不想重新按照 Markdown 的语法敲一次，可以使用在线工具转换。

**方法：** 选中网页中的文字或链接，右击，在浏览器弹出窗空中选择`查看选中部分的源码`，然后把弹出的源码粘贴到如下在线工具中，转换成 Markdown 格式。

- HTML → Markdown :  [BeJSON](http://www.bejson.com/convert/html2markdown/)  | [aTool](http://www.atool.org/html2markdown.php) 

### 2.6 Markdown 与 Word 快捷转换：Writage

Markdown 虽然语法简洁，适合博客、笔记等非正式文档的写作。但对于格式复杂的正式报告、论文等正式文档则不适合。  

解决这一矛盾的基本思路是: 用 Markdown 写初稿，用 Word 进行精细化排版。转换工具为 [Writage](http://www.writage.com/)。详情参见：[简书-Writage](http://www.jianshu.com/p/f9c5da56e0cb)。


### 2.7 有道云笔记 Markdown 编辑器快捷键

- `Ctrl+Shift+Y`: 激活窗口
- 选中多行文本后，按下 `Tab` 键，可以整体右移一个 `Tab` 键位；
- `Ctrl+D`: 删除光标所在的段落
- `Ctrl+Shift+D`: 复制当前段落内容到下一行；
- `Ctrl+Cmd+K`: (选中部分文本时) 按此键向下查找并选中相同的文本；
- `Ctrl+Shift+L` 选中光标所在的段落，多次按下该组合键，可以向下选中多个段落；
- `Ctrl+[`：将光标所在行向**左**缩进一个 Tab 键位；
- `Ctrl+]`：将光标所在行向**右**缩进一个 Tab 键位


---
## 参考文献 (相关资料)

- [完整版 Markdown 手册](http://wowubuntu.com/markdown/)
- [Markdown 编辑器语法指南](https://segmentfault.com/markdown) 写的非常精炼，清晰
- [码云项目-Markdown笔记](https://gitee.com/6296777/nxin-docs/blob/master/Markdown.md)(很详细)



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
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-31a58c8f29ee6c2c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
