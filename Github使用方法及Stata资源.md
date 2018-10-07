> 作者：王若溪、李缘  &emsp;  ([知乎](https://zhuanlan.zhihu.com/arlion) | [简书](http://www.jianshu.com/u/69a30474ef33) | [码云](https://gitee.com/arlionn))


> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)


# GitHub 使用方法
## GitHub 是什么？
- GitHub 是一个免费代码托管平台，用于管理代码历史纪录与远程协作，可以让你和他人在任何地方共同开展项目！

[图片上传失败...(image-94d4d2-1510565857205)]
***
## 功能简要介绍
### 基本界面
打开 GitHub 网站 https://github.com/， 注册账号并登录，进入个人主页。

![个人主页](http://upload-images.jianshu.io/upload_images/7692714-6a3ee6e6688cfc5d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

页面中间的菜单栏显示了你使用 GitHub 的基本情况。

![菜单栏](http://upload-images.jianshu.io/upload_images/7692714-5c7c454f6a934ccb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![贡献情况](http://upload-images.jianshu.io/upload_images/7692714-ccd7f8ee4fa57359.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- Repository :存放代码的储存库,通常用于组织单个项目,存储库可以包含文件夹和文件、图像、视频、电子表格和数据集等任何你的项目所需要的内容
- Star :你收到的赞
- Follower :关注你的人
- Following :你关注的人
- Contributions :你在 GitHub 的使用或贡献情况,每个方格代表一个日期,贡献程度随颜色加深而递增

### 主要操作：
- Fork ：
将别人建立好的储存库 repository 全部复制到自己的账户中，会在自己的账户中出现同样名字的repository
- Clone ：
将 repository 复制到本地或客户端
- Roll back to this commit ：
回退到之前的版本
- Branch ：
分支，是同时对同一储存库进行编辑的方法， GitHub 储存库默认有一个主分支 master ，当我们在主分支 Master 开发过程中遇到一个新的功能需求，我们就可以新建一个分支同步开发而互不影响，开发完成后，再合并 merge 到主分支Master上
- Commits :提交,保存更改

#### GitHub Desktop 的操作
- Add ：
加入到已有的 repository 中
- Clone ：
复制到本地
- Create ：
创建新的 repository
- Publish ：
将本地的更新同步到 GitHub 中
***
## 使用步骤
掌握以下简单几步，我们就可以开始使用 GitHub 啦！

```
graph TD
创建与使用存储库-->启动与管理新分支
启动与管理新分支-->修改与提交文件
修改与提交文件-->提出与合并请求
```
***
### 1. 创建与使用存储库
- 页面右上角,在你的头像旁边找到“+”,点击并选择新的存储库 New Repository

![创建储存库](http://upload-images.jianshu.io/upload_images/7692714-8b6cf8b4328b50a7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 命名你的存储库
- 写一个简短的描述
- 选择以自述文件初始化 Initi a lize this Repository with a README

![创建储存库](http://upload-images.jianshu.io/upload_images/7692714-bc903a52e19edfe8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 单击创建储存库 Create Repository
***
### 2. 启动与管理新分支

默认情况下，你的存储库有一个名为 Master 的主分支，也叫最终分支。我们使用其他分支进行实验并在提交给主分支Master之前进行编辑

当你在主分支上创建一个分支时，你在主分支的基础上复制了一个分支。如果有人在你对分支工作时对主分支进行了更改，你可以将这些更新拖进主分支，分支间的关系如下所示

![分支关系示意图](http://upload-images.jianshu.io/upload_images/7692714-811023d4ee6b86ca.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*具体操作：*
- 在新建的储存库里,单击文件列表顶部的下拉框,显示主分支 master
- 在文本框内输入新分支的名称,如在 readme - edits
- 选择蓝色创建分支框或单击键盘上的“Enter”

![创建分支](http://upload-images.jianshu.io/upload_images/7692714-94cc610813e908d5.gif?imageMogr2/auto-orient/strip)
***
### 3. 修改与提交文件

现在，你在 readme - edits 分支的代码视图中，这是主分支的一个副本。我们开始编辑。

在 GitHub 上，保存的变化称为提交 commits 。每个提交都有一个关联的提交消息，解释为什么做出了特定更改。提交消息捕获更改的历史，因此其他贡献者可以理解您所做的工作和原因。

*具体操作：*
- 单击 readme . md 文件
- 点击位于文件预览右上角的铅笔图标,进行编辑
- 在编辑窗口内进行编辑
- 写明提交信息,描述你的修改
- 点击 Commit Changes 按钮

![修改与提交](http://upload-images.jianshu.io/upload_images/7692714-61f47b904e4153d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这些修改仅被保存在 readme - edits 分支，这使得它与主分支 master 有所不同。
***
### 4. 提出请求 Pull Request

由于刚刚的编辑， readme - edits 分支已经能区别于主分支 master ，我们就能提出请求（合并）。

提出请求 Pull Request 是 GitHub 协作的核心。当你提出请求时，你在提议并请求他人查看你的修改，并将修改合并入他们的分支。提出请求显示了分支之间的差异，绿色表示添加，红色表示删减。

*具体操作：*
- 单击 Pull Request 按钮,然后页面单击绿色的 New Pull Request按钮

![image](http://upload-images.jianshu.io/upload_images/7692714-ca48f340fed4b2b1.gif?imageMogr2/auto-orient/strip)
- 选择你所编辑的分支,与主分支进行比较

![image](http://upload-images.jianshu.io/upload_images/7692714-ab5c710327737991.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 在对比页面检查分支间的差异,确保它们是你想提交的内容

![image](http://upload-images.jianshu.io/upload_images/7692714-c32b333ac8070810.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 当你对想要提交的修改满意时,单击绿色的 Create Pull Request 按钮

![image](http://upload-images.jianshu.io/upload_images/7692714-ce223f385680faff.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 为你创建的 Pull Request 命名,并简要说明你做出的修改

![image](http://upload-images.jianshu.io/upload_images/7692714-6575ed2c8c395fcb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- 确认好以上信息，单击 Create pull request 就可以啦！
***
### 5.合并请求 Pull Request

到了最后一步，是时候把你的更改放在一起啦——将你编辑的分支合并到主分支中。

*具体操作：*
- 单击绿色的合并请求 Merge Pull Request 按钮,将更改合并到主目录中
- 单击确认合并 Confirm merge
- 更改已被合并,原来编辑的分支就可以删除了,点击紫色的删除分支 Delete branch 按钮

![image](http://upload-images.jianshu.io/upload_images/7692714-0685917aa77e1fbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## GitHub 与 Stata 结合
- 在 GitHub 中搜索 stata 相关信息，并 fork 到自己的账户：
1. 登录 GitHub ，在搜索框中输入关键字，如 stata ，单击回车

![image](http://upload-images.jianshu.io/upload_images/7692714-a1c10794f8e569fc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2.选择查找的内容

![image](http://upload-images.jianshu.io/upload_images/7692714-d3a2382fa17d750e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

3.选择排序方式

![image](http://upload-images.jianshu.io/upload_images/7692714-63cd1f887b358fad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


4.选择一个 repository ， fork 到自己的账户中

单击 fork ，保存到自己的账户中
![image](http://upload-images.jianshu.io/upload_images/7692714-0d8a62eac2dfdae4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


fork 成功的 repository 会出现在自己的账户中
![image](http://upload-images.jianshu.io/upload_images/7692714-39a67856c53a6a43.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5.取消 fork ：

单击 setting
![image](http://upload-images.jianshu.io/upload_images/7692714-51354f1f0b1a7050.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下拉出现“Danger Zone”，点击“Delete this repository”
![image](http://upload-images.jianshu.io/upload_images/7692714-65086a6882c84475.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在输入框中正确输入 repository 名称，下方按钮“I understand the consequences, delete this repository”会亮起，单击即可成功取消 fork
![image](http://upload-images.jianshu.io/upload_images/7692714-26fd55cedf9cf8c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

成功删除后，界面会出现提示
![image](http://upload-images.jianshu.io/upload_images/7692714-eaf7a33a1d802740.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### Stata 中的 GitHub 命令

#### 1. 安装 GitHub 命令包
在 stata 中输入如下命令即可： 
```
net install github , from ("https://raw.githubusercontent.com/haghish/github/master/")
```

#### 2. 用 github 安装 GitHub 用户开发的命令

要安装命令包，需要 GitHub 用户名和存储库的名称。 例如，要安装 `devtools` ，你会发现并不能用 ssc 安装，也就是说 `devtools` 并没有被 `ssc` 收录。
![](http://upload-images.jianshu.io/upload_images/7692714-d138ed24a1019657.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在 GitHub 中可以搜索到用户  **gvegayon** 开发了 `devtools` 。

输入以下命令来安装 **gvegayon** 开发的  `devtools` 命令包：
```stata
github install gvegayon/devtools
```
可见，安装时基本的语法是 `github install 用户名/命令`。

#### 3. 卸载安装包

要卸载命令包，请使用 `uninstall 命令名称`，后跟程序包名称。 例如，卸载刚刚安装好的 `devtools` 命令的安装包：
```stata
github uninstall devtools
```
卸载 `github` 本身的安装包，命令为：
```stata
ado uninstall github
```

> 看到这里，你已经初步学会如何创建项目并进行修改与合并啦！
> 可喜可贺！
> 继续开始新的探索吧！




***
#### 参考网址：

[Hello World GitHub Guides](https://guides.github.com/activities/hello-world/)
[ GitHub for Windows使用教程](http://youngxhui.github.io/2016/05/03/GitHub-for-Windows%E4%BD%BF%E7%94%A8%E6%95%99%E7%A8%8B(%E4%B8%80)/)
[怎样使用 GitHub？](https://www.zhihu.com/question/20070065)
[GitHub使用](http://www.cnblogs.com/yanliujun-tangxianjun/)
[SSC的好兄弟“github”](http://www.sohu.com/a/160488855_697896)




---
![Stata连享会二维码](http://upload-images.jianshu.io/upload_images/7692714-0e3b1f51662b6563.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")



>## 关于我们
- 【**Stata 连享会**】由中山大学连玉君老师团队创办，旨在定期与大家分享 Stata 应用的各种经验和技巧。
- 公众号推文同步发布于 [【简书-Stata连享会】](http://www.jianshu.com/u/69a30474ef33) 和 [【知乎-连玉君Stata专栏】](https://www.zhihu.com/people/arlionn)。可以在**简书**和**知乎**中搜索关键词`Stata`或`Stata连享会`后关注我们。
- 推文中的相关数据和程序，以及 [Markdown 格式原文](https://gitee.com/arlionn/jianshu) 可以在 [【Stata连享会-码云】](https://gitee.com/arlionn) 中获取。[【Stata连享会-码云】](https://gitee.com/arlionn) 中还放置了诸多 Stata 资源和程序。如 [Stata命令导航](https://gitee.com/arlionn/stata/wikis/Home) ||  [stata-fundamentals](https://gitee.com/arlionn/stata-fundamentals) ||  [Propensity-score-matching-in-stata](https://gitee.com/arlionn/propensity-score-matching-in-stata) || [Stata-Training](https://gitee.com/arlionn/StataTraining) 等。


>### 联系我们
- **欢迎赐稿：** 欢迎将您的文章或笔记投稿至`Stata连享会`，我们会保留您的署名；录用稿件达`五篇`以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **意见和资料：** 欢迎您的宝贵意见，您也可以来信索取推文中提及的程序和数据。
- **招募英才：** 欢迎加入我们的团队，一起学习 Stata。合作编辑或撰写稿件五篇以上，即可**免费**获得 Stata 现场培训 (初级或高级选其一) 资格。
- **联系邮件：** StataChina@163.com
