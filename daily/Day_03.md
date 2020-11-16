## Day 03(栈) Nov.1381
### 问题描述
请你设计一个支持下述操作的栈。

实现自定义栈类 CustomStack ：

**CustomStack(int maxSize)**：用 maxSize 初始化对象，maxSize 是栈中最多能容纳的元素数量，栈在增长到 maxSize 之后则不支持 push 操作。
**void push(int x)**：如果栈还未增长到 maxSize ，就将 x 添加到栈顶。
**int pop()**：弹出栈顶元素，并返回栈顶的值，或栈为空时返回 -1 。
**void inc(int k, int val)**：栈底的 k 个元素的值都增加 val 。如果栈中元素总数小于 k ，则栈中的所有元素都增加 val 。

示例：

输入：
["CustomStack","push","push","pop","push","push","push","increment","increment","pop","pop","pop","pop"]
[[3],[1],[2],[],[2],[3],[4],[5,100],[2,100],[],[],[],[]]
输出：
[null,null,null,2,null,null,null,null,null,103,202,201,-1]
解释：
CustomStack customStack = new CustomStack(3); // 栈是空的 []
customStack.push(1); // 栈变为 [1]
customStack.push(2); // 栈变为 [1, 2]
customStack.pop(); // 返回 2 --> 返回栈顶值 2，栈变为 [1]
customStack.push(2); // 栈变为 [1, 2]
customStack.push(3); // 栈变为 [1, 2, 3]
customStack.push(4); // 栈仍然是 [1, 2, 3]，不能添加其他元素使栈大小变为 4
customStack.increment(5, 100); // 栈变为 [101, 102, 103]
customStack.increment(2, 100); // 栈变为 [201, 202, 103]
customStack.pop(); // 返回 103 --> 返回栈顶值 103，栈变为 [201, 202]
customStack.pop(); // 返回 202 --> 返回栈顶值 202，栈变为 [201]
customStack.pop(); // 返回 201 --> 返回栈顶值 201，栈变为 []
customStack.pop(); // 返回 -1 --> 栈为空，返回 -1

### 问题分析
该问题需要自己实现一个栈的功能。在普通栈的功能的基础上加入栈底k个元素的值增加的操作。**和一般的栈的区别在于一般栈中只有栈顶元素可见，而自定义栈需要所有元素可见才可以进行增加val操作。**

### 思路&代码
1.用数组模拟栈的操作，数组中的数都是可见的。可以直接进行最后的inc操作。
对于 push 操作，首先判断当前元素的个数是否达到上限，如果没有达到，就把 top 后移一个位置并添加一个元素。
对于 pop 操作，首先判断当前栈是否为空，非空返回栈顶元素并将 top 前移一位，否则返回 -1−
对于 inc 操作，直接对栈底的最多 k 个元素加上 val。
``` c++
class CustomStack {
public:

    vector<int> stk;
    int top;
    CustomStack(int maxSize) {//初始化，调整栈的大小，设定指针top
        stk.resize(maxSize);
        top=-1;

    }
    
    void push(int x) {//入栈操作，先加后赋值
        if(top!=stk.size()-1){
            ++top;
            stk[top]=x;
        }
    }
    
    int pop() {//弹出操作，返回栈顶值
        if(top==-1)
        return -1;
        --top;
        return stk[top+1];
        
    }
    
    void increment(int k, int val) {//取k和栈顶的较小值。遍历加val
        int lim=min(k,top+1);
        for(int i=0;i<lim;i++){
            stk[i]+=val;
        }
    }
};
```
2.直接将栈底元素增加val的操作的时间复杂度为O(k)，这里可以对该操作做一些优化，使其复杂度变为O(1)。设定一个辅助数组add用来存储增量。空间换时间。因为只有在出栈操作时才会需要知道最后增加后的结果。在未出栈之前不需要知道原本栈的数据，将val存储在对应的（下图中不对应，但是出栈时加栈顶即可，不需要对应）add数组中即可。
inc操作：
![image-20201116100432034](C:\Users\destinyu\AppData\Roaming\Typora\typora-user-images\image-20201116100432034.png)

pop操作：![image-20201116100617965](C:\Users\destinyu\AppData\Roaming\Typora\typora-user-images\image-20201116100617965.png)


``` c++
class CustomStack {
public:
    vector<int> stk, add;
    int top;

    CustomStack(int maxSize) {
        stk.resize(maxSize);
        add.resize(maxSize);//辅助数组，用于存放增量。大小为maxszie。
        top = -1;
    }
    
    void push(int x) {//正常入栈操作
        if (top != stk.size() - 1) {
            ++top;
            stk[top] = x;
        }
    }
    
    int pop() {
        if (top == -1) {
            return -1;
        }
        int ret = stk[top] + add[top];//出栈时栈顶值加上辅助数组的增量
        if (top != 0) {
            add[top - 1] += add[top];//把增量向后移一位存储，有可能会叠加。
        }
        add[top] = 0;//辅助数组顶位清零
        --top;
        return ret;
    }
    void increment(int k, int val) {//只增加add[k-1]
        int lim = min(k - 1, top);
        if (lim >= 0) {
            add[lim] += val;
        }
    }

```
3.上面的两种解法都要维护一个大小为maxsize的数组，甚至是两个。优化时可以维护一个当前栈长度的add数组。即每次push时add也push一个0，pop时add也pop。