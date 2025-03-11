<!-- title: A test html -->

## DS2

==背诵==标志：✅。==易错==标志：🔨

#### 线性表的定义✅

**线性表**是具有*相同*数据类型的$n$个数据元素的有限序列，其中$n$为表长，当$n=0$时为空表。

#### 线性表的基本操作

```cpp
InitList(&L)
Length(L)
LocateElem(L, e)
GetElem(L, i)
ListInsert(&L, i, &e)
PrintList(L)
Empty(L)
DestroyList(&L)
```

#### 顺序表的定义✅

**顺序表**即为线性表的顺序存储，所以顺序表中元素的逻辑顺序与其存储的物理顺序相同[^1]。另外顺序表是一种随机存取的存储结构。

静态分配：
```cpp
# define Maxsize 50
typedef struct{
    ElemType data[Maxsize];
    int length;
}Sqlist;
```
动态分配：
```cpp
# define InitSize 100
typedef struct{
    ElemType* data;
    int MaxSize, length;
}

L.data = (Elemtype*)malloc(sizeof(ElemType)*InitSize);
// L.data = new ElemType(InitSize);
```

顺序表插入平均需要移动$\frac{n}{2}$个元素，删除平均需要移动$\frac{n-1}{2}$个元素。

在长度为$n$的线性表中查找值为$e$的元素需要的平均次数为$\frac{n+1}{2}$。

#### 顺序表基本操作

```cpp
void InitList(SqList &L) {
    L.length = 0;
}

// Dynamic
void InitList(SqList &L) {
    L.data = (ElemType*) malloc(InitSize*sizeof(ElemType));
    L.length = 0;
    L.MaxSize = InitSize;
}

// Insertion
bool ListInsert(SqList &L, int i, Elemtype e) {
    if (i < 1 || i > L+1) return false;
    if (L.length == MaxSize) return false;
    for (int j = L.length; j >= i; j--) {
        L.data[j] = L.data[j-1];
    }
    L.data[i-1] = e;
    L.length++;
    return true;
}

// Deletion
bool ListDelete(SqList &L, int i, Elemtype &e) {
    if (i < 1 || i > L.length) return false;
    e = L.data[i-1];
    for (j = i; j < length; j++) {
        L.data[j-1] = L.data[j];
    }
    L.length--;
    return true;
}

// Search
int LocateElem(SqList L, ElemType e) {
    int i;
    for (i = 0; i < L.length; i++) {
        if (L.data[i] == e) return i + 1;
    }
    return 0;
}
```



- [DS2](#ds2)
    - [线性表的定义✅](#线性表的定义)
    - [线性表的基本操作](#线性表的基本操作)
    - [顺序表的定义✅](#顺序表的定义)
    - [顺序表基本操作](#顺序表基本操作)

[^1]: 线性表中元素的位序从1开始，而数组中元素的下标从0开始。