---
title: leetcode刷题记录
date: 2018-07-20 23:56:20
tags: leetcode
mathjax: true
---

# leetcode刷题记录
## 用C++做的leetcode题目
* Given a 32-bit signed integer, reverse digits of an integer.  
Note:  
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range $[-2^{31},2^{31}-1]$ 
For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.  

反转整数问题，利用"弹出"思想，将原数最高位一个一个弹出，构成新数的最低位。  
代码:  
<pre name="code" class = "c++">
#incdlue &ltiostream>
#include &ltcmath>
using namespace std;
class Solution
{
    public:
    int reverse(int x)
    {
        while(x != 0)
        {
            int pop = 0;
            int reverse = 0;
            pop = x%10;
            x = x/10
            reverse = reverse*10 + pop;
        }
        return reverse;       
    }
};
</pre>  
以上代码段是不考虑溢出的。题目要求考虑溢出，也给出整数范围。我们计算$-2^{31}=-2 147 483 648$, $2^{31}-1 = 2 147 483 647$  
考虑溢出可能发生的地方 reverse = reverse*10 + pop，所以要在进行该操作之前进行一次判断  
<pre name="code" class = "c++">
if (reverse > INT_MAX/10 || (reverse == INT_MAX/10 && pop > 7))   return 0;
if (reverse < INT_MIN/10 || (reverse == INT_MIN/10 && pop < -8))   return 0;

reverse = reverse*10 + pop;
</pre>