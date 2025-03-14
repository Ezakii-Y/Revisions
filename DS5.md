<!-- title: A test html -->

## DS5

==背诵==标志：✅。==易错==标志：🔨

#### 栈的定义✅

**栈**是只允许在一端进行插入或删除操作的线性表。

**栈顶**是允许进行插入和删除的一端，**栈底**是固定的，不允许进行插入和删除操作的一端。**空栈**是不含任何元素的空表。

栈的操作特性可以简单概括为**后进先出**(LIFO)。

#### 基本操作✅

```cpp
InitStack(&S);
StackEmpty(S);
Push(&S, x);
Pop(&S, &x);
GetTop(S, &x);
DestroyStack(&S);
```

栈的数学性质：当$n$个不同元素入栈时，出栈元素不同排列的个数为$\frac{1}{n+1}C_{2n}^{n}$[^1]。

#### 栈的顺序存储结构

栈的顺序存储类型可描述为：
```cpp
# define MaxSize 50
typedef struct {
    ElemType data[MaxSize];
    int top;
} SqStack;
```

栈顶指针：S.top，初始设置其为-1；栈顶元素：S.data[S.top]。入栈：栈不满时，S.top加1，再对栈顶赋值；出栈：栈非空时取栈顶元素，再将S.top减1[^2]。

值得注意顺序栈受数组上界限制，可能发生**上溢**。

#### 顺序栈的基本操作

```cpp
// Initialize
void InitStack(SqStack &S) {
    S.top = -1;
}

// NULL-check
bool StackEmpty(SqStack S) {
    if (S.top == 1) {
        return true;
    } else {
        return false;
    }
}

// push-in
bool Push(SqStack &S, ElemType x) {
    if (S.top == MaxSize - 1) {
        return false;
    }
    S.data[++S.top] = x;
    return true;
}

//pop-out
bool Pop(SqStack &S, ElemType &x) {
    if (S.top == -1) {
        return false;
    }
    x = S.data[S.top--];
    return true;
}

// read the top element
bool GetTop(SqStack S, ElemType &x) {
    if (S.top == -1) {
        return false;
    }
    x = S.data[S.top];
    return true;
}
```

#### 共享栈

可让两个顺序栈共享一个数组空间，栈底设置在数组两端，两个栈的指针都指向栈顶元素，当两个栈顶指针相邻时(相减为1)，判断为栈满。

#### 栈的链式存储结构

采用链式存储的栈称为**链栈**，其不存在栈满上溢的情况。通常采用单链表实现链栈，并规定所有操作在单链表的表头进行，如果链栈没有头结点，那么Lhead将指向栈顶元素。

栈的链式存储类型可描述为：
```cpp
typedef struct Linknode {
    ElemType data;
    struct Linknode* next;
} LiStack;
```

#### 栈在括号匹配的应用🔨

思想简述：初始设置一空栈，顺序读入括号；若是左括号，则压入栈中；若是右括号，则或使相应的栈顶元素弹出，或是不合法的情况(退出程序或返回不匹配的信息)。算法结束时，栈为空，否则括号序列不匹配。

```cpp
// incorporates std::stack from the C++ Standard Template Library
bool isValidParentheses(const std::string &s) {
    std::stack<char> stack;
    for (char ch : s) {
        if (ch == '(' || ch == '{' || ch == '[') {
            stack.push(ch);
        } else {
            if (stack.empty()) {
                return false;
            }
            char top = stack.top();
            if ((ch == ')' && top == '(') ||
                (ch == '}' && top == '{') ||
                (ch == ']' && top == '[')) {
                stack.pop();
            } else {
                return false;
            }
        }
    }
    return stack.empty();
}
```

#### 栈在表达式求值中的应用

**中缀转后缀**思想简述：遇到**操作数**，直接加入后缀表达式；遇到**界限符**，若为“(”，直接入栈，若为“)”，依次弹出栈中运算符加入后缀表达式，直到弹出“(”为止；遇到**运算符**，若其优先级高于栈顶运算符或栈顶为“(”，则直接入栈，否则，从栈顶依次弹出栈中优先级高于或等于当前运算符的所有运算符，并加入后缀表达式，直到遇到优先级低于它的运算符或遇到“(”，之后将运算符入栈。

```cpp
// the code below incoporates the std::ctype from C++ STL 
// get the precedence
int precedence(char op) {
    if (op == '+' || op == '-') {
        return 1;
    }
    if (op == '*' || op == '/') {
        return 2;
    }
    return 0;
}

// transformation
std::string infixToPostfix(const std::string &infix) {
    std::stack<char> stack;
    std::string postfix;

    for (char ch : infix) {
        if (std::isdigit(ch) || std::isalpha(ch)) {
            // an operand is encountered, add the postfix expression directly
            postfix += ch;
        } else if (ch == '(') {
            // the open parenthesis is encountered, push it into the stack directly
            stack.push(ch);
        } else if (ch == ')') {
            // the close parenthesis is encountered, the stack operator is added to the postfix expression in sequence until the open parenthesis is popped
            while (!stack.empty() && stack.top() != '(') {
                postfix += stack.top();
                stack.pop();
            }
            stack.pop(); // pop the open bracket
        } else {
            // the operator is encountered
            while (!stack.empty() && precedence(stack.top()) >= precedence(ch)) {
                postfix += stack.top();
                stack.pop();
            }
            stack.push(ch);
        }
    }

    // pop the remaining operators in the stack to add postfix expressions
    while (!stack.empty()) {
        postfix += stack.top();
        stack.pop();
    }

    return postfix;
}
```

后缀表达式计算

```cpp
// Calculates the result of two operands
int applyOperation(int a, int b, char op) {
    switch (op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
        default: return 0;
    }
}

// Evaluates the postfix expression
int evaluatePostfix(const std::string &postfix) {
    std::stack<int> stack;

    for (char ch : postfix) {
        if (std::isdigit(ch)) {
            // an operand is encountered, push it onto the stack
            stack.push(ch - '0');
        } else {
            // the operator is encountered, two operands are popped to perform the operation and the result is pushed onto the stack
            int b = stack.top();
            stack.pop();
            int a = stack.top();
            stack.pop();
            int result = applyOperation(a, b, ch);
            stack.push(result);
        }
    }

    // the last element in the stack is the result of the postfix expression
    return stack.top();
}
```


- [DS5](#ds5)
    - [栈的定义✅](#栈的定义)
    - [基本操作✅](#基本操作)
    - [栈的顺序存储结构](#栈的顺序存储结构)
    - [顺序栈的基本操作](#顺序栈的基本操作)
    - [共享栈](#共享栈)
    - [栈的链式存储结构](#栈的链式存储结构)
    - [栈在括号匹配的应用🔨](#栈在括号匹配的应用)
    - [栈在表达式求值中的应用](#栈在表达式求值中的应用)

[^1]:卡特兰公式，参考组合数学
[^2]:如果将栈顶指针初始化为0，相关操作会有所不同。