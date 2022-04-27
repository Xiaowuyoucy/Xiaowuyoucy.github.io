---
title: 图(graph)
date: 2022-04-08 02:03:59
tags:
categories: cpp
doc:
---

## 图的相关概念

### 图的定义和术语

图的定义：图是由**顶点的有穷非空集合**和**顶点之间的边的集合**组成，通常表示为：G = (V,E)，其中，G表示一个图，V是图G中顶点的集合，E是图G中边的集合。

图中的元素称为顶点(Vertex)

顶点必须是有穷的非空集合,因此一个图至少有一个顶点。

顶点之间的逻辑关系用边(Edge)来表示,边集可以是空的。

**无向边**：若顶点Vi 到Vj 的边没有方向，则称这条边为无向边，用无序偶对`（Vi ，Vj）`来表示。

**有向边**：若从顶点Vi 到Vj的边有方向，则称这条边为有向边，也称为弧(Arc)。用有序偶对`<Vi ，Vj>`来表示。`Vi`称为弧尾(Tail)或初始点，`Vj`称为弧头(Head)或终端点。

注意: `< Vi ，Vj >`和`< Vj ，Vi >`是两条不同的有向边。
**无向图**: 如果图中任意两个顶点之间的边都是无向边，则称该图为无向图。

图例(G1): 

![image-20220408020846032](/images/javawz/image-20220408020846032.png)



**有向图**: 如果图中任意顶点之间的边都是有向边，则称该图为有向图。
图例(G2):

![image-20220408020925632](/images/javawz/image-20220408020925632.png)



**无向完全图**：在无向图中，如果任意两个顶点之间都存在边，则称该图为无向完全图。含有n个顶点的无向完全图有`n(n-1)/2`条边。
图例(G3)

![image-20220408021013033](/images/javawz/image-20220408021013033.png)



**有向完全图**：在有向图中，如果任意两个顶点之间都存在方向互为相反的两条弧，则称该图为有向完全图。含有n个顶点的有向完全图有n(n-1)条边。

![image-20220408021041958](/images/javawz/image-20220408021041958.png)



总结: 图G的顶点数n和边数e的关系
**（1）若G是无向图，则0≤e≤n(n-1)/2**
    恰有n(n-1)/2条边的无向图称无向完全图(Undireet-ed Complete Graph)
**（2）若G是有向图，则0≤e≤n(n-1)。**
    恰有n(n-1)条边的有向图称为有向完全图(Directed Complete Graph)。
  注意：
    　完全图具有最多的边数。任意一对顶点间均有边相连。
**稀疏图**: 有很少条边或弧的图。
**稠密图**: 有很多条边或弧的图。
**权**: 有时图的边或弧具有与它相关的数,这种与图的边或弧相关的数叫做权。

![image-20220408025046325](/images/javawz/image-20220408025046325.png)

**网**：带权的图通常称为网。
**度**：顶点的度是指**和该顶点关联的边的数目**。

**无向图的度**：与这个顶点相关联的边的条数，边的数量等于各个顶点度数和的一半。

**有向图的度：**

- **入度**：有向图中以顶点（v）为头的弧的数目，称为（v）的入度。
- **出度**：有向图中以顶点（v）为尾的弧的数目，称为（v）的出度。
- **弧的数量=各个顶点出度和 = 各个顶点的入度和**

**邻接点**：对于无向图，同一边上的两个顶点称为邻接点。
**子图**: 假设两个图G=(V,E)和G1=(V1,E1),如果V1⊆V且E1⊆E则G1为G的子图
**路径的长度**: 路径上的边或弧的数目。

![image-20220408030319392](/images/javawz/image-20220408030319392.png)

上图中左侧B到D的路径长为2，右侧B到D的路径为3

###  连通图相关术语

在无向图G=(V,E)中，如果从顶点v到顶点w有路径，则称v和w是相通的。**如果对图中任意两个顶点Vi和Vj 属于E，则两个顶点是连通的，则称G是连通图。**如下图1，它的顶点A都顶点B、C、D都是连通的，但显然顶点A与顶点E或F就无路径，因此不能算是连通图。而图2，顶点A、B、C、D相互都是连通的，所以它本身是连通图。

![image-20220408021246414](/images/javawz/image-20220408021246414.png)

#### 连通图生成树

连通图的生成树**是一个极小的连通子图，它含有图中全部的n个顶点，但只有足以构成一棵树的n-1条边。**
**极小连通子图是相对于连通图来说的。**
比如下图的图1是一个普通图，但显然它不是生成树，当去掉两条构成环的边后，比如图2或图3，就满足n个顶点n-1条边且连通的定义了。它们都是一棵生成树。从这里也知道，如果一个图有n个顶点和小于n-1条边，则是非连通图，如果它多于n-1条边，必定构成一个环，因为这条边使得它依附的那两个顶点之间有了第二条路径。比如图2和图3，随便加哪两顶点的边都将构成环。**不过有n-1条边并不一定是生成树**，比如图4。

