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
>  正如大家所想， 就是将“小甲”原样打印出来, 再把“小甲”存到3个文件中（shift-jis是日文编码格式）。
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
>  1. 它的意思是python3编译器在读取该.py文件时候，我应该用什么格式将它 **“解码”**？只和读取有关，所以当你确定你代码编辑时候用的是什么格式编码的，你才能把相应的编码格式写入头文件。
>  （在此示范代码中，我用的是linux的默认编码编辑，也就是utf-8，那么在后面运行的时候，却要求解释器用gbk去解码，自然很过分，就会出现了s=“小甲” 乱码的问题）
>  （大家一定要知道，编码是 **“编”** 和 **“解”** 的两个步骤，一定要一一对应才能正确解码！虽然通常我们都叫“编码格式”，这是有一定误导性的。
>   实际上另一半是“解码格式”，要有意识地区分 **“编”** 和 **“解”** ，我们不能像网上有些文章一样将这两者混为一谈！！）
>  2. 根据上面的解释应该可以明白，写了它之后，并不会更改本地、系统默认编码。
>  （本地默认编码只跟操作系统相关，linux中是utf-8，windows中是gbk。）
>  （系统默认编码实际是有python3和python2的差异的，python3是utf-8，python2是ascii。）
>  3. 那么，上面两种编码的作用体现在哪里呢？
>  **敲黑板，划重点：**
>  ***系统默认编码***  指：在python3编译器读取.py文件时，若没有头文件编码声明，则默认使用“utf-8”来对.py文件进行解码。
>  ***本地默认编码***  指：在你编写的python3程序时，若使用了 *open( )函数* ，而不给它传入 *“ encoding ”* 这个参数，那么会自动使用本地默认编码。没错，如果在Windows系统中，就是默认用*gbk格式*！！！
>  （这个问题困扰了我好久， 不说好了一直默认utf-8到天长地久的嘛，咋换成win后就频频失信呢。所以请大家在这里**注意**：linux中可以不用传“ encoding” 的参数， 而win中不能忘了~~~）
>  4. 再来回答一下报错的问题：
>  因为我们的编译器已经用了gbk来解码此.py文件了，所以读取出来的变量 s 已经变成了我们现在看到的“ 灏忕敳 ” 了！那么此时把 s 存到磁盘文件中，实际上存的是乱码后的 “ 灏忕敳 ”。而在日文中，是没有这3个字的， 所以自然反馈说 “ 在 position 0 的位置，编码失败了” 


**现在我们再来分别查看utf2 、gbk2、jis2 这三个文件的内容：**

```python
utf2 : 灏忕敳
gbk2 : 小甲
jis2 : 
```
（跟你想象中的结果是否一样呢？？嘿嘿嘿~~）
> ### 问题：
> 1. 为什么 我用 “utf-8 ” 去编码存储，后来用linux默认的 “ utf-8 ” 去解码，却出现乱码？
> 2. 为什么我用“ gbk ” 去编码存储， 后面用linux默认的 “ utf-8 ” 去解码，明明编码、解码格式不一致，却能够正常显示？

> ### 解释：
> #### 1. 实际上面两个问题是同一个问题，相信细心的同学已经知道问题出在哪里了，我上文已经说的很清楚了。此时的变量 s 已经变成了“ 灏忕敳 ”， 那么utf2这个文本文件自然是显示“灏忕敳”。
> #### 2. 而“灏忕敳”这三个字符是怎么来的呢？
> ```
> 第1步：  小甲（unicode）   ---用 "utf-8" 编码--->    e5b0 8fe7 94b2 (utf-8编码后的二进制代码)
> 第2步：  e5b0 8fe7 94b2   ---用 “gbk” 解码--->     " 灏忕敳 " （unicode）(乱码)
> 第3步：  “ 灏忕敳 ”     --- 用 “ gbk ” 编码--->     e5b0 8fe7 94b2 ( 第2步的逆向)
> 第4步：  e5b0 8fe7 94b2     ---用 “ utf-8 ” 解码--->    小甲（unicode）
>  ```
>  *我想上述的步骤够清楚了吧 ~ 
>  第3、 4 步就是逆推回去，就变成了正常的 “ 小甲 ”
>  看懂了这个 **“ 编码 ”** 和 **“ 解码 ”** 的过程，你的编码问题已经解决大半了！*

--- 
<br>

