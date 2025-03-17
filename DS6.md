<!-- title: A test html -->

## DS6

==背诵==标志：✅。==易错==标志：🔨

#### 队列的定义

队列(Queue)，是操作受限的线性表，只允许在表的的一端插入，在另一端删除。操作特性**先进先出**(FIFO)。

**队头(Front)**：允许删除的一端。
**队尾(Rear)**：允许插入的一端。
**空队列**：不含任何元素的空表。

常见操作：
```cpp
InitQueue(&Q);
QueueEmpty(Q);
EnQueue(&Q, x);
DeQueue(&Q, &x);
GetHead(Q, &x);
```

#### 队列的顺序存储结构

此实现分配一块连续的存储单元存放队列中的元素，并设队首指针front指向队首元素，队尾指针rear指向队尾元素的下一个位置[^1]。

队列的顺序存储类型可描述为：
```cpp
# define MaxSize 50
typedef struct {
    ElemType data[MaxSize];
    int front, rear;
} SqQueue;
```

入队时，先向队尾元素赋值，然后rear+1；出队时，先取队首元素，然后front+1。

#### 循环队列

```cpp
Q.front = Q.rear = 0; // initial
Q.front = (Q.front + 1); // move front
Q.rear = (Q.rear + 1); // move rear
(Q.rear + MaxSize - Q.front) % MaxSize // length of the queue
Q.rear == Q.front // null-check
```

为了区分队空还是队满的情况，有3种方式

1. 牺牲一个存储单元：
```cpp
(Q.rear + 1) % MaxSize == Q.front; // queue is full
```
2. 增设size数据成员，表示元素个数。
3. 增设tag数据成员，删除置0，插入置1。

循环队列的操作：
```cpp
void InitQueue(SqQueue &Q) {
    Q.rear = Q.front = 0;
}

bool isEmpty(SqQueue Q) {
    if (Q.rear == Q.front) {
        return true;
    } else {
        return false;
    }
}

bool EnQueue(SqQueue &Q, ElemType x) {
    if ((Q.rear + 1) % MaxSize == Q.front) {
        return false;
    }
    Q.data[Q.rear] = x;
    Q.rear = (Q.rear + 1) % MaxSize;
    return true;
}

bool DeQueue(SqQueue &Q, ElemType &x) {
    if (Q.rear == Q.front) {
        return false;
    }
    x = Q.data[Q.front];
    Q.front = (Q.front + 1) % MaxSize;
    return true;
}
```

#### 队列的链式存储结构

队列的链式表示称为**链式队列**，是一个同时拥有队首指针和队尾指针的单链表。

队列的链式存储类型可描述为：
```cpp
typedef struct LinkNode {
    ElemType data;
    struct LinkNode *next;
} LinkNode;
typedef struct {
    LinkNode *front, *rear;
} LinkQueue;
```

不带头结点时，当Q.front == NULL且Q.rear == NULL时，队列为空。

注意插入和删除时两个指针可能都要移动。

基本操作：
```cpp
void InitQueue(LinkQueue &Q) {
    Q.front = Q.rear = (LinkNode*) malloc(sizeof(LinkNode));
    Q.front->next = NULL;
}

bool IsEmpty(LinkQueue Q) {
    if (Q.front == Q.rear) {
        return true;
    }
    else {
        return false;
    }
}

void EnQueue(LinkQueue &Q, ElemType x) {
    LinkNode *s = (LinkNode*) malloc(sizeof(LinkNode));
    s->data = x;
    s->next = NULL;
    Q.rear->next = s;
    Q.rear = s;
}

bool DeQueue(LinkQueue &Q, ElemType &x) {
    if (Q.front == Q.rear) {
        return false;
    }
    LinkNode *p = Q.front->next;
    if (Q.rear == p) {
        Q.rear = Q.front;
    }
    free(p);
    return true;
}
```

#### 双端队列

