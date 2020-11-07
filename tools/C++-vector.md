## C++ vector用法总结

### 1. vector初始化
``` 
#include<vector>;

vector<int> a(10);//定义10个元素，无初值

vector<int> a(10,1);//初值是1

vector<int>a(b);//复制b向量给a

vector<int>a(b.begin(),b.begin()+3);//a为b中第0个到第2个元素

int b[7]={1,2,3,4,5};
vector<int> a(b,b+3);
```


### 2.vector重要操作
**在插入和删除操作时都不算最后一个元素**

```
a.assign(b.begin(),b.begin()+3);//b也为vector，将b的0-2个元素赋值给a

a.assign(4,2);//a有4个元素，且每个元素值为2；

a.back();//返回a的最后一个元素

a.front();//返回a的第一个元素

a[i];//返回a的第i个元素，a中元素必须已经存在

a.clear();//清空a的元素

a.empty();//判断a是否为空

a.pop_back();//删除a中的最后一个元素

a.erase(a.begin()+1,a.begin()+3);//删除a中第1个-第2个元素，不包括第3个

a.push_back(5);//在a向量最后插入一个元素5

a.insert(a.begin()+1,5);//在a第一个元素的位置插入5

a.insert(a.begin()+1,3,5);//在a第一个元素的位置插入3个5

a.insert(a.begin()+1,b+3,b+6);//在a的第一个元素的位置插入b的第3个-第5个元素，不包括第6个

a.size();//返回a中元素的个数

a.capacity();//返回a在内存中总共可容纳的元素个数

a.resize(10);//将a的元素个数调整为10个，多删少补，值随机

a.resize(10,2);将a的元素个数调整为10个，多删少补，值为2

a.reserve(100);//扩充a的元素为100，一般用于capacity后

a.swap(b);将a和b中的元素整体性交换

a==b;//向量比较操作

```

### 3.访问vector的方式
- 添加元素
```
//向a中添加元素
vector<int> a;
for(int i=0;i<10;i++)
a.push_back(i)

//从数组中选择元素添加
int a[6]={1,2,3,4,5,6};
vector<int> b;
for(int i=1;i<=4;i++)
b.push_back(a[i]);

//从现有向量中选择元素
int a[6]={1,2,3,4,5,6};
vector<int> b;
vector<int> c(a,a+4);//1,2,3,4
for(vector<int>::iterator it=c.begin();it<c.end();it++)
b.push_back(*it)

```
**误区**

```
//下标只能获取已存在的元素
vector<int> a;
for(int i=0;i<10;i++)
    a[i]=i;
```
- 读取元素
```
//下标读取
int a[6]={1,2,3,4,5,6};
vector<int> b(a,a+4);
for(int i=0;i<=b.size()-1;i++)
    cout<<b[i]<<" ";  
    
//遍历器读取
int a[6]={1,2,3,4,5,6};
vector<int> b(a,a+4);
for(vector<int>::iterator it=b.begin();it!=b.end();it++)
    cout<<*it<<" ";
    
```

### 4.常用算法
```
//都不包括a.end()
#inlcude<algorithm>
sort(a,begin(),a.end());
reverse(a.begin(),a.end());
copy(a.begin(),a.end(),b.begin()+1);
find(a.begin(),a.end(),10);
```