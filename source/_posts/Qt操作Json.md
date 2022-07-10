---
title: Qt操作Json
date: 2022-06-26 00:30:11
tags:
categories: Qt
doc:
---

### json介绍:

json分为:

- json对象
- json数组

#### json对象:

```

{
	"class":31,
	"count":51,
	"master":"yalong",
	"banzhuren":"chenlaoshi",
	"like":["dancing", "sing", "drinking"], 
	"hometown":{"hebei":"baoding", "henan":"xinyang"}
	
}
```

<br/>

#### json数组:

```
[10, 10.5, ["xiaohu", "liming", 10], {"name":"xiaohu", "age":32, "sex":"man"}]
```

1 json对象格式的数据都是以key:value的形式存在的, 其中key值是字符串形式的.
2 在json对象中, value值可以是子对象, 也可以是数组
3 在json格式的文件中, 只能是json对象或者是json数组
4 在json数组中, 数据的类型不定, 可以是double int string 子对象, 子数组.

<br/>

<br/>

### 头文件

```
#include <QJsonDocument>
#include <QJsonArray>
#include <QJsonObject>
#include <QByteArray>
#include <QJsonValue>
#include <QFile>
```

<br/>

<br/>

### json对象操作

#### 创建json对象

```
QJsonObject json;
```

<br/>

#### 给json对象插入值

```
iterator QJsonObject::insert(const QString &key, const QJsonValue &value)
```

<br/>

### json数组操作

#### 创建json数组

```
QJsonArray jsonArr;
```

<br/>

给json数组插入值

```
void QJsonArray::append(const QJsonValue &value)
```

<br/>

#### 将QJsonObject 或QJsonArray转换为 QJsonDocument

```
JsonDocument::QJsonDocument(const QJsonObject &object) //将json对象转换为QJsonDocument

JsonDocument::QJsonDocument(const QJsonArray &array)	//将json数组转换为QJsonDocument
```

<br/>

#### 将QJsonDocument对象转换为QByteArray对象

```
QByteArray QJsonDocument::toJson(JsonFormat format = Indented) const
```

<br/><br/>

### 操作文件

#### 创建文件对象

```
QFile::QFile(const QString &name)

QFile file(fileName);
```

<br/><br/>

#### 打开文件

```
virtual bool open(OpenMode mode)


QIODevice::NotOpen		0x0000					The device is not open.
QIODevice::ReadOnly		0x0001					The device is open for reading.
QIODevice::WriteOnly	0x0002					The device is open for writing. Note that this mode implies Truncate.
QIODevice::ReadWrite	ReadOnly | WriteOnly	The device is open for reading and writing.
QIODevice::Append		0x0004					The device is opened in append mode so that all data is written to the end of the file.

QIODevice::Truncate		0x0008					If possible, the device is truncated before it is opened. All earlier contents of the device are lost.

QIODevice::Text			0x0010					When reading, the end-of-line terminators are translated to '\n'. When writing, the end-of-line terminators are translated to the local encoding, for example '\r\n' for Win32.

QIODevice::Unbuffered	0x0020					Any buffer in the device is bypassed.

```

<br/>

<br/>

#### 读文件

```
QByteArray QIODevice::readAll()		//一次性读完

QByteArray byteArray = file.readAll();
```

<br/>

#### 将QByteArray类对象转换为QJsonDocument

```
QJsonDocument fromJson(const QByteArray &json, QJsonParseError *error = Q_NULLPTR)

QJsonDocument jsonDoc = QJsonDocument::fromJson(byteArray);
```

<br/>

#### 判断是不是对象

```
bool QJsonDocument::isObject() const
```

<br/>

#### 将QJsonDocument转换为QJsonObject

```
QJsonObject QJsonDocument::object() const
```

#### 将QJsonDocument转换为QJsonArray

```
QJsonArray QJsonDocument::array() const
```

#### 获取对象中所有的key值

```
QStringList QJsonObject::keys() const
```