![image-20220408021408335](/images/javawz/image-20220408021408335.png)

![image-20220408021419655](/images/javawz/image-20220408021419655.png)

### 总结

图按照**有无方向**分为**无向图**和**有向图**。
无向图由**顶点**和**边**构成。
有向图由**顶点**和弧构成，弧有**弧头**和**弧尾**之分。
图按照**边或弧的多少**分为**稀疏图**和**稠密图**。
如果**任意两个顶点之间都存在边**叫**完全图**，有向的叫有向完全图。
与顶点相关联的边的条数叫做度，有向图顶点分为入度和出度。
图上的边或弧带权则称为网。

无向图中**连通**且**n个顶点n-1条边**叫生成树。



## 1.2、图的存储结构

**图**可以用**顺序存储**或链式存储

**顺序存储:邻接矩阵**

**链式存储:邻接表**

### 1.2.1邻接矩阵

![image-20220408041335770](/images/javawz/image-20220408041335770.png)

图的邻接矩阵存储方式是用两个数组来表示图。

- 一个一维数组存储图中顶点信息。
- 一个二维数组（邻接矩阵）存储图中的边或弧的信息。

设图G有n个顶点，则邻接矩阵是一个n*n的方阵，定义为：

![image-20220408021823273](/images/javawz/image-20220408021823273.png)

看一个实例，下图左就是一个**无向图**。

![image-20220408021856686](/images/javawz/image-20220408021856686.png)

从上面可以看出，**无向图的边数组是一个对称矩阵**。所谓对称矩阵就是n阶矩阵的元满足a<sub>ij</sub> = a<sub>ji</sub>  ，即从矩阵的左上角到右下角的主对角线为轴，右上角的元和左下角相对应的元全都是相等的。
    从这个矩阵中，很容易知道图中的信息。

- （1）**判断任意两顶点是否有边无边；**

- （2）**某个顶点的度，其实就是这个顶点vi在邻接矩阵中第i行或（第i列）的元素之和；**

- （3）**求顶点vi的所有邻接点就是将矩阵中第i行元素扫描一遍，**arc\[i][j]为1就是邻接点；

  

  而有向图讲究入度和出度，顶点v2的入度为2，正好是第i列各数之和。顶点v2的出度为1，即第i行的各数之和。

![image-20220408034350690](/images/javawz/image-20220408034350690.png)

若图G是**网图**，有n个顶点，则邻接矩阵是一个n*n的方阵，定义为：

![image-20220408034407319](/images/javawz/image-20220408034407319.png)

这里的w<sub>ij</sub>表示(v<sub>i</sub>,v<sub>j</sub>)上的权值。无穷大表示一个计算机允许的、大于所有边上权值的值，也就是一个不可能的极限值。下面左图就是一个有向网图，下图就是它的邻接矩阵。

![image-20220408034504461](/images/javawz/image-20220408034504461.png)



### 创建无向图和有向图代码示例

#### 无向图

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
using namespace std;

//最大的顶点数
#define MaxVertex 50 
//存放顶点名称的数组
typedef char VertexInfo[9];


// 定义图的结构
struct Graph
{
	// 顶点数组 - 存储顶点的名字  等价于  vertex[MaxVertex][9]  最多存放MaxVertex个顶点
	VertexInfo vertex[MaxVertex];
	// 边的数组  二维矩阵
	int edge[MaxVertex][MaxVertex];
	// 顶点的个数
	int vertexNum;
	// 边的条数
	int edgeNum;
};

// 求用户输入的顶点在顶点数组中的位置
int LocalVertex(Graph &g, VertexInfo v)
{
	// 遍历顶点数组
	for (int i = 0; i < g.vertexNum; ++i)
	{
		if (strcmp(v, g.vertex[i]) == 0)
		{
			// 找到了,返回元素的下标
			return i;
		}
	}
	// 没找到
	return -1;
}


