### 命令下载
```stata
ssc install fastcd, replace 
```
### 命令简介
```stata
  c code          to change directories (改变当前工作路径)
  c cur  code     to add current directory to database (为当前工作路径添加快捷标签)
  c drop code     to drop entry (删除快捷标签)
```





### Stata 范例
* 第一步：使用 `cd` 命令更改当前工作路径
```stata
cd D:\stata15\ado\personal\PX_A_2018a
```
* 第二步：使用 `c cur code` 命令定义当前工作路径的快接标签(**code**)，标签名称自行定义
```stata
c cur pxa D:\stata15\ado\personal\PX_A_2018a
```
* 第三步：查看已经定义的快捷标签：
进而输入如下命令，即可呈现已经定义的工作路径链接，单击即可转换当前工作路径：
```stata
c
```




