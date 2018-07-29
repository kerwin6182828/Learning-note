# 吐血总结，彻底明白 *python3* 编码原理
![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1533460367&di=dbe25c7343ee074a2043704c08270267&imgtype=jpg&er=1&src=http%3A%2F%2Fimg.hibor.com.cn%2FsinaImg%2F201705%2F57e1552a0296fde7184f225ddd1a8d13.jpeg)


> 关于编码的历史演变，utf-8是如何一步步发展来的，windows为啥依旧保持gbk的编码。。。
> 等等这些问题，网上一搜一大堆，大部分都是转发、分享后的雷同内容，依旧解决不了我内心的疑惑。。。

> 编码是个蛋疼的事情，倘若不弄清楚， 怎么在中国混？
> 经过自己查阅多方文档、多次深入实验，我树立了对编码的基本世界观。

`基础内容请自行谷歌..废话不多说，直接上干货！！`
> 下面用几个简单的代码段， 一步步讲解编码中“编”和“解”的问题！！（linux中运行）

<br>

---
###   “ 代码 一 ”：

```python
import sys, locale

s = "小甲"
print(s)
print(type(s))
print(sys.getdefaultencoding())
print(locale.getdefaultlocale())

with open("utf1","w",encoding = "utf-8") as f:
    f.write(s)
with open("gbk1","w",encoding = "gbk") as f:
    f.write(s)
with open("jis1","w",encoding = "shift-jis") as f:
    f.write(s)
```
> 代码很**简单**，学过Python的人应该都能看懂是啥意思~~
> 我们看一下运行结果：

### “  代码 一 ”  运行结果：
```python
小甲
<class 'str'>
utf-8
('en_US', 'UTF-8')

```
>  正如大家所想， 就是将“小甲”原样打印出来, 再把“小甲”存到3个文件中。
>  这么~~傻逼~~的代码我还不知道吗？ *( 重点在后面 )*
>  

> 这里解释一下打印出来的两个“utf-8”是什么意思：
>  > #### 上面的 utf-8 指：系统默认编码
>  - ###### 注： 不要把系统以为是操作系统，这里可以理解成python3的编译器本身
>  > #### 下面的 utf-8 指：本地默认编码
>  - ###### 注： 这个才是操作系统的编码。
>     ###### （在Windows运行会变成gbk）

**现在我们分别查看utf1 、gbk1、jis1 这三个文件的内容：**
```python
utf1 : 小甲
gbk1 : С▒▒▒
jis1 : ▒▒▒b

```
> ### 问题：
> 为什么 utf1 的内容很清楚，没有编码问题，而gbk1 、jis1 的内容都出现了乱码？
> ### 解释：
> #### 因为我文件存储时用的编码格式不是utf-8，而此时读取这两个文件时，使用的是linux操作系统的默认编码“utf-8”。
> ####  那么写入磁盘时不是用utf-8， 读出时却用utf-8，当然读不出来了。
> （这里需要大家了解encoding的真实作用）


--- 
<br>

### “ 代码 二 ”：
```python
#coding=gbk
import sys, locale

s = "小甲"
#coding=gbk
import sys, locale

s = "小甲"
print(s)
print(type(s))
print(sys.getdefaultencoding())
print(locale.getdefaultlocale())

with open("utf2","w",encoding = "utf-8") as f:
    f.write(s)
with open("gbk2","w",encoding = "gbk") as f:
    f.write(s)
with open("jis2","w",encoding = "shift-jis") as f:
    f.write(s)

```
> 代码框架一样很简单
但是请大家注意： **我在头部加了某个编码声明**
在代码运行前， 请大家自行猜测结果~~~
### “ 代码 二 ” 运行结果 ：
```python
灏忕敳
<class 'str'>
utf-8
('en_US', 'UTF-8')
Traceback (most recent call last):
  File "2", line 15, in <module>
    f.write(s)
UnicodeEncodeError: 'shift_jis' codec can't encode character '\u704f' in position 0: illegal multibyte sequence
```

> #### 问题来了：
> 1. 代码中明明 s = “小甲”， 为什么变成了 “灏忕敳” ？？
> 2. 为什么 jis 的编码失败了？（之前顶多只出现了乱码的问题，还不会报错，那它内部到底发生了什么？）
>  3. “coding=gbk” 到底是什么意思？？
>  4. 我明明写了 “coding=gbk” 的编码声明，为什么系统编码、本地默认编码还是没有改变？（那我写了有啥用？）
>  #### 解释一下：
>  以上这么多问题， 主要是因为没搞清楚头文件的 “coding=gbk” 编码声明是什么意思！！
>  1. 写了它之后，并不会更改本地、系统默认编码。
>  2. 它的意思是python3编译器在读取该.py文件时候，我应该用什么格式将它 **“解码”**？只和读取有关，所以当你确定你代码编辑时候用的是什么格式编码的，你才能把相应的编码格式写入头文件。
>  （在此示范代码中，我用的是linux的默认编码编辑，也就是utf-8，那么在后面运行的时候，却要求解释器用gbk去解码，自然很过分，就会出现了s=“小甲” 乱码的问题）
>  （大家一定要知道，编码是 **“编”** 和 **“解”** 的两个步骤，虽然通常我们都叫“编码格式”，这是有一定误导性的。
>   实际上有一半是“解码格式”，我们不能像网上有些文章一样将这两者混为一谈！！）





















<br>
<br>
<br>
<br>
<br>

![](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1532865629957&di=3d73a5a74934f118228a4e53fa152d14&imgtype=0&src=http%3A%2F%2Fpic.eastlady.cn%2Fuploads%2Ftp%2F201705%2F9999%2F49149eecc3.jpg)
