> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_38410730/article/details/81667885)

> 问题描述有n个物品，它们有各自的体积和价值，现有给定容量的背包，如何让背包里装入的物品具有最大的价值总和？为方便讲解和理解，下面讲述的例子均先用具体的数字代入，即：eg：number＝4，capacity＝8i（物品编号） 1 2 3 4 w（体积） 2 3 4 5 v（价值） 3 4 5 6 总...

问题描述
----

**有 n 个物品，它们有各自的体积和价值，现有给定容量的背包，如何让背包里装入的物品具有最大的价值总和？**

为方便讲解和理解，下面讲述的例子均先用具体的数字代入，即：eg：number＝4，capacity＝8

<table align="center" border="1" cellpadding="1" cellspacing="1"><tbody><tr><td>i（物品编号）</td><td>1</td><td>2</td><td>3</td><td>4</td></tr><tr><td>w（体积）</td><td>2</td><td>3</td><td>4</td><td>5</td></tr><tr><td>v（价值）</td><td>3</td><td>4</td><td>5</td><td>6</td></tr></tbody></table>

总体思路
----

根据[动态规划](https://so.csdn.net/so/search?q=%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92&spm=1001.2101.3001.7020)解题步骤（问题抽象化、建立模型、寻找约束条件、判断是否满足最优性原理、找大问题与小问题的递推关系式、填表、寻找解组成）找出 01 背包问题的最优解以及解组成，然后编写代码实现。

### 动态规划的原理

动态规划与分治法类似，都是把大问题拆分成小问题，通过寻找大问题与小问题的递推关系，解决一个个小问题，最终达到解决原问题的效果。但不同的是，分治法在子问题和子子问题等上被重复计算了很多次，而动态规划则具有记忆性，**通过填写表把所有已经解决的子问题答案纪录下来，在新问题里需要用到的子问题可以直接提取，避免了重复计算，从而节约了时间，所以在问题满足最优性原理之后，用动态规划解决问题的核心就在于填表，表填写完毕，最优解也就找到。**

最优性原理是动态规划的基础，最优性原理是指 “多阶段决策过程的最优决策序列具有这样的性质：**不论初始状态和初始决策如何，对于前面决策所造成的某一状态而言，其后各阶段的决策序列必须构成最优策略**”。

### 背包问题的解决过程

在解决问题之前，为描述方便，首先定义一些变量：**Vi 表示第 i 个物品的价值，Wi 表示第 i 个物品的体积，定义 V(i,j)：当前背包容量 j，前 i 个物品最佳组合对应的价值，**同时背包问题抽象化（X1，X2，…，Xn，其中 Xi 取 0 或 1，表示第 i 个物品选或不选）。

1、建立模型，即求 max(V1X1+V2X2+…+VnXn)；

2、寻找约束条件，W1X1+W2X2+…+WnXn<capacity；

3、**寻找递推关系式**，面对当前商品有两种可能性：

*   **包的容量比该商品体积小，装不下，此时的价值与前 i-1 个的价值是一样的，即 V(i,j)=V(i-1,j)；**
*   **还有足够的容量可以装该商品，但装了也不一定达到当前最优价值，所以在装与不装之间选择最优的一个，即 V(i,j)=max｛V(i-1,j)，V(i-1,j-w(i))+v(i)｝。**

其中 V(i-1,j) 表示不装，V(i-1,j-w(i))+v(i) 表示装了第 i 个商品，背包容量减少 w(i)，但价值增加了 v(i)；

由此可以得出递推关系式：

*   j<w(i)      V(i,j)=V(i-1,j)
*   j>=w(i)     V(i,j)=max｛V(i-1,j)，V(i-1,j-w(i))+v(i)｝

这里需要解释一下，为什么能装的情况下，需要这样求解（这才是本问题的关键所在！）：

**可以这么理解，如果要到达 V(i,j) 这一个状态有几种方式？**

**肯定是两种，第一种是第 i 件商品没有装进去，第二种是第 i 件商品装进去了。**没有装进去很好理解，就是 V(i-1,j)；装进去了怎么理解呢？如果装进去第 i 件商品，那么装入之前是什么状态，肯定是 V(i-1,j-w(i))。由于最优性原理（上文讲到），V(i-1,j-w(i)) 就是前面决策造成的一种状态，后面的决策就要构成最优策略。两种情况进行比较，得出最优。

4、填表，首先初始化边界条件，V(0,j)=V(i,0)=0V(0,j)为第零个物品（即没有没有物品）选中，所以自然没有价值和重量; V(i,0)第i个物品，但是背包没有容量了，自然为零；

![](https://img-blog.csdn.net/20180814153341616?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

然后一行一行的填表：

*   如，i=1，j=1，w(1)=2，v(1)=3，有 j<w(1)，故 V(1,1)=V(1-1,1)=0；
*   又如 i=1，j=2，w(1)=2，v(1)=3，有 j=w(1), 故 V(1,2)=max｛ V(1-1,2)，V(1-1,2-w(1))+v(1) ｝=max｛0，0+3｝=3；
*   如此下去，填到最后一个，i=4，j=8，w(4)=5，v(4)=6，有 j>w(4)，故 V(4,8)=max｛ V(4-1,8)，V(4-1,8-w(4))+v(4) ｝=max｛9，4+6｝=10……

所以填完表如下图：

![](https://img-blog.csdn.net/20180814153424308?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

5、表格填完，最优解即是 V(number,capacity)=V(4,8)=10。

代码实现
----

为了和之前的动态规划图可以进行对比，尽管只有 4 个商品，但是我们创建的数组元素由 5 个。

```
#include<iostream>
using namespace std;
#include <algorithm>
 
int main()
{
	int w[5] = { 0 , 2 , 3 , 4 , 5 };			//商品的体积2、3、4、5
	int v[5] = { 0 , 3 , 4 , 5 , 6 };			//商品的价值3、4、5、6
	int bagV = 8;					        //背包大小
	int dp[5][9] = { { 0 } };			        //动态规划表
 
	for (int i = 1; i <= 4; i++) {
		for (int j = 1; j <= bagV; j++) {
			if (j < w[i])
				dp[i][j] = dp[i - 1][j];
			else
				dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
		}
	}
 
	//动态规划表的输出
	for (int i = 0; i < 5; i++) {
		for (int j = 0; j < 9; j++) {
			cout << dp[i][j] << ' ';
		}
		cout << endl;
	}
 
	return 0;
}
```

背包问题最优解回溯
---------

通过上面的方法可以求出背包问题的最优解，但还不知道这个最优解由哪些商品组成，故要根据最优解回溯找出解的组成，根据填表的原理可以有如下的寻解方式：

*   **V(i,j)=V(i-1,j) 时，说明没有选择第 i 个商品，则回到 V(i-1,j)；**
*   **V(i,j)=V(i-1,j-w(i))+v(i) 时，说明装了第 i 个商品，该商品是最优解组成的一部分，随后我们得回到装该商品之前，即回到 V(i-1,j-w(i))；**
*   **一直遍历到 i＝0 结束为止，所有解的组成都会找到。**

就拿上面的例子来说吧：

*   最优解为 V(4,8)=10，而 V(4,8)!=V(3,8) 却有 V(4,8)=V(3,8-w(4))+v(4)=V(3,3)+6=4+6=10，所以第 4 件商品被选中，并且回到 V(3,8-w(4))=V(3,3)；
*   有 V(3,3)=V(2,3)=4，所以第 3 件商品没被选择，回到 V(2,3)；
*   而 V(2,3)!=V(1,3) 却有 V(2,3)=V(1,3-w(2))+v(2)=V(1,0)+4=0+4=4，所以第 2 件商品被选中，并且回到 V(1,3-w(2))=V(1,0)；
*   有 V(1,0)=V(0,0)=0，所以第 1 件商品没被选择。

![](https://img-blog.csdn.net/20180814160731737?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

代码实现
----

背包问题最终版详细代码实现如下：

```
#include<iostream>
using namespace std;
#include <algorithm>
 
int w[5] = { 0 , 2 , 3 , 4 , 5 };			//商品的体积2、3、4、5
int v[5] = { 0 , 3 , 4 , 5 , 6 };			//商品的价值3、4、5、6
int bagV = 8;					        //背包大小
int dp[5][9] = { { 0 } };			        //动态规划表
int item[5];					        //最优解情况
 
void findMax() {					//动态规划
	for (int i = 1; i <= 4; i++) {
		for (int j = 1; j <= bagV; j++) {
			if (j < w[i])
				dp[i][j] = dp[i - 1][j];
			else
				dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - w[i]] + v[i]);
		}
	}
}
 
void findWhat(int i, int j) {				//最优解情况
	if (i >= 0) {
		if (dp[i][j] == dp[i - 1][j]) {
			item[i] = 0;
			findWhat(i - 1, j);
		}
		else if (j - w[i] >= 0 && dp[i][j] == dp[i - 1][j - w[i]] + v[i]) {
			item[i] = 1;
			findWhat(i - 1, j - w[i]);
		}
	}
}
 
void print() {
	for (int i = 0; i < 5; i++) {			//动态规划表输出
		for (int j = 0; j < 9; j++) {
			cout << dp[i][j] << ' ';
		}
		cout << endl;
	}
	cout << endl;
 
	for (int i = 0; i < 5; i++)			//最优解输出
		cout << item[i] << ' ';
	cout << endl;
}
 
int main()
{
	findMax();
	findWhat(4, 8);
	print();
 
	return 0;
}
```

![](https://img-blog.csdnimg.cn/20190205162940822.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM4NDEwNzMw,size_16,color_FFFFFF,t_70)