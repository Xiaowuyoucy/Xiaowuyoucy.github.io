---
title: STL的string容器
date: 2021-06-29 12:18:51
tags:
categories: cpp
doc:
---

# STL的string容器

### 1String概念

 string是STL的字符串类型，通常用来表示字符串。而在使用string之前，字符串通常是用char*表示的。string与char*都可以用来表示字符串，那么二者有什么区别呢。

string和char*的比较

string是一个类, char*是一个指向字符的指针。

string封装了char*，管理这个字符串，是一个char*型的容器。

string不用考虑内存释放和越界。

string管理char*所分配的内存。每一次string的复制，取值都由string类负责维护，不用担心复制越界和取值越界等。

string提供了一系列的字符串操作函数（这个等下会详讲）

 查找find，拷贝copy，删除erase，替换replace，插入insert

### 2string的构造函数

 默认构造函数：

```
string();    //构造一个空的字符串string s1。
```

 拷贝构造函数：

```
string(const string &str); //构造一个与str一样的string。如string s1(s2)。
```

 带参数的构造函数      

```
string(const char *s);  //用字符串s初始化

string(int n,char c);  //用n个字符c初始化
```



#### 3string的存取字符操作

 string类的字符操作：

```
const char &operator[] (int n) const;

const char &at(int n) const;

char &operator[] (int n);

char &at(int n);
```

 operator[]和at()均返回当前字符串中第n个字符，但二者是有区别的。

​    主要区别在于at()在越界时会抛出异常，[]在刚好越界时会返回(char)0，再继续越界时，编译器直接出错。如果你的程序希望可以通过try,catch捕获异常，建议采用at()。

### 4从string取得const char*的操作

```
const char *c_str() const;   //返回一个以'\0'结尾的字符串的首地址
```

### 5把string拷贝到char*指向的内存空间的操作

```
int copy(char *s, int n, int pos=0) const;  
```

把当前串中以pos开始的n个字符拷贝到以s为起始位置的字符数组中，返回实际拷贝的数目。注意要保证s所指向的空间足够大以容纳当前字符串，不然会越界。

### 6string的长度

```
int length() const;   //返回当前字符串的长度。长度不包括字符串结尾的'\0'。
bool empty() const;     //当前字符串是否为空
```

### 7string的赋值

```
string &operator=(const string &s);//把字符串s赋给当前的字符串

string &assign(const char *s); //把字符串s赋给当前的字符串

string &assign(const char *s, int n); //把字符串s的前n个字符赋给当前的字符串

string &assign(const string &s);  //把字符串s赋给当前字符串

string &assign(int n,char c);  //用n个字符c赋给当前字符串

string &assign(const string &s,int start, int n);  //把字符串s中从start开始的n个字符赋给当前字符串
```

### 8string字符串连接

```
string &operator+=(const string &s);  //把字符串s连接到当前字符串结尾

string &operator+=(const char *s);//把字符串s连接到当前字符串结尾

string &append(const char *s);    //把字符串s连接到当前字符串结尾

string &append(const char *s,int n);  //把字符串s的前n个字符连接到当前字符串结尾

string &append(const string &s);   //同operator+=()

string &append(const string &s,int pos, int n);//把字符串s中从pos开始的n个字符连接到当前字符串结尾

string &append(int n, char c);   //在当前字符串结尾添加n个字符c

```

#### 9string的比较

```
int compare(const string &s) const;  //与字符串s比较
int compare(const char *s) const;   //与字符串s比较
```

compare函数在>时返回 1，<时返回 -1，==时返回 0。比较区分大小写，比较时参考字典顺序，排越前面的越小。大写的A比小写的a小。

### 10string的子串

```
string substr(int pos=0, int n=npos) const;    //返回由pos开始的n个字符组成的子字符串
```

### 11string的查找 和 替换

#### 查找