### “ 代码 三 ”：
```python
#coding=shift-jis
import sys, locale

s = "小甲"
print(s)
print(type(s))
print(sys.getdefaultencoding())
print(locale.getdefaultlocale(), "\n\n")

a = s.encode("shift-jis")
print(a)
print(type(a))
b = a.decode("utf-8")
print(b)
print(type(b))
print(a.decode("gbk"))

with open("utf3","w",encoding = "utf-8") as f:
    f.write(s)
with open("gbk3","w",encoding = "gbk") as f:
    f.write(s)
with open("jis3","w",encoding = "shift-jis") as f:
    f.write(s)

```
> 代码整体结构还是老样子，只不过中间多加了一小段代码，便于解释~
> 
### “ 代码 三 ” 运行结果 ：
```python
蟆冗抜
<class 'str'>
utf-8
('en_US', 'UTF-8')


b'\xe5\xb0\x8f\xe7\x94\xb2'
<class 'bytes'>
小甲
<class 'str'>
灏忕敳

```
> 这里可以看到，此时我们的变量 s 已经变成了“ 蟆冗抜 ”（另一个用jis解码造成的乱码）。
> 那么此时，我把 “ 蟆冗抜 ” 用 “ shift-jis ” 解码回去并赋值给变量 a，打印一下，可以看到 a 就是正常显示的 “ 小甲 ”， 这也证明了我上面的推断是绝对正确的！！

**现在，我们依旧分别查看一下 utf3 、gbk3、jis3 这三个文件的内容：**

```python
utf3 : 蟆冗抜
gbk3 : ▒▒ߒi
jis3 : 小甲
```
（oops~~  见鬼，又是这么乱七八糟的东西）
`这里我澄清一下，实际上utf3这个至少还能有文字，这叫乱码。而gbk3那个东西一团黑是什么鬼，是报错，linux的默认编码无法解码gbk3的文件，所以打印地乱七八糟。`
> ### 问题：
>  - 为什么 utf3 的文件是显示乱码， 而 gbk3 的文件却是报错呢？？
> ### 解释：
>  - 这是因为 utf-8 与 gbk 编码的算法差异。
>  我们最常看到的是utf-8解码报错，因为它是可变长的的编码，有1个字节的英文字符，也有2个字节的阿拉伯文，也有3个字节的中文和日文。
>  gbk是定长的2字节，比较死板，utf-8编出来的二进制文件，它常常也会一股脑的全部按照2个字节、2个字节地去解码，结果可想而知，全是乱码！！！
>  而utf-8是有严格定义的，一个字节的字符高位必须是0；三个字节的字符中，第一个字节的高位是1110开头。（[相关utf-8的编码算法链接](https://blog.csdn.net/hongweigg/article/details/6826836)）

<br>

`至此，代码的示范部分就结束了~~     码字码得我手酸  ~~~~(>_<)~~~~`

---

### 最后，
**tips：**
1. 所有文件的编码格式都由你当下使用的编辑器决定的！！在windows中编辑的文本放在浏览器解析显示的时候，有时乱码，有时又正常，这是由于windows中很多文本编辑器默认使用和操作系统一致的编码格式。
所以在文本存储前，一定要搞清楚我们用的是utf-8还是gbk！！！
而当你使用Python的 *open( )* 函数时，是内存中的进程与磁盘的交互，而这个交互过程中的编码格式则是使用操作系统的默认编码（Linux为utf-8，windows为gbk）
2. 相信学Python的同学们经常会听到，python3 的默认编码是utf-8。而有的时候，又有人说python3 的默认编码是unicode，那么是不是会有人跟我初学时候一样傻傻分不清楚这两者的关系呢？
    - 实际上unicode就是一个字符集，一个字符与数字一一对应的映射关系，因为它一律以2个字节编码（或者也有4个字节的，这里不讨论），所以占用空间会大一些，一般只用于内存中的编码使用。
    - 而 utf-8 是为了实现unicode 的传输和存储的。因为它可变长，存英文时候可以节省大量存储空间。传输时候也节省流量，所以更加 “ international ”~

    所以说，上述两种说法没有歧义，进程在内存中的表现是“ unicode ”的编码；当python3编译器读取磁盘上的.py文件时，是默认使用“utf-8”的；当进程中出现open(), write() 这样的存储代码时，需要与磁盘进行存储交互时，则是默认使用操作系统的默认编码。


<br>

---
###### 我也不知道如何才能成为一条 “ 华丽 ” 的分割线~~~
---
<br>
<br>

`打字、排版、整理思路花了近5个小时，若是这篇文章有帮助到你、有给你带来一些对编码的新灵感，希望可以点个赞。`
`第一次写文章，只是想得到一点点认可，哈哈哈~~~`
`比心~ ❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤❤`
`:-D`

#### 如果觉得知乎上的排版有点看不清楚，[戳这里，阅读原文噢~~](https://github.com/kerwin6182828/Learning-note/blob/master/Python/py3%E7%BC%96%E7%A0%81%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)
若还有对编码的任何问题，欢迎给我[**留言**](https://github.com/kerwin6182828/Learning-note/blob/master/Python/py3%E7%BC%96%E7%A0%81%E9%97%AE%E9%A2%98%E6%80%BB%E7%BB%93.md)~
若是我哪里写的不对，也请斧正，咱们一起探讨哦，哈哈哈~~ 
   \@>_<@~ 












<br>
<br>

![enter image description here](https://lh3.googleusercontent.com/MXyCJ9ZxCiUtVtcdsJhGy-KhIoL2WXI61mTJQ7ezNW23_JNFjXHT2kOew5hEC1vO2VTEJEPwm-U "比心")
