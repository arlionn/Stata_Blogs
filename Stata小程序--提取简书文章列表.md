> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) | [github](http://github.com/StataChina))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


> **问题背景：**
> - 在每一篇简书推文的末尾，我都会加上【往期回顾】栏目，如【图1】。   
> - 这些超链接文字对应的 Markdown 语句挺繁琐，见【图2】。   
> - 我以前笨，手动操作，需要从我的简书主页 (图3) 中逐个【复制】→ 【粘贴】4 次才能完成一个链接。

> **解决办法：用 Stata 写个小程序！**
>
> **思路：**
> 1. 用 `copy` 命令从我的【简书主页】读取 网页数据，存储到一个文本文件中；
> 1. 用 `infix` 命令将上述文本文件读取为 Stata 格式的数据；
> 1. 使用 `subinstr()` 等字符函数或正则表达式处理网页中的无用信息，进而将处理好的**干净**文本改写为 Markdown 格式的链接；
> 1. 用 `export delimited` 命令把处理好的数据读入文本文件，进而用 `shellout` 命令打开这个文本文件；
> 1. 从文本文件中复制自动生成的链接文字，贴入简书 Markdown  编辑器。
>
> **使用方法：**
> 1. 把上述代码写成 Stata 程序，即 `.ado` 格式的程序文件，命名为 `jslist.ado`；
> 1. 每次使用时，在 Stata 命令窗口中输入 `jslist` 即可自动生成【图2】中的 Markdown 文本。参见【图4】和【图5】。




![图1：简书文中【往期回顾】输出效果](http://upload-images.jianshu.io/upload_images/7692714-1698be648ceb4390.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图2：简书文中【往期回顾】的 Markdown 原文](http://upload-images.jianshu.io/upload_images/7692714-8bc7a765fd45a516.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图3：我的简书主页](http://upload-images.jianshu.io/upload_images/7692714-665a4c50bca6e1c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 附：原始 ado 文件

```stata
*! 呈现连玉君的简书文章列表 Markdown 格式
*! 2017/6/18 12:29
*! 后续工作：把这个 txt 文档的内容直接写入 搜狗 短语文件中，
*! 键盘输入时，只需输入 jslist 即可插入最新列表

program define jslist
version 15

  *-从简书个人主页copy个人文章列表

*  cd "D:\stata15\ado\personal\Jianshu\jianshuList"  // 路径可以自行更改或忽略，默认保存在当前工作路径下
 
*----------------------quietly------------bein---  
qui{                          
  copy "http://www.jianshu.com/u/69a30474ef33" "jianshu_list.txt", replace  
  *shellout "jianshu_list.txt"
  *shell rename jianshu_list.txt jianshu_list.do
  *doedit "jianshu_list.do"  
 
  *-将网页 txt 文档读入 stata 
   local ff  "jianshu_list"
   infix strL var 1-3000 using "`ff'.txt", clear
   
  *-去除无用 codes, 保留所需代码  
   format var %-600s   
   keep if strmatch(var, `"*<a class="title" target="_blank" href="/p/*"')
   replace var=subinstr(var,"/p/","http://www.jianshu.com/p/",.)
   replace var=subinstr(var,`"href=""',"<",1)
   replace var=subinstr(var,`"">"',"<",1)
   
  *-将处理后的文本转换成 Markdown 链接语句 
   split var, parse(<)
   keep var3 var4
   rename (var3 var4) (url title)
   order title url
   replace title = "- [" + title
   replace title = title + "]"
   replace   url = subinstr(url,"http","(http",.)
   replace   url = url+")"
   gen v = title+url
   format v %-300s 
   keep v

  *-输出处理好的 txt 文本，并直接打开 txt 文档
   export delimited  using "`ff'_out.txt", novar nolabel replace
   
   shellout "`ff'_out.txt"
}                            
*----------------------quietly------------over---  

end   
```
       **jslist.ado** 文档


![图4：在Stata命令窗口输入 jslist 命令](http://upload-images.jianshu.io/upload_images/7692714-5a124d156512acb0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![图5：输出效果](http://upload-images.jianshu.io/upload_images/7692714-9ab821716587e162.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> ### 可以进一步扩展的地方
> 1. 把 `jslist.ado` 输出的 txt 文档内容直接写入 (`help file  write`) 搜狗 短语文件中；当使用搜狗输入法时，只需输入 jslist 即可插入最新列表；
> 1. 添加一个选项 `url(string)`，可以让用户自己指定需要访问的简书主页；
> 1. 目前的程序还无法获取完整的简述文章列表，还需要分析网页；



### 往期回顾

- [在 Markdown 中使用 HTML 中的特殊符号](http://www.jianshu.com/p/e65e1e36056b)
- [新数据，新视角！获取特殊数据的软件](http://www.jianshu.com/p/7382f9f57605)
- [Stata帮助和网络资源汇总(持续更新中)](http://www.jianshu.com/p/c723bb0dbf98)
- [协整：醉汉牵着一条狗](http://www.jianshu.com/p/08e194e14b7a)
- [在 Markdown 中快速插入文字连接](http://www.jianshu.com/p/ff3b1fa07a97)
- [Stata dofile 转换 PDF 制作讲义方法](http://www.jianshu.com/p/b119033d8b93)
- [Github使用方法及Stata资源](http://www.jianshu.com/p/d2ee76b0f74c)
- [码云：我把常用小软件都放这儿了](http://www.jianshu.com/p/fbb6bdeb9df5)
- [连玉君的链接](http://www.jianshu.com/p/494e6feab565)


---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-bf9d6afa87357522.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")


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
