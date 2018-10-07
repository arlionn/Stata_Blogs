> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


> **开头的话：容忍 J 效应**   
> 开始用快捷键的前两天，你会很不爽，痛恨自己记性不好。  
> 此时，大概会有 60% 的人选择放弃；  
> 然而，你若是那 40% 选择坚持的人，你的长期效率会大幅提高！  
>
> **世间事，概莫如此：恋爱、婚姻……，J 效应！**  
>  



>     Stata 快捷键速记表**（右击保存，人手一份）**

![Stata连享会-Stata15快捷键（右击保存，人手一份）](http://upload-images.jianshu.io/upload_images/7692714-57c4693c234de09b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**目 录**
   1. 为何使用快捷键？
   2. 最基本的快捷键
   3. 命令窗口快捷键
   4. Stata dofile 中的快捷键
   5. Stata 快捷键 GIF 动画演示
     5.1 dofile 快捷键
     5.2 dofile 中一次编辑多行(按列编辑)
     5.3 dofile 中快速添加注释语句
     5.4 Stata 快捷键范例

> ## 1. 为何使用快捷键？

虽然多数 `Stata` 用户还称不上“程序猿”，但“程序猿”们该有的毛病却基本上都不缺，什么颈椎病、鼠标手......。虽说根本的解决方法是少干活，多锻炼。但在任务既定的情况下，若能提高效率，事半功倍，也能一个不错的解决方法。

使用键盘要比鼠标效率高，也更健康一些。一方面，使用键盘时，双手都在不停地运动，不容易导致肌肉僵化(鼠标手其实就是握鼠标的手长期保值一个姿势导致的肌肉劳损)；另一方面，使用键盘时，背部可以得到支撑，甚至颈部也可以得到头枕的支撑。

这篇文章介绍了最为常用的一些 `Stata` 快捷键，下一篇将会介绍 `Stata` 界面和 `dofile` 编辑器的设定，二者配合起来，将打造一个舒适健康的 `Stata` 工作环境。配合`机械键盘`+`人体工学椅`，让我们的`类程序猿`工作不再那么辛劳。


> ## 2. 最基本的快捷键

最基本的那些 `Windows` 下的快捷键在 `Stata` 各个窗口中仍然适用，看似简单，但熟练运用能节省不少时间。

快捷键 | 作用效果 
---:|:----
`Ctrl+C` | 复制
`Ctrl+V` | 粘贴
`Ctrl+Z` | 撤销
`Ctrl+Y` | 恢复撤销
`Ctrl+F` | 查找
`Ctrl+H` | 替换
`Ctrl+A` | 全选
`Ctrl`+`Backspace` | 删除光标左侧的单词
`Alt`+`Ps/SR` | 活动窗口快照


**说明：**
- `Alt+Ps/SR` 的作用在于将活动窗口截屏到剪切板，随后可以以图片形式粘贴到 word 文档。  
- 平时经常用的 `Backspace` 键每次只能删除一个字母(汉字)，而 `Ctrl+Backspace` 则可以删除一个单词(以空格为区分单位)。


> ## 3. 命令窗口快捷键

命令窗口 (`Command Window`) 适合输入一些简短命令，比如 `help cmd`, `des`, `sum` 之类的。当然，有些用户也会输入诸如`reg price mpg weight` 之类的回归命令。

下面列示一些使用频率较高的快捷键：

快捷键 | 作用效果 
---:|:----
`Ctrl+1` | 光标切换到命令窗口
`Ctrl+8` | 查看数据
`Ctrl+9` | 打开 dofile 编辑器
`变量首字母+Tab`| 自动补全变量名
`Esc` | 清空命令窗口
`PgUp (PgDn)` | 重现上(下)一条命令
`F2` | 等价于 `describe` 命令
`Ctrl+F` 或 `F3` | 查找 (按`Esc`关闭)

事实上，除了使用快捷键，知道常用命令的简写方式也可以省去不少时间。比如：
- `regress` 可以简写为 `reg`
- `display` 可以简写为 `di` 或 `dis`
- `describe` 这种很多人都不会拼写的命令则可以简写为 `des` 甚至 `d`。
- `regress price weight length foreign` 可以简写为 `reg p w l f`

> ## 4. Stata dofile 中的快捷键

**dofile** 是我们做实证分析工作的主要战场，因此 `Stata` 也提供了大量的快捷键来提高写代码的效率。

快捷键 | 作用效果 
---|----
**dofile管理** | 
`Ctrl+o` | 打开已有 do 文档 (dofile)
`Ctrl+N` | 新建 do 文档 (dofile)
**执行命令** | 
`Ctrl+D` | (Do) 执行选中行 (选中一个以上的字母即可)
`Ctrl+Shift+D` | (Do) 执行光标所在行以下的所有命令
`Ctrl+R` | (Run) 执行选中行, 屏幕不显示结果
**编辑类** | 
`Alt+鼠标`  | 纵向选择(按列选择)
`Alt+Shift` + ↑/↓ |	纵向选择 (按列选择)
`Ctrl`+→ | 从光标处向右逐个**字母**选中代码
`Ctrl+Shift`+→  | 从光标处向右逐个**单词**选中代码  
`Ctrl+L` | 选中光标所在行(**L**ine)
`Ctrl+Shift+L`  | 删除光标所在行
`Ctrl+I` |	将选中代码整体右移两个空格
`Ctrl+Shift+I` |	将选中代码整体左移两个空格
`Shift`+↑| 从光标处向上逐行选中代码
`Shift+PgUp` | 自光标处向上逐块选中代码
`Home` | 光标跳转到行首
`End`  | 光标跳转到行末
`Ctrl+Home` | 光标跳转到dofile文档首行
`Ctrl+End`  | 光标跳转到dofile文档末行
**添加注释符** | 
`Ctrl+/` | 在选中代码段行首加 `//` 注释符, 复按取消
`Ctrl+Shift+/` | 在选中代码段首尾加 `/*` `*/` 注释符  
`` Ctrl+Shift+\ `` | 删除所选代码段首尾的 `/* */` 注释符
说明 | 
←, ↓, `PgDn`| 与 →, ↑, `PgUp` 作用相反


> ## 5. `Stata 快捷键` GIF 动画演示

### 5.1 命令窗口快捷键
在命令窗口中输入变量的首字母，按 Tab 键后，Stata 会自动补齐变量名：
![Stata 命令窗口中输入变量的首字母，按 Tab 键自动补齐变量名](http://upload-images.jianshu.io/upload_images/7692714-9f550483fa7e638f.gif?imageMogr2/auto-orient/strip)

![Stata 命令窗口中，按 PgUp 显示上一条命令，Esc 清除命令](http://upload-images.jianshu.io/upload_images/7692714-8c77ea6e40381825.gif?imageMogr2/auto-orient/strip)

### 5.2 dofile 快捷键
- 选中一段代码后，按下快捷键 `Ctrl+D` 可以执行这些代码。
- 若按下快捷键 `Ctrl+Shift+D` 可以执行光标所在行以下的所有代码。

![用 `Ctrl+D` 快速执行选中命令](http://upload-images.jianshu.io/upload_images/7692714-c2c10f427ac7c095.gif?imageMogr2/auto-orient/strip)


### 5.3 dofile 中一次编辑多行(按列编辑)

`Alt+鼠标` 或 `Alt+Shift+方向键`：按列编辑

![按住Alt+鼠标按列编辑](http://upload-images.jianshu.io/upload_images/7692714-d93238b9b21cf968.gif?imageMogr2/auto-orient/strip)


### 5.4 dofile 中快速添加注释语句
话不多说，直接上图吧：
- 按下快捷键 `Ctrol+Shift+/`，可以为 `dofile` 中选中的代码段快速添加注释符 `/* */`.
![Stata快捷键-`Ctrol+Shift+/` -快速添加注释语句](http://upload-images.jianshu.io/upload_images/7692714-2ccc4f0c490eca25.gif?imageMogr2/auto-orient/strip)



### 5.5 Stata 快捷键范例

- **例 1：** 快速添加注释符号+快速缩进
![stata快捷键：快速添加注释符号+快速缩进](http://upload-images.jianshu.io/upload_images/7692714-7579f5ee6aeb5a2b.gif?imageMogr2/auto-orient/strip)

- **例 2：** 快速添加注释标记的三种方法

![Stata快捷键-快速添加注释语句的三种方法](http://upload-images.jianshu.io/upload_images/7692714-bab2f0cda4df9cf5.gif?imageMogr2/auto-orient/strip)

> **最后的话：容忍 J 效应**   
> 开始用快捷键的前两天，你会很不爽，痛恨自己记性不好。  
> 此时，会有 60% 的人选择放弃；
> 然而，你若是那 40% 选择坚持的人，你的长期效率会大幅提高！  
> **世间事，概莫如此：恋爱、婚姻……【J 效应】**

- 若需下载原始 Markdown 文档：
  - 请移步至 [码云-Stata连享会][1]，可以选择`一键复制`至剪切板，或`按行查看`；或直接[右击这里][2]下载。
  - 也可以直接[保存][3]到你的有道云笔记

[1]:https://gitee.com/arlionn/jianshu/blob/master/stata15%E5%BF%AB%E6%8D%B7%E9%94%AE-%E9%94%AE%E7%9B%98%E5%B0%B1%E6%98%AF%E4%BD%A0%E7%9A%84%E6%AD%A6%E5%99%A8.md

[2]:https://gitee.com/arlionn/jianshu/attach_files/download?i=97379&u=http%3A%2F%2Ffiles.git.oschina.net%2Fgroup1%2FM00%2F01%2FFF%2FPaAvDFnOOu2ALBfNAAAdiUcl53U4225.md%3Ftoken%3Da0fe861060e9a44d06a5d590f0bee65a%26ts%3D1506687745%26attname%3DStata15%25E5%25BF%25AB%25E6%258D%25B7%25E9%2594%25AE-%25E9%2594%25AE%25E7%259B%2598%25E5%25B0%25B1%25E6%2598%25AF%25E4%25BD%25A0%25E7%259A%2584%25E6%25AD%25A6%25E5%2599%25A8.md

[3]:http://note.youdao.com/noteshare?id=8cd64356620a49974c99ca1f1c339a1d

---
 ![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-e0d33e1d4e0e9437.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

---
往期回顾

  ¤ [Stata兄弟：提问时能否有点诚意？用 dataex 吧](http://www.jianshu.com/p/9870080fe769)  
  ¤ [[转]Unicode 和 UTF-8 有何区别？](http://www.jianshu.com/p/ab72465608d8)  
  ¤ [一个博士生该掌握哪些基本工具(武器)？](http://www.jianshu.com/p/90d6a54e35a5) 
  ¤ [DSGE 模型的 Stata 实现](http://www.jianshu.com/p/4579812bd0f7)
  ¤ [第一届stata中国用户大会嘉宾PDF讲稿下载](http://www.jianshu.com/p/db46fae3fd95)
  ¤ [彼此不再煎熬-如何做好毕业答辩陈述？](http://www.jianshu.com/p/086f9cf2cc30) 
  ¤ [连玉君 Markdown 笔记](http://www.jianshu.com/p/db1d26af109d)                                                                                                                                                                                                                    
  ¤ [如何处理时间序列中的日期间隔 (with gaps) 问题？](http://www.jianshu.com/p/1c8423ee6c13) 
  ¤ [使用 -import fred 命令导入联邦储备经济数据库 (FRED)](http://www.jianshu.com/p/87e4dae27864)
  ¤ [Fuzzy Differences-in-Differences （模糊倍分法）](http://www.jianshu.com/p/8918037d76a1)



>## 关于我们
- 【**Stata 连享会**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。

>### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
