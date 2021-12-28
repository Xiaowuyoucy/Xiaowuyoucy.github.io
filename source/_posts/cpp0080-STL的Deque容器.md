---
title: deque容器
date: 2021-07-01 13:05:02
tags:
categories: cpp
doc:
---

#### Deque简介

deque是“double-ended queue”的缩写，和vector一样都是STL的容器，deque是双端数组，而vector是单端的。

deque在接口上和vector非常相似，在许多操作的地方可以直接替换。

deque可以随机存取元素（支持索引值直接存取， 用[]操作符或at()方法，这个等下会详讲）。

deque头部和尾部添加或移除元素都非常快速。但是在中部安插元素或移除元素比较费时。

#include \<deque> 

#### deque对象的默认构造

```
deque采用模板类实现，deque对象的默认构造形式：deque<T> deqT;  

deque <int> deqInt;            //一个存放int的deque容器。
deque <float> deq Float;     //一个存放float的deque容器。
deque <string> deq String;     //一个存放string的deque容器。
...				   
 //尖括号内还可以设置指针类型或自定义类型。 

```

#### deque末尾的添加移除操作

```
deque.push_back(elem);	//在容器尾部添加一个数据
deque.push_front(elem);	//在容器头部插入一个数据
deque.pop_back();    		//删除容器最后一个数据
deque.pop_front();		//删除容器第一个数据
```

```
deque<int> deqInt;
	deqInt.push_back(1);
	deqInt.push_back(3);
	deqInt.push_back(5);
	deqInt.push_back(7);
	deqInt.push_back(9);
	deqInt.pop_front();
	deqInt.pop_front();
	deqInt.push_front(11);
	deqInt.push_front(13);
	deqInt.pop_back();
	deqInt.pop_back();
//deqInt  { 13,11,5}

```

#### deque的数据存取

```
deque.at(idx);  //返回索引idx所指的数据，如果idx越界，抛出out_of_range。
deque[idx];  //返回索引idx所指的数据，如果idx越界，不抛出异常，直接出错。
deque.front();   //返回第一个数据。
deque.back();  //返回最后一个数据
```

```
deque<int> deqInt;
		deqInt.push_back(1);
		deqInt.push_back(3);
		deqInt.push_back(5);
		deqInt.push_back(7);
		deqInt.push_back(9);

		int iA = deqInt.at(0);		//1
		int iB = deqInt[1];			//3
		deqInt.at(0) = 99;			//99
		deqInt[1] = 88;			//88

		int iFront = deqInt.front();	//99
		int iBack = deqInt.back();	//9
		deqInt.front() = 77;			//77
		deqInt.back() = 66;			//66

```

#### deque与迭代器

```
deque.begin();  //返回容器中第一个元素的迭代器。
deque.end();  //返回容器中最后一个元素之后的迭代器。
deque.rbegin();  //返回容器中倒数第一个元素的迭代器。
deque.rend();   //返回容器中倒数最后一个元素之后的迭代器。
```

```
deque<int> deqInt;
		deqInt.push_back(1);
		deqInt.push_back(3);
		deqInt.push_back(5);
		deqInt.push_back(7);
		deqInt.push_back(9);

		for (deque<int>::iterator it=deqInt.begin(); it!=deqInt.end(); ++it)
		{
			cout << *it;
			cout << "";
		}
	// 1 3 5 7 9

		for (deque<int>::reverse_iterator rit=deqInt.rbegin(); rit!=deqInt.rend(); ++rit)
		{
			cout << *rit;
			cout << "";
		}
	//9 7 5 3 1

```

#### deque对象的带参数构造

```
deque(beg,end);    //构造函数将[beg, end)区间中的元素拷贝给本身。注意该区间是左闭右开的区间。
deque(n,elem);   //构造函数将n个elem拷贝给本身。
deque(const deque  &deq);  //拷贝构造函数。

```

```
deque<int> deqIntA;
		deqIntA.push_back(1);
		deqIntA.push_back(3);
		deqIntA.push_back(5);
		deqIntA.push_back(7);
		deqIntA.push_back(9);

		deque<int> deqIntB(deqIntA.begin(),deqIntA.end());		//1 3 5 7 9
		deque<int> deqIntC(5,8);							//8 8 8 8 8
		deque<int> deqIntD(deqIntA);						//1 3 5 7 9

```

#### deque的赋值

