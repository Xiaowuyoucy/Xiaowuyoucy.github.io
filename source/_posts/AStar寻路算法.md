---
title: AStar寻路算法
date: 2022-04-18 05:12:11
tags:
categories: cpp
doc:
---



![](/images/javawz/无标题-1650229998726.png)





```cpp
#include<iostream>
#include<vector>

#define ROWS 50	// 行
#define COLS 60 // 列

#define ZXDJ 10	// 直线代价
#define XXDJ 14  // 斜线代价


using namespace std;
// 保存树节点的个数
int deepNum = 0;
// 节点结构
struct MyPoint {
	int y;	// y坐标
	int x;	// x坐标
	int g;  // 从起点到当前点的代价
	int h;  // 从当前点到终点的代价
	int w;  // 权值
	int f;  // 总代价

    // 获取F值
	void SetF() {
		f = g + h + w;
	}
};
// 树节点
struct TreeNode {
	MyPoint pos;				 // 位置
	vector<TreeNode *> child;    // 子结点
	TreeNode * Parent; 			 // 父节点
};
// 代表方向
enum FANXIAN{ p_up, p_down, p_left, p_right, p_upleft,p_upright, p_downleft, p_downright};
// 地图
unsigned char map[ROWS][COLS] = {
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,0,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
	{0,0,0,0,0,1,0,0,0,0},
};
// 标记地图,如果是false代表没有访问过,如果是true代表访问过
bool isMap[ROWS][COLS] = { false };
// 打印地图
void PrintMap(unsigned char m_map[ROWS][COLS]) {
	for (int i = 0; i < ROWS; i++)
	{
		for (int j = 0; j < COLS; j++)
		{
			switch (m_map[i][j])	
			{
			case 0:
				cout << "- ";
				break;

			case 1:
				cout << "| ";
				break;
				
			case 255:
				cout << "* ";
			default:
				break;
			}
		}
		cout << endl;
	}
}

// 获取H值
int GetHValue(MyPoint cur,MyPoint endPoint) {

	int x = ((endPoint.x > cur.x) ? endPoint.x - cur.x : cur.x - endPoint.x);
	int y = ((endPoint.y > cur.y) ? endPoint.y - cur.y : cur.y - endPoint.y);

	return (x + y) * ZXDJ;
}
// 判断是否满足入树条件
bool IsNeedAdd(MyPoint cur,unsigned	char m_map[ROWS][COLS],bool m_isMap[ROWS][COLS] ) {
    // 判断位置是否在地图中
	if (cur.x < 0 || cur.x >= COLS || cur.y < 0 || cur.y >= ROWS ) {
		return false;
	}
	// 判断当前位置是不是障碍点
	if (m_map[cur.y][cur.x] == 1)
	{
		return false;
	}
	// 判断当前点有没有被访问过
	if (m_isMap[cur.y][cur.x]) {
		return false;
	}

	return true;
}
// 获取存放最小F值的结点容器指针
vector<TreeNode *>::iterator GetMinFValue(vector<TreeNode *> & buff) {

	vector<TreeNode*>::iterator it = buff.begin();
	vector<TreeNode*>::iterator min = it;

	for (;it != buff.end();it++)
	{
		if ((*min)->pos.f > (*it) ->pos.f)
		{
			min = it;
		}
	}
	return min;
}
// 根节点
TreeNode * pRoot;
// 删除树
void DeleteTree(TreeNode * root) {
	if (root == NULL)
	{
		return;
	}
	
	for (int i = 0; i < root->child.size(); i++)
	{
		DeleteTree(root->child[i]);
	}

	root->child.clear();
	vector<TreeNode*>(root->child).swap(root->child);
	delete root;
	deepNum--;
		

}
int main() {

	// 存放发散的结点
	vector<TreeNode*>buff;
	// 存放当前结点
	TreeNode * curTreeNode = new TreeNode;
	memset(curTreeNode,0x00,sizeof TreeNode);
    
	deepNum++;
	pRoot = curTreeNode;

	TreeNode * newTreeNode;
    
	MyPoint  beginPoint = { 0,0 };				// 起点
	MyPoint  endPoint = { ROWS - 2,COLS-9 };    // 终点
    // 设置当前点位置是起点位置
	curTreeNode->pos.x = beginPoint.x;
	curTreeNode->pos.y = beginPoint.y;
	// 存放是否找到终点,true代表找到
	bool isFindEnd= false;
	
	// 将起点设置为已经访问过
	isMap[beginPoint.y][beginPoint.x] = true;
	
	while (true)
	{
		//从当前点开始发散
		for (int i = 0; i < 8; i++)
		{
            // 创建一个新结点用来存放发散出来的点
			newTreeNode = new TreeNode;
			// 使新结点的位置等于当前结点的位置
			newTreeNode->pos = curTreeNode->pos;
			switch (i)
			{
            // 上
			case p_up:
				newTreeNode->pos.y--;
				newTreeNode->pos.g = ZXDJ;  // g值是直线代价
				break;
			// 下	
			case p_down:
				newTreeNode->pos.y++;
				newTreeNode->pos.g = ZXDJ;
				break;
			// 左
			case p_left:
				newTreeNode->pos.x--;
				newTreeNode->pos.g = ZXDJ;
				break;
			// 右
			case p_right:
				newTreeNode->pos.x++;
				newTreeNode->pos.g = ZXDJ;
				break;
			// 左上
			case p_upleft:
				newTreeNode->pos.x--;
				newTreeNode->pos.y--;
				newTreeNode->pos.g = XXDJ; //g值是斜线代价
				break;
			// 右上
			case p_upright:
				newTreeNode->pos.x++;
				newTreeNode->pos.y--;
				newTreeNode->pos.g = XXDJ;
				break;
			// 左下
			case p_downleft:
				newTreeNode->pos.x--;
				newTreeNode->pos.y++;
				newTreeNode->pos.g = XXDJ;
				break;
			// 右下
			case p_downright:
				newTreeNode->pos.x++;
				newTreeNode->pos.y++;
				newTreeNode->pos.g = XXDJ;
				break;
			default:
				break;
			}
			// 计算 新结点的h值
			newTreeNode->pos.h = GetHValue(newTreeNode->pos,endPoint);
			// 计算新结点的f值
			newTreeNode->pos.SetF();
			// 判断新结点是否满足入树的条件
			if (IsNeedAdd(newTreeNode->pos,map,isMap)) {
				// 将新结点添加到当前点的子结点中
				curTreeNode->child.push_back(newTreeNode);
				// 使新结点中的父节点指针成员指向当前结点
				newTreeNode->Parent = curTreeNode;
				// 添加到buff数组中
				buff.push_back(newTreeNode);
				// 标记新结点已经被访问过
				isMap[newTreeNode->pos.y][newTreeNode->pos.x] = true;
				deepNum++;
			}
			else {
                // 不满足入树条件则删除新结点
				delete newTreeNode;
			}


		}
        // 判断buff数组是否为空,如果为空则退出循环
		if (buff.empty()) {
			break;
		}
        // 获取最小F值的容器位置
		vector<TreeNode*>::iterator it = GetMinFValue(buff);
		// 使当前点指向最小F值的结点
		curTreeNode = *it;
		// 删除buff中最小F值的结点
		buff.erase(it);
		// 判断是否找到终点
		if (curTreeNode->pos.x == endPoint.x && curTreeNode->pos.y == endPoint.y)
		{
			isFindEnd = true;
			break;
		}


	}
    
    // 判断是否找到终点
	if (isFindEnd)
	{
		cout << "找到了" << endl;
	}
	// 设置起点到终点的路径路线
	while (curTreeNode)
	{
		map[curTreeNode->pos.y][curTreeNode->pos.x] = 255;
		curTreeNode = curTreeNode->Parent;
	}
    // 打印地图
	PrintMap(map);
	cin.get();
    // 删除树
	DeleteTree(pRoot);
	cin.get();
	return 0;
}
```

