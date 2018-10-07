> 作者：连玉君 ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))

> [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)


> 导言：我是老师。我不想吃粉笔灰！我不喜欢用打滑的油性笔！我怎么写出清新的板书？其实很简单，用数位板写电子板书！在电脑上书写，实时投影到教室的大屏幕上，课后还可把所有的电子板书转换为 PDF 发送给学生。

>![连老师授课中：表情很萌呆](http://upload-images.jianshu.io/upload_images/7692714-ca00a4e8fa0d663f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![连老师授课中：漂亮的电子板书](http://upload-images.jianshu.io/upload_images/7692714-fa40fa347f1e29b5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![连老师上课中：在电脑屏上随意标注](http://upload-images.jianshu.io/upload_images/7692714-0fb85ff7d30b87a9.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![连老师授课中](http://upload-images.jianshu.io/upload_images/7692714-3e678cefdfd452cb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



我是高校教师，教授的课程中经常要写数学和统计学公式。现在很少有带`黑板+粉笔`的教室了。而`白板+马克笔`写出来的效果实在是不敢恭维，况且，反光和打滑问题更是让人无法忍受。

考虑到多数教室都有更高端的配置 —— `投影仪`，为何不用数位板写电子版书呢？

  

其实，在全球范围内颇受欢迎的可汗学院公开课 (  [Khan Academy](https://www.khanacademy.org/)  ) 就是全程使用电子板书在讲解。这可以在很大程度上避免 PPT 放映带来的诸多弊端。让学生能紧紧跟随教师的思路而行。

要实现电子板书，其实只需要一个数位板即可。便宜的几百块钱，贵一点的，我选择了和冠 (Wacom CTH 680) 数位板。为了保证能够逼真地呈现出粉笔甚至是毛笔的书写效果，还需要安装一个绘图会手写软件，我选择了 ArtRage (绘图精灵)。这款软件本是用来绘制油画、动漫，或书法作品。用它来写板书，其实多少有点「杀鸡用牛刀」的感觉，但书写效果确实是毫无瑕疵，有剑笔如飞的感觉。

![Khan 工作中](http://upload-images.jianshu.io/upload_images/7692714-aa4368cde6a56586.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 我的电子板书展示

>![DID-Stata板书.png](http://upload-images.jianshu.io/upload_images/7692714-6b53b1adafdd5605.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![Stata板书-随机边界分析.png](http://upload-images.jianshu.io/upload_images/7692714-adc7fb2a0de7727b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>![动态面板-Stata板书.png](http://upload-images.jianshu.io/upload_images/7692714-19845dcebe50627c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- ArtRage 工作界面
![ArtRage界面.png](http://upload-images.jianshu.io/upload_images/7692714-ee40e5068f992f37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 工具

- Wacom CTH 680/690 (数位板和笔)
- ArtRage 软件 (书写板书)  [--下载页面--](http://www.hanzify.org/software/13772.html)


### ArtRage 使用说明

#### 图层与板书

在 ArtRage 里写电子板书是通过图层来实现的。每个图层相当于一张透明的胶片，你可以自行设定图层的开关状态。

基本的原则是：共同的信息可以写在一个或多个图层上，一直保持为 **「可视」** 状态；其他内容可以写在新的图层上，写满后保存，将其设定为 **「隐藏」** 状态，进而新建下一个图层写新的内容。
 
- 新建一个 **图层 1**，使用填充工具将其填充为黑色或墨绿色，这是电子黑板的底色；设定为  **「可视」**
- 再建立一个 **图层 2**，写上基本信息，如 **「Stata板书」** 或 **「日期：2017/12/15」** 等；设定为  **「可视」**
- 第一页板书：新建 **图层 3**，可以正常开始写板书了；
- 第二页板书：将图层3设定为**「隐藏」**状态，新建 **图层 4**，开始写第二页板书。

#### 将电子板书转换为 PDF 文档

- 右击想要输出的图层，按快捷键 「Ctrl+E」将该图层输出为图片；其他需要输出的图层也都按此处理。保存时，最好按照编号保存，以方便后续合并。
- 将上述图片转换为 PDF 文件合集。可以使用 「Adobe Acrobat Pro」软件将上述图片文件合并到一个文档中。  
**方法**：依次点击「合并」&rarr;「合并文件到单个PDF(M)」，然后按照提示操作即可。 




#### ArtRage 常用快捷键和设定


- 1.界面相关
  - 左右下角的托盘缩放   `Tab`
- 2.笔相关
  - 钢笔 i (inkPen)； 铅笔 p (pencil)； 马克笔 m； 水笔 w
  - 改变笔的粗细： `Shift+左右滑动笔尖`
- 3.画布相关
  - 原始宽度设定为 1000 左右即可，长度可以设为 5000;
  - 书写过程中可以更改画布尺寸： `Edit-->Reset the painting`
  - 画布材质：`Ctrl+shift+C` `「View」`&rarr; `「Cavas settings」`
- 4.屏幕相关
  - Artrage 窗口最小化 `Ctrl+H (hidden)`
  - 屏幕缩放： `Ctrl + +`; `Ctrl + -` (快速缩放)； 平滑缩放：拇指+食指缩放； 
  - `D`: 画布全屏显示; `Ctrl+D`  画布按 100% 比例显示
  - 画布旋转：拇指+食指扭动； 
  - 画布角度端正：`Alt+D`
  - 设定画布：Ctrl+Alt+C, 可以打开画布的背景灯(建议不要打开背景灯，否则会导致页面上方和下方文字颜色不均匀)
- 5.选择对象
  - 选择 `Shift+S`
  - 取消选择 `Ctrl+D`
- 6.保存和导出图片
  - 保存 `Ctrl+S`
  - 导出图片 `Ctrl+Shift+E` (特别注意，默认是导出当前图层，若想导出所有图层，则需预先 `Ctrl+A` 选中所有图层)
- 7.黑板和白板模式
  - 黑板模式：建立一个图层，设定为墨绿色；然后新建一个图层，在这个图层上写板书；好处在于，可以随时更换黑板的底色。
  - 白板也可以按照这个方式建立

### 我目前的一些基本设定

- 画布尺寸  
依次点击**Edit** &rarr; **Resize the painting**
![画布尺寸-Edit-Resize Painting Size](http://upload-images.jianshu.io/upload_images/7692714-763625489715e581.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 钢笔设定
![我最喜欢的钢笔设定](http://upload-images.jianshu.io/upload_images/7692714-ae56ddbbc1678c00.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 我的模板   
设定好这些参数后开一 ArtRage 画布文件另存一份，作为后续板书的模板。我的模板存放于[--点击下载模板--](https://pan.baidu.com/s/1nvqYrDB)

### 相关资料

网上有不少关于 ArtRage 软件使用的介绍，可以自行搜索。下面是我记录的一些笔记，供参考。

- [连玉君 Wacom 笔记](http://note.youdao.com/noteshare?id=8bb81823d95051825a339185817d637c&sub=F0B53FE4416B4A83874736D8439398C7)
- [Set Keyboard Commands in ArtRage 4.5](http://note.youdao.com/noteshare?id=8bb81823d95051825a339185817d637c&sub=F0B53FE4416B4A83874736D8439398C7)

![2018年寒假班与助教和学员留影](http://upload-images.jianshu.io/upload_images/7692714-9afe0e1dcf04ccdc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



>#### 关于我们

- 【**Stata 连享会(公众号：StataChina)**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](https://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://zhuanlan.zhihu.com/arlion)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 点击推文底部【阅读原文】可以查看推文中的链接并下载相关资料。
- [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)

>#### 联系我们

- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会(公众号: StataChina)`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com

>#### 往期精彩推文
[Stata连享会推文列表](https://www.jianshu.com/p/de82fdc2c18a)

> [Stata 现场培训报名中](https://www.jianshu.com/p/af6fb0448297)
>
>![连玉君Stata现场班报名中](https://upload-images.jianshu.io/upload_images/7692714-78fa5fece25aa2fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