```
int find(char c,int pos=0) const;  //从pos开始查找字符c在当前字符串的位置 

int find(const char *s, int pos=0) const;  //从pos开始查找字符串s在当前字符串的位置

int find(const string &s, int pos=0) const;  //从pos开始查找字符串s在当前字符串中的位置

find函数如果查找不到，就返回-1

int rfind(char c, int pos=npos) const;   //从pos开始从后向前查找字符c在当前字符串中的位置 

int rfind(const char *s, int pos=npos) const;

int rfind(const string &s, int pos=npos) const;
//rfind是反向查找的意思，如果查找不到， 返回-1

```

#### 替换

```
string &replace(int pos, int n, const char *s);//删除从pos开始的n个字符，然后在pos处插入串s

string &replace(int pos, int n, const string &s);  //删除从pos开始的n个字符，然后在pos处插入串s

void swap(string &s2);    //交换当前字符串与s2的值

```

### 12String的区间删除和插入

```
string &insert(int pos, const char *s);
string &insert(int pos, const string &s);
//前两个函数在pos位置插入字符串s
string &insert(int pos, int n, char c);  //在pos位置 插入n个字符c

string &erase(int pos=0, int n=npos);  //删除pos开始的n个字符，返回修改后的字符串

```



```cpp
#define _CRT_SECURE_NO_WARNINGS

#include<iostream>
#include<string>
using namespace std;


//char* 和 string转换
void test01(){

	//string类型转换为char*字符串
	string s = "abc";
	const char* str = s.c_str();

	//char*类型字符串转换为string类型字符串
	char* str2 = "acbd";
	string s2(str2);

}

//string初始化
void test02(){
	
	string s; //默认构造
	string s2 = "acbd";
	string s3(s2);

	string s4(10, 'c');


	cout << s2 << endl;
	cout << s3 << endl;
	cout << s4 << endl;

}

//string容器字符串赋值和存取
void test03(){
	

	//string容器赋值
	string s;
	s = "abcd";

	string s2;
	s2.assign("pppp");


	//string容器存取
	string s3 = "abcdefg";
	for (int i = 0; i < s3.size();i++){
		cout << s3[i] << " ";
	}
	cout << endl;

	for (int i = 0; i < s3.size();i++){
		cout << s3.at(i) << " ";
	}
	cout << endl;


	//【】访问方式访问越界时候，不会抛异常，直接挂掉
	//at会抛出异常
	try{
		//cout << s3[100] << endl;
		cout << s3.at(100) << endl;
	}
	catch (...){
		cout << "访问越界!" << endl;
	}


}

//string容器拼接操作
void test04(){

	string s1 = "aaa";
	string s2 = "bbb";

	//string s3 = s1 + s2;
	//cout << s3 << endl;

	//s1 += s2;
	//cout << s1 << endl;

	//成员方法方式 append
	s1.append(s2);
	cout << s1 << endl;
}

//string查找和替换 比较
void test05(){

	string s = " acbdefg";
	//查找
	string target = "bd";
	int pos = s.find(target);
	char* target2 = "ef";
	int pos2 = s.find(target2);

	cout << "pos:" << pos << endl;
	cout << "pos2:" << pos2 << endl;

	int pos3 = s.rfind(target);
	cout << "pos3:" << pos3 << endl;

	//字符串替换
	string s1 = "acbd";
	s.replace(0, 2, s1);
	cout << s << endl;
	

}

//string 比较 子串 插入和删除
void test06(){
	
	//比较
	string s1 = "abc";
	string s2 = "abd";

	int ret = s1.compare(s2);
	if (ret == 1){

		cout << "s1 > s2" << endl;
	}
	else if (ret == -1){

		cout << "s1 < s2" << endl;
	
	}
	else{
		cout << "s1 == s2" << endl;
	}

	//子串
	string s3 = "abcdefg";
	string s4 = s3.substr(0,2);
	cout << "s4" << s4 << endl;

	//插入和删除
	string s5 = "abcd";
	s5.insert(0, "pppp");
	string s6 = "qqqq";
	s5.insert(s5.size(), s6);

	cout << s5 << endl;

	s5.erase(0,4);
	cout << s5 << endl;

	 
}

int main(){


	//test02();
	//test03();
	//test04();
	//test05();
	test06();

	system("pause");
	return EXIT_SUCCESS;
}

```

