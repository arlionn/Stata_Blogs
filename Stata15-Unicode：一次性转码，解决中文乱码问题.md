> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


## ua 命令使用说明

### ua 命令
适用于 stata15 用户。可以一次性对当前工作路径以及所有子文件夹中的文件进行转码(unicode)，以保证中文字符可以正常显示。

#### 安装
- **方法1：**
`2018/1/6 更新：` **ua** 命令已经被 **[SSC](http://www.repec.org)** (`help ssc`) 收录。
在 Stata 命令窗口中输入如下命令即可自动下载安装最新版的 `ua` 命令：
```stata
ssc install ua, replace
```

- **方法2：**
[- 到这里 -](https://gitee.com/arlionn/ua) 下载 `ua.ado` 和 `ua.hlp`，放置于 `D:\stata15\ado\base\u` 或 `D:\stata15\ado\plus\u` 文件夹中。（下载地址：https://gitee.com/arlionn/ua）

#### 使用
- 在 Stata 命令窗口中输入 `help ua`，查看命令介绍和 Stata 范例。参照范例使用即可。

#### Stata 范例
```stata

  * 改变当前工作路径（进入这个文件夹）
  * change current working directory (CWD)
    . cd "D:\stata15\ado\personal\mypaper"

  * 对当前文件夹以及所有子文件夹中的所有文件进行转码
  * Unicode all files (.do, .ado, .dta, .hlp, etc.) in CWD and files in sub-directories
    . ua: unicode encoding set gb18030
    . ua: unicode translate *

```

#### 你如何用 ？
复制如下代码，每次使用时，选中如下三行，按快捷键 `Ctrl+D` 即可完成转码。你只需修改需要转码的文件路径即可：
```stata
. cd "D:\stata15\ado\personal\mypaper"   // 请自行修改路径
. ua: unicode encoding set gb18030
. ua: unicode translate *
```

&emsp;
&emsp;
&emsp;

>**注：后续内容与 `ua` 命令的使用没有直接关系，只是记录了程序的撰写过程。**

&emsp;
&emsp;
&emsp;


## 故事背景（程序撰写过程）

> 我昨天下午 5 点有个需求：我是从 Stata 13 直接跳到 15 的。可是，Stata 15 的中文编码方案全变了，导致 do-file 和数据文件中的中文字符全是乱码。Stata 提供了一组 `unicode` 开头的命令，可以很方便地进行转码。但只能一个文件夹一个文件夹地转。我用了 14 年的 stata，有成百上千个文件夹需要转码！ 搜索了半天无果。只好求助涛哥 (李春涛是也)。  
>
>涛哥的第一反应是需要编程。我们的共识是需要遍历所有文件夹，记录下来，然后用循环语句进入每一个文件夹，进行转码。
>
> 我以为他会停几天再开始做这个工作。又不好催促他，以便搭他的便车。只好自己开始弄。或许他也不好意思搭我的便车，哈哈。
>
> 没想到今天晚上写好所有程序和说明文档后分享给他时，他居然也完工了！

> 看来，有好奇心的人都是一样的亟不可待！


## 背靠背工作记录

### 有想法：2017/12/18 17:05
>![](http://upload-images.jianshu.io/upload_images/7692714-3b85635cc3c3d61d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>![](http://upload-images.jianshu.io/upload_images/7692714-4ef182da2033c125.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 同时完工：2017/12/19 22:28
>![](http://upload-images.jianshu.io/upload_images/7692714-0d5367eaee8f0689.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>![](http://upload-images.jianshu.io/upload_images/7692714-f2854e7da17aca73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
>![](http://upload-images.jianshu.io/upload_images/7692714-2e767c22c231d8dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 我们的思路有何差别
> **整体思路**：遍历当前文件夹下的所有子文件夹，对每个文件夹进行转码。
>
> **问题的关键**：如何遍历所有的子文件夹，并记录这些文件夹的名称。

> **涛哥的思路**：使用如下 `dos` 命令遍历当前工作文件夹下的所有子文件夹，并将他们存入一个文本文件 `output.txt`，随后使用`infix`命令读入内存：
```stata
!dir /B /S /ad >> output.txt
infix strL v 1-2000 using "output.txt"
```
>最终，涛哥的程序长这样：
![](http://upload-images.jianshu.io/upload_images/7692714-4c3f1d481556b4fd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **君哥的思路**：使用外部命令`rcd`实现上述功能，所有子文件夹名称均以返回值的形式存储于内存中。执行过程如下：
![image.png](http://upload-images.jianshu.io/upload_images/7692714-b7c0704c03fa65c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后就可以写一个循环逐个文件夹进行转码了：
![image.png](http://upload-images.jianshu.io/upload_images/7692714-cf27f6e549f7ecde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最终，君哥的程序长这样：
![image.png](http://upload-images.jianshu.io/upload_images/7692714-e39f8800b387428b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 原始程序和使用方法

### 涛哥的程序
涛哥的程序尚未封装成 `.ado` 文件，但用起来到也方便。直接复制如下代码到一个 Do-file 中，使用 `cd` 命令进入需要转码的文件夹，然后选中如下命令，执行即可（快捷键是 `Ctrl+D`）：
```stata
cd "D:\stata15\ado\personal\mypaper" //自行修改
!dir /B /S /ad >> output.txt
clear
unicode analyze output.txt
unicode encoding set gb18030
unicode translate  "output.txt", transutf8
unicode erasebackups, badidea
infix strL v 1-2000 using "output.txt"
levelsof v ,local(urllist)
! del output.txt
clear 
unicode translate * , transutf8
foreach c of local urllist{
	clear
	cd `"`c'"'
	unicode encoding set gb18030
	cap unicode translate * , transutf8
}
```

### 君哥的程序
已经封装成 `.ado` 文件，并配有说明文档。请到 [Stata连享会-码云-ua](https://gitee.com/arlionn/ua/tree/master)项目中（链接为：https://gitee.com/arlionn/ua ）下载 `ua.ado` 和 `ua.hlp` 文件，放置于 `D:\stata15\ado\base\u` 或 `D:\stata15\ado\plus\u` 文件夹中。然后在 Stata 命令窗口中输入 `help ua`，查看命令介绍和 Stata 范例。参照范例使用即可。

#### Stata 范例
```stata
* 程序下载地址和使用说明：
* https://gitee.com/arlionn/ua
* 进入需要转码的文件夹
  . cd D:\stata15\ado\personal\mypaper

* 对当前文件夹及子文件夹中的所有 .dta 文件转码
  . ua: unicode encoding set gb18030
  . ua: unicode translate *.dta

* 对所有类型的文件转码
  . ua: unicode analyze *
  . ua: unicode encoding set gb18030
  . ua: unicode translate *
```

### 结语
> 生活如此美好！因为有你有我！

&emsp;


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

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-3926517a5af2bde4.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")







