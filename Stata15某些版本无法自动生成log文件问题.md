> 有些人最近使用了一些特殊版本的 Stata 15 软件，为了防止过期，使用了一个插件来修改电脑时间，导致 Stata 15 下的系统日期一直保持在 2017 年 6 月 30 日，Stata 15 下的系统时钟也因此发生的改变。


> 这似乎也不是什么大事，但如果你的 `profile.do` 文件是用如下语句来自动建立每次启动 Stata 时的 log 文件，那么上述系统时间的变化就会导致你无法正常生成 log 文件。即使能偶尔生成一些 log 文件，也会因为日期错乱而导致你日后查找 log 文件时非常费劲。

```stata
* log文件：自动以当前日期为名存放于 stata15\do 文件夹下
* 若 stata15\ 下没有 do 文件夹，则本程序会自动建立一个 
cap cd `c(sysdir_stata)'do
if _rc{
   mkdir `c(sysdir_stata)'do  //检测后发现无 do 文件夹，则自行建立一个
}

local fn   = subinstr("`c(current_time)'",":","-",2)
local fn1 = subinstr("`c(current_date)'"," ","",3)
log    using `c(sysdir_stata)'do\log-`fn1'-`fn'.log, text replace
cmdlog using `c(sysdir_stata)'do\cmd-`fn1'-`fn'.log, replace
```
> 解决思路：
**基本想法：** 由于系统时间已经被篡改，所以不要在试图使用 `c(current_time)` 和 `c(current_date)` 来读取时间和日期了。然而，我们可以从网上读取标准时间。只需要在每次开机时，从网上（如， http://time.tianqi.com）读取开机时的北京时间，然后用这个时间作为 log 文件的标题即可。

> 解决方法：将如下代码贴入你的 **profile.do** 文件即可。ps，如果你的 profile.do 文件中已经有了上面的那些定义语句，可以用 `*`，`//` 或 `\**\` 将其注释掉，或者直接删除。

```stata
tempfile bjtime
copy http://time.tianqi.com/ `bjtime'.txt, replace
infix strL v 1-1000 using "`bjtime'.txt", clear
keep if strmatch(v, "*<meta content=*") 
gen v2=ustrregexs(1) if ustrregexm(v,"([0-9][0-9][0-9][0-9]\-[0-9][0-9]\-[0-9][0-9] [0-9][0-9]\:[0-9][0-9]\:[0-9][0-9])")
split v2, parse(" ")
local fn = subinstr(v22,":","_",2)
local fn1= v21[1]
log    using `c(sysdir_stata)'do\log-`fn1'-`fn'.log, text replace
cmdlog using `c(sysdir_stata)'do\cmd-`fn1'-`fn'.log, replace
clear
```

>#### 关于我们
- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>#### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com


---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-ce6213576c3143e7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

