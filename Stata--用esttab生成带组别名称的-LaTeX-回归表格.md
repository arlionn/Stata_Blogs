
> 作者：华晨 (The University of Manchester)    
>
> &emsp;&emsp;&emsp;Stata 连享会  ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


最近在跑数据的时候做了许多分组的回归，由于在写作论文时使用LaTeX编辑，因此需要产生多个分组回归的表格插入LaTeX文档。

众所周知，LaTeX表格的代码很(fan)复(ren)杂(lei)，不适合人工编辑，因而许多用户都建议利用一些工具直接产生或将电子表格转换成LaTeX表格代码再插入LaTeX文档中进行编译。

我最开始使用常用的esttab命令将表格导入到Excel中，再在Excel中进一步美化和编辑各个分组回归的表格，最后用excel2latex插件转出表格的LaTeX代码再插入LaTeX主文档中。

虽然此方法可行，但毕竟还是需要一定量的手工作业。Excel2latex插件导出的LaTeX代码也比较呆板，在插入主文档时还需要自己不断地调试和debug，尤其是组别名要跨行的时候，简直无力吐槽。

对着满屏的数字和反斜线，头昏眼花。伟大的高尔基同志教导我们说，懒人改变世界。

因此，花了一个小时研究了一下用强大到令人畏惧的 `esttab` 生成分组回归表格的LaTeX表格代码，于此分享。

