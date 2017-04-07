title: LeetCode中关于整数处理的几个题目
date: 2016-5-2 15:03:35
categories: 算法
tags: 
  - algorithm
toc: true
comments: true
---

本文主要介绍LeetCode中关于整数的几个算法问题：
第7题：[Reverse Integer](https://leetcode.com/problems/reverse-integer/)
第8题：[String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)
第9题：[Palindrome Number](https://leetcode.com/problems/palindrome-number/)
第13题：[Roman to Integer](https://leetcode.com/problems/roman-to-integer/)

# Reverse Integer
 
题目描述：
Reverse digits of an integer.
Example1: x = 123, return 321
Example2: x = -123, return -321
 
题目的提示：
Have you thought about this?
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!
If the integer’s last digit is 0, what should the output be? ie, cases such as 10, 100.
Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?
For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.
方法一思路：只要考虑到溢出的情况存在，这道题目的思路还是相对简单的，按顺序把一个数从个位开始分离，每次循环把低位数升一级，最高位变为最低位。
```C++
const int max_int = (1 << 31) - 1;
const int min_int = -max_int - 1;
class Solution {
public:
    int reverse(int x) {
         int y = 0;
        int n;
    while(x != 0) {
        n = x % 10;
        if(y > max_int / 10 || y < min_int / 10)
        return 0;
        y = y * 10 + n;
        x /= 10;
    }  
    return y;
    }
     
};
```
完整代码地址：[ReverseInteger.cpp](https://github.com/Evanqiao/LeetCode-qyh/blob/master/7_ReverseInteger/ReverseInteger.cpp)
 
方法二思路：把整数转换成字符串，然后再把字符串进行逆转。
```Python
#!/usr/bin/env python3    
def reverse(x):    
    s = str(x)    
    if(x >= 0):    
        res = int(str(x)[::-1])    
    else:    
        res = -int(str(-x)[::-1])    
    if res > 2 ** 31 - 1 or res < -2 ** 31:    
        return 0    
    return res    
if __name__ == '__main__':    
    a = reverse(-10324)    
    print(str(a))
 ```
 
# String to Integer (atoi)
 
题目描述：
 
Implement atoi to convert a string to an integer.
Hint: Carefully consider all possible input cases. If you want a challenge, please do not see below and ask yourself what are the possible input cases.
Notes: It is intended for this problem to be specified vaguely (ie, no given input specs). You are responsible to gather all the input requirements up front.
 
需要注意：要考虑边界条件，如输入的字符串为空；输入的字符串带正负号；输入到字符串中包含非数字的字符；转为整数溢出的情况。
```C
int myAtoi(char* str) {
if (str==NULL || *str=='\0'){
        return 0;
    }
     
    int ret=0;
     
    for(;isspace(*str); str++);
     
    bool neg=false;
    if (*str=='-' || *str=='+') {
        neg = (*str=='-') ;
        str++;
    }
     
    for(; isdigit(*str); str++) {
        int digit = (*str-'0');
        if(neg){
            if( -ret < (INT_MIN + digit)/10 ) {
                return INT_MIN;
            }
        }else{
            if( ret > (INT_MAX - digit) /10 ) {
                return INT_MAX;
            }
        }
 
        ret = 10*ret + digit ;
    }
     
    return neg ? -ret : ret;
}
```
完整代码：[StringToInteger.c](https://github.com/Evanqiao/LeetCode-qyh/blob/master/8_StringToInteger/StringToInteger.c)
 
# Palindrome Number
 
题目描述：
Determine whether an integer is a palindrome. Do this without extra space.
思路：注意到负数不是回文数。给一个数32623，首先计算出这个数的长度，即算出它的数量级，最高位是千、万还是十万位，然后逐步分离该数字的最低位和最高位，拿最低位和最高位进行比较，如果不相等就不是回文数，否则即为回文数。
```C++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x < 0) return false;
    int len = 1;
    int lhs = 0;
    int rhs = 0;
    for(int temp = x / 10; temp != 0; temp /= 10, len *= 10);
    while(x) {
        lhs = x / len;
        rhs = x % 10;
        if(lhs != rhs)
        return false;
        x = (x % len) / 10;
        len /= 100;
    }
    return true;
    }
};
```
完整代码：[palindromeNumber.cpp](https://github.com/Evanqiao/LeetCode-qyh/blob/master/9_PalindromeNumber/palindromeNumber.cpp)
 
# Roman to Integer
 
题目描述：
 
Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.
首先要知道Roman数的计数规则：
```
 /*    
 * 简单的罗马数字见下:    
 * I - 1     
 * II - 2     
 * III - 3     
 * IV - 4     
 * V - 5     
 * VI - 6     
 * X - 10     
 * L - 50     
 * C - 100     
 * D - 500     
 * M - 1000     
 * 罗马数字共有七个，即I(1)，V(5)，X(10)，L(50)，C(100)，D(500)，M(1000)。    
 * 按照下面的规则可以表示任意正整数。    
 *    
 * 重复数次：一个罗马数字重复几次，就表示这个数的几倍。     
 * 右加左减：在一个较大的罗马数字的右边记上一个较小的罗马数字，表示大数字加小数字。    
 * 在一个较大的数字的左边记上一个较小的罗马数字，表示大数字减小数字。    
 * 但是，左减不能跨越等级。比如，99不可以用IC表示，用XCIX表示。     
 * 加线乘千：在一个罗马数字的上方加上一条横线或者在右下方写M，    
 * 表示将这个数字乘以1000，即是原数的1000倍。    
 * 同理，如果上方有两条横线，即是原数的1000000倍。     
 * 单位限制：同样单位只能出现3次，如40不能表示为XXXX，而要表示为XL。     
 *    
 * MCXIV----1114    
 * CCCLIX---359    
 */
```
思路：先把每一个罗马数字的字母对应的数表示出来。遵循左减右加原则，从所给罗马字符串的第一个字符起开始处理。
```C++
class Solution    
{    
public:    
    int romanToInt(string s)    
    {    
    int len = s.size();    
    if(s.size() <= 0)    
        return 0;    
    int res = romanCharToInt(s[0]);    
    for(int i = 1; i < len; i++) {    
        int prev = romanCharToInt(s[i-1]);    
        int curr = romanCharToInt(s[i]);    
        if(prev < curr) {    
            res = res - prev + (curr - prev);       
        } else {    
            res += curr;      
        }    
    }    
    return res;    
    }    
    int romanCharToInt(char ch)    
    {    
    int val = 0;     
    switch(ch) {    
    case 'I':    
        val = 1;    
        break;    
    case 'V':    
        val = 5;    
        break;    
    case 'X':    
        val = 10;    
        break;    
    case 'L':    
        val = 50;    
        break;    
    case 'C':    
        val = 100;    
        break;    
    case 'D':    
        val = 500;    
        break;    
        case 'M':    
        val = 1000;    
        break;    
    default:    
        break;    
    }    
    return val;        
    }    
};
```
完整代码：[RomanToInteger.cpp](https://github.com/Evanqiao/LeetCode-qyh/blob/master/13_RomanToInteger/RomanToInteger.cpp)
