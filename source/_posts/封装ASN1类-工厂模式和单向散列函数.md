---
title: 封装ASN1类、工厂模式和单向散列函数
date: 2022-05-23 02:01:12
tags:
categories: cpp
doc:
---

下载地址:

https://gitee.com/xiaochenyan/mypro/tree/master/%E5%A4%9A%E7%AB%AF%E5%AE%89%E5%85%A8%E5%8D%8F%E8%AE%AE%E4%BC%A0%E8%BE%93%E5%B9%B3%E5%8F%B0/%E5%B7%A5%E5%8E%82%E7%B1%BB%E5%92%8Chash

<br />

<br />

<br />



![image-20220523020138351](/images/javawz/image-20220523020138351.png)



<br />

<br />

<br />

### BaseASN1类是ASN1中的API进行封装

### BaseASN1.h

```cpp
#ifndef BASEASN1_H
#define BASEASN1_H
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include "ItcastLog.h"

// #define ITCASTDER_NoErr 0
// 自定义基础数据类型
typedef int              ITCAST_INT;
typedef unsigned char    ITCAST_UINT8;
typedef unsigned short   ITCAST_UINT16;
typedef unsigned long    ITCAST_UINT32;
typedef signed long      ITCAST_SINT32;
typedef unsigned char    ITASN1_BOOLEAN;

// 数据节点结构体(类)
typedef struct ITCAST_ANYBUF_
{
    ITCAST_UINT8 	  *pData;
    ITCAST_UINT32     dataLen;

    ITCAST_UINT32     unusedBits;	/* for bit string */
    ITCAST_UINT32     memoryType;
    ITCAST_UINT32     dataType;
    struct ITCAST_ANYBUF_ *next;    /* for sequence and set */
    struct ITCAST_ANYBUF_ *prev;
}ITCAST_ANYBUF;

// 自定义复合数据类型
typedef ITCAST_ANYBUF    ITASN1_INTEGER;
typedef ITCAST_ANYBUF    ITASN1_OCTETSTRING;
typedef ITCAST_ANYBUF    ITASN1_BITSTRING;
typedef ITCAST_ANYBUF    ITASN1_PRINTABLESTRING;
/*begin of bmpstring*/
typedef ITCAST_ANYBUF	 ITASN1_BMPSTRING;
/*end of bmpstring*/
typedef ITCAST_ANYBUF    ITASN1_ENUMERATED;
typedef ITCAST_ANYBUF    ITASN1_IA5STRING;
typedef ITCAST_ANYBUF    ITASN1_SEQUENCE;
typedef ITCAST_ANYBUF    ITASN1_SET;

class BaseASN1
{
public:
    // 标记内存类型
    enum MemoryType{StaticMemory=1, MallocMemory=2};
    // 错误描述
    enum DerErrType{
        NoErr=0,
        MemoryErr=200,
        LengthErr,
        LengthNotEqual,
        DataRangeErr,
        InvalidTag
    };
    // 整形数的每一位代表不同的数据类型0-31, 即: tag的值对应的数据类型
    enum DerTag{
        ITCAST_DER_ID_RESERVED,
        ITCAST_DER_ID_BOOLEAN,
        ITCAST_DER_ID_INTEGER,
        ITCAST_DER_ID_BITSTRING,
        ITCAST_DER_ID_OCTETSTRING,
        ITCAST_DER_ID_NULL,
        ITCAST_DER_ID_OBJECT_IDENTIFIER,
        ITCAST_DER_ID_OBJECT_DESCRIPTOR,
        ITCAST_DER_ID_EXTERNAL,
        ITCAST_DER_ID_REAL,
        ITCAST_DER_ID_ENUMERATED,
        ITCAST_DER_ID_EMBEDDED_PDV,
        ITCAST_DER_ID_STRING_UTF8,
        ITCAST_DER_ID_13,
        ITCAST_DER_ID_14,
        ITCAST_DER_ID_15,
        ITCAST_DER_ID_SEQUENCE,
        ITCAST_DER_ID_SET,
        ITCAST_DER_ID_STRING_NUMERIC,
        ITCAST_DER_ID_STRING_PRINTABLE,
        ITCAST_DER_ID_STRING_T61,
        ITCAST_DER_ID_STRING_VIDEOTEX,
        ITCAST_DER_ID_STRING_IA5,
        ITCAST_DER_ID_TIME_UTC,
        ITCAST_DER_ID_TIME_GENERALIZED,
        ITCAST_DER_ID_STRING_GRAPHIC,
        ITCAST_DER_ID_STRING_ISO646,
        ITCAST_DER_ID_STRING_GENERAL,
        ITCAST_DER_ID_STRING_UNIVERSAL,
        ITCAST_DER_ID_29,
        ITCAST_DER_ID_STRING_BMP
    };

    // 构造函数
    BaseASN1();
    
	//DER编码整数数据
    ITCAST_INT DER_ItAsn1_WriteInteger(ITCAST_UINT32 integer, ITASN1_INTEGER **ppDerInteger);
    
	//DER解码整数数据
    ITCAST_INT DER_ItAsn1_ReadInteger(ITASN1_INTEGER *pDerInteger, ITCAST_UINT32 *pInteger);
	
    //DER编码BitString类型数据
    ITCAST_INT DER_ItAsn1_WriteBitString(ITASN1_BITSTRING *pBitString, ITASN1_BITSTRING **ppDerBitString);

   	
    //DER解码BitString类型数据
    ITCAST_INT DER_ItAsn1_ReadBitString(ITASN1_BITSTRING *pDerBitString, ITASN1_BITSTRING **ppBitString);
       
	//DER编码CharString类型数据

    ITCAST_INT DER_ItAsn1_WritePrintableString(ITASN1_PRINTABLESTRING *pPrintString, ITASN1_PRINTABLESTRING **ppDerPrintString);

    //DER解码PrintableString类型数据
    ITCAST_INT DER_ItAsn1_ReadPrintableString(ITASN1_PRINTABLESTRING *pDerPrintString, ITASN1_PRINTABLESTRING **ppPrintString);

    
    ITCAST_INT DER_ItAsn1_WriteSequence(ITASN1_SEQUENCE *pSequence, ITCAST_ANYBUF **ppDerSequence);

    ITCAST_INT DER_ItAsn1_ReadSequence(ITCAST_ANYBUF *pDerSequence, ITASN1_SEQUENCE **ppSequence);

    ITCAST_INT DER_ItAsn1_WriteNull(ITCAST_ANYBUF ** ppDerNull);

    ITCAST_INT DER_ItAsn1_ReadNull(ITCAST_ANYBUF * ppDerNull, ITCAST_UINT8 * pInt);

    // 释放一个序列(链表), pAnyBuf为链表的头结点
    ITCAST_INT DER_ITCAST_FreeQueue(ITCAST_ANYBUF *pAnyBuf);

    // 创建ITCAST_ANYBUF, 将strOrigin写入创建的ITCAST_ANYBUF内存中, 通过pOriginBuf将内存地址传出
    ITCAST_INT DER_ITCAST_String_To_AnyBuf(ITCAST_ANYBUF **pOriginBuf, unsigned char * strOrigin, int strOriginLen);

    int WriteNullSequence(ITCAST_ANYBUF **pOutData);

    // 同 EncodeChar 函数
    int EncodeUnsignedChar(unsigned char *pData, int dataLen, ITCAST_ANYBUF **outBuf);

    int DecodeUnsignedChar(ITCAST_ANYBUF *inBuf, unsigned char **Data, int *pDataLen);

    // pData编码为ITCAST_ANYBUF(有malloc动作), 将新的ITCAST_ANYBUF节点地址赋值给outBuf
    int EncodeChar(char *pData, int dataLen, ITCAST_ANYBUF **outBuf);

    // 解析节点inBuf中的字符串数据, 通过第二个参数Data指针传出
    int DecodeChar(ITCAST_ANYBUF *inBuf, char **Data, int *pDataLen);

private:
    ITCAST_INT DER_ItAsn1_Low_GetTagInfo(
            ITCAST_UINT8 **ppDerData,
            ITCAST_UINT32 **ppTagValue,
            ITCAST_UINT32 **ppTagSize);
    ITCAST_UINT32 DER_ItAsn1_Low_Count_LengthOfSize(ITCAST_UINT32 iLength);
    ITCAST_INT DER_ItAsn1_GetLengthInfo(
            ITCAST_ANYBUF *pDerData,
            int *pLengthValue,
            int *pLengthSize);
    ITCAST_INT DER_ItAsn1_Low_GetLengthInfo(
            ITCAST_UINT8 **ppDerData,
            ITCAST_UINT32 **ppLengthValue,
            ITCAST_UINT32 **ppLengthSize);
    ITCAST_INT DER_ItAsn1_Low_IntToChar(
            ITCAST_UINT32 integer,
            ITCAST_UINT8 **ppData,
            ITCAST_UINT32 **ppLength);
    ITCAST_INT DER_ItAsn1_Low_CharToInt(
            ITCAST_UINT8 *aData,
            ITCAST_UINT32 lLength,
            ITCAST_UINT32 **ppInteger);
    ITCAST_INT DER_ItAsn1_Low_WriteTagAndLength(
            ITCAST_ANYBUF *pAnyIn,
            ITCAST_UINT8 cTag,
            ITCAST_ANYBUF **ppAnyOut,
            ITCAST_UINT8 **ppUint8Value);
    ITCAST_INT DER_ItAsn1_Low_ReadTagAndLength(
            ITCAST_ANYBUF *pAnyIn,
            ITCAST_UINT8 **ppUint8Data,
            ITCAST_ANYBUF **ppAnyOut,
            ITCAST_UINT8 **ppUint8Value);
    ITCAST_INT DER_ItAsn1_WriteCharString(
            ITCAST_ANYBUF *pCharString,
            ITCAST_ANYBUF **ppDerCharString);
    ITCAST_INT DER_ItAsn1_ReadCharString(
            ITCAST_ANYBUF *pDerCharString,
            ITCAST_ANYBUF **ppCharString);
    ITCAST_INT DER_ItAsn1_WriteBmpString(
            ITASN1_BMPSTRING *pBmpString,
            ITASN1_BMPSTRING **ppDerBmpString);
    ITCAST_INT DER_ItAsn1_ReadBmpString(
            ITASN1_BMPSTRING *pDerBmpString,
            ITASN1_BMPSTRING **ppBmpString);
    void DER_DI_FreeAnybuf(ITCAST_ANYBUF  * pAnyBuf);
    // 给一个ITCAST_ANYBUF类型空节点指针分配存储空间
    int DER_CREATE_LOW_ITCAST_ANYBUF(ITCAST_ANYBUF *&point);
    // 内联函数
    inline void DER_ITCAST_Free(void *memblock)
    {
        if(memblock)
        {
            free(memblock);
            memblock = NULL;
        }
    }
    // 计算数据类型对应的tag
    inline ITCAST_UINT32 DER_ITASN1_LOW_IDENTIFIER(ITCAST_UINT8 &cTag)
    {
        return cTag & ITCAST_DER_SHORT_ID_MASK;
    }
    // 在堆上创建一个指定大小的数组, 使用指针的引用 == 使用指针的指针
    inline int DER_ITASN1_LOW_CREATEUINT8(ITCAST_UINT8* &point, ITCAST_UINT32 size)
    {
        point = (ITCAST_UINT8*)malloc(size);
        if (point==NULL)
        {
            return 7002;
        }
        //memset(point,0,size)
        return 0;
    }
    // 在堆上创建一个 ITCAST_UINT32 大小的内存
    // 原来的宏参数是指针, 所以此次应该使用指针的引用
    inline int DER_ITASN1_LOW_CREATEUINT32(ITCAST_UINT32* &point)
    {
        point = (ITCAST_UINT32*)malloc(sizeof(ITCAST_UINT32));
        if (point==NULL)
            return 7002;
        memset(point, 0, sizeof(ITCAST_UINT32));
        return 0;
    }
    // 通过检测iResult的值, 返回错误类型
    inline DerErrType DER_ITASN1_LOW_CHECKERR(ITCAST_INT iResult, DerErrType iErrNumber)
    {
        if (iResult == iErrNumber)
            return iErrNumber;
        return NoErr;
    }
    // 释放节点内存
    inline void DER_DACERT_LOW_FREE_ANYBUF(ITCAST_ANYBUF *point)
    {
        DER_ITCAST_Free(point->pData);
        DER_ITCAST_Free(point);
    }

private:
    ItcastLog m_log;       // log类

	const ITCAST_UINT8 TRUE = 1;
	const ITCAST_UINT8 FALSE = 0;
	/* 标记当前数据是原始数据还是编码数据 */
	const ITCAST_UINT32 ITCAST_DER_CONSTRUCTED = 0x20;
	const ITCAST_UINT32 ITCAST_DER_PRIMITIVE = 0x00;

	/* The encodings for the universal types */
	const ITCAST_UINT32 ITCAST_DER_UNIVERSAL = 0x00;
	const ITCAST_UINT32 ITCAST_DER_APPLICATION = 0x40;
	const ITCAST_UINT32 ITCAST_DER_CONTEXT_SPECIFIC = 0x80;
	const ITCAST_UINT32 ITCAST_DER_PRIVATE = 0xC0;

	const ITCAST_UINT32 ITCAST_DER_RESERVED = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_RESERVED);
	const ITCAST_UINT32 ITCAST_DER_BOOLEAN = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_BOOLEAN);
	const ITCAST_UINT32 ITCAST_DER_INTEGER = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_INTEGER);
	const ITCAST_UINT32 ITCAST_DER_BITSTRING = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_BITSTRING);
	const ITCAST_UINT32 ITCAST_DER_OCTETSTRING = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_OCTETSTRING);
	const ITCAST_UINT32 ITCAST_DER_NULL = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_NULL);
	const ITCAST_UINT32 ITCAST_DER_OBJECT_IDENTIFIER = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_OBJECT_IDENTIFIER);
	const ITCAST_UINT32 ITCAST_DER_OBJECT_DESCRIPTOR = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_OBJECT_DESCRIPTOR);
	const ITCAST_UINT32 ITCAST_DER_EXTERNAL = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_EXTERNAL);
	const ITCAST_UINT32 ITCAST_DER_REAL = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_REAL);
	const ITCAST_UINT32 ITCAST_DER_ENUMERATED = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_ENUMERATED);
	const ITCAST_UINT32 ITCAST_DER_EMBEDDED_PDV = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_EMBEDDED_PDV);
	const ITCAST_UINT32 ITCAST_DER_STRING_UTF8 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_UTF8);
	const ITCAST_UINT32 ITCAST_DER_13 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_13);
	const ITCAST_UINT32 ITCAST_DER_14 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_14);
	const ITCAST_UINT32 ITCAST_DER_15 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_15);
	const ITCAST_UINT32 ITCAST_DER_SEQUENCE = (ITCAST_DER_UNIVERSAL | ITCAST_DER_CONSTRUCTED | ITCAST_DER_ID_SEQUENCE);
	const ITCAST_UINT32 ITCAST_DER_SET = (ITCAST_DER_UNIVERSAL | ITCAST_DER_CONSTRUCTED | ITCAST_DER_ID_SET);
	const ITCAST_UINT32 ITCAST_DER_STRING_NUMERIC = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_NUMERIC);
	const ITCAST_UINT32 ITCAST_DER_STRING_PRINTABLE = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_PRINTABLE);
	const ITCAST_UINT32 ITCAST_DER_STRING_T61 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_T61);
	const ITCAST_UINT32 ITCAST_DER_STRING_VIDEOTEX = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_VIDEOTEX);
	const ITCAST_UINT32 ITCAST_DER_STRING_IA5 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_IA5);
	const ITCAST_UINT32 ITCAST_DER_TIME_UTC = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_TIME_UTC);
	const ITCAST_UINT32 ITCAST_DER_TIME_GENERALIZED = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_TIME_GENERALIZED);
	const ITCAST_UINT32 ITCAST_DER_STRING_GRAPHIC = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_GRAPHIC);
	const ITCAST_UINT32 ITCAST_DER_STRING_ISO646 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_ISO646);
	const ITCAST_UINT32 ITCAST_DER_STRING_GENERAL = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_GENERAL);
	const ITCAST_UINT32 ITCAST_DER_STRING_UNIVERSAL = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_UNIVERSAL);
	const ITCAST_UINT32 ITCAST_DER_29 = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_29);
	const ITCAST_UINT32 ITCAST_DER_STRING_BMP = (ITCAST_DER_UNIVERSAL | ITCAST_DER_PRIMITIVE | ITCAST_DER_ID_STRING_BMP);

	/* Masks to extract information from a tag number */
	const ITCAST_UINT32 ITCAST_DER_CLASS_MASK = 0xC0;
	const ITCAST_UINT32 ITCAST_DER_CONSTRUCTED_MASK = 0x20;
	const ITCAST_UINT32 ITCAST_DER_SHORT_ID_MASK = 0x1F;
	const ITCAST_UINT32 ITCAST_DER_FIRST_NOT_ID_MASK = 0x7F;     //xia
	const ITCAST_UINT32 ITCAST_DER_FIRST_YES_ID_MASK = 0x80;     //xia
	const ITCAST_UINT32 ITCAST_DER_ALL_YES_ID_MASK = 0xFF;     //xia
															   /* The maximum size for the short tag number encoding, and the magic value
															   which indicates that a long encoding of the number is being used */
	const ITCAST_UINT32 ITASN1_MAX_SHORT_BER_ID = 30;
	const ITCAST_UINT32 ITASN1_LONG_BER_ID = 0x1F;

};

#endif // BASEASN1_H

```

