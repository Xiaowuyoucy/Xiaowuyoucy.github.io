---
title: occi连接数据库getString函数报错
date: 2022-06-17 21:40:16
tags:
categories: cpp
doc:
---

1、报错代码：

```
while (rs->next())
		{	
			int nID = rs->getInt(1);
			int nDVDID = rs->getInt(2);
			string strName = rs->getString(3);
		}
```

2、报错信息：

OcciToOracle.exe 中的 0x5c99336f (msvcp100d.dll) 处有未经处理的异常: 0xC0000005: 读取位置 0xffffffffffffffff 时发生访问冲突

错误定位：

```
inline void _Container_base12::_Orphan_all()
 { // orphan all iterators
 #if _ITERATOR_DEBUG_LEVEL == 2
 if (_Myproxy != 0)
 { // proxy allocated, drain it
 _Lockit _Lock(_LOCK_DEBUG);


 for (_Iterator_base12 **_Pnext = &_Myproxy->_Myfirstiter;
 *_Pnext != 0; *_Pnext = (*_Pnext)->_Mynextiter)
 (*_Pnext)->_Myproxy = 0;
 _Myproxy->_Myfirstiter = 0;
 }
 #endif /* _ITERATOR_DEBUG_LEVEL == 2 */

 }
```

3、解决方法：

将

```html
string strName = rs->getString(3);
```

更改为：

```html
static string strName = rs->getString(3);
```

前面加上static。

问题解决了，但是不清楚问题原因！

重点：这也造成了多条记录循环取值时只能得到第一次的值。

可能是oracle客户端occi库版本和编译器版本不一致导致的,linux降低了g++版本之后就编译通过了,并且没有出现问题.