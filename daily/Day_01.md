## Day 01(数组)

### 问题描述
给定一个由整数组成的非空数组所表示的非负整数，在该数的基础上加一。

最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

你可以假设除了整数 0 之外，这个整数不会以零开头。

示例 1:

输入: [1,2,3]
输出: [1,2,4]
解释: 输入数组表示数字 123。
示例 2:

输入: [4,3,2,1]
输出: [4,3,2,2]
解释: 输入数组表示数字 4321。

### 问题分析
由于每一位最大值为9，则需要考虑的是进位问题。无进位问题时直接加一。

### 思路&代码
1.从后往前遍历每一位，判断是否为9，若为9则置0，继续遍历；若不为9直接加1，然后返回结果。**当每一位都为9时则会退出循环，返回新数组即可**。
``` c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        for(int i=digits.size()-1;i>=0;i--){
            if(digits[i]!=9){
                digits[i]++;
                return digits;

            }
            digits[i]=0;
        }
        digits.insert(digits.begin(),1);
        return digits;

    }
};
```
2.从后往前遍历每一位同时加1，判断是否为0，若为0则继续往前加1，不为0则返回。**当每一位都为9时则会退出循环，返回新数组即可**。
``` c++
class Solution{
public:
	vector<int> plusone(vector<int>& digits){
		for(int i=digits.size()-1;i>=0;i--){
			digits[i]++;
			digits[i]=digits[i]%10;
			if(digits[i]!=0)
				return digits;
		}
		digits.insert(digits.begin(),1);
		return digits;
	}
};
```
3.使用carry来记录是否有进位。每次更新carry位。这种方法比较巧妙，有进位才会继续往后加，否则当前位就会结束（carry=0），不需要提前返回结果。
``` c++
class Solution{
public:
	vector<int> plusone(vector<int>& digits){
	vector<int> res;
	int carry=1;
	for(int i=digits.Size()-1;i>=0;i++){
		res.push_back((carry+digits[i])%10);
		carry=(carry+digits[i])/10;
	}
	if(carry){
		res.push_back(carry);
	}
	reverse(res.begin(),res.end());
	return res;
	}
};
```
### 复杂度
时间复杂度：O(n)
空间复杂度：O(1)