<br />

<br />

<br />

### BaseASN1.cpp

```cpp
#include "BaseASN1.h"
#include <iostream>
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
using namespace std;

BaseASN1::BaseASN1()
{
}

// 不知道干什么的一个宏, 尼玛...
#define OIDDEF(tag, oidValue) {tag, {oidValue,sizeof(oidValue),0,1,0,0,0} }

ITCAST_INT BaseASN1::DER_ItAsn1_Low_GetTagInfo(ITCAST_UINT8 **ppDerData, ITCAST_UINT32 **ppTagValue, ITCAST_UINT32 **ppTagSize)
{
    ITCAST_UINT8 *pMidData = NULL;
    ITCAST_UINT32 *pMidValue = NULL;
    ITCAST_UINT32 *pMidSize = NULL;

    //初始化
    pMidValue = (ITCAST_UINT32*)malloc(sizeof(ITCAST_UINT32));
    // pMidValue = new ITCAST_UINT32;
    if (pMidValue == NULL)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, MemoryErr, "func DER_ItAsn1_Low_GetTagInfo() err");
        return MemoryErr;
    }
    pMidSize = (ITCAST_UINT32*)malloc(sizeof(ITCAST_UINT32));
    // pMidSize = new ITCAST_UINT32;
    if (pMidSize == NULL)
    {
        if (pMidValue) { free(pMidValue); pMidValue = NULL; }
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, MemoryErr, "func DER_ItAsn1_Low_GetTagInfo() err");
        return MemoryErr;
    }

    *pMidSize = 0;
    *pMidValue = 0;
    pMidData = *ppDerData;
    //读Tag
    if ((*pMidData & ITCAST_DER_SHORT_ID_MASK) != ITCAST_DER_SHORT_ID_MASK)
    {
        if (*pMidData & ITCAST_DER_CONTEXT_SPECIFIC)
            *pMidValue = *(pMidData++);
        else
            *pMidValue = *(pMidData++) & ITCAST_DER_SHORT_ID_MASK;
        (*pMidSize)++;
    }
    else
    {
        do
        {
            *pMidValue = *pMidValue | (*(++pMidData) & ITCAST_DER_FIRST_NOT_ID_MASK);
            *pMidValue <<= 8;
            (*pMidSize)++;
        } while (!(*pMidData & ITCAST_DER_FIRST_YES_ID_MASK));
        *pMidValue |= *(pMidData++);
        (*pMidSize)++;
    }
    //输出变量
    *ppTagValue = pMidValue;
    *ppTagSize = pMidSize;
    *ppDerData = pMidData;
    //中间变量赋空
    pMidValue = NULL;
    pMidSize = NULL;
    pMidData = NULL;

    return 0;
}

ITCAST_UINT32 BaseASN1::DER_ItAsn1_Low_Count_LengthOfSize(ITCAST_UINT32 iLength)
{
    if (iLength <= 0x7F)
        return (1);
    else
        if (iLength <= 0xFF)
            return (2);
        else
            if (iLength < 0xFFFF)
                return (3);
            else
                if (iLength <= 0xFFFFFF)
                    return (4);
                else
                    if (iLength <= 0xFFFFFFFF)
                        return (5);
                    else
                    {
                        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthErr, "func DER_ItAsn1_Low_Count_LengthOfSize() err");
                        return LengthErr;
                    }
}

ITCAST_INT BaseASN1::DER_ItAsn1_GetLengthInfo(ITCAST_ANYBUF *pDerData, int *pLengthValue, int *pLengthSize)
{
    ITCAST_UINT8 *pData;
    int iSizeOf, iSize = 0, i;
    pData = pDerData->pData;
    if ((*pData & ITCAST_DER_SHORT_ID_MASK) != ITCAST_DER_SHORT_ID_MASK)
        pData++;
    else
    {
        ++pData;
        do
        {
            ++pData;
        } while (!(*pData & 80));
    }
    ++pData;
    iSizeOf = *pData & ITCAST_DER_FIRST_NOT_ID_MASK;
    if (!(*pData & 80))
    {
        *pLengthSize = 1;
        *pLengthValue = iSizeOf;
    }
    else
    {
        for (i = 1; i <= iSizeOf; i++)
        {
            iSize |= *(++pData);
            iSize <<= 8;
        }
        *pLengthSize = iSizeOf;
        *pLengthValue = iSize;
    }
    return 0;
}

ITCAST_INT BaseASN1::DER_ItAsn1_Low_GetLengthInfo(ITCAST_UINT8 **ppDerData, ITCAST_UINT32 **ppLengthValue, ITCAST_UINT32 **ppLengthSize)
{
    ITCAST_UINT8 *pMidData, cSizeOf;
    ITCAST_UINT32 lMidLength = 0, *pMidLength, *pSizeOf, i;

    //初始化
    DER_ITASN1_LOW_CREATEUINT32(pSizeOf);
    DER_ITASN1_LOW_CREATEUINT32(pMidLength);
    pMidData = *ppDerData;
    //读长度
    if (!(*pMidData & ITCAST_DER_FIRST_YES_ID_MASK))//short
    {
        cSizeOf = 1;
        lMidLength = (ITCAST_UINT32)(*(pMidData++) & ITCAST_DER_FIRST_NOT_ID_MASK);
    }
    else                                //long
    {
        cSizeOf = *(pMidData++) & ITCAST_DER_FIRST_NOT_ID_MASK;
        if (cSizeOf > 4/* ||cSizeOf <0*/)
        {
            if (pSizeOf) { free(pSizeOf); pSizeOf = NULL; }
            if (pMidLength)
            {
                free(pMidLength);
            }
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthErr, "func DER_ItAsn1_Low_GetLengthInfo() err");
            return LengthErr;
        }
        else
        {
            for (i = 1; i < cSizeOf; i++)
            {
                lMidLength |= (ITCAST_UINT32)*(pMidData++);
                lMidLength <<= 8;
            }
            lMidLength |= (ITCAST_UINT32)*(pMidData++);
            cSizeOf++;
        }
    }
    //输出信息
    *pMidLength = lMidLength;
    *pSizeOf = cSizeOf;
    *ppLengthValue = pMidLength;
    *ppLengthSize = pSizeOf;
    *ppDerData = pMidData;
    //中间变量赋空
    pMidData = NULL;
    pMidLength = NULL;
    pSizeOf = NULL;

    return 0;
}

//将一个ITCAST_UINT32类型的整数转换成字符表示形式
ITCAST_INT BaseASN1::DER_ItAsn1_Low_IntToChar(ITCAST_UINT32 integer, ITCAST_UINT8 **ppData, ITCAST_UINT32 **ppLength)
{
    ITCAST_UINT8 *pMidData = NULL, *pMidSite = NULL;
    ITCAST_UINT32 *pMidLength = NULL;
    ITCAST_UINT32  iValue;

    //初始化
    iValue = integer;
    DER_ITASN1_LOW_CREATEUINT32(pMidLength);
    if (iValue > 0xFFFFFFFF)//检测整数值
        return DataRangeErr;
    else
    {
        if (iValue > 0xFFFFFF)
        {
            if (iValue >= 0x80000000)
            {
                DER_ITASN1_LOW_CREATEUINT8(pMidData, 5);
                pMidSite = pMidData;
                *(pMidData++) = 0;
                *pMidLength = 1;
            }
            else
            {
                DER_ITASN1_LOW_CREATEUINT8(pMidData, 4);
                pMidSite = pMidData;
            }
            *(pMidData++) = (ITCAST_UINT8)(iValue >> 24);
            *(pMidData++) = (ITCAST_UINT8)(iValue >> 16);
            *(pMidData++) = (ITCAST_UINT8)(iValue >> 8);
            *(pMidData++) = (ITCAST_UINT8)(iValue);
            (*pMidLength) += 4;
        }
        else
            if (iValue > 0xFFFF)
            {
                if (iValue >= 0x800000)
                {
                    DER_ITASN1_LOW_CREATEUINT8(pMidData, 4);
                    pMidSite = pMidData;
                    *(pMidData++) = 0;
                    *pMidLength = 1;
                }
                else
                {
                    DER_ITASN1_LOW_CREATEUINT8(pMidData, 3);
                    pMidSite = pMidData;
                }
                *(pMidData++) = (ITCAST_UINT8)(iValue >> 16);
                *(pMidData++) = (ITCAST_UINT8)(iValue >> 8);
                *(pMidData++) = (ITCAST_UINT8)(iValue);
                (*pMidLength) += 3;
            }
            else
                if (iValue > 0xFF)
                {
                    if (iValue >= 0x8000)
                    {
                        DER_ITASN1_LOW_CREATEUINT8(pMidData, 3);
                        pMidSite = pMidData;
                        *(pMidData++) = 0;
                        *pMidLength = 1;
                    }
                    else
                    {
                        DER_ITASN1_LOW_CREATEUINT8(pMidData, 2);
                        pMidSite = pMidData;
                    }
                    *(pMidData++) = (ITCAST_UINT8)(iValue >> 8);
                    *(pMidData++) = (ITCAST_UINT8)(iValue);
                    (*pMidLength) += 2;
                }
                else
                {
#if 0   // 尼玛iValue是unsigned int 这个条件永远成立
                    if (iValue >= 0x0)
                    {
#endif
                        if (iValue >= 0x80)
                        {
                            DER_ITASN1_LOW_CREATEUINT8(pMidData, 2);
                            pMidSite = pMidData;
                            *(pMidData++) = 0;
                            *pMidLength = 1;
                        }
                        else
                        {
                            DER_ITASN1_LOW_CREATEUINT8(pMidData, 1);
                            pMidSite = pMidData;
                        }
                        *pMidData = (ITCAST_UINT8)(iValue);
                        (*pMidLength) += 1;
                    }
#if 0
                }
#endif
    }
    //信息输出
    *ppData = pMidSite;
    *ppLength = pMidLength;
    //中间变量赋空
    pMidData = NULL;
    pMidSite = NULL;
    pMidLength = NULL;

    return 0;
}

//将一个用字符表示的整数转换成ITCAST_UINT32型整数
ITCAST_INT BaseASN1::DER_ItAsn1_Low_CharToInt(ITCAST_UINT8 *aData, ITCAST_UINT32 lLength, ITCAST_UINT32 **ppInteger)
{
    ITCAST_UINT32 lIntMid = 0, i;
    ITCAST_UINT32 *pIntMid = NULL;

    DER_ITASN1_LOW_CREATEUINT32(pIntMid);
    //转换
    if ((*aData == 0) && (lLength > 1))
    {
        aData++;
        lLength--;
    }
    for (i = 1; i < lLength; i++)
    {

        lIntMid |= *aData++;
        lIntMid <<= 8;
    }
    lIntMid |= *aData++;
    //输出
    *pIntMid = lIntMid;
    *ppInteger = pIntMid;
    //中间变量赋空
    pIntMid = NULL;

    return 0;
}

//写Tag和Length值
ITCAST_INT BaseASN1::DER_ItAsn1_Low_WriteTagAndLength(ITCAST_ANYBUF *pAnyIn, ITCAST_UINT8 cTag, ITCAST_ANYBUF **ppAnyOut, ITCAST_UINT8 **ppUint8Value)
{
    ITCAST_ANYBUF *pMidAny = NULL;
    ITCAST_UINT8 *pMidValue = NULL, cIdentifier;
    ITCAST_UINT32 iMidSize, iMidSizeOf, iMidLength, i;

    //计算长度,Bitstring和Integer类型与其他类型分开处理
    if ((cTag != ITCAST_DER_ID_BITSTRING) && (cTag != ITCAST_DER_ID_INTEGER))
        iMidSize = pAnyIn->dataLen;
    else
        if (cTag == ITCAST_DER_ID_INTEGER)
            if (!(*(pAnyIn->pData) & ITCAST_DER_FIRST_YES_ID_MASK))
                iMidSize = pAnyIn->dataLen;
            else
                iMidSize = pAnyIn->dataLen + 1;
        else
            iMidSize = pAnyIn->dataLen + 1;
    iMidLength = iMidSize;
    iMidSizeOf = DER_ItAsn1_Low_Count_LengthOfSize(iMidSize);
    if (/*iMidSizeOf < 0 || */iMidSizeOf > 5)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthErr, "func DER_ItAsn1_Low_WriteTagAndLength() err");
        return LengthErr;
    }
    iMidSize += 1 + iMidSizeOf;
    DER_CREATE_LOW_ITCAST_ANYBUF(pMidAny);
    DER_ITASN1_LOW_CREATEUINT8(pMidValue, iMidSize);
    if (iMidSize == 11)
    {
        iMidSize = 11;
    }
    pMidAny->pData = pMidValue;
    //检测Tag值
    if (cTag & ITCAST_DER_CONTEXT_SPECIFIC)
        cIdentifier = cTag;
    else
    {
        cIdentifier = DER_ITASN1_LOW_IDENTIFIER(cTag);
        //if (cIdentifier != pAnyIn ->dataType)
        //    return MemoryErr;
    }

    *(pMidValue++) = cTag;
    pMidAny->dataType = (ITCAST_UINT32)cIdentifier;
    pMidAny->dataLen = iMidSize;
    if (iMidSizeOf == 1)
    {
        *(pMidValue++) = (ITCAST_UINT8)(iMidLength);
    }
    else
    {
        *(pMidValue++) = ITCAST_DER_FIRST_YES_ID_MASK | (ITCAST_UINT8)(iMidSizeOf - 1);
        for (i = iMidSizeOf - 1; i > 0; i--)
        {
            *(pMidValue++) = (ITCAST_UINT8)(iMidLength >> 8 * (i - 1));
        }
    }
    if (pMidAny->dataType == ITCAST_DER_ID_BITSTRING)
    {
        pMidAny->unusedBits = pAnyIn->unusedBits;
        *(pMidValue++) = (ITCAST_UINT8)(pAnyIn->unusedBits);
    }
    if (pMidAny->dataType == ITCAST_DER_ID_INTEGER)
    {
        if (*(pAnyIn->pData) & ITCAST_DER_FIRST_YES_ID_MASK)
            *(pMidValue++) = 0x0;
    }
    //输出信息
    *ppUint8Value = pMidValue;
    *ppAnyOut = pMidAny;

    pMidAny = NULL;
    pMidValue = NULL;

    return 0;
}

//读Tag和Length值
ITCAST_INT BaseASN1::DER_ItAsn1_Low_ReadTagAndLength(ITCAST_ANYBUF *pAnyIn, ITCAST_UINT8 **ppUint8Data, ITCAST_ANYBUF **ppAnyOut, ITCAST_UINT8 **ppUint8Value)
{
    ITCAST_ANYBUF *pMidAny = NULL;
    ITCAST_UINT32 *pMidTag = NULL;
    ITCAST_UINT8 *pMidValue = NULL;
    ITCAST_UINT32 *pMidSize = NULL, *pMidSizeOf = NULL, iMidLength = 0;

    DER_CREATE_LOW_ITCAST_ANYBUF(pMidAny);
    DER_ItAsn1_Low_GetTagInfo(ppUint8Data, &pMidTag, &pMidSize);
    //检测Tag值是否正确
    if (!(*pMidTag == ITCAST_DER_ID_STRING_PRINTABLE) || (*pMidTag == ITCAST_DER_ID_STRING_BMP))
    {
        /***************************************
            *****  if (*pMidTag != pAnyIn ->dataType)
            ****    return MemoryErr;
            ***************************************wyy*/
    }
    pMidAny->dataType = *pMidTag;
    iMidLength += *pMidSize;
    DER_ITCAST_Free(pMidSize);
    DER_ItAsn1_Low_GetLengthInfo(ppUint8Data, &pMidSize, &pMidSizeOf);
    //检测总长度是否正确
    iMidLength += *pMidSize + *pMidSizeOf;
    if (iMidLength != pAnyIn->dataLen)
    {
        DER_ITCAST_Free(pMidSize);
        DER_ITCAST_Free(pMidSizeOf);
        DER_ITCAST_Free(pMidTag);
        DER_ITCAST_Free(pMidAny);
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthNotEqual, "func DER_ItAsn1_Low_ReadTagAndLength() err");
        return LengthNotEqual;

    }

    //Bitstring和Integer类型与其他类型不同
    if (pAnyIn->dataType == ITCAST_DER_ID_BITSTRING)
    {
        pMidAny->unusedBits = pAnyIn->unusedBits;
        //检测unusedBits值是否正确
        //if ((ITCAST_UINT8)(pMidAny ->unusedBits) != **ppUint8Data)
        //	return MemoryErr;
        (*ppUint8Data)++;
        (*pMidSize)--;
    }
    if (*pMidTag == ITCAST_DER_ID_INTEGER)
    {
        pMidValue = *ppUint8Data;
        if ((*pMidValue == 0x0) && (*(++pMidValue) & ITCAST_DER_FIRST_YES_ID_MASK))
        {
            (*ppUint8Data)++;
            (*pMidSize)--;
        }
        pMidValue = NULL;
    }
    DER_ITCAST_Free(pMidTag);
    //创建pMidAny的pData空间
    if (*pMidSize > 0)
    {
        DER_ITASN1_LOW_CREATEUINT8(pMidValue, *pMidSize);
        if (pMidValue == NULL)
        {
            DER_ITCAST_Free(pMidSize);
            DER_ITCAST_Free(pMidSizeOf);
            DER_ITCAST_Free(pMidAny);
            return MemoryErr;
        }
    }
    else
    {
        pMidValue = NULL;
    }

    //输出
    *ppUint8Value = pMidValue;
    pMidAny->dataLen = *pMidSize;
    pMidAny->pData = *ppUint8Value;
    *ppAnyOut = pMidAny;
    DER_ITCAST_Free(pMidSize);
    DER_ITCAST_Free(pMidSizeOf);

    //中间变量赋空
    pMidAny = NULL;
    pMidValue = NULL;

    return 0;
}

//DER编码整数数据
ITCAST_INT BaseASN1::DER_ItAsn1_WriteInteger(ITCAST_UINT32 integer, ITASN1_INTEGER **ppDerInteger)
{
    ITCAST_UINT8 *pData, *pMidSite;
    ITCAST_UINT32 *pLength;
    ITASN1_INTEGER *pInteger = NULL;
    ITCAST_UINT8 *pMidUint8 = NULL, cTag = ITCAST_DER_INTEGER;
    int iResult;

    //编码pData域
    iResult = DER_ItAsn1_Low_IntToChar(integer, &pData, &pLength);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_IntToChar() err != NoErr");
        return  iResult;
    }
    DER_ITASN1_LOW_CREATEUINT8(pMidUint8, *pLength + 2);
    pMidSite = pMidUint8;
    *(pMidUint8++) = cTag;
    *(pMidUint8++) = (ITCAST_UINT8)*pLength;
    memcpy(pMidUint8, pData, *pLength);
    //形成ITASN1_INTEGER结构
    DER_CREATE_LOW_ITCAST_ANYBUF(pInteger);
    DER_ITASN1_LOW_CHECKERR(iResult, MemoryErr);
    pInteger->pData = pMidSite;
    pInteger->unusedBits = 0;
    pInteger->dataType = DER_ITASN1_LOW_IDENTIFIER(cTag);
    pInteger->memoryType = MallocMemory;
    pInteger->dataLen = *pLength + 2;
    pInteger->next = NULL;
    pInteger->prev = NULL;
    //输出
    *ppDerInteger = pInteger;
    DER_ITCAST_Free(pData);
    DER_ITCAST_Free(pLength);
    //中间变量赋空
    pMidSite = NULL;
    pInteger = NULL;
    pMidUint8 = NULL;

    return 0;
}

//DER解码整数数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadInteger(ITASN1_INTEGER *pDerInteger, ITCAST_UINT32 *pInteger)
{
    ITCAST_UINT8 *pMidData = NULL;
    ITCAST_UINT32 *pTag = NULL;
    ITCAST_UINT32 lMidLength, *pMidSize = NULL, *pMidSizeOf = NULL;
    int iResult;
    //解码
    pMidData = pDerInteger->pData;
    iResult = DER_ItAsn1_Low_GetTagInfo(&pMidData, &pTag, &pMidSize);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_ReadInteger() err check iResult != NoErr");
        return  iResult;
    }
    DER_ITCAST_Free(pTag);
    DER_ITCAST_Free(pMidSize);
    iResult = DER_ItAsn1_Low_GetLengthInfo(&pMidData, &pMidSize, &pMidSizeOf);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_GetLengthInfo() err");
        return  iResult;
    }
    lMidLength = *pMidSize;
    DER_ITCAST_Free(pMidSize);
    DER_ITCAST_Free(pMidSizeOf);
    iResult = DER_ItAsn1_Low_CharToInt(pMidData, lMidLength, &pMidSize);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_CharToInt() err");
        return  iResult;
    }
    //输出
    *pInteger = *pMidSize;
    DER_ITCAST_Free(pMidSize);
    //中间变量赋空
    pMidData = NULL;

    return 0;
}

//DER编码BitString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_WriteBitString(ITASN1_BITSTRING *pBitString, ITASN1_BITSTRING **ppDerBitString)
{
    ITASN1_BITSTRING *pMidBitString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    ITCAST_UINT8 cTag = ITCAST_DER_BITSTRING;
    int iResult;

    //编码
    iResult = DER_ItAsn1_Low_WriteTagAndLength(pBitString, cTag, &pMidBitString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_WriteTagAndLength() err");
        return  iResult;
    }

    pMidData = pBitString->pData;
    memcpy(pMidValue, pMidData, pBitString->dataLen);
    //输出
    *ppDerBitString = pMidBitString;
    //中间变量赋空
    pMidBitString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER解码BitString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadBitString(ITASN1_BITSTRING *pDerBitString, ITASN1_BITSTRING **ppBitString)
{
    ITASN1_BITSTRING *pMidBitString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    int iResult;

    //解码
    pMidData = pDerBitString->pData;
    iResult = DER_ItAsn1_Low_ReadTagAndLength(pDerBitString, &pMidData, &pMidBitString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_ReadTagAndLength() err");
        return iResult;
    }
    memcpy(pMidValue, pMidData, pMidBitString->dataLen);
    //输出
    *ppBitString = pMidBitString;
    //中间变量赋空
    pMidBitString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER编码CharString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_WriteCharString(ITCAST_ANYBUF *pCharString, ITCAST_ANYBUF **ppDerCharString)
{
    ITCAST_ANYBUF *pMidCharString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    ITCAST_UINT8 cTag = ITCAST_DER_ID_STRING_PRINTABLE;
    int iResult;

    //编码
    iResult = DER_ItAsn1_Low_WriteTagAndLength(pCharString, cTag, &pMidCharString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_WriteTagAndLength() err");
        return  iResult;
    }

    pMidData = pCharString->pData;
    memcpy(pMidValue, pMidData, pCharString->dataLen);
    //输出
    *ppDerCharString = pMidCharString;
    //中间变量赋空
    pMidCharString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER解码PrintableString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadCharString(ITCAST_ANYBUF *pDerCharString, ITCAST_ANYBUF **ppCharString)
{
    ITCAST_ANYBUF *pMidCharString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    int iResult;

    //解码
    pMidData = pDerCharString->pData;
    iResult = DER_ItAsn1_Low_ReadTagAndLength(pDerCharString, &pMidData, &pMidCharString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_ReadTagAndLength() err");
        return  iResult;
    }

    memcpy(pMidValue, pMidData, pMidCharString->dataLen);
    //输出
    *ppCharString = pMidCharString;
    //中间变量赋空
    pMidCharString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER编码BmpString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_WriteBmpString(ITASN1_BMPSTRING *pBmpString, ITASN1_BMPSTRING **ppDerBmpString)
{
    ITCAST_ANYBUF *pMidBmpString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    ITCAST_UINT8 cTag = ITCAST_DER_ID_STRING_BMP;
    int iResult;

    //编码
    iResult = DER_ItAsn1_Low_WriteTagAndLength(pBmpString, cTag, &pMidBmpString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_WriteTagAndLength() err");
        return  iResult;
    }

    pMidData = pBmpString->pData;
    memcpy(pMidValue, pMidData, pBmpString->dataLen);
    //输出
    *ppDerBmpString = pMidBmpString;
    //中间变量赋空
    pMidBmpString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER解码BmpString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadBmpString(ITASN1_BMPSTRING *pDerBmpString, ITASN1_BMPSTRING **ppBmpString)
{
    ITCAST_ANYBUF *pMidBmpString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    int iResult;

    //解码
    pMidData = pDerBmpString->pData;
    iResult = DER_ItAsn1_Low_ReadTagAndLength(pDerBmpString, &pMidData, &pMidBmpString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_ReadTagAndLength() err");
        return  iResult;
    }

    memcpy(pMidValue, pMidData, pMidBmpString->dataLen);
    //输出
    *ppBmpString = pMidBmpString;
    //中间变量赋空
    pMidBmpString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER编码PrintableString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_WritePrintableString(ITASN1_PRINTABLESTRING *pPrintString, ITASN1_PRINTABLESTRING **ppDerPrintString)
{
    int iResult;

    if (pPrintString->dataType == ITCAST_DER_STRING_BMP)
    {
        iResult = DER_ItAsn1_WriteBmpString(pPrintString, ppDerPrintString);
        if (iResult != NoErr)
        {
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_WriteBmpString() err");
            return  iResult;
        }

    }
    else
    {
        iResult = DER_ItAsn1_WriteCharString(pPrintString, ppDerPrintString);
        if (iResult != NoErr)
        {
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_WriteCharString() err");
            return  iResult;
        }
    }

    return 0;
}

//DER解码PrintableString类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadPrintableString(ITASN1_PRINTABLESTRING *pDerPrintString, ITASN1_PRINTABLESTRING **ppPrintString)
{
    ITASN1_PRINTABLESTRING *pMidPrintString = NULL;
    ITCAST_UINT8 *pMidData = NULL, *pMidValue = NULL;
    int iResult;

    //解码
    pMidData = pDerPrintString->pData;
    iResult = DER_ItAsn1_Low_ReadTagAndLength(pDerPrintString, &pMidData, &pMidPrintString, &pMidValue);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_ReadTagAndLength() err");
        return  iResult;
    }
    memcpy(pMidValue, pMidData, pMidPrintString->dataLen);
    //输出
    *ppPrintString = pMidPrintString;
    //中间变量赋空
    pMidPrintString = NULL;
    pMidData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER编码Sequence类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_WriteSequence(ITASN1_SEQUENCE *pSequence, ITCAST_ANYBUF **ppDerSequence)
{
    ITASN1_SEQUENCE *pMidSequence = NULL, *pMidNext1 = NULL;
    ITCAST_UINT8 *pMidValue = NULL, *pMidSite;
    ITCAST_UINT32 lSizeOf = 0, i, lMidLength = 0;
    ITCAST_UINT8 cTag = ITCAST_DER_SEQUENCE;


    //计算Sequence数据长度
    pMidNext1 = pSequence;
    while (pMidNext1 != NULL)
    {
        lMidLength += pMidNext1->dataLen;
        pMidNext1 = pMidNext1->next;
    }
    lSizeOf = DER_ItAsn1_Low_Count_LengthOfSize(lMidLength);
    DER_ITASN1_LOW_CREATEUINT8(pMidValue, lMidLength + 1 + lSizeOf);

    pMidSite = pMidValue;
    //写Tag值
    *(pMidValue++) = cTag;
    //写长度
    if (lSizeOf == 1)
        *(pMidValue++) = (ITCAST_UINT8)lMidLength;
    else
    {
        *(pMidValue++) = ITCAST_DER_FIRST_YES_ID_MASK | ((ITCAST_UINT8)(lSizeOf - 1));
        for (i = lSizeOf - 1; i > 0; i--)
            *(pMidValue++) = (ITCAST_UINT8)(lMidLength >> 8 * (i - 1));
    }
    pMidNext1 = pSequence;
    //copy数据
    while (pMidNext1 != NULL)
    {
        memcpy(pMidValue, pMidNext1->pData, pMidNext1->dataLen);

        pMidValue += pMidNext1->dataLen;
        pMidNext1 = pMidNext1->next;
    }
    //创建ITCAST_ANYBUF结构
    DER_CREATE_LOW_ITCAST_ANYBUF(pMidSequence);
    pMidSequence->dataLen = lMidLength + 1 + lSizeOf;
    pMidSequence->pData = pMidSite;
    pMidSequence->dataType = DER_ITASN1_LOW_IDENTIFIER(cTag);
    //输出
    *ppDerSequence = pMidSequence;
    //中间变量赋空
    pMidSequence = NULL;
    pMidNext1 = NULL;
    pMidValue = NULL;
    pMidSite = NULL;

    return 0;
}

void BaseASN1::DER_DI_FreeAnybuf(ITCAST_ANYBUF  * pAnyBuf)
{
    ITCAST_ANYBUF * pTmp;
    pTmp = pAnyBuf;
    if (pAnyBuf == NULL)
    {
        return;
    }
    while (pAnyBuf->next)
    {
        pTmp = pAnyBuf->next;
        if (pAnyBuf->pData)
        {
            DER_ITCAST_Free(pAnyBuf->pData);
        }
        DER_ITCAST_Free(pAnyBuf);
        pAnyBuf = pTmp;
    }
    if (pAnyBuf->pData)
    {
        DER_ITCAST_Free(pAnyBuf->pData);
    }
    DER_ITCAST_Free(pAnyBuf);
    return;
}

//DER解码Sequence类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadSequence(ITCAST_ANYBUF *pDerSequence, ITASN1_SEQUENCE **ppSequence)
{
    ITASN1_SEQUENCE *pMidNext1 = NULL, *pMidNext2 = NULL, *pMidSequence = NULL;
    ITCAST_UINT8   *pMidData = NULL, *pMidItemData = NULL, *pMidValue = NULL;
    ITCAST_UINT32  lMidLength = 0, *pTagValue = NULL, *pLengthValue = NULL;
    ITCAST_UINT32  *pTagSize = NULL, *pLengthSize = NULL;
    int        lTotalLength = 0;
    int iResult;
    ITCAST_UINT8 firstTag = TRUE;
    int i = 0;

    //初始化
    pMidData = pDerSequence->pData;
    //检测Tag值
    iResult = DER_ItAsn1_Low_GetTagInfo(&pMidData, &pTagValue, &pTagSize);
    if (iResult != NoErr)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "func DER_ItAsn1_Low_GetTagInfo() err");
        return  iResult;
    }

    if ((ITCAST_UINT8)*pTagValue != ITCAST_DER_ID_SEQUENCE)
    {
        DER_ITCAST_Free(pTagValue);
        DER_ITCAST_Free(pTagSize);
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, InvalidTag, "check pTagValue != ITCAST_DER_ID_SEQUENCE err");
        return InvalidTag;
    }
    //检测长度值
    iResult = DER_ItAsn1_Low_GetLengthInfo(&pMidData, &pLengthValue, &pLengthSize);
    if (iResult != NoErr)
    {
        DER_ITCAST_Free(pTagValue);
        DER_ITCAST_Free(pTagSize);
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "fun DER_ItAsn1_Low_GetLengthInfo() err");
        return iResult;
    }

    if (pDerSequence->dataLen != *pTagSize + *pLengthSize + *pLengthValue)
    {
        DER_ITCAST_Free(pLengthValue);
        DER_ITCAST_Free(pLengthSize);
        DER_ITCAST_Free(pTagValue);
        DER_ITCAST_Free(pTagSize);
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthNotEqual, "fun check length  err");
        return LengthNotEqual;
    }
    lTotalLength = *pLengthValue;
    DER_ITCAST_Free(pTagValue);
    DER_ITCAST_Free(pTagSize);
    DER_ITCAST_Free(pLengthValue);
    DER_ITCAST_Free(pLengthSize);
    //创建Sequence链,头及其余元素
    while (lTotalLength > 0)
    {
        pMidItemData = pMidData;
        iResult = DER_ItAsn1_Low_GetTagInfo(&pMidItemData, &pTagValue, &pTagSize);
        if (iResult != NoErr)
        {
            DER_ITCAST_Free(pTagValue);
            DER_ITCAST_Free(pTagSize);
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "fun DER_ItAsn1_Low_GetTagInfo() err");
            return iResult;
        }
        iResult = DER_ItAsn1_Low_GetLengthInfo(&pMidItemData, &pLengthValue, &pLengthSize);
        if (iResult != NoErr)
        {
            DER_ITCAST_Free(pLengthValue);
            DER_ITCAST_Free(pLengthSize);
            DER_ITCAST_Free(pTagValue);
            DER_ITCAST_Free(pTagSize);
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, iResult, "fun DER_ItAsn1_Low_GetLengthInfo() err");
            return iResult;
        }
        lMidLength = *pTagSize + *pLengthSize + *pLengthValue;

        DER_ITASN1_LOW_CREATEUINT8(pMidValue, lMidLength);

        if (pMidValue == NULL)
        {
            DER_ITCAST_Free(pLengthValue);
            DER_ITCAST_Free(pLengthSize);
            DER_ITCAST_Free(pTagValue);
            DER_ITCAST_Free(pTagSize);
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, MemoryErr, "pMidValue is null err");
            return MemoryErr;
        }


        memcpy(pMidValue, pMidData, lMidLength);
        if (firstTag)
        {
            firstTag = FALSE;
            DER_CREATE_LOW_ITCAST_ANYBUF(pMidSequence);
            pMidSequence->pData = pMidValue;
            pMidSequence->dataType = *pTagValue;
            pMidSequence->dataLen = lMidLength;
            pMidNext2 = pMidSequence;
            pMidNext2->prev = NULL;
            pMidNext2->next = NULL;
        }
        else
        {
            DER_CREATE_LOW_ITCAST_ANYBUF(pMidNext1);
            pMidNext1->pData = pMidValue;
            pMidNext1->dataType = *pTagValue;
            pMidNext1->dataLen = lMidLength;
            pMidNext1->prev = pMidNext2;
            pMidNext2->next = pMidNext1;
            pMidNext2 = pMidNext2->next;
            pMidNext2->next = NULL;

        }
        pMidData += lMidLength;
        lTotalLength -= lMidLength;
        DER_ITCAST_Free(pTagValue);
        DER_ITCAST_Free(pTagSize);
        DER_ITCAST_Free(pLengthValue);
        DER_ITCAST_Free(pLengthSize);
        i++;
    }
    if (lTotalLength != 0)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthErr, "lTotalLength != 0 err");
        DER_DI_FreeAnybuf(pMidSequence);
        return LengthErr;
    }
    //信息返回
    *ppSequence = pMidSequence;
    //中间变量赋空
    pMidNext1 = NULL;
    pMidNext2 = NULL;
    pMidSequence = NULL;
    pMidData = NULL;
    pMidItemData = NULL;
    pMidValue = NULL;

    return 0;
}

//DER编码Null类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_WriteNull(ITCAST_ANYBUF **ppDerNull)
{
    ITCAST_ANYBUF *pMidNull = NULL;
    ITCAST_UINT8 *pMidValue = NULL, *pMidSite, cTag = ITCAST_DER_NULL;

    //编码
    DER_ITASN1_LOW_CREATEUINT8(pMidValue, 2);
    pMidSite = pMidValue;
    *(pMidValue++) = cTag;
    *pMidValue = 0x0;
    //创建ITCAST_ANYBUF类型结构
    DER_CREATE_LOW_ITCAST_ANYBUF(pMidNull);
    pMidNull->pData = pMidSite;
    pMidNull->dataLen = 2;
    pMidNull->dataType = DER_ITASN1_LOW_IDENTIFIER(cTag);
    //输出
    *ppDerNull = pMidNull;
    //中间变量赋空
    pMidNull = NULL;
    pMidValue = NULL;
    pMidSite = NULL;

    return 0;
}

//DER解码Null类型数据
ITCAST_INT BaseASN1::DER_ItAsn1_ReadNull(ITCAST_ANYBUF *pDerNull, ITCAST_UINT8 *pInt)
{
    unsigned char  temp[2];
    unsigned char  derNULL[2];
    memcpy(temp, pDerNull->pData, 1);
    memcpy(temp + 1, pDerNull->pData + 1, 1);
    memset(derNULL, 5, 1);
    memset(derNULL + 1, 0, 1);

    if (memcmp(temp, derNULL, 2))
    {
        *pInt = 0;
        return 1;
    }

    *pInt = 5;

    return 0;
}

ITCAST_INT BaseASN1::DER_ITCAST_FreeQueue(ITCAST_ANYBUF *pAnyBuf)
{
    ITCAST_ANYBUF * pTmp;
    pTmp = pAnyBuf;
    if (pAnyBuf == NULL)
    {
        return NoErr;
    }
    while (pAnyBuf->next)
    {
        pTmp = pAnyBuf->next;
        if (pAnyBuf->pData)
        {
            DER_ITCAST_Free(pAnyBuf->pData);
        }
        DER_ITCAST_Free(pAnyBuf);
        pAnyBuf = pTmp;
    }
    if (pAnyBuf->pData)
    {
        DER_ITCAST_Free(pAnyBuf->pData);
    }
    DER_ITCAST_Free(pAnyBuf);
    return NoErr;
}

ITCAST_INT BaseASN1::DER_ITCAST_String_To_AnyBuf(ITCAST_ANYBUF **pOriginBuf, unsigned char *strOrigin, int strOriginLen)
{
    ITCAST_ANYBUF *pValueBuf;

    pValueBuf = (ITCAST_ANYBUF*)malloc(sizeof(ITCAST_ANYBUF));
    if (pValueBuf == NULL)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, MemoryErr, "malloc err");
        return MemoryErr;
    }
    memset(pValueBuf, 0, sizeof(ITCAST_ANYBUF));

    if (strOriginLen <= 0)
    {
        pValueBuf->pData = NULL;
        strOriginLen = 0;
    }
    else
    {
        pValueBuf->pData = (unsigned char *)malloc(strOriginLen);
        if (pValueBuf->pData == NULL)
        {
            DER_ITCAST_Free(pValueBuf);
            m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, LengthErr, "malloc err");
            return MemoryErr;
        }
        memcpy(pValueBuf->pData, strOrigin, strOriginLen);
    }
    pValueBuf->dataLen = strOriginLen;
    pValueBuf->dataType = ITCAST_DER_ID_STRING_PRINTABLE;
    pValueBuf->next = NULL;
    pValueBuf->prev = NULL;
    pValueBuf->unusedBits = (strOriginLen % 8);
    pValueBuf->memoryType = 0;
    *pOriginBuf = pValueBuf;
    return NoErr;
}

#define  DER_INPUTDATA_ERR 106
//对空指针进行编码
int BaseASN1::WriteNullSequence(ITCAST_ANYBUF **pOutData)
{
    int				rv = 0;
    ITCAST_ANYBUF		*pTmp = NULL, *pHead = NULL;

    rv = DER_ItAsn1_WriteNull(&pTmp);
    if (rv != 0)
    {
        DER_ITCAST_FreeQueue(pTmp);
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, rv, "func DER_ItAsn1_WriteNull() err");
        return rv;
    }

    rv = DER_ItAsn1_WriteSequence(pTmp, &pHead);
    if (rv != 0)
    {
        DER_ITCAST_FreeQueue(pTmp);
        DER_ITCAST_FreeQueue(pHead);
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, rv, "func DER_ItAsn1_WriteSequence() err");
        return rv;
    }
    DER_ITCAST_FreeQueue(pTmp);
    if (pHead == NULL)
    {
        m_log.Log(__FILE__, __LINE__, ItcastLog::ERROR, -1, " check (pHead == NULL) err");
        return -1;
    }
    *pOutData = pHead;
    return 0;
}

int BaseASN1::EncodeUnsignedChar(unsigned char *pData, int dataLen, ITCAST_ANYBUF **outBuf)
{
    ITCAST_ANYBUF	*pHeadBuf = NULL, *pTmp = NULL;
    int			rv;

    //输入值不合法
    if (pData == NULL && dataLen != 0)
    {

        return DER_INPUTDATA_ERR;
    }
    //输入值不合法
    else if (pData != NULL && dataLen == 0)
    {
        return DER_INPUTDATA_ERR;
    }
    else if (pData == NULL && dataLen == 0)
    {
        rv = DER_ITCAST_String_To_AnyBuf(&pTmp, NULL, 0);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pTmp);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");

            return rv;
        }

        rv = DER_ItAsn1_WriteBitString(pTmp, &pHeadBuf);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pTmp);
            DER_ITCAST_FreeQueue(pHeadBuf);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");
            return rv;
        }

    }
    else
    {
        rv = DER_ITCAST_String_To_AnyBuf(&pTmp, pData, dataLen);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pTmp);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");
            return -1;
        }
        rv = DER_ItAsn1_WriteBitString(pTmp, &pHeadBuf);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pHeadBuf);
            DER_ITCAST_FreeQueue(pTmp);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");

            return -1;
        }
    }
    DER_ITCAST_FreeQueue(pTmp);

    *outBuf = pHeadBuf;

    return 0;
}

int BaseASN1::DecodeUnsignedChar(ITCAST_ANYBUF *inBuf, unsigned char **Data, int *pDataLen)
{
    ITCAST_ANYBUF	*pTmp = NULL;
    int			rv;

    rv = DER_ItAsn1_ReadBitString(inBuf, &pTmp);
    if (rv != 0)
    {
        DER_ITCAST_FreeQueue(pTmp);
        //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"DecodeUnsignedChar  error");
        return -1;
    }

    if (pTmp->dataLen == 0)
    {
        DER_ITCAST_FreeQueue(pTmp);
        *Data = NULL;
        *pDataLen = 0;
        return 0;
    }

    *Data = (unsigned char*)malloc(pTmp->dataLen + 1);
    if (*Data == NULL)
    {
        DER_ITCAST_FreeQueue(pTmp);
        //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"DecodeUnsignedChar_an Mallco *Data Malloc  error");
        return -1;
    }
    memset(*Data, 0, pTmp->dataLen + 1);
    memcpy(*Data, pTmp->pData, pTmp->dataLen);
    *pDataLen = pTmp->dataLen;

    DER_ITCAST_FreeQueue(pTmp);

    return 0;
}

int BaseASN1::EncodeChar(char *pData, int dataLen, ITCAST_ANYBUF **outBuf)
{
    ITCAST_ANYBUF	*pHeadBuf = NULL, *pTmp = NULL;
    int			rv;

    //输入值不合法
    if (pData == NULL && dataLen != 0)
    {
        return DER_INPUTDATA_ERR;
    }
    //输入值不合法
    else if (pData != NULL && dataLen == 0)
    {
        return DER_INPUTDATA_ERR;
    }
    else if (pData == NULL && dataLen == 0)
    {
        rv = DER_ITCAST_String_To_AnyBuf(&pTmp, NULL, 0);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pTmp);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");

            return -1;
        }

        rv = DER_ItAsn1_WritePrintableString(pTmp, &pHeadBuf);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pTmp);
            DER_ITCAST_FreeQueue(pHeadBuf);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");

            return -1;
        }

    }
    else
    {
        rv = DER_ITCAST_String_To_AnyBuf(&pTmp, (unsigned char*)pData, dataLen);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pTmp);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");
            return -1;
        }
        rv = DER_ItAsn1_WritePrintableString(pTmp, &pHeadBuf);
        if (rv != 0)
        {
            DER_ITCAST_FreeQueue(pHeadBuf);
            DER_ITCAST_FreeQueue(pTmp);
            //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"unsigned char*编码   error");

            return -1;
        }

    }
    DER_ITCAST_FreeQueue(pTmp);

    *outBuf = pHeadBuf;

    return 0;
}

int BaseASN1::DecodeChar(ITCAST_ANYBUF *inBuf, char **Data, int *pDataLen)
{
    ITCAST_ANYBUF	*pTmp = NULL;
    int			rv;

    rv = DER_ItAsn1_ReadPrintableString(inBuf, &pTmp);
    if (rv != 0)
    {
        DER_ITCAST_FreeQueue(pTmp);
        //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"DecodeUnsignedChar  error");
        return -1;
    }

    if (pTmp->dataLen == 0)
    {
        DER_ITCAST_FreeQueue(pTmp);
        *Data = NULL;
        *pDataLen = 0;
        return 0;
    }

    *Data = (char*)malloc(pTmp->dataLen + 1);
    if (*Data == NULL)
    {
        DER_ITCAST_FreeQueue(pTmp);
        //DER_DAPR_DebugMessage(_DEBUG_FILE_,__FILE__,__LINE__,"DecodeChar_an Mallco *Data Malloc  error");
        return -1;
    }
    memset(*Data, 0, pTmp->dataLen + 1);
    memcpy(*Data, pTmp->pData, pTmp->dataLen);
    *pDataLen = pTmp->dataLen;

    DER_ITCAST_FreeQueue(pTmp);

    return 0;
}

int BaseASN1::DER_CREATE_LOW_ITCAST_ANYBUF(ITCAST_ANYBUF *&point)
{
    point = (ITCAST_ANYBUF *)malloc(sizeof(ITCAST_ANYBUF));
    if (point == NULL)
    {
        return 7002;
    }
    memset(point, 0, sizeof(ITCAST_ANYBUF));
    point->dataLen = 0;
    point->dataType = 0;
    point->memoryType = MallocMemory;
    point->pData = NULL;
    point->unusedBits = 0;
    point->next = NULL;
    point->prev = NULL;

    return 0;
}

```