```
deque.assign(beg,end);    //将[beg, end)区间中的数据拷贝赋值给本身。注意该区间是左闭右开的区间。
deque.assign(n,elem);  //将n个elem拷贝赋值给本身。
deque& operator=(const deque &deq);	//重载等号操作符 
deque.swap(deq);  // 将vec与本身的元素互换

```

```
deque<int> deqIntA,deqIntB,deqIntC,deqIntD;
		deqIntA.push_back(1);
		deqIntA.push_back(3);
		deqIntA.push_back(5);
		deqIntA.push_back(7);
		deqIntA.push_back(9);

		deqIntB.assign(deqIntA.begin(),deqIntA.end());	// 1 3 5 7 9
		
		deqIntC.assign(5,8);						//8 8 8 8 8

		deqIntD = deqIntA;							//1 3 5 7 9

		deqIntC.swap(deqIntD);						//互换

```

#### deque的大小

```
deque.size();	   //返回容器中元素的个数
deque.empty();	   //判断容器是否为空
deque.resize(num);   //重新指定容器的长度为num，若容器变长，则以默认值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。
deque.resize(num, elem);  //重新指定容器的长度为num，若容器变长，则以elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除。

```

```
deque<int> deqIntA;
deqIntA.push_back(1);
deqIntA.push_back(3);
deqIntA.push_back(5);

int iSize = deqIntA.size();  //3

if (!deqIntA.empty())
	{
		deqIntA.resize(5);		//1 3 5 0 0
		deqIntA.resize(7,1);	//1 3 5 0 0 1 1
		deqIntA.resize(2);		//1 3
	}

```

#### deque的插入

```
deque.insert(pos,elem);   //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
deque.insert(pos,n,elem);   //在pos位置插入n个elem数据，无返回值。
deque.insert(pos,beg,end);   //在pos位置插入[beg,end)区间的数据，无返回值。
```

```
deque<int> deqA;
	deque<int> deqB;

	deqA.push_back(1);
	deqA.push_back(3);
	deqA.push_back(5);
	deqA.push_back(7);
	deqA.push_back(9);

	deqB.push_back(2);
	deqB.push_back(4);
	deqB.push_back(6);
	deqB.push_back(8);
	
	deqA.insert(deqA.begin(), 11);		//{11, 1, 3, 5, 7, 9}
	deqA.insert(deqA.begin()+1,2,33);		//{11,33,33,1,3,5,7,9}
	deqA.insert(deqA.begin() , deqB.begin() , deqB.end() );	//{2,4,6,8,11,33,33,1,3,5,7,9}

```

#### deque的删除

```
deque.clear();	//移除容器的所有数据
deque.erase(beg,end);  //删除[beg,end)区间的数据，返回下一个数据的位置。
deque.erase(pos);    //删除pos位置的数据，返回下一个数据的位置。
```

```
删除区间内的元素
deqInt是用deque<int>声明的容器，现已包含按顺序的1,3,5,6,9元素。
deque<int>::iterator itBegin=deqInt.begin()+1;
deque<int>::iterator itEnd=deqInt.begin()+3;
deqInt.erase(itBegin,itEnd);
//此时容器deqInt包含按顺序的1,6,9三个元素。



假设 deqInt 包含1,3,2,3,3,3,4,3,5,3，删除容器中等于3的元素
for(deque<int>::iterator it=deqInt.being(); it!=deqInt.end(); )    //小括号里不需写  ++it
{
   if(*it == 3)
   {
        it  =  deqInt.erase(it);       //以迭代器为参数，删除元素3，并把数据删除后的下一个元素位置返回给迭代器。
         //此时，不执行  ++it；  
   }
   else
   {
       ++it;
   }
}

//删除deqInt的所有元素
deqInt.clear();			//容器为空


```





### 基本操作代码

