### Stata 默认图形


### 方法1：设定 graphregion() 选项
```stata
sysuse auto, clear
set scheme s2color // 只有彩色模板下述方式才奏效
twoway (scatter price mpg) (lfit price mpg),  ///
       graphregion(fcolor(white))
```



### 方法2：更换模板

> Source: [stata作图怎样去除边框](http://bbs.pinggu.org/thread-3941744-1-1.html)