<br />

<br />

<br />

### SequenceASN1类是对BaseASN1进行二次封装,变得更容易使用

### SequenceASN1.h

```
#ifndef ASN1DER_H
#define ASN1DER_H
#include "BaseASN1.h"
#include <string>

class SequenceASN1  : public BaseASN1
{
public:
    SequenceASN1();

    // 添加头结点
    int writeHeadNode(int iValue);
    int writeHeadNode(char* sValue, int len);
    // 添加后继结点
    int writeNextNode(int iValue);
    int writeNextNode(char* sValue, int len);

    // 读头结点数据
    int readHeadNode(int &iValue);
    int readHeadNode(char* sValue);
    // 读后继结点数据
    int readNextNode(int &iValue);
    int readNextNode(char* sValue);

    // 打包链表
    int packSequence(char** outData, int &outLen);
    // 解包链表
    int unpackSequence(char* inData, int inLen);

    // 释放链表
    void freeSequence(ITCAST_ANYBUF* node = NULL);

private:
    ITCAST_ANYBUF* m_header = NULL;
	ITCAST_ANYBUF* m_next   = NULL;
	ITCAST_ANYBUF* m_temp   = NULL;
};

#endif // ASN1DER_H

```

<br />

<br />

<br />

### SequenceASN1.cpp