// 构建一个图
void CreateGraph(Graph &g)
{
	cout << "请输入图的顶点数和边数: 顶点 边" << endl;
	cin >> g.vertexNum >> g.edgeNum;
	cout << "请输入" << g.vertexNum << "个顶点的值" << endl;
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cin >> g.vertex[i]; //初始化每个顶点
	}

	// 初始化所有边都不存在
	for (int i = 0; i < g.vertexNum; ++i)
	{
		for (int j = 0; j < g.vertexNum; ++j)
		{
			g.edge[i][j] = 0;
		}
	}
	cout << "请输入" << g.edgeNum << "条边, 顶点1 顶点2" << endl;
	VertexInfo v1, v2;
	for (int i = 0; i < g.edgeNum; ++i)
	{
		cin >> v1 >> v2;
		// 求用户输入的顶点在顶点数组中的位置
		int m = LocalVertex(g, v1); //获取v1在二维数组中的位置
		int n = LocalVertex(g, v2); //获取v2在二维数组中的位置

		// 边对应的二维数组赋值
		g.edge[m][n] = 1;
		g.edge[n][m] = 1;	// 无向图 对称关系
	}
}

// 打印图 - 
void PrintGraph(Graph& g)
{
	// 水平表头
	cout << "\t";
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cout << g.vertex[i] << "\t";
	}
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cout << endl;
		// 垂直的
		cout << g.vertex[i] << "\t";
		for (int j = 0; j < g.vertexNum; ++j)
		{
			cout << g.edge[i][j] << "\t";
		}
	}
	cout << endl;
}
//构建图
void test01()
{
	Graph graph;
	CreateGraph(graph);
	PrintGraph(graph);
}


int main(){

	test01();


	system("pause");
	return EXIT_SUCCESS;
}
```

#### 有向图

```cpp
#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
using namespace std;

#define MaxVertex 50
typedef char VertexInfo[9];

// 定义图的结构
struct Graph
{
	// 顶点数组 - 存储顶点的名字
	VertexInfo vertex[MaxVertex];
	// 边的数组
	int edge[MaxVertex][MaxVertex];
	// 顶点的个数
	int vertexNum;
	// 边的条数
	int edgeNum;
};

// 求用户输入的顶点在顶点数组中的位置
int LocalVertex(Graph &g, VertexInfo v)
{
	// 遍历顶点数组
	for (int i = 0; i < g.vertexNum; ++i)
	{
		if (strcmp(v, g.vertex[i]) == 0)
		{
			// 找到了,返回元素的下标
			return i;
		}
	}
	// 没找到
	return -1;
}

// 构建一个图
void CreateGraph(Graph &g)
{
	cout << "请输入图的顶点数和边数: 顶点 边" << endl;
	cin >> g.vertexNum >> g.edgeNum;
	cout << "请输入" << g.vertexNum << "个顶点的值" << endl;
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cin >> g.vertex[i];
	}

	// 初始化所有边都不存在
	for (int i = 0; i < g.vertexNum; ++i)
	{
		for (int j = 0; j < g.vertexNum; ++j)
		{
			g.edge[i][j] = INT_MAX;
		}
	}
	// <B, A>
	cout << "请输入" << g.edgeNum << "条边, 弧尾 弧头 权重" << endl;
	int w;
	VertexInfo v1, v2;
	for (int i = 0; i < g.edgeNum; ++i)
	{
		cin >> v1 >> v2 >> w;
		// 求用户输入的顶点在顶点数组中的位置
		int m = LocalVertex(g, v1);
		int n = LocalVertex(g, v2);

		// 边对应的二维数组赋值
		g.edge[m][n] = w;
	}
}
// 打印图 - 
void PrintGraph(Graph& g)
{
	// 水平表头
	cout << "\t";
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cout << g.vertex[i] << "\t";
	}
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cout << endl;
		// 垂直的
		cout << g.vertex[i] << "\t";
		for (int j = 0; j < g.vertexNum; ++j)
		{
			if (g.edge[i][j] == INT_MAX)
			{
				cout << "∞" << "\t";
			}
			else
			{
				cout << g.edge[i][j] << "\t";
			}
		}
	}
	cout << endl;
}

void test01()
{
	Graph g;
	CreateGraph(g);
	PrintGraph(g);

}

int main(){

	test01();

	system("pause");
	return EXIT_SUCCESS;
}
```



### 邻接表

![image-20220408220553196](/images/javawz/image-20220408220553196.png)

邻接矩阵是不错的一种图存储结构，但是，对于边数相对顶点较少的图，这种结构存在对存储空间的极大浪费。因此，找到一种数组与链表相结合的存储方法称为邻接表。

邻接表的存储方式是这样的：
（1）图中**顶点用一个一维数组存储**，当然，**顶点也可以用单链表来存储**，
不过，数组可以较容易的读取顶点的信息，更加方便。
（2）图中**每个顶点vi的所有邻接点构成一个线性表**，由于邻接点的个数不定，所以，用单链表存储，无向图称为顶点vi的边表，有向图则称为顶点vi作为弧尾的出边表。
数据结构定义:

![image-20220408220721949](/images/javawz/image-20220408220721949.png)



例如，下图就是一个无向图的邻接表的结构。

![image-20220408220750174](/images/javawz/image-20220408220750174.png)

从图中可以看出，顶点表的各个结点由data和firstedge两个域表示，data是数据域，存储顶点的信息，firstedge是指针域，指向边表的第一个结点，即此顶点的第一个邻接点。边表结点由adjvex和next两个域组成。adjvex是邻接点域，存储某顶点的邻接点在顶点表中的下标，next则存储指向边表中下一个结点的指针。



对于带权值的网图，可以在边表结点定义中再增加一个weight的数据域，存储权值信息即可。如下图所示。

![image-20220408220813779](/images/javawz/image-20220408220813779.png)

![image-20220408220827738](/images/javawz/image-20220408220827738.png)

```cpp
#include <iostream>
#include <stack>
#include <queue>
using namespace std;

