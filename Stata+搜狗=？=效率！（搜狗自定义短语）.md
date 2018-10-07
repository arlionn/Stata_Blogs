> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



[原始 Markdown 文档下载](https://gitee.com/arlionn/jianshu/blob/master/Stata%20%E6%90%9C%E7%8B%97=%EF%BC%9F=%E6%95%88%E7%8E%87%EF%BC%81%EF%BC%88%E6%90%9C%E7%8B%97%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9F%AD%E8%AF%AD%E4%B9%8BStata%E5%BA%94%E7%94%A8%EF%BC%89.md)  | [码云-原始短语定义文件](https://gitee.com/arlionn/sougou/tree/master)(持续更新中……)

![Stata连享会：搜狗+Stata = 效率](http://upload-images.jianshu.io/upload_images/7692714-621cf6f4b9915079.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




> ## 1. Stata 输入：蜗牛变猎豹

8 月中旬参加完首届 stata 用户大会后，测试了多种 Stata 编辑器，包括 `sublime text 3 (ST3)`，`atom`, `VScode` 等等。最终发现我的需求似乎还无需用这些高大上的东西，因为 **MC>>MR**。

我的需求很简单：**把重复的工作系统化、自动化**。
我的思路很简单：**优化组合现有工具 = 新工具**

比如，我做 Stata 讲义时，经常要输入 `sysuse "auto.dta", clear`，或者 `sysuse "nlsw88.dta", clear`，每次都输入这么长一串，很烦。又如，每次做论文中的几张基本表格（表1：基本统计量；表2：相关系数矩阵；表3：回归结果），都要找出以前的代码，复制粘贴过来。若是自己重新写，怎么着也要折腾个 5-10 分钟吧。

若是有个快捷命令就好了。于是，我用搜狗输入法自带的**【搜狗自定义短语配置】**功能，把这些需要经常用的命令、代码都做成短语，只要输入几个字母就出现一大串代码。

然后，我的打字效率就从**蜗牛**变**猎豹**了：

![Stata+搜狗自定义短语-范例：一分钟做好三张表](http://upload-images.jianshu.io/upload_images/7692714-b3c68e826ffa3025.gif?imageMogr2/auto-orient/strip)

![Stata+搜狗自定义短语-范例2：如此快乐滴写Stata程序](http://upload-images.jianshu.io/upload_images/7692714-6ad8ae9681eb382c.gif?imageMogr2/auto-orient/strip)

## 2. 如何定义【搜狗自定义短语】

你可以在简书上搜索关键词【搜狗自定义短语】，找到很多相关的教程，例如：[输入法“自定义短语设置”还可以这么玩](http://www.jianshu.com/p/122df638c620)；[搜狗输入法-自定义短语设置的神奇妙用](http://www.jianshu.com/p/ebc1d5e65628)。这里，我只做简要说明。

![第一步：点击[设置]，快捷键 Ctrl+Shift+M](http://upload-images.jianshu.io/upload_images/7692714-b67e379ff1f9ac61.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![第二步：设置属性](http://upload-images.jianshu.io/upload_images/7692714-4bf4f93917c1c989.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![第三步：依次点击【高级设置】-->【自定义短语设置】-->【直接编辑配置文件】](http://upload-images.jianshu.io/upload_images/7692714-d3ae9b6386d5d431.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![第四步：在弹出的文本文档底部添加自定义短语设置，保存即可](http://upload-images.jianshu.io/upload_images/7692714-75abe6b3755df49e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![第五步：按图依次点击“应用”--> “确定” 即可开始愉快的输入了](http://upload-images.jianshu.io/upload_images/7692714-baeefc6480aa51a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>## 3. 我的配置文件
下面贴出我的部分配置文件，你只需贴入你的配置文件中，根据自己的习惯修改关键词或缩写设定即可。

### 3.1 Stata 常用命令之搜狗短语设置

```
st,1= Stata  
lc,1=local ""
gl,1=global ""
ests,1=est store m
sh,1=shellout "$R\.pdf"
vb,1=view browse ""
trace,1=set trace on
trace,2=set trace off

sysuse,1=sysuse "auto.dta", clear
sysuse,2=sysuse "nlsw88.dta", clear
use, 1=use ".dta", clear

pre,1=
preserve

restore

log,1=
cap log close
log using logname, text replace

log close

deli,1=
#delimit ;

#delimit cr
```

### 3.2 基本统计和回归结果相关
``` 
fsum,1=
  local v " " //连续变量
  local c " " //类别变量
  local s "$Out\Table1_sum" //文件名(或路径\文件名)
  logout, save("`s'") excel replace: ///
          fsum `v', s(mean sd p50 min max) cat(`c') label 

tabstat,1=
*-----表1：基本统计量-------
  local v " " //填入变量名
  local s "$Out\Table1_sum" //存储的文件名(或路径\文件名)
  logout, save("`s'") excel replace: ///
          tabstat `v', stat(mean sd p50 min max) f(%6.2f) c(s) 

pwcorr,1=
*-----表2：相关系数矩阵-------
  local v " " //填入变量名
  local s "$Out\Table2_corr" //存储的文件名(或路径\文件名)
  logout, save("`s'") excel replace: ///
          pwcorr_a `v', format(%6.2f) //star(0.05)

esttab,1=
*-----表3：回归结果-------
  local s "using $Out\Table3_reg.csv"  //执行时包括这一行会输出Excel表格
  local m "m1 m2 m3"
  esttab `m' `s', nogap compress replace   ///
         b(%6.3f) s(N r2_a) drop(`drop')   ///
         star(* 0.1 ** 0.05 *** 0.01)      ///
  		 addnotes("*** 1% ** 5% * 10%") 

esttab,2=
  *----------------------------------------------------begin--------
    local s "using $Out\Table3_reg.csv" 
    local m "m1 m2 m3"
    local drop ""
    #d ;
    esttab `m' `s', compress nogap replace
      b(%6.3f) t(%6.2f) star(* 0.1 ** 0.05 *** 0.01) 
      stats(Cluster N r2_a, fmt(%3s %12.0f  %9.3f)) varwidth(20) 
      drop(`drop') 
      title("Table1 Determinants of Women's Wage") 
      mtitle("OLS" "OLS" "OLS with Occupation dummies") ;
    #d cr
  *----------------------------------------------------over---------
  
twoway,1= 
    *------------------------------------------------------------Begin
    local gname "$Out\Fig01.wmf" //图形名称和存储位置
    #delimit ;
      twoway ( )
             ( )
	  , 
      ylabel(, angle(0) grid)
      legend(ring(0) position(3))
      note("数据来源: 雅虎财经！")
	  ;
    #delimit cr
    graph export "`gname'", replace
   *------------------------------------------------------------Over
```

### 3.3 Stata 程序相关：循环语句和条件语句等
```
prog,1=
capture program drop 
program define 
  version 13.0
  
end  

if,1=
if {
    
}
else{
    
}

for,1=
forvalues i=1/`N'{
    
}
```

### 3.4 Stata 讲义短语

```
title,1=
*------------------
*- 
*------------------

title,2=
*===================
*- 
*===================

dotitle,1=
*-------------------------------------
*-日期：
*-目的：
*-方法：
*-作者：连玉君，中山大学岭南学院金融系
*-------------------------------------

statalxh,1=
*------------ Stata连享会 ---------------	
*-

fanli,1=Stata 范例：

path,1=
*-注意：执行后续命令之前，请先执行如下三条命令
  global path "`c(sysdir_personal)'\PX_A_2017b\A1_intro"  //定义课程目录
  global D    "$path\data"      //范例数据
  global R    "$path\refs"      //参考文献
  global Out  "$path\out"       //结果：图形和表格
  adopath +   "$path\adofiles"  //自编程序 
  cd "$D"
  set scheme s2color  
  *-note: 
  *   `c(sysdir_personal)' 等价于 D:\stata15\ado\personal

scom,1=
  *-Comments:
  * 1 
  * 2 

begin,1=
*-----------------------------------------Begin-----------

*-----------------------------------------Over------------
*-Notes: 
* 1. 
* 2. 
```

### 3.5 Markdown 相关短语和关键词

```
md,1=Markdown 
toc,1=[toc]
yy,1=``
lianjie,2=- []()
lianj,1=[]()
btsan,1=###
bter,1=##
tupian,1=![]()
zuozhe,1=> 作者：连玉君，中山大学岭南学院金融系
zuozhe,2=
> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [github](http://github.com/StataChina))

lianxh,1=
---
![Stata连享会二维码](http://wx1.sinaimg.cn/mw690/8abf9554gy1fj9p14l9lkj20m30d50u3.jpg "扫码关注 Stata 连享会")

riqi,1=#$year.$month.$day
riqi,2=#$year年$month月$day日星期$weekday
```


### 致谢
本文受 [《永澄：凡是出现两次的事情就要考虑系统化、自动化》](http://mp.weixin.qq.com/s/LT0loggUUY2Ho74VrE2UCA) 的启发。

### 欢迎补充
可以直接在本文底部添加评论，亦可发送邮件至 StataChian@163.com

---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-68f66284ca33cd18.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")



>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