```cpp
#include "SequenceASN1.h"

SequenceASN1::SequenceASN1()
{
}

int SequenceASN1::writeHeadNode(int iValue)
{
	DER_ItAsn1_WriteInteger(iValue, &m_header);
	m_next = m_header;

	return 0;
}

int SequenceASN1::writeHeadNode(char * sValue, int len)
{
	EncodeChar(sValue, len, &m_header);
	m_next = m_header;

	return 0;
}

int SequenceASN1::writeNextNode(int iValue)
{
	DER_ItAsn1_WriteInteger(iValue, &m_next->next);
	m_next = m_next->next;

	return 0;
}

int SequenceASN1::writeNextNode(char * sValue, int len)
{
	EncodeChar(sValue, len, &m_next->next);
	m_next = m_next->next;

	return 0;
}

int SequenceASN1::readHeadNode(int & iValue)
{
	DER_ItAsn1_ReadInteger(m_header, (ITCAST_UINT32 *)&iValue);
	m_next = m_header->next;
	return 0;
}

int SequenceASN1::readHeadNode(char * sValue)
{
	DER_ItAsn1_ReadPrintableString(m_header, &m_temp);
	memcpy(sValue, m_temp->pData, m_temp->dataLen);
	DER_ITCAST_FreeQueue(m_temp);
	m_next = m_header->next;
	return 0;
}

int SequenceASN1::readNextNode(int & iValue)
{
	DER_ItAsn1_ReadInteger(m_next, (ITCAST_UINT32 *)&iValue);
	m_next = m_next->next;
	return 0;
}

int SequenceASN1::readNextNode(char * sValue)
{
	DER_ItAsn1_ReadPrintableString(m_next, &m_temp);
	memcpy(sValue, m_temp->pData, m_temp->dataLen);
	DER_ITCAST_FreeQueue(m_temp);
	m_next = m_next->next;
	return 0;
}

int SequenceASN1::packSequence(char ** outData, int & outLen)
{
	DER_ItAsn1_WriteSequence(m_header, &m_temp);
	//传输参数赋值
	*outData = (char *)m_temp->pData;
	outLen = m_temp->dataLen;

	//释放整个链表
	DER_ITCAST_FreeQueue(m_header);

	return 0;
}

int SequenceASN1::unpackSequence(char * inData, int inLen)
{
	DER_ITCAST_String_To_AnyBuf(&m_temp, (unsigned char *)inData, inLen);
	DER_ItAsn1_ReadSequence(m_temp, &m_header);

	DER_ITCAST_FreeQueue(m_temp);
	return 0;
}

void SequenceASN1::freeSequence(ITCAST_ANYBUF * node)
{

}

```