#### 返回key的数量

```
int QJsonObject::size() const
```

#### 根据key值获取value值

```
QJsonValue QJsonObject::value(const QString &key) const
QJsonValue QJsonObject::value(QLatin1String key) const
```

#### 判断value值的类型

```
bool isArray() const
bool isBool() const
bool isDouble() const
bool isNull() const
bool isObject() const
bool isString() const
bool isUndefined() const
```

#### 将value转换到对应类型的值

```
QJsonArray toArray(const QJsonArray &defaultValue) const				//转换到数组
QJsonArray toArray() const												//转换到数组

bool toBool(bool defaultValue = false) const							//转换到布尔

double toDouble(double defaultValue = 0) const							//转换到到double

int toInt(int defaultValue = 0) const									//转换到整型

QJsonObject toObject(const QJsonObject &defaultValue) const				//转换到对象
QJsonObject toObject() const											//转换到对象

QString toString() const												//转换到字符串
QString toString(const QString &defaultValue) const						//转换到字符串

QVariant toVariant() const
```

<br/><br/>

#### 将QJsonDocument转换为const char *

```
const char *QJsonDocument::rawData(int *size) const
```

#### 将const char * 转换为QJsonDocument

```
fromRawData(const char *data, int size, DataValidation validation = Validate)
```

<br/><br/><br/>

## 例子