#if 1
#define MaxVertex 100
// 邻接点的结构体
struct edgeNode
{
	// 当前顶点在顶点数组中的位置
	int position;
	// 指向后继节点的指针
	struct edgeNode* next;
	// 节点相关的信息 - info
	int weight;
};

// 顶点数组的结构体
struct Vertex
{
	// 顶点的名字
	char name[9];
	// 指向临接点结构体指针
	struct edgeNode* first;
};
// 邻接表图结构
struct GraphList
{
	// 顶点数组
	Vertex head[MaxVertex];
	// 顶点的个数
	int vertexNum;
	// 边的条数
	int edgeNum;
};

//获取点对应的下标
int LocalVertex(GraphList&g, char* name)
{
	for (int i = 0; i < g.vertexNum; ++i)
	{
		if (strcmp(name, g.head[i].name) == 0)
		{
			return i;
		}
	}
	return -1;	// 没找到
}

// 创建一个图
void CreateGraph(GraphList &g)
{
	cout << "请输入图的顶点数和边数: 顶点 边" << endl;
	cin >> g.vertexNum >> g.edgeNum;
	cout << "请输入" << g.vertexNum << "个顶点的值" << endl;
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cin >> g.head[i].name;
		g.head[i].first = NULL;	// 目前没有邻接点
	}

	cout << "请输入" << g.edgeNum << "条边, 顶点1 顶点2" << endl;
	char v1[9], v2[9];
	for (int i = 0; i < g.edgeNum; ++i)
	{
		cin >> v1 >> v2;
		// 以M为头结点的链表, n是m的;邻接点
		// 求用户输入的顶点在顶点数组中的位置
		int m = LocalVertex(g, v1);
		int n = LocalVertex(g, v2);

		// 链表中添加邻接点
		edgeNode* pNew = new edgeNode;
		// init pNew
		pNew->position = n;	// 当前的节点在顶点数组中的位置
		// pNew添加到头结点数组第m个元素 对应的链表中
		// 头插法  尾插法需要遍历到尾部 ，麻烦，因此用头插法
		pNew->next = g.head[m].first;
		g.head[m].first = pNew;
#if 1
		// 以N为头结点的链表, m是n的;邻接点
		edgeNode* pNew1 = new edgeNode;
		// init pNew1
		pNew1->position = m;	// 当前的节点在顶点数组中的位置
		// pNew添加到头结点数组第m个元素 对应的链表中
		// 头插法
		pNew1->next = g.head[n].first;
		g.head[n].first = pNew1;
#endif
	}
}
// 打印图
void PrintGraphList(GraphList& g)
{
	for (int i = 0; i < g.vertexNum; ++i)
	{
		edgeNode* pNode = g.head[i].first;
		cout << g.head[i].name << ": ";
		while (pNode != NULL)
		{
			int index = pNode->position;
			cout << g.head[index].name << " ,";
			pNode = pNode->next;
		}
		cout << endl;
	}
	cout << endl;
}



int main()
{
	GraphList g;
	CreateGraph(g);
	PrintGraphList(g);

	system("pause");
	return 0;
}

