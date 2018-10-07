>作者：Stata连享会 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))    
>&emsp;&emsp;
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
  
> **这让我很费解**——每次亮出这幅图形，问学生：Z 变量是什么？他们的第一反应总是【小三】！我想，难道不应该是【女儿】吗？\^\-^……这个让我抓狂的世道！
![为什么第一个被想到的调节变量总是【小三】，而不是 【女儿】？ ](https://upload-images.jianshu.io/upload_images/7692714-e9397899f714ca87.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> **导言：** 在现有文献的实证分析中，交乘项的使用非常的普遍。然而，对于**在什么情况下使用交乘项？** **如何正确使用交乘项？如何正确的解释交乘项的经济含义？** 以及 **如何采用图形的方式更为直观地展示估计系数？** 诸如此类的问题都没有得到足够的重视，以至于在大量的文献中，对交乘项的使用错漏百出。

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


### 相关推文
- [Stata：因子变量的全攻略](http://www.jianshu.com/p/16b08797e591)
- [Stata：边际效应分析](http://www.jianshu.com/p/012d8a6159cf)

### 重要参考资料：
- **温馨提示：** 微信用户，点击推文底部【阅读原文】查看下载链接
如下几篇文章，对交乘项的使用进行了非常详细的梳理和说明，尤其是涉及到了诸多实操方面的要点。文中涉及到的各种估计和处理方法，以及图形展示等内容，都可以在 Stata 中找到相应的命令，快捷地加以实现。
- Brambor T, Clark W R, Golder M. Understanding Interaction Models: Improving Empirical Analyses[J]. *Political Analysis*, 2006, 14(1):63-82. [-
 PDF -](https://gitee.com/arlionn/jianshu/raw/master/PDF%E4%B8%8B%E8%BD%BD/Brambor_2006.pdf) || [Replication data and Programs - Stata](https://dataverse.harvard.edu/dataset.xhtml?persistentId=hdl:1902.1/10483)
- Hainmueller J, Mummolo J, Xu Y. How Much Should We Trust Estimates from Multiplicative Interaction Models? Simple Tools to Improve Empirical Practice[J]. *SSRN working paper*, 2016.  [- PDF 1-](https://scholar.princeton.edu/sites/default/files/jmummolo/files/interaction_02_12_2017.pdf) || [- PDF 2 -](https://gitee.com/arlionn/jianshu/raw/master/PDF%E4%B8%8B%E8%BD%BD/Hainmueller_2016_Interaction.pdf)
- 黄河泉，2017，When Interaction Actions?，淡江大学财务金融系. [- PDF -](https://gitee.com/arlionn/jianshu/raw/master/PDF%E4%B8%8B%E8%BD%BD/Huang2007-interaction.pdf)

### 黄河泉老师的 PPT
> 经黄河泉老师授权，在这里展示他的 PPT，非常细致的梳理了交乘项使用中的各种陷阱和应对方法。特此致谢！

![1.jpg](https://upload-images.jianshu.io/upload_images/7692714-cb9237951abed6db.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![2.jpg](https://upload-images.jianshu.io/upload_images/7692714-8976773f58edee11.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![3.jpg](https://upload-images.jianshu.io/upload_images/7692714-6cc9b6f3c64873cf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![4.jpg](https://upload-images.jianshu.io/upload_images/7692714-8a8dda3d931e55bc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![5.jpg](https://upload-images.jianshu.io/upload_images/7692714-83ba893f232e75ec.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![6.jpg](https://upload-images.jianshu.io/upload_images/7692714-b50589d7b510fb9b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![7.jpg](https://upload-images.jianshu.io/upload_images/7692714-1a3309d8e5ed94ce.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![8.jpg](https://upload-images.jianshu.io/upload_images/7692714-d15f0e9cc3057503.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![9.jpg](https://upload-images.jianshu.io/upload_images/7692714-91b8dddbe9ccb64b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![10.jpg](https://upload-images.jianshu.io/upload_images/7692714-c888bc5a2738dd7e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![11.jpg](https://upload-images.jianshu.io/upload_images/7692714-bcf2688047f9514d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![12.jpg](https://upload-images.jianshu.io/upload_images/7692714-fc65bfc46c87a011.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![13.jpg](https://upload-images.jianshu.io/upload_images/7692714-740b0c51462e30bf.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![14.jpg](https://upload-images.jianshu.io/upload_images/7692714-8a5be4a491b0fb43.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![15.jpg](https://upload-images.jianshu.io/upload_images/7692714-6668fa51c91895d8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![16.jpg](https://upload-images.jianshu.io/upload_images/7692714-c1719b89ffcb394d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![17.jpg](https://upload-images.jianshu.io/upload_images/7692714-a2f227e41e76de3d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![18.jpg](https://upload-images.jianshu.io/upload_images/7692714-4ff9d405a9baa6c8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![19.jpg](https://upload-images.jianshu.io/upload_images/7692714-bd67bc32a3ee759e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![20.jpg](https://upload-images.jianshu.io/upload_images/7692714-725ccc62f1023d1d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![21.jpg](https://upload-images.jianshu.io/upload_images/7692714-050981527ce78d89.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![22.jpg](https://upload-images.jianshu.io/upload_images/7692714-8f77b9a254a19283.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![23.jpg](https://upload-images.jianshu.io/upload_images/7692714-f3a89cca02054036.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![24.jpg](https://upload-images.jianshu.io/upload_images/7692714-efece633473be6e6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![25.jpg](https://upload-images.jianshu.io/upload_images/7692714-8558f2290dc77ed2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![26.jpg](https://upload-images.jianshu.io/upload_images/7692714-43b8a7d76232336b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![27.jpg](https://upload-images.jianshu.io/upload_images/7692714-746450365201bfc3.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![28.jpg](https://upload-images.jianshu.io/upload_images/7692714-8a053707445a6b9c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![29.jpg](https://upload-images.jianshu.io/upload_images/7692714-faa0149cfa3688a9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![30.jpg](https://upload-images.jianshu.io/upload_images/7692714-3616e86b577c5bc1.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> [……Stata 连享会精彩推文……](https://blog.csdn.net/arlionn/article/details/82746992)



![31.jpg](https://upload-images.jianshu.io/upload_images/7692714-2326c15ee434cb4c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![32.jpg](https://upload-images.jianshu.io/upload_images/7692714-60042f09905bede9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![33.jpg](https://upload-images.jianshu.io/upload_images/7692714-57b956b75bf2ef6c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![34.jpg](https://upload-images.jianshu.io/upload_images/7692714-3a347275edfd0665.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![35.jpg](https://upload-images.jianshu.io/upload_images/7692714-7367c6c58405a0be.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![36.jpg](https://upload-images.jianshu.io/upload_images/7692714-17a86719b2cc9bec.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![37.jpg](https://upload-images.jianshu.io/upload_images/7692714-0a604bf2b904410e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![38.jpg](https://upload-images.jianshu.io/upload_images/7692714-ca9d62ac896b27db.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![39.jpg](https://upload-images.jianshu.io/upload_images/7692714-f3fcce06f5a430ad.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![40.jpg](https://upload-images.jianshu.io/upload_images/7692714-dfe8e90413742f6d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![41.jpg](https://upload-images.jianshu.io/upload_images/7692714-f44cee8c314f967d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![42.jpg](https://upload-images.jianshu.io/upload_images/7692714-6300626d286e244f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![43.jpg](https://upload-images.jianshu.io/upload_images/7692714-a7dfb1d7d88f37e8.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> [……Stata 连享会精彩推文……](https://blog.csdn.net/arlionn/article/details/82746992)

### 相关推文
- [Stata：因子变量的全攻略](http://www.jianshu.com/p/16b08797e591)
- [Stata：边际效应分析](http://www.jianshu.com/p/012d8a6159cf)

>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
>[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)
> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-a064cea5db22267a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)
