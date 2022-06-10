---
title: 哈希算法hmac的使用方法
date: 2022-05-31 07:58:39
tags:
categories: cpp
doc:
---

> HMAC是密钥相关的哈希运算消息认证码，HMAC运算利用哈希算法，以一个密钥和一个消息为输入，生成
> 一个消息摘要作为输出。
> Hash-based Message Authentication Code - HMAC



### 需要包含头文件

```
hmac.h
```



- 第一种方式

```
// 适合处理少量数据
unsigned char *HMAC(const EVP_MD *evp_md, const void *key, int key_len,
const unsigned char *d, size_t n, unsigned char *md,
unsigned int *md_len);
- 参数evp_md:
- EVP_md5()
- EVP_sha1()
- EVP_sha224()
- EVP_sha256()
........
- 参数key: 传入, 秘钥, 需要将其和原始数据组合进行哈希运算
- 参数key_len: 秘钥的长度
- 参数d: 原始数据
- 参数n: 原始数据d的长度
- 参数md: 传出, 保存的是生成的散列值
- 参数md_len: 传出, md的长度
```

evp_md 参数在evp.h头文件中记录着,使用什么加密方式可以去查

```
# include <openssl/evp.h>
```



- 第二种方式:

```
// 适合处理数据量比较大的情况
HMAC_CTX *HMAC_CTX_new(void);
int HMAC_Init_ex(HMAC_CTX *ctx, const void *key, int len,
					const EVP_MD *md, ENGINE *impl)
int HMAC_Init(HMAC_CTX *ctx, const void *key, int len,
					const EVP_MD *md))
	- ctx: 通过调用HMAC_CTX_new(void)得到的指针
	- key: 秘钥
	- len: 秘钥长度
	- md:
		- EVP_md5();
		- EVP_sha1();
		- EVP_sha256();
		- ...
		
		
// 添加数据
int HMAC_Update(HMAC_CTX *ctx, const unsigned char *data,
					size_t len);
	- ctx: 通过调用HMAC_CTX_new(void)得到的指针
	- data: 原始数据
	- len: 原始数据长度
	
	
	
// 计算结果
int HMAC_Final(HMAC_CTX *ctx, unsigned char *md,
				unsigned int *len);
	- ctx: 通过调用HMAC_CTX_new(void)得到的指针
	- md: 传出参数, 存储得到的散列值
	- len: 传出, md散列值的长度
```