#endif
```



### 图的遍历

图的遍历和树的遍历类似，希望从图中某一顶点出发访遍图中其余顶点，且使每一个顶点仅被访问一次，这一过程就叫图的遍历。

对于图的遍历来说，如何避免因回路陷入死循环，就需要科学地设计遍历方案，通常有两种遍历次序方案：**深度优先遍历**和**广度优先遍历**。



#### 深度优先遍历(DFSdepth first search)

![image-20220409030209152](/images/javawz/image-20220409030209152.png)

**深度优先遍历，也有称为深度优先搜索，简称DFS**。其实，**就像是一棵树的前序遍历**。
它从图中某个结点v出发，访问此顶点，然后从v的未被访问的邻接点出发深度优先遍历图，直至图中所有和v有路径相通的顶点都被访问到。若图中尚有顶点未被访问，则另选图中一个未曾被访问的顶点作起始点，重复上述过程，直至图中的所有顶点都被访问到为止。
**深度优先搜索是通过栈来实现的。**
下图中的数字显示了深度优先搜索顶点被访问的顺序

![image-20220408221013803](/images/javawz/image-20220408221013803.png)

为了实现深度优先搜索，首先选择一个起始顶点并需要遵守三个规则：

- 如果可能，访问一个邻接的未访问顶点，标记它，并把它放入栈中。
- 当不能执行规则1时，如果栈不空，就从栈中弹出一个顶点。
- 如果不能执行规则1和规则2，就完成了整个搜索过程。

#### 邻接矩阵深度优先遍历

```cpp

// 深度优先搜索
void DFS(Graph& g)
{
	bool* visited = new bool[g.vertexNum];
	// init
	for (int i = 0; i < g.vertexNum; ++i)
	{
		visited[i] = false;
	}

	// 从顶点数组中的第一个开始访问
	stack<int> st;	// int - 顶点数组的下标
	visited[0] = true;
	cout << g.vertex[0] << " ";
	st.push(0);

	while (!st.empty())
	{
		// 遍历所有的顶点, 找邻接点 - 栈顶元素对应的邻接点
		for (int i = 0; i < g.vertexNum; ++i)
		{
			// 栈顶元素在顶点数组中的位置
			int top = st.top();
			if (!visited[i] && g.edge[top][i] > 0)
			{
				// 遍历该顶点
				visited[i] = true;
				cout << g.vertex[i] << " ";
				// 邻接点压栈
				st.push(i);
			}
		}
		// 栈顶的顶点与其余的顶点组成的边全部判断了一遍
		st.pop();
	}
	delete[] visited;
}

```

### 邻接表深度优先遍历

```cpp
// 深度优先搜索
void DFS(GraphList& g)
{
	// 保证顶点不被重复遍历
	bool* visited = new bool[g.vertexNum];
	// init
	for (int i = 0; i < g.vertexNum; ++i)
	{
		visited[i] = false;
	}
	// 从顶点数组中找一个顶点, 开始遍历 - 0
	stack<int> st;	// int - 顶点在顶点数组中的下标
	st.push(0);
	// 访问
	visited[0] = true;
	cout << g.head[0].name << " ";

	// 当栈为空, 遍历完成
	while (!st.empty())
	{
		// 顶点在顶点数组中的下标取出来
		int top = st.top();
		// 找下标对应的顶点的邻接点
		edgeNode* pNode = g.head[top].first;
		while (pNode)
		{
			// 如果节点被遍历过了
			while (pNode && visited[pNode->position])
			{
				// 指针后移
				pNode = pNode->next;
			}
			// 找到了没有被访问的
			if (pNode)
			{
				// 访问
				visited[pNode->position] = true;
				cout << g.head[pNode->position].name << " ";
				// 找新的顶点pNode->position的邻接点
				// 链表和链表直接做跳转
				pNode = g.head[pNode->position].first;
				st.push(pNode->position);
			}
		}
		st.pop();
	}
	delete[] visited;
}

```



### 广度优先遍历(BFS Breadth First Search)

广度优先遍历，又称为广度优先搜索，简称BFS。图的广度优先遍历就类**似于树的层序遍历**了。

在深度优先搜索中，算法表现得好像要尽快地远离起始点似的。相反，在广度优先搜索中，**算法好像要尽可能地靠近起始点。它首先访问起始顶点的所有邻接点，然后再访问较远的区域。它是用队列来实现的。**
下面图中的数字显示了广度优先搜索顶点被访问的顺序。

![image-20220409004002534](/images/javawz/image-20220409004002534.png)

实现广度优先搜索，也要遵守三个规则：

- 访问下一个未来访问的邻接点，这个顶点必须是当前顶点的邻接点，标记它，并把它插入到队列中。
- 如果因为已经没有未访问顶点而不能执行规则1时，那么从队列头取一个顶点，并使其成为当前顶点。
- 如果因为队列为空而不能执行规则2，则搜索结束。

#### 邻接矩阵广度优先遍历

```cpp
// 广度优先搜索
void BFS(Graph& g)
{
	// 保证顶点不被重复遍历
	bool* visited = new bool[g.vertexNum];
	// init
	for (int i = 0; i < g.vertexNum; ++i)
	{
		visited[i] = false;
	}

	// 找一个顶点, 开始访问 - 0
	queue<int> q;	// 存储顶点的下标
	visited[0] = true;
	cout << g.vertex[0] << " ";
	q.push(0);

	// 如果队列为空, 遍历完成
	while (!q.empty())
	{
		// 队头顶点的下标值拿出来
		int front = q.front();
		// 遍历所有的顶点, 找邻接点
		for (int i = 0; i < g.vertexNum; ++i)
		{
			// 如果没被访问, 并且两顶点互为邻接点
			if (!visited[i] && g.edge[front][i] > 0)
			{
				// 访问,并且入队列
				visited[i] = true;
				cout << g.vertex[i] << " ";
				q.push(i);
			}
		}
		// 所有的邻接点都访问完成,出队列
		q.pop();
	}
	delete[] visited;
}

