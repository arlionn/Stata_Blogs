> [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)


知乎上截取的：

![Stata-心形-生日快乐！](https://upload-images.jianshu.io/upload_images/7692714-65266a077f374ff5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```stata
drop _all
range t 0 2*_pi 1000
gen x=16*sin(t)^3
gen y=13*cos(t)-5*cos(2*t)-2*cos(3*t)-cos(4*t)
egen x_min=min(x)
egen x_max=max(x)
egen y_min=min(y)
egen y_max=max(y)
gen a=(x-x_min)/(x_max-x_min)
gen b=(y-y_min)/(y_max-y_min)
line b a

gr_edit yaxis1.draw_view.setstyle, style(no)
gr_edit xaxis1.draw_view.setstyle, style(no)
gr_edit plotregion1.AddTextBox added_text editor .605 .33
gr_edit plotregion1.added_text_new = 1
gr_edit plotregion1.added_text_rec = 1
gr_edit plotregion1.added_text[1].style.editstyle angle(default) size(medsmall) color(red) horizontal(left) vertical(middle) margin(zero) linegap(zero) drawbox(no) boxmargin(zero) fillcolor(bluishgray) linestyle( width(thin) color(black) pattern(solid)) box_alignment(east) editcopy
gr_edit plotregion1.added_text[1].style.editstyle size(large) editcopy
gr_edit plotregion1.added_text[1].text = {}
gr_edit plotregion1.added_text[1].text.Arrpush 亲爱的，生日快乐！
```

## 美化一下：其实是有动画效果的——我的心在颤抖！
```stata
drop _all
range t 0 2*_pi 1000
gen x=16*sin(t)^3
gen y=13*cos(t)-5*cos(2*t)-2*cos(3*t)-cos(4*t)
egen x_min=min(x)
egen x_max=max(x)
egen y_min=min(y)
egen y_max=max(y)
gen a=(x-x_min)/(x_max-x_min)
gen b=(y-y_min)/(y_max-y_min)
line b a, lc(red*1.2) lw(*20)

gr_edit yaxis1.draw_view.setstyle, style(no)
gr_edit xaxis1.draw_view.setstyle, style(no)
gr_edit plotregion1.AddTextBox added_text editor .605 .33
gr_edit plotregion1.added_text_new = 1
gr_edit plotregion1.added_text_rec = 1
gr_edit plotregion1.added_text[1].style.editstyle angle(default) size(medsmall) color(red) horizontal(left) vertical(middle) margin(zero) linegap(zero) drawbox(no) boxmargin(zero) fillcolor(bluishgray) linestyle( width(thin) color(black) pattern(solid)) box_alignment(east) editcopy
gr_edit plotregion1.added_text[1].style.editstyle size(large) editcopy
gr_edit plotregion1.added_text[1].text = {}
gr_edit plotregion1.added_text[1].text.Arrpush 亲爱的，生日快乐！
```
![My_heart-Stata.png](https://upload-images.jianshu.io/upload_images/7692714-94b827431c47daee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




作者：章维龙
链接：https://www.zhihu.com/question/25366124/answer/42388196
来源：知乎

>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

> [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)
>
>![连玉君Stata现场班报名中](https://upload-images.jianshu.io/upload_images/7692714-78fa5fece25aa2fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-aade9edbd8c86a75.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
