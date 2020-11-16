## Day 04（栈）Nov.394
### 问题描述
给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: k[encoded_string]，表示其中方括号内部的 encoded_string 正好重复 k 次。注意 k 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 k ，例如不会出现像 3a 或 2[4] 的输入。

 

示例 1：

输入：s = "3[a]2[bc]"
输出："aaabcbc"
示例 2：

输入：s = "3[a2[c]]"
输出："accaccacc"
示例 3：

输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
示例 4：

输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"

### 问题分析
因为存在左右括号，可以比较直接的想到括号匹配的解法。但需要思考多位数字以及括号嵌套的具体情况。还可以用递归求解。

### 思路&代码
1.建立两个栈，一个数字栈一个字符栈。同时用一个字符串变量存储当前已经遇到的字符，用一个数字变量计算倍数。
``` c++
class Solution {
public:
    string decodeString(string s) {
        stack<int> nums;//数字栈
        stack<string> res;//字符栈
        int num=0;//倍数
        string cur;//字符串

        for(int i=0;i<s.size();i++){//每一位遍历

            if((s[i]>='a'&&s[i]<='z')||(s[i]>='A'&&s[i]<='Z')){
                cur+=s[i];//遇到字符加到cur里
            }

            else if(s[i]>='0'&&s[i]<='9'){
                num=num*10+s[i]-'0';//遇到数字计算，同时将字符转换为数字
            }

            else if(s[i]=='['){//遇到左括号将之前的字符和数字分别入栈，同时变量置零来应对括号嵌套。
                nums.push(num);
                num=0;
                res.push(cur);
                cur="";
            }
            else {//遇到右括号
                int times=nums.top();//nums栈栈顶的值就是倍数
                nums.pop();
                for(int i=0;i<times;i++){
                    res.top()+=cur;//将字符串翻倍后赋给res字符串栈的栈顶
                }
                cur=res.top();//再将得到的值赋给cur
                res.pop();
            }
            
        }
        return cur;
    }
};
```
2.递归。**要解决外层括号，先要解决内层括号**，所以可以用递归的思想来思考。
碰到左括号时，解析字符串。如果在字符串中遇到左括号，则可以开始递归。直到碰到右括号结束。
``` c++
class Solution{
public:
	string analysis(string S,int &index){//递归函数
		string res;
		int num=0;
		string temp;
		while(index<S.size()){
			if(S[index]>='0'&& S[index]<='9'){
				num=10*num+s[index]-'0';//计算倍数
		}
			else if(S[index]=='['){
				temp=analysis(S,++index);//遇到左括号，进行递归
				for(int i=0;i<num;i++){//将res求倍数赋予temp，同时将倍数置零
					res+=temp;
					num=0;
			}
			else if(S[index]==']')
				break;//遇到右括号退出，递归结束
			else res+=S[index];//遇到字符加入res内
			index++;
		}
			return res;
	}
	string decodeString(string S){
		int index=0;
		return analysis(S,index);
}
};


```
**递归的想法非常巧妙，虽然没有比栈的做法简单多少，但思想值得学习。**