```

#### 邻接表广度优先遍历

```cpp

void BFS(GraphList& g)
{
	// 保证顶点不被重复遍历
	bool* visited = new bool[g.vertexNum];
	// init
	for (int i = 0; i < g.vertexNum; ++i)
	{
		visited[i] = false;
	}
	// 从顶点数组中找一个顶点, 开始遍历 - 0
	queue<int> q;	// int - 顶点在顶点数组中的下标
	q.push(0);
	// 访问
	visited[0] = true;
	cout << g.head[0].name << " ";

	// 队列为空,遍历完成
	while (!q.empty())
	{
		// 取出队头元素值, 顶点在顶点数组中的下标
		int front = q.front();
		// 找队头元素对应的定点的所有的邻接点
		edgeNode* pNode = g.head[front].first;
		while (pNode)
		{
			// 如果没有被访问
			if (!visited[pNode->position])
			{
				visited[pNode->position] = true;
				cout << g.head[pNode->position].name << " ";
				// 邻接点入队列
				q.push(pNode->position);
			}
			pNode = pNode->next;
		}
		// 所有的临界点发全部被访问
		q.pop();
	}
	delete[] visited;
}
```

### 迪杰斯特拉(Dijkstra)算法

visit表示该点是否被访问过

dist数组存放起点到各个点的距离

![image-20220414071801628](/images/javawz/image-20220414071801628.png)



以济南为中间结点，济南到武汉的距离为400且小于北京直接到武汉的距离，所以更新dist数组中北京到武汉的距离为400，并将visit数组中的济南点设置为1，代表已访问过



![image-20220414072024176](/images/javawz/image-20220414072024176.png)

以武汉为中间结点，武汉到北京的距离是400 + 200 等于600，更新dist数组，因为武汉到其他点只有北京一个没有被访问过，所以当更新了武汉到北京的距离之后，visit数组中的武汉点可以设置为1，代表已经访问过。

最后剩下北京点没有被访问过，因为北京点是目标点，所以visit数组中的北京点可以直接设置为1



![image-20220414072333490](/images/javawz/image-20220414072333490.png)





![image-20220414072738740](/images/javawz/image-20220414072738740.png)



初始化北京到其他点的距离

![image-20220414072819782](/images/javawz/image-20220414072819782.png)



选择北京到达其他点最近的点，北京到天津的距离为100，是最近的点，所以选择天津作为中间结点

天津到郑州的距离为1000,1000小于1200，所以更新dist数组

天津到济南的距离为400 更新dist数组

![image-20220414073224878](/images/javawz/image-20220414073224878.png)



将天津设置为已访问的结点

现在剩下济南，郑州，长沙，海南没有被访问过

![image-20220414073244767](/images/javawz/image-20220414073244767.png)



选择北京到剩下的结点中最短距离的结点济南作为中间点

济南到郑州的距离为400 + 400 = 800,  800 < 1000所以更新dist数组

济南到长沙的距离为400 + 1300 = 1700,更新dist数组

济南到海南的距离为400 + 1400 = 1800,更新dist数组

![image-20220414073614721](/images/javawz/image-20220414073614721.png)



将济南结点设置为已访问过的结点

现在剩下郑州,长沙,海南三个结点没有被访问过.

选择北京到剩下结点中最短距离的结点作为中间结点

这里选择郑州作为中间结点

郑州到长沙的距离为800 + 500 = 1300,1300 < 1700,所以更新dist数组



![image-20220414073955303](/images/javawz/image-20220414073955303.png)



将郑州设置为已访问过的结点

选择北京到剩下结点中最短的距离作为中间结点

这里选择长沙,长沙到海南的距离是1300 + 1500 = 2800 ,2800 > 1800,所以不用更新dist数组





![image-20220414074324676](/images/javawz/image-20220414074324676.png)

将长沙设置为已访问的结点

最后剩下海南结点没有被访问

因为海南结点是最后一个结点,所以直接将海南结点设置为已访问结点即可

**最后dist数组中存放了北京到达其他城市的最短距离**

![image-20220414074546109](/images/javawz/image-20220414074546109.png)



```cpp
#include <iostream>
#include <stack>
#include <queue>
using namespace std;


