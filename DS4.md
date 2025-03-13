<!-- title: A test html -->

## DS4

==背诵==标志：✅。==易错==标志：🔨

#### 双链表

**双链表**结点中有两个指针prior和next，其结点类型的描述如下：

```cpp
typedef struct DNode {
    ElemType data;
    struct DNode* prior, * next;
} D
```

**插入操作**，假设要将s所指结点插入p之后

```cpp
s->next = p->next;
p->next->prior = s;
s->prior = p;
p->next = s;
```

注意上方代码中第一步必须在第四步之前。

**删除操作**，假设要删去p的后继结点q

```cpp
p->next = q->next;
q->next->prior = p;
free(q);
```
#### 循环链表

**循环单链表**，其表尾结点next域指向L，故表中没有指针域为NULL的结点，判空条件为头结点指针是否等于自身[^1]。如果只设头指针，表尾插入元素需要$O(n)$的时间复杂度，如果设置尾指针，表尾或表头的插入只需要$O(1)$的时间复杂度。

**循环双链表**，头结点的prior指针还要指向表尾结点。判空条件为头结点的prior域和next域都等于L。

#### 静态链表

**静态链表**是由数组来描述线性表的链式存储结构，这里的指针域储存的是结点在数组中的相对地址(数组下标)，也称**游标**。静态链表需要预先分配一块连续的存储空间。

#### 几个双指针案例

```cpp
// detect the loop of the giving linklist and return the entry point
LinkList Findloop(LinkList &L) {
    LNode* fast = L, * slow = L;
    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) {
            break;
        }
    }
    if (fast == NULL || fast->next == NULL) {
        return NULL;
    }
    LNode* p1 = L, * p2 = slow
    while (p1 != p2) {
        p1 = p1->next;
        p2 = p2->next;
    }
    return p1;
}

// assume that the length of the linklist is even, the function is to find the greatest pair of the sum that of the L[i] and the L[n-i-1]
int PairSum(Linklist L) {
    LNode* fast = L->next, * slow = L;
    while (fast != NULL && fast->next != NULL) {
        fast = fast->next->next;
        slow = slow->next;
    }
    LNode* newH = NULL, * p = slow->next, * tmp;
    while (p != NULL) {
        tmp = p->next;
        p->next = newH;
        newH = p;
        p = tmp;
    }
    int mx = 0, p = L;
    LNode* q = newH;
    while (q != NULL) {
        if ( (p->data + q->data) > mx) {
            mx = p->data + q->data;
        }
        p = p->next;
        q = q->next;
    }
    return mx;
}
```

- [DS4](#ds4)
    - [双链表](#双链表)
    - [循环链表](#循环链表)
    - [静态链表](#静态链表)
    - [几个双指针案例](#几个双指针案例)

[^1]:考虑链表含头结点的情况。