<!-- title: A test html -->

## DS3

==背诵==标志：✅。==易错==标志：🔨

#### 单链表的定义✅

线性表的链式存储也称**单链表**

其结点类型描述为

```cpp
typedef struct Lnode {
    ElemType data;
    struct Lnode* next;
} Lnode, *linkList
```

单链表可以解决顺序表需要大量连续存储空间的缺点，但存储效率没有顺序表高，是**非随机存取**的存储结构。

通常用**头指针L**来标识一个单链表，头指针为NULL时代表一个空表。此外，为了**操作上的方便**，在第一个结点前附加一个数据域可以不带任何信息的**头节点**[^1]。

#### 基本操作实现✅

```cpp
bool InitList(Linklist &L) {
    L = (LNode*) malloc(sizeof(LNode));
    L->next = NULL;
    return true;
}

// for the linklist without head node
bool InitList(LinkList &L) {
    L = NULL;
    return true;
}

// find the length
int Length(LinkList &L) {
    int len = 0;
    LNode* p = L->next;
    while (p != NULL) {
        len++;
        p = p->next;
    }
    return len;
}

// find the node by giving position
LNode* GetElem(LinkList L, int i) {
    int j = 0;
    LNode* p = L;
    while (p != NULL && j < i) {
        p = p->next;
        j++;
    }
    return p;
}

// find the node by giving value
LNode* LocateElem(Linklist L, int e) {
    LNode* p = L->next;
    while (p != NULL && p->data != e) {
        p = p->next;
    }
    return p;
}

// insertion
bool ListInsert(LinkList &L, int i, ElemType e) {
    LNode* p = L;
    int j = 0;
    while (p != NULL && j < i - 1) {
        p = p->next;
        j++;
    }
    if (!p) {
        return false;
    }
    LNode* s = (LNode*) malloc(sizeof(LNode));
    s->data = e;
    s->next = p->next;
    p->next = s;
    return true;
}

// deletion
bool ListDelete(LinkList &L, int i, ElemType &e) {
    LNode* p = L;
    int j = 0;
    while (p->next != NULL && j < i - 1) {
        p = p->next;
        j++;
    }
    if (p->next == NULL || j > i - 1) {
        return false;
    }
    LNode* q = p->next;
    e = q->data;
    p->next = q->next;
    free(q);
    return true;
}

// head insertion
LinkList List_HeadInsert(LinkList &L){
    LNode* s;
    int x;
    L = (LNode*) malloc(sizeof(LNode));
    L->next = NULL;
    scanf("%d", &x);
    while (x != 9999) {
        s = (LNode*) malloc(sizeof(LNode));
        s->data = x;
        s->next = L->next;
        L->next = s;
        scanf("%d", &x);
    }
    return L;
}

// tail insertion
LinkList List_TailInsert(LinkList &L) {
    int x;
    L = (LNode*) malloc(sizeof(LNode));
    LNode* s, * r = L;
    scanf("%d", &x);
    while (x != 9999) {
        s = (LNode*) malloc(sizeof(LNode));
        s->data = x;
        r->next = s;
        r = s;
        scanf("%d", &x);
    }
    r->next = NULL;
    return L
}
```


- [DS3](#ds3)
    - [单链表的定义✅](#单链表的定义)
    - [基本操作实现✅](#基本操作实现)

[^1]:头结点数据域也可以记录表长等信息。