//定义顶点的最大值
#define MaxVertex 50
//定义顶点类型
typedef char VertexInfo[4];
//定义一个图的结构体
struct Graph
{
	//一维数组 -- 顶点信息
	VertexInfo vertex[MaxVertex];
	//二维数组 -- 边或者弧 对应关系 或者 权
	int edge[MaxVertex][MaxVertex];
	//顶点的个数
	int vertexNum;
	//边的条数
	int edgeNum;
};

//寻找顶点在一维数组中的位置
int localVertex(Graph &g, VertexInfo v)
{
	//遍历顶点数组
	for (int i = 0; i < g.vertexNum; ++i)
	{
		//寻找值与v相等的顶点
		if (strcmp(v, g.vertex[i]) == 0)
		{
			return i;
		}
	}
	return -1;
}

//用邻接矩阵创建图 -- 有向的网图
void createGraph(Graph &g)
{
	cout << "输入图的顶点数和边数(用空格间隔)" << endl;
	cin >> g.vertexNum >> g.edgeNum;

	cout << "请输入图的" << g.vertexNum << "个顶点: " << endl;
	//循环输入
	for (int i = 0; i < g.vertexNum; ++i)
	{
		//接收顶点的值
		cin >> g.vertex[i];
	}

	//初始化顶点之间的对应关系
	for (int i = 0; i < g.vertexNum; ++i)
	{
		for (int j = 0; j < g.vertexNum; ++j)
		{
			//初始化为最大值
			g.edge[i][j] = INT_MAX;
		}
	}

	//定义变量
	int w;	//权重
	VertexInfo v1, v2;	//顶点
	//输入边和权重
	cout << "依次输入" << g.edgeNum << "条边的 弧尾 弧头  权重" << endl;
	//循环输入
	for (int i = 0; i < g.edgeNum; i++)
	{
		cin >> v1 >> v2 >> w;

		//找到v1在一维数组中的位置
		int m = localVertex(g, v1);
		//找到v2在一维数组中的位置
		int n = localVertex(g, v2);
		//赋值
		g.edge[m][n] = w;
		//如果是无向图
		g.edge[n][m] = w;
	}
}

//打印图
void printGraph(Graph &g)
{
	cout << "打印图 -- 邻接矩阵:" << endl;
	cout << "\t";
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cout << g.vertex[i] << "\t";
	}
	for (int i = 0; i < g.vertexNum; ++i)
	{
		cout << endl;
		cout << g.vertex[i] << "\t";
		for (int j = 0; j < g.vertexNum; ++j)
		{
			if (g.edge[i][j] == INT_MAX)
			{
				cout << "∞" << "\t";
			}
			else
			{
				cout << g.edge[i][j] << "\t";
			}
		}
	}
	cout << endl;
}

//深度优先搜索
void DFS(Graph &g)
{
	//创建一个数组,标记节点是否已经被访问
	bool *visited = new bool[g.vertexNum];
	//数组初始化
	for (int i = 0; i < g.vertexNum; ++i)
	{
		//全部标记为未访问
		visited[i] = false;
	}

	//创建栈对象
	stack<int> st;
	//从数组的第一个顶点开始
	//访问第一个顶点
	visited[0] = true;
	//打印第一个顶点
	cout << g.vertex[0] << " ";
	//下标压栈
	st.push(0);

	while (!st.empty())
	{
		int i;
		for (i = 0; i < g.vertexNum; ++i)
		{
			//取出顶点
			int top = st.top();
			//顶点的邻接点 -- 没有被访问过
			if (!visited[i] && g.edge[top][i] < INT_MAX)
			{
				//访问
				visited[i] = true;
				//打印
				cout << g.vertex[i] << " ";
				//下标压栈
				st.push(i);
			}
		}
		//查找完一遍,栈顶元素出栈
		if (i >= g.vertexNum)
		{
			st.pop();
		}
	}

	delete[] visited;
}

//广度优先搜索
void BFS(Graph &g)
{
	//创建一个数组,标记顶点是否被访问
	bool *visited = new bool[g.vertexNum];
	//初始化
	for (int i = 0; i < g.vertexNum; ++i)
	{
		//false -- 未访问
		visited[i] = false;
	}

	//定义一个队列对象
	queue<int> q;
	//第一个顶点标记为已访问
	visited[0] = true;
	//打印第一个顶点
	cout << g.vertex[0] << " ";
	//顶点在数组中的下标,入队列
	q.push(0);

	while (!q.empty())
	{
		int front = q.front();
		for (int i = 0; i < g.vertexNum; ++i)
		{
			//寻找队列中队头的顶点的邻接点
			if (!visited[i] && g.edge[front][i] < INT_MAX)
			{
				//标记为已访问
				visited[i] = true;
				//打印
				cout << g.vertex[i] << " ";
				//下标入队列
				q.push(i);
			}
		}
		//寻找完所有的邻接点之后,下标出队列
		q.pop();
	}
	delete[] visited;
}