```cpp
#define _CRT_SECURE_NO_WARNINGS

#include<iostream>
#include<deque>
using namespace std;


//deque容器初始化
void test01(){
	deque<int> d1; //默认构造函数
	deque<int> d2(10,5); //带参数构造函数
	deque<int> d3(d2.begin(),d2.end());
	deque<int> d4(d3); //拷贝构造
}

//deque赋值操作
void test02(){
	

	deque<int> d1(10, 3);

	deque<int> d;
	//d.assign(10,5);  赋值
	//d.assign(d1.begin(),d1.end());

	d = d1;  //重载= 赋值

}

//大小操作
void test03(){
	
	deque<int> d1(10, 3);
	cout << d1.size() << endl;
	if (d1.empty()){
		cout << "空！" << endl;
	}
	else{
		cout << "不空!" << endl;
	}



	//d1.resize(5);
	//cout << d1.size() << endl;
	d1.resize(15);
}

//deque插入和删除
void test04(){
	
	deque<int> d;

	d.push_back(10);
	d.push_back(20);
	d.push_front(30);
	d.push_front(40);


	//第一种遍历方式
	for (int i = 0; i < d.size();i++){
		cout << d[i] << " ";
	}
	cout << endl;

	//第二种遍历方式
	for (int i = 0; i < d.size(); i++){
		cout << d.at(i) << " ";
	}
	cout << endl;

	//第三种方式
	for (deque<int>::iterator it = d.begin(); it != d.end(); it++){
		cout << *it << " ";
	}
	cout << endl;


	//删除元素
	/*
		while (d.size() > 0){
		cout << d.back() << "被删除!" << endl;
		d.pop_back();
	}
	cout << "大小:" << d.size() << endl;
	*/


	//头删除
	while (!d.empty()){
		cout << d.front() << "被删除!" << endl;
		d.pop_front();
	}


}

//deque容器插入
void test06(){
	
	deque<int> d;
	d.insert(d.begin(),100); //头插法
	d.insert(d.end(), 200); //尾差法

	for (deque<int>::iterator it = d.begin(); it != d.end(); it++){
		cout << *it << " ";
	}
	cout << endl;
}


int main(){


	//test03();
	//test04();
	test06();

	system("pause");
	return EXIT_SUCCESS;
}

```

### 案例代码



sort排序第三个参数是判断规则函数

```
bool mycompare(int v1,int v2){

	return v1 > v2; // 排序从大大小
	//return v1 < v2; //从小到大
	
}
```



```cpp
#define _CRT_SECURE_NO_WARNINGS

#include<iostream>
#include<string>
#include<vector>
#include<deque>
#include<algorithm>
using namespace std;


//选手类
class Player{
public:
	Player(string name, int score) :name(name), score(score){}
	string name;
	int score; //分数
};


//创建选手
void Create_Player(vector<Player>& v){

	string nameseed = "ABCDE";
	for (int i = 0; i < 5;i++){
		string name = "选手";
		name += nameseed[i];
		Player p(name,0);  //创建选手
		v.push_back(p);
	}
}

//打分
bool mycompare(int v1,int v2){
	if (v1 > v2){  //从大到小
		return true;
	}
	else{
		return false;
	}
}
void Set_Player_Score(vector<Player>& plist){
	
	for (vector<Player>::iterator it = plist.begin(); it != plist.end();it++){
		
		deque<int> dscore;
		for (int i = 0; i < 10;i++){
			int score = 50 + rand() % 50; //打分
			dscore.push_back(score);
		}
		
		//排序 sort
		sort(dscore.begin(), dscore.end(), mycompare);
		/*
				for (deque<int>::iterator it = dscore.begin(); it != dscore.end();it ++){
			cout << *it << " ";
		}
		cout << endl;
		*/

		dscore.pop_front(); //去除最高分
		dscore.pop_back(); //去除最低分
		//分数总结
		int totalscore = 0;
		for (deque<int>::iterator dit = dscore.begin(); dit != dscore.end(); dit++){
			totalscore += *dit;
		}

		int scoreavg = totalscore / dscore.size();
		(*it).score = scoreavg;
	}

}
//显示5名选手得分
void Show_Player_Score(vector<Player>& plist){

	for (vector<Player>::iterator it = plist.begin(); it != plist.end(); it++){
		cout << "姓名:" << it->name << " 分数:" << it->score << endl;
	}
	cout << endl;

}
int main(){


	vector<Player>  vPlayer;  //存放我们的选手信息
	Create_Player(vPlayer); //创建选手
	Set_Player_Score(vPlayer);
	Show_Player_Score(vPlayer);

	//评委打分案例(sort 算法排序)
	//创建 5 个选手(姓名，得分)，10 个评委对 5 个选手进行打分
	//得分规则：去除最高分，去除最低分，取出平均分
	//按得分对 5 名选手进行排名

	system("pause");
	return EXIT_SUCCESS;
}

```

