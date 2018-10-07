> 导言：

## Stata 13 及以前
比较简单。只是因为中文粗体字符无法正常显示而已。
只需依次点击主界面中的 Edit &rarr; Preferences &rarr; General Preferences


![Paste_Image.png](http://upload-images.jianshu.io/upload_images/7692714-3d313332428fede0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## Stata 14 

第一次用Stata14并导入之前版本的do文档的时候会发现，原来do文档里面的中文都变成了乱码（其实Stata的其他版本打开Stata14的do文档也会出现中文乱码的问题，本人亲测），原因是Stata14采用了unicode（具体是什么我也不太清楚，感觉是一种语言的编码，使得Stata14可以识别不同的语言，可以自行help unicode），和之前版本的Stata不同。具体的解决方法如下：

第一步：把需要转换的其他版本的do文档放到Stata14的工作路径中，或者用cd命令把Stata14的工作路径设定到do文档所在的目录中；
第二步：输入命令：
unicode encoding set gb18030
  . unicode analyze *
  . unicode translate * 


完成以上这两步后Stata14就会把工作路径下的所有文件（包括do文档，dta数据储存文档等）转换为可以识别的中文。简单的说就是do文档的中文可以正常阅读，也可以在dta文档中的变量名也可以是中文了。

简单解释一下上面三条命令的意思：
第一条的意思是把unicode编码设定为简体中文，其中gb18030就是简体中文的代码；

第二条的意思是将Stata14工作路径下的所有文件进行分析，看有多少是需要转换的，之前已经转过过的文件会被识别为已经转换过，Stata14不会进行二次转换。命令中*号的意思是所有文件，如果你只需要转换特定的文件，如do文档，只需要吧do文档的名字替代表*号就可以了；

第三条就是进行转换，*号的意思和第二条相同。

转换后就可以像之前版本那样进行阅读中文啦

来源：[经管之家论坛](http://bbs.pinggu.org/thread-4169938-1-1.html)

参见：
- [Chinese support in Stata 14](http://jackson-woods.net/?p=59)
- [Stata 14 PDF 手册对应说明](https://www.stata.com/manuals14/dunicodetranslate.pdf)
## Stata 15 及以后