<br />

<br />

<br />

## 业务逻辑类



### 业务逻辑类父类

使用多态的方式去调用子类的具体业务方法

### Codec.h

```
#pragma once
#include "SequenceASN1.h"

// 编解码的父类
class Codec : public SequenceASN1
{
public:
	Codec();
	virtual ~Codec();
	
	// 数据编码
	virtual int msgEncode(char** outData, int &len);
	// 数据解码
	virtual void* msgDecode(char *inData, int inLen);
};


```



### Codec.cpp

```
#include "Codec.h"

Codec::Codec()
{
}

Codec::~Codec()
{
}

int Codec::msgEncode(char ** outData, int & len)
{
	return 0;
}

void * Codec::msgDecode(char * inData, int inLen)
{
	return NULL;
}

```





### 业务逻辑子类

### RespondCodec.h

```
#pragma once
#include "Codec.h"

struct  RespondMsg
{
    int	rv;		// 返回值
	char	clientId[12];	// 客户端编号
	char	serverId[12];	// 服务器编号
    char	r2[64];		// 服务器端随机数
    int		seckeyid;	// 对称密钥编号    keysn
	RespondMsg() {}
	RespondMsg(char* clientID, char* serverID, char* r2, int rv, int seckeyID)
	{
		this->rv = rv;
		this->seckeyid = seckeyid;
		strcpy(this->clientId, clientID);
		strcpy(this->serverId, serverID);
		strcpy(this->r2, r2);
	}
};

class RespondCodec : public Codec
{
public:
	RespondCodec();
	RespondCodec(RespondMsg *msg);
	~RespondCodec();

	// 函数重载
	int msgEncode(char** outData, int &len);
	void* msgDecode(char *inData, int inLen);
	

private:
	RespondMsg m_msg;
};


```