```cpp
#include <QCoreApplication>
#include <QJsonDocument>
#include <QJsonArray>
#include <QJsonObject>
#include <QByteArray>
#include <QJsonValue>
#include <QFile>
#include <QDebug>

void writeJsonToFile(char *fileName)
{
    //第一种: 将json对象写入磁盘文件
    //创建一个json对象
    /*QJsonObject json;

    //给json对象插入值
    // iterator insert(const QString &key, const QJsonValue &value)
    json.insert("name", "xiaowu");
    json.insert("age", 21);
    json.insert("sex", "female");

    //插入子对象
    QJsonObject subJson;
    subJson.insert("father", "longji");
    subJson.insert("mather", "liwei");
    subJson.insert("sister", "wangjin");
    json.insert("family", subJson);

    //插入json数组
    QJsonArray jsonArr;
    jsonArr.append("english"); 
    jsonArr.append("chinese");
    jsonArr.append("math");
    jsonArr.append("history");
    json.insert("course", jsonArr);*/

    //第二种: 将json数组写入磁盘文件
    //构造一个QJsonArray对象
    QJsonArray array;

    //void append(const QJsonValue &value)
    //给array对象添加值
    array.append(10);
    array.append("jinyanlong");
    array.append(true);

    //添加子数组到array中
    QJsonArray subArray;
    subArray.append("english");
    subArray.append("chinese");
    subArray.append("history");
    array.append(subArray);

    //添加对象到array中
    QJsonObject subObj;
    subObj.insert("mother", "xiaoling");
    subObj.insert("father", "xiaotong");
    subObj.insert("sister", "damiao");
    array.append(subObj);

    //将QJsonObject 转换为 QJsonDocument
    //JsonDocument::QJsonDocument(const QJsonObject &object)
    //QJsonDocument jsonDoc(json);

    //QJsonDocument(const QJsonArray &array)
    QJsonDocument jsonDoc(array);

    //将QJsonDocument对象转换为QByteArray对象
    //QByteArray toJson(JsonFormat format = Indented) const
    QByteArray byteArray = jsonDoc.toJson();

    //文件操作--将byteArray写入文件
    //QFile(const QString &name)
    QFile file(fileName);

    //打开文件
    //virtual bool open(OpenMode mode)
    file.open(QIODevice::WriteOnly);

    //写文件
    //qint64 write(const QByteArray &byteArray)
    file.write(byteArray);

    //关闭文件
    //virtual void close()
    file.close();

    return;
}

void readJsonFromFile(char *fileName)
{
    //构造QFile类对象
    QFile file;

    //设置要读的文件
    //void setFileName(const QString &name)
    file.setFileName(fileName);

    //打开文件
    //virtual bool open(OpenMode mode)
    file.open(QIODevice::ReadOnly);

    //读文件
    //QByteArray readAll()
    QByteArray byteArray = file.readAll();

    //关闭文件
    //virtual void close()
    file.close();

    //将QByteArray类对象转换为QJsonDocument
    //QJsonDocument fromJson(const QByteArray &json, QJsonParseError *error = Q_NULLPTR)
    QJsonDocument jsonDoc = QJsonDocument::fromJson(byteArray);

    //判断是数组还是对象
    if(jsonDoc.isObject())
    {
        //QJsonObject object() const
        QJsonObject jsonObj = jsonDoc.object();

        //获取对象中所有的key值
        //QStringList keys() const
        QStringList keys = jsonObj.keys();

        for(int i=0; i<keys.size(); i++)
        {
            //获取每一个key值
            //QString key = keys[i];
            QString key = keys.at(i);
            //qDebug() << key << ":";

            //根据key值获取value值
            //QJsonValue value(const QString &key) const
            //QJsonValue operator[](const QString &key) const
            QJsonValue jsonValue = jsonObj.value(key);

            //判断value值的类型
            //bool isString() const
            if(jsonValue.isString())
            {
                qDebug() <<  key << ":" << jsonValue.toString();
            }
            else if(jsonValue.isDouble())
            {
                qDebug() <<  key << ":" << jsonValue.toInt();
            }
            else if(jsonValue.isBool())
            {
                qDebug() <<  key << ":" << jsonValue.toBool();
            }
            else if(jsonValue.isObject())
            {
                //QJsonObject toObject() const
                 QJsonObject obj = jsonValue.toObject();
                 QStringList subKeys = obj.keys();

                 qDebug() << key << ":{";
                 for(int k=0; k<subKeys.size(); k++)
                 {
                     QString subkey = subKeys[k];
                     QJsonValue subJsonValue = obj.value(subkey);
                     qDebug() << "  " << subJsonValue.toString();
                 }
                 qDebug() << "}";
            }
            else if(jsonValue.isArray())
            {
               //QJsonArray toArray() const
               qDebug() << key << ":[";
               QJsonArray arr =  jsonValue.toArray();
               for(int j=0; j<arr.size(); j++)
               {
                   QJsonValue va = arr[j];
                   if(va.isString())
                   {
                       qDebug() << "    " << va.toString();
                   }
               }
               qDebug() <<"]";

            }

        }

    }
    else if(jsonDoc.isArray())
    {
        //QJsonArray array() const
        QJsonArray array = jsonDoc.array();
        for(int i=0; i<array.size(); i++)
        {
            QJsonValue value = array[i];

            //判断值的类型
            if(value.isString())
            {
                qDebug() << value.toString();
            }
            else if(value.isDouble())
            {
                qDebug() << value.toInt();
            }
            else if(value.isBool())
            {
                qDebug() << value.toBool();
            }
            else if(value.isArray())
            {
                qDebug() << "[";
                QJsonArray subArray = value.toArray();
                for(int j=0; j<subArray.size(); j++)
                {
                    qDebug() << "   " << subArray[j].toString();
                }
                qDebug() << "]";
            }
            else if(value.isObject())
            {
                qDebug() << "{";
                QJsonObject subObj = value.toObject();
                QStringList subKeys = subObj.keys();
                for(int k=0; k<subKeys.size(); k++)
                {
                    QString subkey = subKeys[k];
                    QJsonValue value = subObj[subkey];
                    if(value.isString())
                    {
                        qDebug() << "   " << value.toString();
                    }
                }
                qDebug() << "}";

            }
        }

    }


}

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);

    //写json数据到文件
    writeJsonToFile("test.json");

    //读json文件
    readJsonFromFile("test.json");



    return a.exec();
}

```

