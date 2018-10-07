> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn) )

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


#### 1. dofile 转码问题 

**问题：** 用 Stata15 打开 Stata14 以下的 dofile 时，若你的 dofile 中包含非英文字符（直接打开的话，中文会显示为乱码），屏幕会提示 

```
The document is not encoded in UTF-8! 
```

**处理方法：** 在 **Encoding:** 下拉菜单中选择 「Chinese(GBK)」，点击 `OK` 即可    
- **注意 ！**：不要勾选「&loz; `Dot not show this message again`」



#### 2. 一次性处理 .dta 转码问题)

若文件较多，也可以使用命令一次性完成转码。这里以 **.dta** 数据文件为例进行说明。
                      
*  Step 1: 分析当前工作路径下的编码情况                         
```stata
unicode analyze .dta   
```
* Step 2: 设定转码类型                                          
```stata
unicode encoding set gb18030  // 设定中文编码方案
```
* Step 3: 转换文件     
```stata
unicode translate *.dta   
```

### 附：一次性转换当前工作路径下的所有文件
```stata
* Stata 15 中文乱码转码方法
*-Step 0：进入需要转码的文件路径
  cd "D:\stata15\ado\personal\mypaper"  //填入你的文件路径
*-Step 1: 分析当前工作路径下的编码情况   help unicode translate 
  unicode analyze *   // 分析当前工作路径下的编码情况    
  *-Note: * 是通配符，表示当前路径下的所有文件               
*-Step 2: 设定转码类型                    
  unicode encoding set gb18030  // 中文编码
*-Step 3: 转换文件                         
  unicode translate *                  
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

> [Stata 现场培训报名中……](https://www.jianshu.com/p/af6fb0448297)


---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-460104de9c274bb0.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")


                                         
       
