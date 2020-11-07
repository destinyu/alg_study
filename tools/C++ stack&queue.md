## C++ stack用法总结

### 1.stack初始化
```
#include<stack>

stack<int> s1;
stack<int> s2(自己定义的数据结构);//用自己的数据结构构造一个栈
stack<int,vector<int>> s3;//用vector构造一个栈
```

### 2.stack的成员函数
```
s.push(x);//入栈
s.pop();//出栈，只删除栈顶元素，并不返回该元素
s.top();//返回栈顶元素
s.empty();//判断栈空
s.size();//返回栈中元素个数
```

## C++ queue用法总结

### 1.queue的成员函数
```
q.push(x);//将x元素添加至队尾
q.pop();//弹出队列的第一个元素，并不返回该元素
q.front;//访问队首元素
q.back();//访问队尾元素
q.empty();//判断队空
q.size();//返回队列中元素个数

```

**stack和queue都没有迭代器，不支持随机访问**