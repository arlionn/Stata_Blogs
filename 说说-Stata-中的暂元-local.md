>作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
>&emsp;&emsp;[ …… Stata连享会 - 往期精彩推文 ……](https://www.jianshu.com/p/de82fdc2c18a)
  
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

[toc]

#### 临时性存储单元

我们在厨房里做饭时，虽然只做 3 盘菜，但通常会用掉 5-10 个碗碟，用于打鸡蛋、拌肉、放葱花之类的。在写程序或做实证分析过程中，这些最终只留在厨房中而不会上餐桌的碗碗碟碟叫`“临时性存储单元”`。Stata 中有多种临时性存储单元：`暂时性变量`、`暂时性文件`、`暂时性矩阵`、`局部暂元(local)` 以及`全局暂元 (global)`, 等等。

当程序运行完后，这些暂时性存储单元里面的内容会自动消失，也就不会对用户的数据产生任何干扰和影响。

#### 暂元 (local)
下面说说伟大又神奇的 **local**。      
就我所知，**暂元** 这个中文名字是我的导师**钟经樊**先生给出的。
我很喜欢这个名字，它比诸如`局部xx`、`局域xx`之类的名字好多了。**暂元**这个称呼深刻地道出了 Stata 中 **local** 的所有特征。

##### 定义和引用方法
```stata 
local a = 4
local b = `a' + 5
display `b'
```
这里，第一句使用 `local a ...` 方式定义了一个暂元 `a`，为其赋值为 4。
第二句，我们使用 `` ` ' `` 符号引用了暂元 a 中的内容。需要特别注意的是引用符号的录入问题：`` `a' `` 左侧的单引号是键盘上 **Tab** 键上方的那个键；`` `a' `` 右侧的单引号是回车键 **Enter** 左侧的那个键。初学者往往忽略这个微小的差异，导致 Stata 反复提示 `syntax invalid` 这样的错误提示信息。

暂元可以视为**小盒子**，因此，可以并排放置(像**俄罗斯方块**)，也可以嵌套放置(像**俄罗斯套娃**)。
```stata
local s1  "中山大学"
local s2  "岭南学院"
local s3  `s1'`s2' 
display "`s3'"
```

#### 暂元的能活多久？
汉语中描述时间长短的词有很多：一瞬间、一刹那、弹指间、一须叟……。那么**暂元**能存活多久呢？答案是：可以很短，也可以很长！她的生命期决定于她所在的`环境`。
- 在 dofile 中定义的暂元，其存活期仅在于一次命令的执行；
- 在命令窗口中定义的暂元，只要不关闭 Stata，它一直都是活着的。

##### 环境的概念
与执行方法有关系：
```stata 
local a = 4
local b = `a' + 5
display `b'
```
截图呈现 .temp 的概念

```stata 
local a = 4
local b = `a' + 5
display `b'
```
#### Stata 命令中内置的暂元

```stata
sysuse "auto.dta", clear
forvalues i=1/5{
    display price[`i']
}
```

#### 如何使用暂元？

##### 灵活执行（充分利用环境的概念）

##### 返回值中的暂元
```stata
sysuse "auto.dta", clear
summarize price
return list
gen pricegap = price-r(min)
```
##### 系统内置的暂元
```stata
help creturn list
```





##### 暂元扩展功能
```stata
help extended_fcn
```


