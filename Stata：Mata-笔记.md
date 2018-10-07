> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)



### 参考资料
#### 入门
- [A Brief Stata Primer, with Mata and Linear Algebra](http://rlhick.people.wm.edu/econ407/notes/stata_mata.html)

#### 进阶

##### 非线性最优化
- [The Stata Blog » Programming an estimation command in Stata: A map to posted entries](http://blog.stata.com/2016/01/15/programming-an-estimation-command-in-stata-a-map-to-posted-entries/)

- [The Stata Blog » Programming an estimation command in Stata: Nonlinear least-squares estimators](http://blog.stata.com/2016/05/12/programming-an-estimation-command-in-stata-nonlinear-least-squares-estimators/)

- [The Stata Blog » Programming an estimation command in Stata: A review of nonlinear optimization using Mata](https://blog.stata.com/2016/01/26/programming-an-estimation-command-in-stata-a-review-of-nonlinear-optimization-using-mata/)


### Stata Blogs: 模型估计编程系列
> 15 January 2016
> [David M. Drukker, Executive Director of Econometrics](https://blog.stata.com/author/ddrukker/ "Posts by David M. Drukker, Executive Director of Econometrics")               
I have posted a series of entries about programming an estimation command in Stata. They are best read in order. The comprehensive list below allows you to read them from first to last at your own pace.

1.  [Programming estimators in Stata: Why you should](http://bit.ly/1jzr6MR)               
 To help you write Stata commands that people want to use, I illustrate how Stata syntax is predictable and give an overview of the estimation-postestimation structure that you will want to emulate in your programs.

2.  [Programming an estimation command in Stata: Where to store your stuff](https://t.co/TIJqkSScvc)               
I discuss the difference between scripts and commands, and I introduce some essential programming concepts and constructions that I use to write the scripts and commands.

3.  [Programming an estimation command in Stata: Global macros versus local macros](http://bit.ly/1ksYHrI)               
I discuss a pair of examples that illustrate the differences between global macros and local macros.

4.  [Programming an estimation command in Stata: A first ado-command](http://bit.ly/1WSaZGo)               
 I discuss the code for a simple estimation command to focus on the details of how to implement an estimation command. The command that I discuss estimates the mean by the sample average. I begin by reviewing the formulas and a do-file that implements them. I subsequently introduce ado-file programming and discuss two versions of the command. Along the way, I illustrate some of the postestimation features that work after the command.

5.  [Programming an estimation command in Stata: Using Stata matrix commands and functions to compute OLS objects](http://blog.stata.com/2015/11/17/programming-an-estimation-command-in-stata-using-stata-matrix-commands-and-functions-to-compute-ols-objects/)               
I present the formulas for computing the ordinary least-squares (OLS) estimator, and I discuss some do-file implementations of them. I discuss the formulas and the computation of independence-based standard errors, robust standard errors, and cluster–robust standard errors. I introduce the Stata matrix commands and matrix functions that I use in ado-commands that I discuss in upcoming posts.

6.  [Programming an estimation command in Stata: A first command for OLS](http://bit.ly/1O6zkrN)               
 I show how to write a Stata estimation command that implements the OLS estimator by explaining the code.

7.  [Programming an estimation command in Stata: A better OLS command](http://bit.ly/1Ly4lyg)               
I use the  **syntax**  command to improve the command that implements the OLS estimator that I discussed in  [Programming an estimation command in Stata: A first command for OLS](http://bit.ly/1O6zkrN). I show how to require that all variables be numeric variables and how to make the command accept time-series operated variables.

8.  [Programming an estimation command in Stata: Allowing for sample restrictions and factor variables](http://bit.ly/1IbiS8j)               
 I modify the OLS command discussed in  [Programming an estimation command in Stata: A better OLS command](http://bit.ly/1Ly4lyg) to allow for sample restrictions, to handle missing values, to allow for factor variables, and to deal with perfectly collinear variables.

9.  [Programming an estimation command in Stata: Allowing for options](http://bit.ly/1YFKl6o)               
I make three improvements to the command that implements the OLS estimator that I discussed in  [Programming an estimation command in Stata: Allowing for sample restrictions and factor variables](http://bit.ly/1IbiS8j). First, I allow the user to request a robust estimator of the variance–covariance of the estimator. Second, I allow the user to suppress the constant term. Third, I store the residual degrees of freedom in **e(df_r)**
so that **test** will use the *t* or *F* distribution instead of the normal or chi-squared distribution to compute the *p*-value of Wald tests.

10.  [Programming an estimation command in Stata: Using a subroutine to parse a complex option](http://bit.ly/1TUQwjW)               
I make two improvements to the command that implements the OLS estimator that I discussed in  [Programming an estimation command in Stata: Allowing for options](http://bit.ly/1YFKl6o). First, I add an option for a cluster–robust estimator of the variance–covariance of the estimator (VCE). Second, I make the command accept the modern syntax for either a robust or a cluster–robust estimator of the VCE. In the process, I use subroutines in my ado-program to facilitate the parsing, and I discuss some advanced parsing tricks.

11.  [Programming an estimation command in Stata: Mata 101](http://bit.ly/1mYx8by)               
 I introduce Mata, the matrix programming language that is part of Stata.

12.  [Programming an estimation command in Stata: Mata functions](http://bit.ly/1l64tPU)                
I show how to write a function in Mata, the matrix programming language that is part of Stata.

13.  [Programming an estimation command in Stata: A first ado-command using Mata](http://bit.ly/1RDr8lo)              
I discuss a sequence of ado-commands that use Mata to estimate the mean of a variable. The commands illustrate a general structure for Stata-Mata programs.

14.  [Programming an estimation command in Stata: Computing OLS objects in Mata](http://bit.ly/1mYzuay)            
 I present the formulas for computing the OLS estimator and show how to compute them in Mata. This post is a Mata version of  [Programming an estimation command in Stata: Using Stata matrix commands and functions to compute OLS objects](http://blog.stata.com/2015/11/17/programming-an-estimation-command-in-stata-using-stata-matrix-commands-and-functions-to-compute-ols-objects/). I discuss the formulas and the computation of independence-based standard errors, robust standard errors, and cluster–robust standard errors.

15.  [Programming an estimation command in Stata: An OLS command using Mata](http://bit.ly/1Q5hKFB)            
 I discuss a command that computes OLS results in Mata, paying special attention to the structure of Stata programs that use Mata work functions.

16.  [Programming an estimation command in Stata: Adding robust and cluster–robust VCEs to our Mata-based OLS command](http://blog.stata.com/2016/01/19/programming-an-estimation-command-in-stata-adding-robust-and-cluster-robust-vces-to-our-mata-based-ols-command/)       
 I show how to use the  [undocumented](http://www.stata.com/help.cgi?undocumented)  command
 [_vce_parse](http://www.stata.com/help.cgi?_vce_parse)  to parse the options for robust or cluster–robust estimators of the  **VCE**. I then discuss    [myregress12.ado](http://www.stata.com/users/ddrukker/blog/myregress12.ado), which performs its computations in Mata and computes an    **IID**-based, a robust, or a cluster–robust estimator of the    **VCE**.

17.  [Programming an estimation command in Stata: A review of nonlinear optimization using Mata](http://blog.stata.com/2016/01/26/programming-an-estimation-command-in-stata-a-review-of-nonlinear-optimization-using-mata/)     
 I review the theory behind nonlinear optimization and get some practice in Mata programming by implementing an optimizer in Mata. This post is designed to help you develop your Mata programming skills and to improve your understanding of how the Mata optimization suites **optimize()** and **moptimize()** work.

18.  [Programming an estimation command in Stata: Using optimize() to estimate Poisson parameters](http://blog.stata.com/2016/01/28/programming-an-estimation-command-in-stata-using-optimize-to-estimate-poisson-parameters/)      
I show how to use **optimize()**  in Mata to maximize a Poisson log-likelihood function and to obtain estimators of the   **VCE** based on  **IID** observations or on robust methods.

19.  [Programming an estimation command in Stata: A poisson command using Mata](http://blog.stata.com/2016/02/02/programming-an-estimation-command-in-stata-a-poisson-command-using-mata/)
    I discuss **mypoisson1**, which computes Poisson-regression results in Mata. The code in  **mypoisson1.ado** is remarkably similar to the code in **myregress11.ado**, which computes  **OLS**  results in Mata, as I discussed in [Programming an estimation command in Stata: An OLS command using Mata](http://blog.stata.com/2016/01/12/programming-an-estimation-command-in-stata-an-ols-command-using-mata/).

20.  [Programming an estimation command in Stata: Handling factor variables in optimize()](http://blog.stata.com/2016/02/09/programming-an-estimation-command-in-stata-handling-factor-variables-in-optimize/)      
I discuss a method for handling factor variables when performing nonlinear optimization using  **optimize()**. After illustrating the issue caused by factor variables, I present a method and apply it to an example using  **optimize()**.

21.  [Programming an estimation command in Stata: Handling factor variables in a poisson command using Mata](http://blog.stata.com/2016/02/17/programming-an-estimation-command-in-stata-handling-factor-variables-in-a-poisson-command-using-mata/)       
**mypoisson2.ado**  handles factor variables and computes its Poisson–regression results in Mata. I discuss the code for **mypoisson2.ado**, which I obtained by adding the method for handling factor variables discussed in  [Programming an estimation command in Stata: Handling factor variables in optimize()](http://blog.stata.com/2016/02/09/programming-an-estimation-command-in-stata-handling-factor-variables-in-optimize/)  to **mypoisson1.ado**, discussed in [Programming an estimation command in Stata: A poisson command using Mata](http://blog.stata.com/2016/02/02/programming-an-estimation-command-in-stata-a-poisson-command-using-mata/).

22.  [Programming an estimation command in Stata: Allowing for robust or cluster–robust standard errors in a poisson command using Mata](http://blog.stata.com/2016/02/23/programming-an-estimation-command-in-stata-allowing-for-robust-or-clusterrobust-standard-errors-in-a-poisson-command-using-mata/)    
  **mypoisson3.ado** adds options for a robust or a cluster–robust estimator of the variance–covariance of the estimator (**VCE**) to **mypoisson2.ado**, which I discussed in  [Programming an estimation command in Stata: Handling factor variables in a poisson command using Mata](http://blog.stata.com/2016/02/17/programming-an-estimation-command-in-stata-handling-factor-variables-in-a-poisson-command-using-mata/). **mypoisson3.ado**  parses the **vce()**  option using the techniques I discussed in [Programming an estimation command in Stata: Adding robust and cluster–robust VCEs to our Mata-based OLS command](http://blog.stata.com/2016/01/19/programming-an-estimation-command-in-stata-adding-robust-and-cluster-robust-vces-to-our-mata-based-ols-command/). I show how to use **optimize()** to compute the robust or cluster–robust **VCE**.

23.  [Programming an estimation command in Stata: Adding analytical derivatives to a poisson command using Mata](http://blog.stata.com/2016/03/02/programming-an-estimation-command-in-stata-adding-analytical-derivatives-to-a-poisson-command-using-mata/)     
 Using analytically computed derivatives can greatly reduce the time required to solve a nonlinear estimation problem. I show how to use analytically computed derivatives with **optimize()**, and I discuss  **mypoisson4.ado**, which uses these analytically computed derivatives. Only a few lines of **mypoisson4.ado** differ from the code for **mypoisson3.ado**, which I discussed in [Programming an estimation command in Stata: Allowing for robust or cluster–robust standard errors in a poisson command using Mata](http://blog.stata.com/2016/02/23/programming-an-estimation-command-in-stata-allowing-for-robust-or-clusterrobust-standard-errors-in-a-poisson-command-using-mata/).

24.  [Programming an estimation command in Stata: Making predict work](http://blog.stata.com/2016/03/17/programming-an-estimation-command-in-stata-making-predict-work/)    
    I make  **predict** work after **mypoisson5** by writing an ado-command that computes the predictions and by having **mypoisson5** store the name of this new ado-command in **e(predict)**.

25.  [Programming an estimation command in Stata: Certifying your command](http://blog.stata.com/2016/03/31/programming-an-estimation-command-in-stata-certifying-your-command/)     
Before you use or distribute your estimation command, you should verify that it produces correct results and write a do-file that certifies that it does so. I discuss the processes of verifying and certifying an estimation command, and I present some techniques for writing a do-file that certifies  **mypoisson5**, which I discussed in previous posts.

26.  [Programming an estimation command in Stata: Nonlinear least-squares estimators](http://blog.stata.com/2016/05/12/programming-an-estimation-command-in-stata-nonlinear-least-squares-estimators/)     
    I want to write ado-commands to estimate the parameters of an exponential conditional mean (**ECM**) model and probit conditional mean (**PCM**) model by nonlinear least squares (**NLS**). Before I can write these commands, I need to show how to trick **optimize()** into performing the Gauss–Newton algorithm and apply this trick to these two problems.

27.  [Programming an estimation command in Stata: Consolidating your code](http://blog.stata.com/2016/05/18/programming-an-estimation-command-in-stata-consolidating-your-code/)     
I write ado-commands that estimate the parameters of an exponential conditional mean model and a probit conditional mean model by nonlinear least squares, using the methods that I discussed in the post  [Programming an estimation command in Stata: Nonlinear least-squares estimators](http://blog.stata.com/2016/05/12/programming-an-estimation-command-in-stata-nonlinear-least-squares-estimators/). These commands will either share lots of code or repeat lots of code, because they are so similar. It is almost always better to share code than to repeat code. Shared code only needs to be changed in one place to add a feature or to fix a problem; repeated code must be changed everywhere. I introduce Mata libraries to share Mata functions across ado-commands, and I introduce wrapper commands to share ado-code.

28.  [Programming an estimation command in Stata: Writing an estat postestimation command](http://blog.stata.com/2016/10/20/programming-an-estimation-command-in-stata-writing-an-estat-postestimation-command/) 
   **estat** commands display statistics after estimation. Many of these statistics are diagnostics or tests used to evaluate model specification. Some statistics are available after all estimation commands; others are command specific. I illustrate how **estat** commands work and then show how to write a command-specific **estat** command for the  **mypoisson**command that I have been developing.


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

> Stata连享会 [精彩推文1](https://gitee.com/arlionn/stata_training/blob/master/README.md)  || [精彩推文2](https://github.com/arlionn/stata/blob/master/README.md)

---
![欢迎加入Stata连享会(公众号: StataChina)](http://upload-images.jianshu.io/upload_images/7692714-0957520049a134dc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "扫码关注 Stata 连享会")