闲言少叙，书归正传。对于分组的样本的回归，我们通常想要一个这样的表格:
![](http://upload-images.jianshu.io/upload_images/9797551-081b6fb56fd5221b.PNG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

怎样用Stata来生成它的LaTeX表格代码呢？我们直接来看个例子。
```stata
cls
sysuse atuo, clear
eststo clear
eststo: quietly reg weight mpg
eststo: quietly reg weight mpg foreign
eststo: quietly reg price weight mpg
eststo: quietly reg price weight mpg foreign
```
我们利用 `auto.dta` 模拟了四个回归模型，下面我们想把前两个模型的结果归为一组，名称叫“A”，后两个结果的模型归为一组，叫“B”。根据[esttab网站](http://repec.org/bocode/e/estout/esttab.html#esttab012)上提供的例子，我们可以先执行以下一段代码：
```stata
esttab using mgroups1.tex, replace                        ///
       star(*0.1 ** 0.05 *** 0.01)                        ///
       mgroups(A B,                                       ///
               pattern(1 0 1 0) span                      ///
               prefix(\multicolumn{@span}{c}{) suffix(})  ///
               erepeat(\cmidrule(lr){@span}))
```
相信大家对 `esttab` 的基本用法已经熟悉了，下面主要解释 `mgroups()`
 选项的含义。`mgroup()` 就是将模型分组的变量。括号中的参数 A B 就是指定两个组别的名称。注意，B 后面紧跟着一个逗号，逗号后面是`mgroups()` 的子选项，用来设定组别名称的性质：

*   `pattern(1 0 1 0)` 这个选项是用来从左到右将各个模型归入每个组别的。1表示该组别的第一个模型，0表示该组别的其他模型。因此，第一个和第二个回归的估计结果会被归入A组，第三个回归估计结果是第二组，也就是B组的第一个模型，第三四个回归估计结果会被归入B组。
*   `span` 是指定组别名能在表格中跨列，不指定则组别名称会保留在该组别第一个模型的顶部的单元格内。
*   `prefix(\multicolumn{@span}{c}{)` 和 `suffix(})` 这一前一后两个选项是用来设定组别名在LaTeX代码中跨行的。前面已经说了，LaTeX表格中跨行的内容设定非常的不友好，手工写代码或者修改都极容易出错，强大的Stata免去了让我们手工数需要跨的行的数目，这就是`@span`的作用。熟悉LaTeX的胖友们应该一眼就能看出这就是LaTeX表格中设置跨行内容的代码。
*   `erepeat(\cmidrule(lr){@span})` 这一次选项一看也能知道是设定跨行代码的。对，它是用来给组别名执行命令后在当前工作目录会生成一个tex文件，打开后如下图所示：下面加底部表格线的。

执行命令后在当前工作目录会生成一个tex文件，打开后如下图所示：
![](http://upload-images.jianshu.io/upload_images/9797551-3e01bb4a6f11c096.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这一段代码不能直接编译，熟悉LaTeX的朋友知道，这个文件可以插入已经加载了必备的package的主文档方可编译。当然，我们也可以在这个tex文件上补充成完整的LaTeX文档代码结构进而编译。可是每次都要补充不麻烦吗？有没有打开就能编译的tex表格？答案当然是Yes！执行以下Stata代码：
```stata
esttab using mgroups2.tex, replace                        ///
       star(* 0.1 ** 0.05 *** 0.01)                       ///
       mgroups(A B,                                       ///
               pattern(1 0 1 0) span                      ///
               prefix(\multicolumn{@span}{c}{) suffix(})  ///
               erepeat(\cmidrule(lr){@span}))             ///
       page booktabs
```
注意到，在底部加了两个选项 `page` 和 `booktabs`。`page` 的作用是在生成的tex文件中添加生成LaTeX文章的代码，而 `booktabs` 的作用是在生成的tex文档中添加加载booktabs宏包的代码。生成的tex文件如下图所示：
![](http://upload-images.jianshu.io/upload_images/9797551-391ceb20845d46dc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意比较该tex文件和上一个tex文件在头部和底部的差异。这个tex文件是可以直接编译的，生成的PDF表格如下图，已经几乎达到我们的要求了。
![](http://upload-images.jianshu.io/upload_images/9797551-d37771dc44f999c9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

需要说明的是，也可以使用选项page(booktabs)产生可以直接编译的tex文件，但是生成的PDF表格的顶部和底部均为双线，这既浪费纸张油墨，也不像Journal of Finance里表格的高大(bi)上(ge)，还不是男神连老师清新大气的审美风格，所以本人不推荐这个方案。

已经完了吗？既然我这么说那就肯定还没完。细心的胖友们肯定发现了，系数里的负号似乎不够长啊？是的，这个短短的横线不是负号(minus) 而是连字符 (hyphen)。之前用 `excel2latex` 插件导出 Excel 表格时，连字符不会自动改成负号，而我的变量里又有一个 Market-to-book ratio。因而每次选中表格替换 hyphen 都要将这个变成负号的连字符再改正过来，心中就是一万头草泥马飞奔。温柔体贴的Stata其实早就想到了这一点，不信我们执行以下代码：
```stata
#delimit ;
 esttab using mgroups3.tex, replace
        star(* 0.1 ** 0.05 *** 0.01)
        compress nogaps
        title(\textbf{An Illustration of mgroup() in esttab})
        mgroups(A B,
                pattern(1 0 1 0) span
                prefix(\multicolumn{@span}{c}{) suffix(})
                erepeat(\cmidrule(lr){@span}))
                booktabs page(dcolumn) alignment(D{.}{.}{-1})
;
#delimit cr
```
这段代码中，我们加入了 `page(dcolumn)` 和 `alignment(D{.}{.}{-1})` 两个选项。
*   `page(dcolumn)` 的作用是在生成的tex文件中添加完整LaTeX文章文档代码的同时，添加加载宏包dcolumn的代码。
*   `alignment(D{.}{.}{-1})` 的作用是调整单元格对齐方式的。(具体是啥意思其实我也不懂啊啊啊！至少有一个功能是小数点对齐。)

生成的tex文件如下图所示：
![](http://upload-images.jianshu.io/upload_images/9797551-6904a9d97a3a1b83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

编译之后呢？![](http://upload-images.jianshu.io/upload_images/9797551-d40b27e12b7868f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

惊喜地发现，所有hyphen都变成minus了！几乎产生了完美的分组回归表格的LaTeX代码！妈妈再也不用担心我的分组回归表格了！

其实 `esttab` 还有好多灵活用于生成 LaTeX 表格代码的选项，比如还有`longtable` 的选项，期待胖友们一起来发掘更多更方便更强大的功能吧！

* * *

样例代码
```stata
*  = Example =
*
*  - If you did not install external commands below, please install them first.
*    Otherwise, the code would not be executed properly.
*  - For compiling LaTeX files, you should have installed necessary LaTeX systems
*    e.g. MikTeX on Windows
*
*      ssc install esttab
*	   ssc install estout
*      ssc install texify
	 
	 cap mkdir "D:\_Temp_\"
	 cd "D:\_Temp_\"
	 
     cls
     sysuse auto

     eststo clear
     eststo: quietly reg weight mpg
     eststo: quietly reg weight mpg foreign
     eststo: quietly reg price weight mpg
     eststo: quietly reg price weight mpg foreign
     	
	 *- Basic code -
     esttab using mgroups1.tex, replace							///
	        star(* 0.1 ** 0.05 *** 0.01)						///
            mgroups(A B, 										///
			        pattern(1 0 1 0) span						///
	 				prefix(\multicolumn{@span}{c}{) suffix(})	///
	 				erepeat(\cmidrule(lr){@span}))
	 
    *- Generate full LaTeX code -	 
    esttab using mgroups2.tex, replace							///
	       star(* 0.1 ** 0.05 *** 0.01)							///
           mgroups(A B, pattern(1 0 1 0)						///
				   prefix(\multicolumn{@span}{c}{) suffix(})	///
	 			   span erepeat(\cmidrule(lr){@span}))			///
		   page booktabs /* Strongly suggest */
	
	texify mgroups2.tex
	
	
	*- Beautify the LaTeX table -
	#delimit ;
     esttab using mgroups3.tex, replace							
			star(* 0.1 ** 0.05 *** 0.01)						
			compress nogaps										
			title(\textbf{An Illustration of mgroup() in esttab})
            mgroups("Group A" "Group B",
			        pattern(1 0 1 0) span               		
                    prefix(\multicolumn{@span}{c}{) suffix(})	
                    erepeat(\cmidrule(lr){@span}))
			booktabs page(dcolumn) alignment(D{.}{.}{-1})			
	 ;
	#delimit cr
	
	texify mgroups3.tex	  
```




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


> [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)
>
>![连玉君Stata现场班报名中](https://upload-images.jianshu.io/upload_images/7692714-78fa5fece25aa2fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-5c36ef6877312e7f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
