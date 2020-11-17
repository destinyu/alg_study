## Day 05 Nov.232
### 问题描述
请你仅使用两个栈实现先入先出队列。队列应当支持一般队列的支持的所有操作（push、pop、peek、empty）：

实现 MyQueue 类：

void push(int x) 将元素 x 推到队列的末尾
int pop() 从队列的开头移除并返回元素
int peek() 返回队列开头的元素
boolean empty() 如果队列为空，返回 true ；否则，返回 false


说明：

你只能使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。


进阶：

你能否实现每个操作均摊时间复杂度为 O(1) 的队列？换句话说，执行 n 个操作的总时间复杂度为 O(n) ，即使其中一个操作可能花费较长时间。


示例：

输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false


提示：

1 <= x <= 9
最多调用 100 次 push、pop、peek 和 empty
假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作）

### 问题分析
用栈模拟队列操作，但是栈只能在顶端操作。所以可以想到需要一个辅助栈。在具体操作时，可以将栈顶当作队头，也可以将栈底作为队头。

### 思路&代码
1.入栈时做调整，把栈顶当作队头。使用辅助栈，在入栈的时候将原栈出栈转移，形成倒序排列。使得最后结果以栈顶为队首，出栈时直接弹出栈顶，返回队首元素时直接返回top。

``` c++
class MyQueue {
public:
    stack<int> Sin;//原本栈
    stack<int> Sout;//辅助栈
    //int front;
    /** Initialize your data structure here. */
    MyQueue() {

    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        //if(Sin.empty())
            //front=x;
        while(!Sin.empty()){//如果原栈不空，先弹出到辅助栈，再将下一个数入辅助栈栈顶，（这里也可以是入原栈栈底），最后将辅助栈出栈再入到原栈（倒序）得到以栈顶为首的队列
            int a=Sin.top();
            Sout.push(a);
            Sin.pop();
        }
        Sout.push(x);
        while(!Sout.empty()){
            int b=Sout.top();
            Sin.push(b);
            Sout.pop();
        }

    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {//弹出时直接弹出栈顶
        int c=Sin.top();
        Sin.pop();
        //if(!Sin.empty())
            //front=Sin.top();
        return c;
    }
    
    /** Get the front element. */
    int peek() {//返回队首元素之间返回top
        return Sin.top();
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {//最终只有一个栈内保存数据，只需检查原栈
        return Sin.empty();
    }
};
```
2.同样使用两个栈，但是当将栈底作为队首时，**可以将入队操作的复杂度从O(n)降到O(1)**。入队时不变，直接入栈。出队时因为要出栈底元素，可以用辅助栈将原本顺序逆转，直接弹出辅助栈栈顶，**辅助栈空了可以再将原栈的元素导入**
``` c++
class MyQueue {
public:
stack<int> Sin;//原栈
stack<int> Sout;//辅助栈
int front;
    /** Initialize your data structure here. */
    MyQueue() {

    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        if(Sin.empty())//这里需要将第一个入栈元素赋给front，不然会找不到。
            front=x;
        Sin.push(x);//直接入栈
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if(Sout.empty()){//只有当辅助栈空的时候才可以把原栈数据压入，如果不为空不能叠加。有可能在辅助栈出栈时又有新的元素入原栈。
            while(!Sin.empty()){
                int a=Sin.top();
                Sout.push(a);
                Sin.pop();
            }
        }
        int b=Sout.top();
        Sout.pop();
        return b;
    }
    
    /** Get the front element. */
    int peek() {//如果辅助栈有数据，栈顶就是队头元素，否则就是最开始入栈的front。
        if(!Sout.empty())
            return Sout.top();
        return front; 
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return Sin.empty()&&Sout.empty();//两个栈都为空时才是空栈
    }
};
```
### 摊还分析

![average_analysis](https://github.com/destinyu/alg_study/blob/master/pic/average_analysis.png)
