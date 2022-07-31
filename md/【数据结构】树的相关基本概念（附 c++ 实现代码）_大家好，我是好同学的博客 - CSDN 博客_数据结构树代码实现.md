> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/weixin_50941083/article/details/124385293)

> 介绍了数的基础知识，并对其使用C++程序进行了表示

### 文章目录

*   [1. 什么是树？](#1__6)
*   [2. 相关概念](#2__16)
*   *   [2.1 节点分类](#21__18)
    *   [2.2 度](#22__27)
    *   [2.3 层](#23__32)
    *   [2.4 深度和高度](#24__35)
    *   [2.5 树的分类](#25__46)
    *   [2.6 完全二叉树](#26__55)
    *   [2.7 满二叉树](#27__64)
*   [3. 二叉树性质](#3__69)
*   [4. 多叉树的表示](#4__86)
*   *   [4.1 双亲表示法](#41__92)
    *   [4.2 孩子表示法](#42__109)
    *   [4.5 双亲孩子表示法](#45__125)
    *   [4.4 孩子兄弟表示法](#44__141)
    *   [C++ 代码实现双亲表示法](#C_156)
*   [5. 二叉树表示法](#5__206)
*   *   [5.1 顺序表表示](#51__207)
    *   [5.2 链式存储](#52__226)

1. 什么是树？
========

树 (Tree）是 n（n>=0) 个结点的有限集。n=0 时称为空树。在任意一棵非空树中满足以下两个要求：

1.  有且仅有一个特定的称为根（root）的结点。如图 1 所示即不是一棵树。
2.  当 n>1 时，其余结点的子节点互不相交。如图 2 所示即不是一棵树。

![](https://img-blog.csdnimg.cn/a8dbc4c6d53744178dc4871f0607a548.png#pic_center)

![](https://img-blog.csdnimg.cn/234099648e054e64b470596cb5ca0ca1.png#pic_center)

2. 相关概念
=======

2.1 节点分类
--------

1.  根节点：没有父节点只有子节点的节点
2.  叶子节点：没有子节点或者子节点是空的节点
3.  双亲节点（父节点）：若一个节点含有子节点，则这个节点称为其子节点的父节点。
4.  孩子节点或子节点 (Child)：一个节点含有的子树的根节点称为该节点的子节点。
5.  兄弟节点 (Sibling)：具有相同父节点的节点互称为兄弟节点。
6.  节点的祖先：从根到该节点所经分支上的所有节点；
7.  节点的子孙：以某节点为根的子树中任一节点都称为该节点的子孙。

2.2 度
-----

度的概念可以根据针对树还是节点分别进行理解：

1.  节点的度数：节点孩子的个数
2.  树的度：树中节点的度最大值。

2.3 层
-----

我们从根开始定义起，根为第 1 层，根的子节点为第 2 层，以此类推将树进行分层。如下图：  
![](https://img-blog.csdnimg.cn/f7683ec966cb40f7957eca7f21b8e60c.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_8,color_FFFFFF,t_70,g_se,x_16#pic_center)

2.4 深度和高度
---------

在不同的书籍中对于这部分的定义各不相同，我们理解以下这两个定义的含义即可。

> 对于节点而言
> 
> 1.  节点的高度：节点到最远叶子节点的最长路径上边的数量。叶子节点高度为 0。
> 2.  节点的深度：节点到根节点的路径上边的数量。所有根节点深度为 0。

> 对于树而言
> 
> 1.  树的高度：树的高度等于根节点的高度，等于最远叶子节点的深度。
> 2.  树的深度：树的深度等于树的高度。
> 3.  树的宽度：两个最长路径的叶子节点之间节点数。

2.5 树的分类
--------

我们根据**子节点的数量**可以将树分为二叉树和多叉树，就如定义，二叉树即是指该树上的任何一个节点最多有两个字节点。具体定义如下：

1.  二叉树是 n(n>=0) 个结点的有限集合, 该集合或者为空集 (称为空二叉树), 每个节点最多有 2 个子节点, 分别称为根结点的左子树和右子树。
2.  多叉树是 n(n>=0) 个结点的有限集合, 该集合或者为空集 (称为空树)。每个节点有多个子节点。

除此以外我们还可以根据**树是否有序**将树分为有序树和无序树。具体概念如下：  
3. 有序树：节点各子树从左到右有次序（不能互换）。  
4. 无序树：节点各子树从左到右无次序（能互换）。

2.6 完全二叉树
---------

完全二叉树 (Complete Binary Tree)： 每层结点都完全填满，在最后一层上如果不是满的，则只缺少右边的若干结点。如下图所示：  
![](https://img-blog.csdnimg.cn/498620111b9e4df2bf2886f92bbb114b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_8,color_FFFFFF,t_70,g_se,x_16#pic_center)

当断续的缺失一个节点时就不再是一棵完全二叉树，如下图所示：  
![](https://img-blog.csdnimg.cn/ebe175e80778424aaf967df99dab3439.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_8,color_FFFFFF,t_70,g_se,x_16#pic_center)

2.7 满二叉树
--------

满二叉树 (Full Binary Tree)：二叉树中的每个结点恰好有两个孩子结点且所有叶子结点都在同一层。如下图所示：  
![](https://img-blog.csdnimg.cn/f6bf3ae5c8e94e4fa929287097b8cea9.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_10,color_FFFFFF,t_70,g_se,x_16#pic_center)

3. 二叉树性质
========

> 二叉树满足的性质

1.  在非空二叉树上，第 i 层至多有 2^{i-1} 个结点。
2.  深度为 k 的二叉树至多有 2^k-1 个结点；
3.  对任何一个二叉树，若其叶子结点数为 n_0，度为 2 的节点数为 n_2，则 n_0=n_2+1；

> 满二叉树的性质

1.  深度为 k 的满二叉树且有 2^k-1 个结点。

> 完全二叉树的性质

1.  n 个结点的完全二叉树的深度 k=[log2 n]+1, 这里这个符号 [x] 表示小于等于 x 的整数;
2.  若对一棵有 n 个结点的完全二叉树 (深度为[log2 n]+1) 的结点按层（从第 1 层到第 [log2 n]+1 层) 序自左至右进行编号, 则对于编号为 i（1≦i≦n)的结点：

> *   如果 i=1，则结点 i 是二叉树的根，无双亲结点；如果 i>1，则其双亲结点编号是 [i/2]。
> *   如果 2i>n：则结点 i 为叶子结点，无左孩子；否则，其左孩子结点编号是 2i。  
>     如果 2i+1>n：则结点 i 无右孩子；否则，其右孩子结点编号是 2i+1。

4. 多叉树的表示
=========

我们给出如下图的树：  
![](https://img-blog.csdnimg.cn/bfae4c9ba3a0457dad1955ae6eb88ba4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_8,color_FFFFFF,t_70,g_se,x_16#pic_center)  
然后我们使用如下所提出的表示法进行表示。

4.1 双亲表示法
---------

使用双亲表示法中的节点信息为：

```
struct Node{
   char data;
   int parent; 
};

Node nodes[n];
```

使用双亲表示法比较容易找到双亲，但是不容易找到孩子。利用该方法得出表格：  
![](https://img-blog.csdnimg.cn/46a3a1d86e644d4386c38cef6501566e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_6,color_FFFFFF,t_70,g_se,x_16#pic_center)

4.2 孩子表示法
---------

使用孩子表示法中的节点信息为：

```
struct Node{
   char data;
   vector<int> children; 
};

Node nodes[n];
```

这种方式比较容易找到孩子，但是不容易找到双亲，我们可以得到如下图表  
![](https://img-blog.csdnimg.cn/9b3f7f2f1fd64af19d1d7dfdf8fdc407.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_11,color_FFFFFF,t_70,g_se,x_16#pic_center)

4.5 双亲孩子表示法
-----------

这种方法节点中的信息如下：

```
struct Node{
   char data;
   int parent; 
   vector<int> children; 
};

Node nodes[n];
```

使用这种方式我们很容易找到双亲和孩子，但是显然这种方式所占用的内存空间也更加大。我们可以得到如下的表格：  
![](https://img-blog.csdnimg.cn/c5c57d6a5cd64a6190f204d519990ea1.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_13,color_FFFFFF,t_70,g_se,x_16#pic_center)

4.4 孩子兄弟表示法
-----------

节点信息如下：

```
struct Node{
   char data;
   int firstchild;
   int rightsib; 
};

Node nodes[n];
```

这种方式显然比较容易找到孩子和兄弟，但是不容易找到双亲。  
![](https://img-blog.csdnimg.cn/ae6c607f97c841a7b34bb9e0cc624bed.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_8,color_FFFFFF,t_70,g_se,x_16#pic_center)

C++ 代码实现双亲表示法
-------------

```
#include <iostream>
#include <vector>
using namespace std;
// 打印多叉树
// 多叉树的表示

class Tree{
struct Node{
    char data;
    int parent;
    Node(char data, int parent):data(data),parent(parent){}
};

vector<Node> tree;
public:
        int Add(char data, int parent){
            tree.emplace_back(data, parent);// 直接进行移动，避免了拷贝>构造
            return tree.size()-1;
        }
        void Print() const{
            for(auto node:tree){
                if(node.parent!=-1){
                    cout << tree[node.parent].data << "->" << node.data << endl;
                }
            }
        }
};

int main(){
    Tree tree;
    int a = tree.Add('A', -1);
    int b = tree.Add('B', a);
    int c = tree.Add('C', b);
    int d = tree.Add('D', b);
    int e = tree.Add('E', c);
    int f = tree.Add('F', c);
    tree.Add('G', d);
    tree.Add('H', d);
    tree.Add('I', d);
    tree.Add('J', e);

    tree.Print();
}
```

最好不写class,只写struct就可以 //node (char data....... 这一段是用来初始化的（先记住

实现后得到如下结果：  
![](https://img-blog.csdnimg.cn/57842a34537a4760a09699e8eaff4b52.png#pic_center)

5. 二叉树表示法
=========

5.1 顺序表表示
---------

相比较以上多叉树的表示方法，我们其是更常用顺序表表示二叉树，这也是二叉树特有的表示方式，因为二叉树节点之间的关系如下：

<table><thead><tr><th>节点</th><th>下标</th></tr></thead><tbody><tr><td>根节点</td><td>1</td></tr><tr><td>叶子节点 i</td><td>2i &gt; 2^k-1</td></tr><tr><td>节点 i 的父节点</td><td>i/2</td></tr><tr><td>节点 i 的左孩子节点</td><td>2i</td></tr><tr><td>节点 i 的右孩子节点</td><td>2i+1</td></tr></tbody></table>

故而当我们有如下的二叉树：  
![](https://img-blog.csdnimg.cn/7619b6fc7d664e40beaa879eb89bb140.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5aSn5a625aW977yM5oiR5piv5aW95ZCM5a2m,size_12,color_FFFFFF,t_70,g_se,x_16#pic_center)  
我们可以使用如下顺序表表示这个树：  
![](https://img-blog.csdnimg.cn/6b6bd49d45914d328d7dd4a84ef3cbd3.png#pic_center)  
相关实现代码如下：

5.2 链式存储
--------

二叉树比较常见的存储方式是链式存储方式，代码实现如下：

```
#include <iostream>
#include <vector>
using namespace std;
// 打印多叉树
// 多叉树的表示

class Tree{
struct Node{
	char data;
	Node* left = nullptr;
	Node* right = nullptr;
	Node(char data):data(data){}
	friend ostream& operator<<(ostream& os, const Node* node){
		if(nullptr != node.left){
			os << node.data << "->" << node.left->data << endl;
			os << *node.left;
		}
		if(nullptr != node.right){
			os << node.data << "->" << node.right->data << endl;
			os << *node.right;
		}
		return os;
	}
};

Node* root = nullptr;
public:
	Node* AddLeft(char data, Node* parent){
		Node* node = new Node(data);
		if(Null == parent) root = node;
		else parent->left = node;
		return node;
	}
	Node* AddRight(char data, Node* parent){
		Node* node = new Node(data);
		if(Null == parent) root = node;
		else parent->right = node;
		return node;
	}
	void Print()const{
		cout << *root << endl;
	}
};

int main(){
    Tree tree;
    auto root = tree.AddLeft('A', nullptr);
    auto b = tree.AddLeft('B', root);
    auto c = tree.AddRight('C',root);
    auto d = tree.AddLeft('E', b);
    auto e = tree.AddRight('J',e);
    auto f = tree.AddLeft('F', c);
    auto g = tree.AddRight('G',c);
    tree.Print();
}
```