### RequestCodec.cpp

```cpp
#include "RequestCodec.h"
#include <iostream>
using namespace std;

//解码的时候使用
RequestCodec::RequestCodec() : Codec()
{
}

//编码的时候调用
RequestCodec::RequestCodec(RequestMsg * msg)
{
	// 赋值操作
	memcpy(&m_msg, msg, sizeof(RequestMsg));
}

RequestCodec::~RequestCodec()
{
}

/*
	struct RequestMsg
	{
		//1 密钥协商  	//2 密钥校验; 	// 3 密钥注销
		int		cmdType;		// 报文类型
		char	clientId[12];	// 客户端编号
		char	authCode[65];	// 认证码
		char	serverId[12];	// 服务器端编号 
		char	r1[64];			// 客户端随机数
	};
*/
int RequestCodec::msgEncode(char ** outData, int & len)
{
	writeHeadNode(m_msg.cmdType);
	writeNextNode(m_msg.clientId, strlen(m_msg.clientId)+1);
	writeNextNode(m_msg.authCode, strlen(m_msg.authCode) + 1);
	writeNextNode(m_msg.serverId, strlen(m_msg.serverId) + 1);
	writeNextNode(m_msg.r1, strlen(m_msg.r1) + 1);
	packSequence(outData, len);

	return 0;
}

void * RequestCodec::msgDecode(char * inData, int inLen)
{
	//反序列化
	unpackSequence(inData, inLen);
	readHeadNode(m_msg.cmdType);
	readNextNode(m_msg.clientId);
	readNextNode(m_msg.authCode);
	readNextNode(m_msg.serverId);
	readNextNode(m_msg.r1);

	cout << "解码成功" << endl;
	return &m_msg;
}

```



