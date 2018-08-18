# 高中生也能看懂的 “ 梯度下降 ” 算法 （线性回归篇）
![enter image description here](https://lh3.googleusercontent.com/Ypr1Drx6BvV9ZfCEmr0tgEKlIfs4OcImtmwCe4MybiJo2ttGHbu5ISR8UqR1yYXOSwBlgIZNchOU)


### ## 文章背景：
> 鄙人最近在研究机器学习的相关算法，查阅很多专业书籍、csdn文章、知乎大神回答、youtube视频，都发现一个高度统一的 “ 问题 ”（严格意义上不能说是问题）：
>  大家在介绍算法的时候，总是喜欢陈列各种复杂的公式、各种见都没见过的符号，虽然这样在“数学上”是严谨的，看起来专业感十足，但不给详细的解释，着实给我这样的小白学习者非常不友好，带来了很多负担。。。（似乎在传达着“这是你应该知道的东西”， 于是作为文科生的我，耗尽毕生精力将“谷歌大法”发挥到极致，才对“梯度下降”算法有了一定程度的理解）
>  当我发现“梯度下降”算法也不过如此的时候，我在想：会不会有些人也和我一样是 “ 非科班出身 ”的，还惆怅于数学公式带来的“距离感”，望而生畏？

### ## 说明：
> 这篇文章将从文科生小白的视角细细说来，本着让 “ 初中生 ” 也能看懂的精神，力求使用“最直白”、“最简洁”的话术来解释！
> （可能在数学专业者看来不严谨，但我觉得对读者“亲切”、“友好” 比 “为了严谨而曲高和寡更重要”。）
> 若大神觉得我哪里表达得不到位，欢迎指正~~
> (`・ω・´)

### ## 正文开始：
既然你诚心诚意地想知道 “ 梯度下降 ” 的算法到底是什么样的，相信你应该也了解到了：**“线性回归” 是 “梯度下降” 的基础。**

那么，我自然是要先给各位童鞋介绍一下：什么是线性回归咯~~
在机器学习的现实应用领域，就是用一条直线来近似地表示“自变量” 和 “因变量” 的关系。

### # 第 ① 步

*举个例子：*
*一天， 隔壁老王用  1两  的面粉， 做了 2个 大饼。*
（这里， 面粉的数量是自变量， 大饼的数量是因变量， 将自变量作为x轴， 因变量作为y轴， 可以用1 个红点来表示 **“面粉——大饼”** 的数量关系，如下图）

![enter image description here](https://lh3.googleusercontent.com/fKWIx9iYhyjgh3Dm0F-tzbiVzIw_VqYJClhVUjKBxC6EHgkLbYKaLbddDcSWBPZDNCAG1_kKvyA)

而如果我们尝试着用一条线来描述 **“面粉——大饼”** 的数量关系，我们自然而然会想到从原点O出发， 画一条过红点的线， 如下：

![enter image description here](https://lh3.googleusercontent.com/7X_uWNsZIXg55TW3Lsb6vTnPpa_RPl8ovSM0gtS4lVRcZsNz_HWTPpweQTGjN6HFAg9Pc8vk9t4)

用一条直线来描述现实中收集到的数据——“1两面粉， 2个大饼”
你看， 这就是线性回归，简单到令人发指吧？？哈哈哈~
### # 第 ② 步
住在我家隔壁的不仅仅有老王，还有小王：
*第二天， 隔壁小王用 2两 的面粉， 做了1 个大饼。*
此时，我们连线后，有下图：

![enter image description here](https://lh3.googleusercontent.com/uPW5AXWSr-7CPxEgNecwIPyeveqkKhdHumTES29fOZcC1YAXrw6QRldRkVTEG7vEPDa-y8xwuYo)

既然画了两条线，那么我们到底取哪条线，才能更恰当地描述 **“面粉——大饼”** 的数量关系呢？

好像这两条线都不合适吧，似乎中庸一点比较好，那就取它俩中间的线吧~
如下，我多画了两条线夹在中间：
![enter image description here](https://lh3.googleusercontent.com/1k6jLklSMVGvSeQ80d62hukLeEp5h9dK7ceCniSHIRNvRLGXpvSaXkDbpsJVzhGMJSxguH2smsw)

但是，他俩中间理论上可以画出无数条线，那我到底取哪条线最好？
（换句话说，就是我这条线的**斜率a**到底取多大的值最好？）
### # 第 ③ 步
下面开始介绍简单的算法：
我们可以直观地认为，当这条线离那两个红点的距离越近，那么这条线就越能够准确地描述 **“面粉——大饼”** 的数量关系。而这条线，恰恰也就是我们的目标回归线，我们可以用:
![**y{p} = a * x{a}**](https://lh3.googleusercontent.com/eX4znCng63O3h0xzsUzcT7NnIC1RNLWCaJsHiBznloerDdj3S8f184AH_JLJmISO2yoEu1QtVWc)       
 来表示这条未知的目标直线。
 
 **注意：**
 ```
  a：表示“斜率”
 下标{p}：predicted value， 表示“预测值”
 下标{a}：actual value， 表示“实际值”

（此处一定不要把a和下标{ａ}混淆了）
 ```
为了找到那条最合适的直线，我们现在的最终目标就是找到最合适的**斜率a**， 或者说**参数a**。

根据上面的定义，我们要找到同时距离那两个红点最近的直线，我们也把“每个红点到直线的距离” 叫做 **“损失”** （e）。
而所有 **“损失之和”** ，记作：**∑e**。

目标：找到一个 **参数a** ，使得 **∑e** 最小。
（换句话说就是：通过算出 **∑e** 的最小值，找到我们需要的 **参数a** ）
（ **∑e** 的值越小越好，巴不得它等于0， 这样的话算出的 **参数a** 就是刚好同时经过那两个红点）

**注：**
```
e:error， 表示“损失”

数学上，∑ 是求和符号。即：把每个红点的 e 相加求和。
```
而对于 e 的表示，我们有以下几个思路：
#### # 思路 1：

![enter image description here](https://lh3.googleusercontent.com/vrxeVb9un5b2fcuTLWCup9TFlgOhYh2LH7b27J6Z925GBIFQ5_QxjCKNhQ4fSj7dYTaPfKy7sVA)（实际值 - 预测值）


把两个小红点代入上式，计算如下：

![enter image description here](https://lh3.googleusercontent.com/pA5EIxBE2oapGmkkNH2g2rInPgyF39bjF9ILcTqvqIl1hzQtn0uMiBIEJgVknGAJKO-otyEzssA)


为了使**∑e** 的值尽可能的小，那我们岂不是a取无穷大就好了吗？
但这明显不符合常理， 因为“距离”在数学中要用绝对值来表示，所以推翻思路1。
#### # 思路 2：

![enter image description here](https://lh3.googleusercontent.com/H66DsJ_PkJ7hixDgqQKiOAWSv5Fnbb5xX9P-NA3ij6XZkJNBcdlWni9UgJ_j5PdM1wg-prUc7sg)（实际值 - 预测值）


把两个小红点代入上式，计算如下：

![enter image description here](https://lh3.googleusercontent.com/tKn_PkpmxnI2njVg1KeDq09ufXgId2fvm5UleZFxikBKtn9iWjyL3T2r30mssxtQqsx52JhFGmI)


此时，我们需要分类讨论：
```
①  当 2-a >= 0 且 1-2a >= 0 ， 即 a <= 1/2时， ∑e = -3a + 3  

→→→  此时，a = 1/2 时， ∑e最小，为 3/2；
```

```
②  当 2-a >= 0 且 1-2a <= 0 ， 即 1/2 <= a <= 2时， ∑e = a + 1  

→→→  此时，a = 1/2 时， ∑e最小，为 3/2；
```

```
③  当 a >= 2 ， ∑e = 3a - 3  

→→→  此时，a = 2 时， ∑e最小，为 3；
```

**综上，** a = 1/2时， ∑e 最小为 3/2 .
但是，我想大家也觉这样的分类讨论太麻烦了，在数学上不好处理~
#### # 思路 3：
既然绝对值在数学上不好处理，我们发现有个东西可以结合前面思路的优点，那就是 **“平方和”** 。

于是乎：![enter image description here](https://lh3.googleusercontent.com/C1gyLP2giiPbCxeNPXqKi6AoLjIzkCTacu4eBiUO55z7OA0XTlrynp_8oInTbvD_sEq0A_x_5zs)（实际值 - 预测值）


我们可以简单地认为，以这种形式去计算我们的 **损失函数** 是比较合理的！

计算如下：（把俩小红点代入下式）

![enter image description here](https://lh3.googleusercontent.com/8sVaAbXBvbv15vjTktuZQbALpXcut59zKfb5YC9zp_JyL38nCnNLnWQ-dGEjoCh4vL8XpdxyfwI)


对上述损失函数进行求导，得到:


![enter image description here](https://lh3.googleusercontent.com/3WcaOn6BWdtFQxGkkEwh1E99vQHUYW3iRqrfE-k06N6s6w2QfhLon8xm3Bzb2J0S0oT1BIAhc-Ed)


（导函数上的每一个点，代表着原函数每一个点上的斜率）
（高中就有学求导啦，应该不需要我过多解释吧？哈哈）

如下图：
![enter image description here](https://lh3.googleusercontent.com/JyOcIEwNlzDgxZ_i4jdhToeNyAo8xTRheeoiE8HO39qg7c98K3A6XuHxNZlazqI1DSX3BnENBFa6)

蓝曲线：原损失函数
红直线：导函数
（导函数的值越接近0， 损失函数的斜率越平，当它躺平的时候，我们可以说此时就是我们要找的损失函数的最小值点）

**综上，** 解：10a -8 = 0，a = 8/10 时， 导数为0， 斜率为0， ∑e 为5/9。

聪明如你， 你肯定发现了：此思路下的 a的取值 与 “思路2” 中的 a的取值 不一样。
这是因为，“平方” 有放大、缩小的功能。对偏差值大的数，平方后更大了；而偏差值小的数， 平方后显得更没啥偏差了........

我们平时更常使用的正是 “ 思路3 ” 中对 **损失函数e** 的表示方法。

我认为优点有二：
① 可导，方便计算。
② 不会为了得到最小的损失函数，而一味地贴近某一点，平方在这里就显得更具备全局观，似乎也有助于避免[过拟合](https://baike.baidu.com/item/%E8%BF%87%E6%8B%9F%E5%90%88/3359778?fr=aladdin)。



### # 第 ④ 步
实际上，住在我家隔壁的远不止老王、小王，还有小吕、小布 等等......：

*第三天， 隔壁小吕用 3两 的面粉， 做了 3个大饼。*
*第四天， 隔壁小布用 4两 的面粉， 做了 3个大饼。*
*第五天， 隔壁小孙用 5两 的面粉， 做了 6个大饼。*
*第六天， 隔壁小艺用 6两 的面粉， 做了 8个大饼。*
*第七天， 隔壁小珍用 8两 的面粉， 做了 7个大饼。*

此时，我们也依旧用红点来表示“面粉——大饼”的“自变量——因变量”组合，有下图：

![enter image description here](https://lh3.googleusercontent.com/wsK4ykVNnt88qfyWTzAoEBCmt-IIa1rTxgGZFF9vkod2fffIMa2yzGep5qEcLOxMMUPggaWZV7E)

上图总共7个红点，我们依旧希望用一条直线来做回归，使损失函数**∑e**的值最小。
即：最合适的直线，来描述 **“面粉——大饼”** 的数量关系。

依据 “ 第 ③ 步 ” 中“ 思路 3 ” 所说，我们的 **e** 首先选择：
![enter image description here](https://lh3.googleusercontent.com/C1gyLP2giiPbCxeNPXqKi6AoLjIzkCTacu4eBiUO55z7OA0XTlrynp_8oInTbvD_sEq0A_x_5zs)


将上面的7个红点的坐标，分别代入上面的 **e** 中，得到**损失函数 ∑e**的表达式：**∑e** = 155a^2 - 318a + 172；
它的导函数为：310a - 318

所以， 当导函数为0，即 a = 318/310 时， ∑e 约等于8.90。
于是，在此案例中， 最佳的回归拟合直线就是：
y = 318/310 * x 


**至此，** 回过头看一下，我们已经一步步演示了线性回归当中的一元线性回归。
（" 一元 "：指只有一个自变量“面粉”，一个因变量“大饼”）
而在一元线性回归中， 我们又分步骤讲解了“单样本”、“双样本”、“多样本” 的情况。
（" 样本 "：指邻居的个数，“老王”、“小王”、“ 小吕 ”.......）

好了，看到这里，你已经大体明白了什么是“ 线性回归 ”。但是鼠标滚轮拨回文章的第①步，我说过：尝试着用一条线来描述 **“面粉——大饼”** 的数量关系。但是，为什么一定要从原点出发呢？我可以任意画一条直线啊！！
这就意味着，我直接把那两个小红点连成一条直线不就好了嘛，这样不是更能描述两个红点的数量关系吗？！！

### # 第 ⑤ 步
现在，我们把直线的函数表示形式从`y = a*x` 升级成`y = a*x + b`的形式。
通过参数“ b ” 的调整，我们可以在XOY这个平面画出任意的直线。
（此函数拥有了 a , b 这两个参数，但还是算一元函数，因为一元指的是自变量的个数，还是只有一个x。参数b可以理解为常数1的系数。）

接下来，我想要引入“ 模型 ” 的概念。
这个两个函数的不同表达形式，就可以说成是两个数学模型，表示你认为用什么模样、什么类型的直线去描述 **“面粉——大饼”** 的数量关系 是最合理的。

**注：**
``` 
我们现在讨论的是直线，当然还可以用曲线模型，以后再作深入探讨。
比如： y = x^2  或者  y = x^3 + bx^2 等等，你想到更天马行空都可以。
（但你要有依据，比如通过xxx东西让你觉得xxx模型更好....）
```
依旧根据 “ 第 ③ 步 ” 中 “ 思路 3 ” 所说，我们的 **e** 首先选择：
![enter image description here](https://lh3.googleusercontent.com/C1gyLP2giiPbCxeNPXqKi6AoLjIzkCTacu4eBiUO55z7OA0XTlrynp_8oInTbvD_sEq0A_x_5zs)


把模型“ y = a*x + b ” 代入上面 **e** 表示的损失函数中，可以得到：
![enter image description here](https://lh3.googleusercontent.com/AUKidDxHg2QL0VpbK7XeTCLCeN0oMW47jxzsmnS09wgR_49BU8mNS2-T4R866a3L7gnRIhykrEEz)
`这里的y和x都是实际的小红点，真正要求的未知数是a、b 这两个家伙。`


把两个小红点代入上式，计算如下：
![enter image description here](https://lh3.googleusercontent.com/kkoJ6V9wwLzOAHIpjvSi8Cn7MYsX0b0Z2Byd2D8JY2TR5MBqWVQclarSDgDg5OJ6UGuybKS-0YEr)


我们可以直观地看到，将求和函数中的两个“二项式”展开、合并同类项后，我们惊喜地发现“未知数”项 从2个 变成了 5个，这让我咋整呢？
一元二次函数求个导，再等于一下0，就算出 **e** 的最小值了，但上面这个二元二次六项式咋求解最小值呢？
（哈哈哈，被标题“忽悠”的高中生会不会哭晕在厕所呢）

当然，我们可以用一个比较简单的思维：
先确定好一个未知数的值，然后求解另一个未知数。

![enter image description here](https://lh3.googleusercontent.com/PcTMTHt8PKRudTx12eQ2K7EIj8yRxnAXU42_p9vyXEpeNHpBuq6Z1ve0dBfKPNnEwwEpfS11xwYW)

举几种情况：
(大家也可以跟着我一起动笔写一写，加深理解~)
```
当 b = 0 时， ∑e = 5a^2 - 8a + 5;  易得：a = 0.8时，∑e有最小值= 1.8 。
当 b = 1 时， ∑e = 5a^2 - 2a + 1;  易得：a = 0.2时，∑e有最小值= 0.8 。
当 b = 2 时， ∑e = 5a^2 + 4a + 1;  易得：a = -0.4时，∑e有最小值= 0.2 。
当 b = 3 时， ∑e = 5a^2 + 10a + 5;  易得：a = -1时，∑e有最小值= 0 。
当 b = 4 时， ∑e = 5a^2 + 16a + 13;  易得：a = -1.6时，∑e有最小值= 0.2 。
当 b = 5 时， ∑e = 5a^2 + 22a + 25;  易得：a = -2.2时，∑e有最小值= 0.8 。
```
哟 ，真巧 ~ 才列举了5种情况，就发现了 “ **∑e** = 0 ”的情况了。
即：当 a = -1，  b = 3时，损失函数存在最小值，且损失为0，也就是“完美拟合”，没有误差 。
再把 a = -1，  b = 3 代入 之前的 “ y = a*x + b ” 模型 中可得最终的模型：
 **y = -a + 3** 
 （以上列举的只是一种思路，不可能真的让你穷举所有情况，会死人的）
 （当然，你要是会编程的话，穷举也不是不可以，一个循环就搞定了，让机器帮你做这蠢事~~）


你会发现， **y = -a + 3** 就是那两个红点的连线啊，那干嘛这么费事？
因为我这里只给了老王、小王两个“样本”，如果把后面的“小吕”、“小布” 都画上，你不可能再用直接连线的方法了吧？
再如果说，上面的例子没有那么巧合，可能你手动列举很多种情况都没有找到最优解。
这时候，使用上面的损失函数，并且用科学的数学算法就显得异常重要。
（** 偏导、最小二乘、梯度下降）






**注：**
```
刚才说的一元函数，是指我们的 “假设模型”。
（a、b为参数，函数的值为“预测的大饼数量”）

而后面说的二元函数，是指把模型代入损失函数后的函数 
（此时，a、b变为未知数，函数的值为“所有损失值e的和”）

既然有两个未知数，那么这个坐标系就从
{X轴表示“未知数”，Y表示“损失值 e ”} 
变成了 →→→→→→→ 
{ X轴、Y轴的两个“未知数”共同来表示一个“样本”，而用Z轴来表示样本的“损失值 e”}

也就是说，我们现在需要一个三维的坐标系，来表示上面的二元函数。
而连接z轴上的所有点，就会织成一个“面”。
即：“ ∑e ” 现在是一个“面”，而不是“线”。
（为了更生动地描述这个具有“立体感”的二元函数，我可能会再写一篇文章，用图形来说明）

```
<br>

![enter image description here](https://lh3.googleusercontent.com/fIjf24Zry3CQD-UaCGQIs642SA6PC4YcvonsEqU5LTqCGWDqYaX-e1HMqsaEo50vLMDEnclCNL7_)


<br>

---
---
<br>

### ## 尾声：
> 本来想把上面二元函数的正规求解过程介绍一下的，但是担心文章太长，大家会懒得看，于是决定把“偏导”、“梯度下降”、“最小二乘”、“曲线模型”等内容放到后面的文章~~（我会用更多的“图形”来介绍，让大家更容易理解“高维”的概念）
> 
> 如果发现文章哪里出现问题，请告诉我，我会第一时间更改！！
> 如果觉得这篇文章给了你一定的启发，请顺手点个赞，让我知道这篇文章对大家是否有帮助！！
> 如果大家还有兴趣进一步了解的话，也请留言告诉我，我会加快速度编写下一篇！！
> 
> 
> 写文章的目的很纯粹，把自己所学与各位分享，是对是错可以一起交流~
>  
>  致敬，程序员的伟大革命。

<br>

**附：**


<br>

![enter image description here](https://lh3.googleusercontent.com/LqQkNQCcKvsEEHuJI10QASVNA4gS2v1D9iYOxHFNzJG_S9iuPdsPI46543qrv-ymAVo1by8sZCMt)