// 最短路径
// 迪杰斯特拉(Dijkstra)算法
// path哪一顶点到当前点的距离最近
void dijkstraPath(Graph &g, int *path, int *dist, int v0)
{
	int min = 0;
	int pos = v0;	// 访问的起始顶点
	//定义一个数组, 标记顶点是否已经被访问
	bool *visited = new bool[g.vertexNum];
	//初始化
	for (int i = 0; i < g.vertexNum; ++i)
	{
		visited[i] = false;	//顶点未访问
		if (i != v0) //排除顶点到出发点的计算
		{
			//初始化所有点的最近邻接点都是V0点
			path[i] = v0;
			// v0到各个顶点的权重
			dist[i] = g.edge[v0][i];
			cout << g.vertex[v0] << " 到 " << g.vertex[i]
				<< " 距离: dist[" << i << "]=" << dist[i] << endl;
		}
		else
		{
			// path[]数组 - 到当前点的最近的邻接点
			// dist[] 数组 - 从出发点到各个点的最短距离
			// i == v0没有任何意义, 不存在路径
			path[i] = -1;
			dist[i] = INT_MAX;
		}
	}
	//把v0标记为已访问
	visited[v0] = true;

	for (int i = 0; i < g.vertexNum; ++i)
	{
		min = INT_MAX;
		for (int j = 0; j < g.vertexNum; ++j)
		{
			// 没有被访问, 并且找到了拥有更小权值的边
			// path[]数组 - 到当前点的最近的邻接点
			// dist[] 数组 - 从出发点到各个点的最短距离
			if (!visited[j] && min>dist[j])
			{
				//保存最小值
				min = dist[j];
				//保存位置
				pos = j;
				cout << "+++ 顶点更新: pos =" << pos
					<< "顶点为: " << g.vertex[pos] << endl;
			}
		}
		//pos位置的顶点标记为已访问
		visited[pos] = true;

		// dist V0点到各个点的距离
		for (int j = 0; j < g.vertexNum; ++j)
		{
			// g.edge[pos][j] < INT_MAX - 判断pos->j这条边是存在的
			if (!visited[j] && dist[pos] + g.edge[pos][j] < dist[j] && g.edge[pos][j] < INT_MAX)
			{
				// 更新最短距离
				//例如 将j看成E  pos看成B，求出A到E最短路径
				dist[j] = dist[pos] + g.edge[pos][j];
				//更新路径, 到顶点j最近的顶点是pos
				path[j] = pos;
				cout << "=== 更新最短距离: dist[" << j
					<< "] = " << dist[j] << endl;
			}
		}
	}
}

// v0 - 起始点
// v  - 到达点
void showPath(Graph &g, int *path, int v0, int v)
{
	//创建一个栈对象
	stack<int> st;
	int temp = v;
	while (temp != v0)
	{
		st.push(temp);
		//寻找上一个顶点
		temp = path[temp];
	}
	st.push(v0);

	//打印路径
	while (!st.empty())
	{
		cout << g.vertex[st.top()] << " ";
		st.pop();
	}
}

int main()
{
	//定义一个图的对象
	Graph g;
	//用邻接矩阵创建图
	createGraph(g);
	//打印
	printGraph(g);

	//深度优先搜索
	cout << "深度优先搜索" << endl;
	DFS(g);
	cout << endl;

	//广度优先搜索
	cout << "广度优先搜索" << endl;
	BFS(g);
	cout << endl;

	/*==================================================
	==================================================*/
	cout << "迪杰斯特拉(Dijkstra)算法" << endl;
	// path数组 - 到当前点的最近的邻接点
	int path[50];
	// dist[] 数组 - 从出发点到各个点的最短距离
	int dist[50];
	// 出发点
	int v0 = 0;
	dijkstraPath(g, path, dist, v0);
	// A->D怎么走?多么长?
	// dist[3] = A->D的最短距离
	// path[3]
	for (int i = 1; i < g.vertexNum; ++i)
	{
		cout << "路径: ";
		showPath(g, path, v0, i);
		cout << "路径长度: " << dist[i] << endl;
	}

	cout << "Keyboard not found, press F1 to continue..." << endl;
	system("pause");
	return 0;
}

```