<br />

<br />

<br />



## 工厂模式

### 简单工厂模式 - 只需要一个工厂类

> 工厂: 使用一个单独的类来做创建实例的过程, 这就是工厂。
>
>简单工厂：把对象的创建放到一个工厂类中，通过参数来创建不同的对象。 特点：
>
>- 缺点：每添一个对象，就需要对简单工厂进行修改（尽管不是删代码，仅仅是添一个switch case，但仍
>  然违背了“不改代码”的原则, 尽量做到添加代码而不是修改原有代码）
>- 优点：去除了与具体产品的依赖。
>  使用流程
>
>1. 创建一类, 用于生产对象, 这个类就是工厂类
>2. 在这个类中添加一工厂函数



```cpp

class A : pulbic C
{

}

class B : pulbic C
{

}
class C
{

}


class Factory
{
public:
	Factory(){
	
	}
	Factory(int flag){
		m_flag = flag
	}
	
	C * CreateObject(){
	
		switch(m_flag){
			case 1:
				return new A;
			break;
			
			case 2:
				return new B;
			break;
			
			default:
				break;
		}
	}
private:
	int m_flag;
	
}

```



<br />

<br />

<br />

### 工厂模式 - 需要有N个工厂类

> 工厂方法：每种产品由一种工厂来创建, 不同工厂创建不同的对象
> 特点：基本完美，完全遵循 “不改代码”的原则
>
> 1. 创建一个工厂类 - 基类
> 2. 在基类的工厂类中添加工厂函数, 这是一个虚函数
> 3. 根据要创建的子对象添加子工厂类, 每个子对象的创建都对应一个子工厂类
> 4. 在子工厂类中实现父类的工厂函数, 完成创建对象的操作



```cpp
// 创建的子对象
class RequestCodec
{

};

class RespondCodec
{
};


class Factory{
public:
	Factory();
	~Factory();
	// 工厂函数
	// 创建RequestCodec对象
	// 创建RespondCodec对象
	// flag == 0 创建RequestCodec对象
	// flag == 1 创建RespondCodec对象
	virtual Codec* createFactory(int flag) = 0;
};

// 创建子工厂类1
class RequestFactory : public Factory{
public:
	RequestFactory()
	{
		flag = 0;
	}
	RequestFactory(RequestMsg* msg)
	{
		m_request = msg;
		flag = 1;
	}
	~RequestFactory();
	// 只创建RequestCodec对象
	Codec* createFactory()
	{
		if(flag)
		return new RequestCodec(&msg);
		else
		return new RequestCodec();
	}
private:
	RequestMsg *m_request;
	bool flag;
};

// 创建子工厂类2
class RespondFactory : public Factory
{
public:
	RequestFactory();
	~RequestFactory();
	// 只创建RespondCodec对象
	Codec* createFactory()
	{
		return new RespondCodec();
	}
}
```





<br />

<br />

<br />

### RespondFactory.h

```cpp
#pragma once
#include "CodecFactory.h"
#include "RespondCodec.h"

class RespondFactory :
	public CodecFactory
{
public:
	RespondFactory();
	RespondFactory(RespondMsg *msg);
	~RespondFactory();

	Codec* createCodec();

private:
	bool m_flag;
	RespondMsg * m_respond;
};


```

<br />

<br />

<br />







### RespondFactory.cpp



```cpp
#include "RequestFactory.h"

RequestFactory::RequestFactory()
{
	m_flag = false;
}

RequestFactory::RequestFactory(RequestMsg * msg)
{
	m_request = msg;
	m_flag = true;
}

RequestFactory::~RequestFactory()
{
}

Codec * RequestFactory::createCodec()
{
	if (m_flag == true)
	{
		//用于编码的类对象
		return new RequestCodec(m_request);
	}
	else
	{
		//用于解码的类对象
		return new RequestCodec();
	}
}

```





### CodecFactory.h

```cpp
#pragma once
#include "Codec.h"

class CodecFactory
{
public:
	CodecFactory();
	virtual ~CodecFactory();

	virtual Codec* createCodec();
};


```



### CodecFactory.cpp

```cpp
#include "CodecFactory.h"

CodecFactory::CodecFactory()
{
}

CodecFactory::~CodecFactory()
{
}

Codec * CodecFactory::createCodec()
{
	return NULL;
}

```









### main.cpp

```cpp
#include <iostream>
#include "RequestCodec.h"
#include "CodecFactory.h"
#include "RequestFactory.h"

using namespace std;


int main()
{
#if 1
	// 编码
	RequestMsg req;
	req.cmdType = 100;
	strcpy(req.clientId, "hello");
	strcpy(req.authCode, "1111");
	strcpy(req.serverId, "22222");
	strcpy(req.r1, "abcdefg");

	char* outData;
	int len;

	//使用工厂类构造编解码对象
	//1-new子工厂类对象
	CodecFactory *factory = new RequestFactory(&req);
	Codec *codec = factory->createCodec();
	codec->msgEncode(&outData, len);
	delete factory;
	delete codec;

	RequestMsg* tmp;
	factory = new RequestFactory();
	codec = factory->createCodec();
	tmp = (RequestMsg *)codec->msgDecode(outData, len);

	cout << "cmdtype: " << tmp->cmdType << endl;
	cout << "serverID: " << tmp->serverId << endl;
	cout << "clientID: " << tmp->clientId << endl;
	cout << "r1: " << tmp->r1 << endl;

#endif

#if 0
//测试RequestCodec类

	// 编码
	RequestMsg req;
	req.cmdType = 100;
	strcpy(req.clientId, "hello");
	strcpy(req.authCode, "1111");
	strcpy(req.serverId, "22222");
	strcpy(req.r1, "abcdefg");

	Codec* codec = new RequestCodec(&req);
	// 调用编码函数
	char* outData;
	int len;
	codec->msgEncode(&outData, len);

	// ========================
	// 解码
	codec = new RequestCodec();
	RequestMsg* tmp = (RequestMsg*)codec->msgDecode(outData, len);
	cout << "cmdtype: " << tmp->cmdType << endl;
	cout << "serverID: " << tmp->serverId << endl;
	cout << "clientID: " << tmp->clientId << endl;
	cout << "r1: " << tmp->r1 << endl;


#endif
	return 0;
}
```



<br />

<br />

<br /><br />

<br />

<br />

## 单向散列函数（哈希函数）

##### 概念

> 单向散列函数（one-wayftnction）有一个输入和一个输出，其中输入称为消息（message），输出称
>
> 为**散列值**（hashvalue）。单向散列函数可以根据消息的内容计算出散列值，而散列值就可以被用来检
>
> 查消息的完整性。
>
> - 单向散列函数也称为**消息摘要函数**（message digest function）、**哈希函数**或者**杂凑函数**。
>
> - 单向散列函数输出的散列值也称为**消息摘要**（message digest）或者**指纹**（fifingerprint）。
>
> - **完整性**也称为一致性。



- 压缩性
  - 任意长度的数据，算出的值长度都是固定的。

![image-20220523021602267](/images/javawz/image-20220523021602267.png)



-  容易计算
  - 计算散列值所花费的时间必须要短。尽管消息越长，计算散列值的时间也会越长，但如果不能在现实的时间内完成计算就没有意义了。

- 抗修改性
  - 对原数据进行任何改动，哪怕只修改1个字节，所得到的值都有很大区别



![image-20220523021836248](/images/javawz/image-20220523021836248.png)



- 强抗碰撞性
  - 已知原数据和其哈希值，想找到一个具有相同哈希值的数据（即伪造数据）是非常困难的。
    

![image-20220523021934069](/images/javawz/image-20220523021934069.png)

- 单向性（不可逆）
  ![image-20220523022005944](/images/javawz/image-20220523022005944.png)

### 哈希函数的应用

1. 检测软件是否被篡改

> 我们可以使用单向散列函数来确认自己下载的软件是否被篡改。
>
>很多软件，尤其是安全相关的软件都会把通过单向散列函数计算出的散列值公布在自己的官方网站上。
>用户在下载到软件之后，可以自行计算散列值，然后与官方网站上公布的散列值进行对比。通过散列
>值，用户可以确认自己所下载到的文件与软件作者所提供的文件是否一致。
>
>这样的方法，在可以通过多种途径得到软件的情况下非常有用。为了减轻服务器的压力，很多软件作者
>都会借助多个网站（镜像站点）来发布软件，在这种情况下，单向散列函数就会在检测软件是否被篡改
>方面发挥重要作用。



![image-20220523022106162](/images/javawz/image-20220523022106162.png)



2. 消息认证码 

> 使用单向散列函数可以构造消息认证码。
> 消息认证码是将“发送者和接收者之间的共享密钥”和“消息，进行混合后计算出的散列值。使用消息认证
> 码可以检测并防止通信过程中的错误、篡改以及伪装。



对于发送端tom:

