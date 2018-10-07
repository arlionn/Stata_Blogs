
> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


**问题：** 如何绘制如下图形 ？
**核心障碍：** 横坐标中的 `t-3`, `t-2` 等如何标注？
![](https://upload-images.jianshu.io/upload_images/7692714-a204a5e863f59f2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Stata 范例：
- **关键语句：** 使用 `xlabel(# "标注文字")` 选项来修改横轴刻度标签的显示。其中，`#` 表示横轴变量 (x) 的真实刻度值，绘图时的定位依赖于该值；而 `"标注文字"` 中则是最终在图形中显示出来的刻度标记。
```stata

*-虚构一份数据
clear
input  invt t0
        116 2001
        118 2002
        106 2003
        118 2004
         80 2005
         79 2006 
	     63 2007
end

*-重新定义时间变量
gen t1 = t0-2004

*-设定为时间序列数据
tsset t1 

*-简单图形
twoway connect invt t1, ylabel(0(20)140) xlabel(-3(1)3)

*-完整图形
#d ;
twoway connect invt t1, ylabel(0(20)140) ///
       xlabel(-3.5 " " -3 "t-3" -2 "t-2" -1 "t-1" 
	             0 "t"  1 "t+1"  2 "t+2"  3 "t+3" 3.5 " ") 
	   ytitle("Investment")
	   xtitle("Time of Event")
	   ;
#d cr

*-保存图片
graph export "Fig01.png", replace
```
#### 输出图片效果：
>![Stata绘图：使用 xlabel(# "标注文字") 选项修改坐标刻度标签](https://upload-images.jianshu.io/upload_images/7692714-0d9280bc5a747200.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-49ac3fbb26dc3132..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
