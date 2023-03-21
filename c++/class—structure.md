在 C++ 中，如果想要在类或结构体的构造函数中为某些参数设置默认值，可以使用函数重载或 C++11 引入的默认参数的方式。

```c++
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int val = 0, ListNode* next = nullptr) : val(val), next(next) {}
};
```
