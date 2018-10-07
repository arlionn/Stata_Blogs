> 作者：连玉君 ( [知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) | [github](http://github.com/StataChina) )

---
> Stata现场研讨班即将开班（2018年1月中旬，北京）   
[Stata现场班-初级](http://www.peixun.net/view/307.html)｜[Stata现场班-高级](http://www.peixun.net/view/308.html)


-----
#### 1. bytwoway命令介绍

`bytwoway` 是一个在实现快速分组绘图上相当便捷的命令，可以适用于对数据进行任意分组的情形，不同组别的颜色和类型可以通过选择项`aesthetics`设置。

#### 2. bytwoway命令安装


```stata
net install bytwoway, from(https://github.com/matthieugomez/stata-bytwoway/raw/master)
```
#### 3. bytwoway命令举例
  > 范例1：


```stata
sysuse nlsw88.dta, clear
collapse (mean) wage, by(grade race)
bytwoway (line wage grade), by(race) aes(color lpattern)
```
> 根据分组变量race绘图
![](http://upload-images.jianshu.io/upload_images/8580278-d952311341dea00d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```stata
bytwoway (scatter wage grade, connect(l)), by(race) aes(color msymbol)
```

> 根据分组变量race绘图，改变了分组线条的类型
![](http://upload-images.jianshu.io/upload_images/8580278-1b9a7e57ae9d0223.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```stata
bytwoway line wage grade, bysmsa race) aes(color) palette(GnBu)
```
> 根据分组变量smsa、race绘图，改变了分组线条的颜色深浅
![](http://upload-images.jianshu.io/upload_images/8580278-c6962c107e620bc9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


```stata
bytwoway line wage grade, by(race) aes(color) colors("248 118 109" "0 186 56"  "97 156 255")

```
> 根据分组变量race绘图，改变了分组线条的颜色类型
![](http://upload-images.jianshu.io/upload_images/8580278-00e49895f61e9d65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **说明：**  
  `palette`等用于改变分组线条的颜色的选项需要安装 `colorscheme`命令安装包才可以使用。安装命令如下：

 ```stata
  net install colorscheme, from(https://github.com/matthieugomez/stata-colorscheme/raw/master/)

```
  > 范例2：下面进行多变量分组绘图
 
  ```stata
sysuse nlsw88.dta, clear
collapse (mean) wage, by(grade smsa race)
bytwoway line wage grade, by(smsa race)
```
> 根据分组变量smsa、race绘图
![](http://upload-images.jianshu.io/upload_images/8580278-13d15e028247cdca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 
> Stata现场研讨班即将开班（2018年1月中旬，北京）   
[Stata现场班-初级](http://www.peixun.net/view/307.html)｜[Stata现场班-高级](http://www.peixun.net/view/308.html)  

 


![Stata连享会二维码](http://wx1.sinaimg.cn/mw690/8abf9554gy1fj9p14l9lkj20m30d50u3.jpg "扫码关注 Stata 连享会")