- 将要发送的数据进行哈希运算, 参与运算的数据是: 原始数据+秘钥, 可以得到一个散列值
- 将散列值与原始数据进行拼接, 一起发送给对方;

对于接收端:

- 接收对方发来的数据, 并将原始数据和散列值拆分开, 得到散列值和原始数据
- 对原始数据进行哈希运算: 参与运算的数据也是:原始数据+秘钥, 可以得到一个散列值
- 将计算得到的散列值与接受到的散列值进行比较, 相同则认为没有被篡改, 否则认为被篡改了.

##### 注意: 发送方和接受方使用的秘钥是同一个秘钥(对称秘钥,秘钥分发困难), 这个秘钥只有发送方和接受方知道,若这个秘钥被第三方知道了, 就没有办法保证数据的是安全的了



![image-20220523022356814](/images/javawz/image-20220523022356814.png)

### 数字签名

> 在进行数字签名时也会使用单向散列函数。
> 数字签名是现实社会中的签名（sign）和盖章这样的行为在数字世界中的实现。数字签名的处理过程非
> 常耗时，因此一般不会对整个消息内容直接施加数字签名，而是先通过单向散列函数计算出消息的散列
> 值，然后再对这个散列值施加数字签名。



- 使用的是非对称加密, 有公钥和私钥

  - A给B发送数据:

  - A先将公钥发送给B
    - 将发送的数据+私钥进行哈希运算, 得到一个散列值
    - 将原数据+散列值拼接一起发送给对方B
    - B收到数据之后, 将数据进行拆分成原数据和散列值
    - B也对原数据+公钥进行哈希运算, 得到散列值
    - B将自己计算出的散列值与A发送来的散列值进行比较:
      - 相同, 认为数据没有被篡改
      - 不相同, 认为数据篡改过.



数字签名: A将明文进行哈希运算得到一个散列值, 并且用私钥对哈希值进行加密, 然后将明文和加了密的哈希
值一起发送给B;

B收到之后, 使用公钥对哈希值进行解密, 得到原始的哈希值, 并且对明文进行哈希运算也得到一个哈希值, 最后
对自己生成的哈希值和A发送过来的哈希值进行比较, 如果一样, 则认为没有被篡改.

使用数字签名的目的是为了不可抵赖性.(表明拿私钥的一方发送的数据, 是不可抵赖的)



##### 一次性口令或登录验证

> 使用单向散列函数可以构造一次性口令（one-time password）。
> 一次性口令经常被用于服务器对客户端的合法性认证。在这种方式中，通过使用单向散列函数可以保证
> 口令只在通信链路上传送一次（one-time），因此即使窃听者窃取了口令，也无法使用。
> 当用户登录的时候, 需要输入密码, 这个密码会经过加密运算, 经过运算之后的值再与服务器中保存的密
> 码进行比较, 若相同则输入密码正确, 允许登录.
> 若用户忘记密码, 则只能重置密码, 服务端并不知道用户原来的明文, 只能是重新设置.





### 常用的哈希函数

#### 1. Md4、Md5

> MD4是由Rivest于1990年设计的单向散列函数，能够产生==128比特==的散列值（RFC1186，修订版
> RFC1320）。不过，随着Dobbertin提出寻找MD4散列碰撞的方法，因此现在它已经不安全了。
>
> MD5是由Rwest于1991年设计的单项散列函数，能够产生==128比特==的散列值（RFC1321）。
> MD5的强抗碰撞性已经被攻破，也就是说，现在已经能够产生具备相同散列值的两条不同的消息，因此
> 它也已经不安全了。
>
> MD4和MD5中的MD是消息摘要（Message Digest）的缩写。



#### 2. SHA-1、SHA-256、SHA-384、SHA-512

>SHA-1是由NIST（National Institute Of Standardsand Technology，美国国家标准技术研究所）设计
>的一种能够产生==160比特==的散列值的单向散列函数。1993年被作为美国联邦信息处理标准规格
>（FIPS PUB 180）发布的是SHA,1995年发布的修订版FIPS PUB 180-1称为SHA-1。
>
>SHA-1的消息长度存在上限，但这个值接近于2^64^比特，是个非常巨大的数值，因此在实际应用中没
>有问题。
>
>SHA-256、SHA-384和SHA-512都是由NIST设计的单向散列函数，它们的散列值长度分别为==256比特
>==、==384==比特和==512比特==。这些单向散列函数合起来统称SHA-2，它们的消息长度也存在上限
>（SHA-256的上限接近于 2^64^ 比特，SHA-384 和 SHA-512的上限接近于 2^128^ 比特）。这些单向
>散列函数是于2002年和 SHA-1 一起作为 FIPS PUB 180-2发布的 SHA-1 的强抗碰撞性已于2005年被攻
>破, 也就是说，现在已经能够产生具备相同散列值的两条不同的消息。不过，SHA-2还尚未被攻破。



![image-20220523022911024](/images/javawz/image-20220523022911024.png)



最终需要对得到的散列值做转换, 以16进制格式的字符串表示

```cpp
// 第一种方式

// 初始化一个MD5_CTX类型的变量
int MD5_Init(MD5_CTX *c);
- c: 传出

// 给哈希函数添加要运算的数据
int MD5_Update(MD5_CTX *c, const void *data, size_t len);
- C: 传入
- data: 传入, 要进行哈希运算的数据
- len: data参数的长度
文件1G


while(read(fd, buf, len))
{
	MD5_Update(c, buf, buflen);
}

// 计算结果
int MD5_Final(unsigned char *md, MD5_CTX *c);
- md传出参数, 需要将计算出的散列值保存到md中
- c: 出入
//=========================================


// 第二种方式
unsigned char *MD5(const unsigned char *d, size_t n, unsigned char *md);
- d: 要进行哈希运算的字符串
- n: 第一个参数的长度
- md: 计算出的散列值
返回值:
	得到的散列值


//===========================================================================
int SHA1_Init(SHA_CTX *c);
int SHA1_Update(SHA_CTX *c, const void *data, size_t len);
int SHA1_Final(unsigned char *md, SHA_CTX *c);
unsigned char *SHA1(const unsigned char *d, size_t n, unsigned char *md);
上述几个函数的用法同md5的几个函数.
```



相关的函数说明可以在openssl安装目录中去查看:

- C:\OpenSSL-Win32\include\openssl, 如md5.h和sha.h
- 能够用到的库名: libssl.lib libcrypto.lib
- 使用到的库所在的路径: C:\OpenSSL-Win32\lib
- 使用到的头文件所在的路径: C:\OpenSSL-Win32\include

注意: 在使用vs进行项目开发的时候, 需要指定头文件所在路径和库文件所在路径

<br />

<br />

<br />

<br />

<br />

### ItcastLog.h

```
#ifndef _ITCAST_LOG_H_
#define _ITCAST_LOG_H_
#include <cstdarg>
/************************************************************************/
/* 
const char *file：文件名称
int line：文件行号
int level：错误级别
		0 -- 没有日志
		1 -- debug级别
		2 -- info级别
		3 -- warning级别
		4 -- err级别
int status：错误码
const char *fmt：可变参数
*/
/************************************************************************/
// 日志类
class ItcastLog
{
public:
    enum LogLevel{NOLOG, DEBUG, INFO, WARNING, ERROR};
    void Log(const char *file, int line, int level, int status, const char *fmt, ...);
    ItcastLog();
    ~ItcastLog();

private:
    int ITCAST_Error_GetCurTime(char* strTime);
    int ITCAST_Error_OpenFile(int* pf);
    void ITCAST_Error_Core(const char *file, int line, int level, int status, const char *fmt, va_list args);
};

#endif

```





### ItcastLog.cpp

```cpp
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <stdarg.h>
#include <string.h>
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
//#include <unistd.h>
#include "ItcastLog.h"
#include <string>
using namespace std;

const string ITCAST_DEBUG_FILE  = "itderlog.log";
const int ITCAST_MAX_STRING_LEN =  10240;

//Level的名称
const string ICLevelName[] = { "NOLOG", "DEBUG", "INFO", "WARNING", "ERROR" };
ItcastLog::ItcastLog()
{
}

ItcastLog::~ItcastLog()
{
}

int ItcastLog::ITCAST_Error_GetCurTime(char *strTime)
{
    struct tm*	tmTime = NULL;
    size_t		timeLen = 0;
    time_t		tTime = 0;

    tTime = time(NULL);
    tmTime = localtime(&tTime);
    //timeLen = strftime(strTime, 33, "%Y(Y)%m(M)%d(D)%H(H)%M(M)%S(S)", tmTime);
    timeLen = strftime(strTime, 33, "%Y.%m.%d %H:%M:%S", tmTime);

    return timeLen;
}

int ItcastLog::ITCAST_Error_OpenFile(int *pf)
{
    char	fileName[1024];
    memset(fileName, 0, sizeof(fileName));

#ifdef WIN32
    sprintf(fileName, "d:\\%s", ITCAST_DEBUG_FILE.data());
#else
    sprintf(fileName, "./log/%s", ITCAST_DEBUG_FILE.data());
#endif
#if 0
    *pf = open(fileName, O_WRONLY | O_CREAT | O_APPEND, 0666);
    if (*pf < 0)
    {
        return -1;
    }
#endif
    return 0;
}

void ItcastLog::ITCAST_Error_Core(const char *file, int line, int level, int status, const char *fmt, va_list args)
{
    char str[ITCAST_MAX_STRING_LEN];
    int	 strLen = 0;
    char tmpStr[64];
    int	 tmpStrLen = 0;
    int  pf = 0;

    //初始化
    memset(str, 0, ITCAST_MAX_STRING_LEN);
    memset(tmpStr, 0, 64);

    //加入LOG时间
    tmpStrLen = ITCAST_Error_GetCurTime(tmpStr);
    tmpStrLen = sprintf(str, "[%s] ", tmpStr);
    strLen = tmpStrLen;

    //加入LOG等级
    tmpStrLen = sprintf(str + strLen, "[%s] ", ICLevelName[level].data());
    strLen += tmpStrLen;

    //加入LOG状态
    if (status != 0)
    {
        tmpStrLen = sprintf(str + strLen, "[ERRNO is %d] ", status);
    }
    else
    {
        tmpStrLen = sprintf(str + strLen, "[SUCCESS] ");
    }
    strLen += tmpStrLen;

    //加入LOG信息
    tmpStrLen = vsprintf(str + strLen, fmt, args);
    strLen += tmpStrLen;

    //加入LOG发生文件
    tmpStrLen = sprintf(str + strLen, " [%s]", file);
    strLen += tmpStrLen;

    //加入LOG发生行数
    tmpStrLen = sprintf(str + strLen, " [%d]\n", line);
    strLen += tmpStrLen;

    //打开LOG文件
    if (ITCAST_Error_OpenFile(&pf))
    {
        return;
    }
#if 0
    //写入LOG文件
    write(pf, str, strLen);
    //IC_Log_Error_WriteFile(str);

    //关闭文件
    close(pf);
#endif
    return;
}

void ItcastLog::Log(const char *file, int line, int level, int status, const char *fmt, ...)
{
    va_list args;
    //判断是否需要写LOG
    if (level == NOLOG)
    {
        return;
    }

    //调用核心的写LOG函数
    va_start(args, fmt);
    ITCAST_Error_Core(file, line, level, status, fmt, args);
    va_end(args);

    return;
}

```

<br />

<br />

<br /><br />

<br />

<br />