**双端队列**指允许两端都可以插入和删除的线性表。只允许在一端输出，而两端皆可输入，称为输出受限；只允许在一端输入，而两端皆可输出，称为输入受限。

#### 队列的应用

```cpp
// define the bitree structure
struct TreeNode {
    int val;
    TreeNode *left;
    TreeNode *right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

// levelorder
void levelOrderTraversal(TreeNode* root) {
    if (root == NULL) {
        return;
    }

    std::queue<TreeNode*> q;
    q.push(root);

    while (!q.empty()) {
        TreeNode* node = q.front();
        q.pop();
        std::cout << node->val << " ";

        if (node->left != NULL) {
            q.push(node->left);
        }
        if (node->right != NULL) {
            q.push(node->right);
        }
    }
}
```

此外在计算机系统中，队列可以解决主机和外部设备之间速度不匹配的问题，也可以解决由多用户引起的资源竞争问题。

#### 数组的定义和索引

数组是由$n(n\geq 1)$个相同类型的数据元素构成的有限序列，每个数据元素称为**数组元素**，每个元素在$n$个线性关系中的序号称为该元素的**下标**，下标的取值范围称为数组的**维界**。

数组通常只有存取元素和修改元素操作。

存储结构关系式为：
$$
LOC(a_i) = LOC(a_0)+i\times L\ (0\leq i < n)
$$

对于多维数组有两种映射方式，设下标范围分别为$[0,h_1]$和$[0, h_2]$。
若以行优先：
$$
LOC(a_{i, j})=LOC(a_{0, 0})+[i\times(h_2+1)+j]\times L
$$
若以列优先：
$$
LOC(a_{i, j})=LOC(a_{0, 0})+[j\times(h_1+1)+i]\times L
$$

#### 特殊矩阵压缩存储

**对称矩阵**：若$n$阶矩阵$A$的任意元素$a_{ij}$都有$a_{ij}=a_{ji}$，则可将其存储在一维数组$B[n(n+1)/2]$中，其元素下标之间的对应关系为[^2]：
$$
k=\left\{\begin{align}
 & \frac{i(i-1)}{2}+j-1 \qquad i\geq j\\
 & \frac{j(j-1)}{2}+i-1 \qquad i < j
\end{align}\right.
$$

**三角矩阵**
下三角矩阵上三角区所有元素为同一常量，$n$阶下三角矩阵可存储在$B[n(n+1)/2+1]$中，元素下标之间的对应关系为：
$$
k=\left\{\begin{align}
 & \frac{i(i-1)}{2}+j-1 \qquad i\geq j\\
 & \frac{n(n+1)}{2} \qquad i < j
\end{align}\right.
$$

若是上三角矩阵：
$$
k=\left\{\begin{align}
 & \frac{(i-1)(2n-i+2)}{2}+j-i \qquad i\leq j\\
 & \frac{n(n+1)}{2} \qquad i > j
\end{align}\right.
$$

**三对角矩阵**也称带状矩阵，非零元素集中在三条对角线区域。元素$a_{ij}$在一维数组中的下标为$2i+j-3$。

**稀疏矩阵**中非零元素的个数相对于矩阵元素的个数来说非常少，通常使用三元组(行标i，列标j，值)来储存稀疏矩阵，此时随机存取特性丧失，也可以用十字链表存储。存储稀疏矩阵时，不仅要保存三元组表，还要保存稀疏矩阵的行数，列数和非零元素的个数。


- [DS6](#ds6)
    - [队列的定义](#队列的定义)
    - [队列的顺序存储结构](#队列的顺序存储结构)
    - [循环队列](#循环队列)
    - [队列的链式存储结构](#队列的链式存储结构)
    - [双端队列](#双端队列)
    - [队列的应用](#队列的应用)
    - [数组的定义和索引](#数组的定义和索引)
    - [特殊矩阵压缩存储](#特殊矩阵压缩存储)

[^1]:front和rear的定义在题目中可能有所不同，具体情况具体分析。
[^2]:以下所有公式皆假定矩阵行号和列号从1开始。