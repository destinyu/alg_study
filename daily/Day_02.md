## Day 02(数组、栈) Nov.821

### 问题描述
给定一个字符串 S 和一个字符 C。返回一个代表字符串 S 中每个字符到字符串 S 中的字符 C 的最短距离的数组。

示例 1:

输入: S = "loveleetcode", C = 'e'
输出: [3, 2, 1, 0, 1, 0, 0, 1, 2, 2, 1, 0]
说明:

字符串 S 的长度范围为 [1, 10000]。
C 是一个单字符，且保证是字符串 S 里的字符。
S 和 C 中的所有字母均为小写字母。

### 问题分析
以当前位为基准，开始寻找对应的字符。记录两个字符之间的距离并存入数组中。**要注意有的字符夹在两个目标字符之间，则需要比较哪个最短。**

### 思路&代码
1.暴力解法（数组）：可以用双层循环遍历，第一层控制当前的元素，第二层向左向右开始遍历并且记录步数，最后返回较小值。
``` c++
class Solution{
public:
vector<int> shortestToChar(string S,char C){
	vector<int>res;
	for(int i=0;i<S.size();i++){
		int countLeft=0,countRight=0;
		int left,right;
		for(left=i;left>=0&&S[left]!=C;left--){//向左遍历，不等则计算步数
			countLeft++;
	}
		for(right=i;right<S.szie()&&S[right]!=C;right++){//向右遍历，不等则计算步数
			countRight++;
		}
		if(left!=-1&&right!=S.size())//左右都遍历结束，返回较小值
			res.push_back(min(countLeft,countRight));
		else if(left==-1)//只有右边有目标
			res.push_back(countRight);
		else if(right==S.size())//只有左边有目标
			res.push_back(countLeft);
	}
	return res;
}
}
```
2.最小数组：在暴力解法的思路上对复杂度做一个优化。**对于每一个字符，找到距离向左或者向右的下一个字符的距离**，最后返回这两值的较小值。从左向右，记录上一个目标字符出现的位置prev，结果是i-prev。从右向左，记录上一个目标字符出现的位置prev，结果就是prev-i。
``` c++
class Solution{
	public:
vector<int> shortestToChar(string S,char C){
	vector<int>pos(S.size(),0);
	int pre=10000;//表示距离为无限大
	for(int i=0;i<S.size();i++){//从左往右遍历
		if(S[i]==C)
		pre=i;//当前目标字符的位置
		pos[i]=ans(pre-i);
	}
	for(int i=S.size()-1;i?=0;i--){//从右往左遍历
		if(S[i]==C)
		pre=i;
		pos[i]=min(pos[i],abs(pre-i));
		
	}
	return pos;
	
}
}
```