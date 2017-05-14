title: C++和Java中字符串与数字的相互转换
date: 2017-4-9 22:31:36
categories: 算法
tags: 
 - Algorithm
toc: true
comments: true
---
## 快速参考
**数字转字符串：**
C++
```C++
方法1)
int sprintf( char* buffer, const char* format, ... );

方法2)
std::ostringstream out("1 2");
out << 3;
std::cout << out.str() << std::endl;

方法3）
int a = 100;  double d = -10.02;
cout << to_string(a) << " " << to_string(d) << endl;

方法4）
char *  itoa ( int value, char * str, int base );
不推荐使用

```
Java
```Java
String s1 = Integer.toString(a, 10);
String s2 = Float.toString(f);
String s3 = Float.toHexString(f);
String s4 = Double.toString(d);
String s5 = Character.toString(c);
String s1 = String.valueOf(a);
String s2 = String.valueOf(f);
String s3 = String.valueOf(d);
String s4 = String.valueOf(c);
String s5 = String.valueOf(arr);
```
**字符串转数字：**
C++
```C++
方法1)
int sscanf( const char* buffer, const char* format, ... ); 

方法2)
std::istringstream in;  // could also use in("1 2")
in.str("1 2");
in >> n;  // n = 1

方法3）
int stoi( const std::string& str, std::size_t* pos = 0, int base = 10 );
float stof( const std::string& str, std::size_t* pos = 0 );

方法4）
int atoi( const char *str );
double atof( const char *str );
long strtol( const char *str, char **str_end, int base );
float strtof( const char* str, char** str_end );  (since C++11)

```
Java
```Java
int i = Integer.parseInt(s1, 10);
float f = Float.parseFloat(s2);
double d = Double.parseDouble(s3);
```

## 数字转字符串
### C++中如何把数字转换为字符串
1. 用`sprintf`函数
2. 用`ostreamstring`对象
3. 用`to_string`函数
4. 用`itoa`函数

#### 用`sprintf`函数
```C++
std::printf, std::fprintf, std::sprintf, std::snprintf
Defined in header <cstdio>
int printf( const char* format, ... );  (1)	
int fprintf( std::FILE* stream, const char* format, ... );  (2)	
int sprintf( char* buffer, const char* format, ... );  (3)	
int snprintf( char* buffer, std::size_t buf_size, const char* format, ... );  (4)	(since C++11)
```
从给定位置加载数据，将它们转换为字符串等效项，并将结果写入各种接收器。
1) Writes the results to stdout.
2) Writes the results to a file stream stream.
3) Writes the results to a character string buffer.
4) Writes the results to a character string buffer. At most `buf_size - 1` characters are written. The resulting character string will be terminated with a null character, unless `buf_size` is zero. If `buf_size` is zero, nothing is written and buffer may be a null pointer, however the return value (number of bytes that would be written) is still calculated and returned.
If a call to sprintf or snprintf causes copying to take place between objects that overlap, the behavior is undefined.

**Parameters**
**stream**	    -	output file stream to write to
**buffer**	    -	pointer to a character string to write to
**buf_size**	-	up to `buf_size - 1` characters may be written, plus the null terminator
**format**	    -	pointer to a null-terminated multibyte string specifying how to interpret the data. The format string consists of ordinary multibyte characters (except %), which are copied unchanged into the output stream, and conversion specifications.

**Return value**
1-2) Number of characters written if successful or a negative value if an error occurred.
3) Number of characters written if successful (not including the terminating null character) or a negative value if an error occurred.
4) Number of characters that would have been written for a sufficiently large buffer if successful (not including the terminating null character), or a negative value if an error occurred. Thus, the (null-terminated) output has been completely written if and only if the returned value is nonnegative and less than buf_size.


----------
例子：
```C++
#include <cstdio>
#include <limits>
#include <cstdint>
#include <cinttypes>
 
int main()
{
    std::printf("Strings:\n");
 
    const char* s = "Hello";
    std::printf("\t[%10s]\n\t[%-10s]\n\t[%*s]\n\t[%-10.*s]\n\t[%-*.*s]\n",
        s, s, 10, s, 4, s, 10, 4, s);
 
    std::printf("Characters:\t%c %%\n", 65);
 
    std::printf("Integers\n");
    std::printf("Decimal:\t%i %d %.6i %i %.0i %+i %u\n", 1, 2, 3, 0, 0, 4, -1);
    std::printf("Hexadecimal:\t%x %x %X %#x\n", 5, 10, 10, 6);
    std::printf("Octal:\t%o %#o %#o\n", 10, 10, 4);
 
    std::printf("Floating point\n");
    std::printf("Rounding:\t%f %.0f %.32f\n", 1.5, 1.5, 1.5);
    std::printf("Padding:\t%05.2f %.2f %5.2f\n", 1.5, 1.5, 1.5);
    std::printf("Scientific:\t%E %e\n", 1.5, 1.5);
    std::printf("Hexadecimal:\t%a %A\n", 1.5, 1.5);
    std::printf("Special values:\t0/0=%g 1/0=%g\n", 0.0/0.0, 1.0/0.0);
 
    std::printf("Variable width control:\n");
    std::printf("right-justified variable width: '%*c'\n", 5, 'x');
    int r = std::printf("left-justified variable width : '%*c'\n", -5, 'x');
    std::printf("(the last printf printed %d characters)\n", r);
 
    // fixed-width types
    std::uint32_t val = std::numeric_limits<std::uint32_t>::max();
    std::printf("Largest 32-bit value is %" PRIu32 " or %#" PRIx32 "\n", val, val);
}
```
```
Strings:
        [     Hello]
        [Hello     ]
        [     Hello]
        [Hell      ]
        [Hell      ]
Characters:     A %
Integers
Decimal:        1 2 000003 0  +4 4294967295
Hexadecimal:    5 a A 0x6
Octal:  12 012 04
Floating point
Rounding:       1.500000 2 1.30000000000000004440892098500626
Padding:        01.50 1.50  1.50
Scientific:     1.500000E+00 1.500000e+00
Hexadecimal:    0x1.8p+0 0X1.8P+0
Special values: 0/0=nan 1/0=inf
Variable width control:
right-justified variable width: '    x'
left-justified variable width : 'x    '
(the last printf printed 40 characters)
Largest 32-bit value is 4294967295 or 0xffffffff
```
详见：[http://en.cppreference.com/w/cpp/io/c/fprintf](http://en.cppreference.com/w/cpp/io/c/fprintf)

#### 用`ostreamstring`对象
```C++
#include <sstream>
#include <iostream>
int main()
{
    int n;
 
    std::istringstream in;  // could also use in("1 2")
    in.str("1 2");
    in >> n;
    std::cout << "after reading the first int from \"1 2\", the int is "
              << n << ", str() = \"" << in.str() << "\"\n";
 
    std::ostringstream out("1 2");
    out << 3;
    std::cout << "after writing the int '3' to output stream \"1 2\""
              << ", str() = \"" << out.str() << "\"\n";
 
    std::ostringstream ate("1 2", std::ios_base::ate);
    ate << 3;
    std::cout << "after writing the int '3' to append stream \"1 2\""
              << ", str() = \"" << ate.str() << "\"\n";
}
```
```
Output:
after reading the first int from "1 2", the int is 1, str() = "1 2"
after writing the int '3' to output stream "1 2", str() = "3 2"
after writing the int '3' to append stream "1 2", str() = "1 23"
```
打开输入输出流的方式：

| Constant  | Explanation | 
| :-----    | :-----|
| app       | seek to the end of stream before each write  | 
| binary    | open in binary mode  | 
| in        | open for reading  | 
| out       | open for writing |
| trunc     | discard the contents of the stream when opening |
| ate       | seek to the end of stream immediately after open |

#### 用`to_string`函数
`to_string`函数是C++11标准引入的，它可以把int,long,double,long long等数据类型的变量转换成string类型，`to_string`函数定义在头文件`<string>`中，把数字转变为string类型和直接用`std::cout`打印出来的效果略有差别，具体看下面的例子。
```C++
#include <iostream>
#include <string>
using namespace std;
int main() 
{
    double f = 23.43;
    double f2 = 1e-9;
    double f3 = 1e40;
    double f4 = 1e-40;
    double f5 = 123456789;
    std::string f_str = std::to_string(f);
    std::string f_str2 = std::to_string(f2); // Note: returns "0.000000"
    std::string f_str3 = std::to_string(f3); // Note: Does not return "1e+40".
    std::string f_str4 = std::to_string(f4); // Note: returns "0.000000"
    std::string f_str5 = std::to_string(f5);
    std::cout << "std::cout: " << f << '\n'
              << "to_string: " << f_str  << "\n\n"
              << "std::cout: " << f2 << '\n'
              << "to_string: " << f_str2 << "\n\n"
              << "std::cout: " << f3 << '\n'
              << "to_string: " << f_str3 << "\n\n"
              << "std::cout: " << f4 << '\n'
              << "to_string: " << f_str4 << "\n\n"
              << "std::cout: " << f5 << '\n'
              << "to_string: " << f_str5 << '\n';
    int a = -100;
    float b = -012.00;
    cout << to_string(a) << " " << to_string(b) << endl;
}
```
```
Output:
std::cout: 23.43
to_string: 23.430000

std::cout: 1e-09
to_string: 0.000000

std::cout: 1e+40
to_string: 10000000000000000303786028427003666890752.000000

std::cout: 1e-40
to_string: 0.000000

std::cout: 1.23457e+08
to_string: 123456789.000000
-100 12.000000
```

#### 用`itoa`函数
函数原型为
```
char *  itoa ( int value, char * str, int base );
```
该函数会把`value`按照`base`进制转换成字符串并存放在`str`所指向的内存单元中，返回值是一个以`null`结尾的字符串，和参数`str`一样。该函数没有在**ANSI-C**中定义，也不是C++的一部分，但是一些编译器支持这样使用，定义在头文件`<stdlib.h>`中。
既然有这么多选择，所以不推荐使用此函数，该函数的可移植性比较差。

### C++中如何把字符串转换为数字
1. 用`sscanf`函数
2. 用`istreamstring`对象
3. 用`stoi` `stof` `stod`函数
4. 用`atoi` `atof` `strtof`函数

#### 用`sscanf`函数
```C++
std::scanf, std::fscanf, std::sscanf
Defined in header <cstdio>
int scanf( const char* format, ... );​  (1)	
int fscanf( std::FILE* stream, const char* format, ... );  (2)	
int sscanf( const char* buffer, const char* format, ... );  (3)
```
> Reads data from the a variety of sources, interprets it according to format and stores the results into given locations.
1) Reads the data from stdin
2) Reads the data from file stream stream
3) Reads the data from null-terminated character string buffer
**Return value:**
Number of receiving arguments successfully assigned (which may be zero in case a matching failure occurred before the first receiving argument was assigned), or EOF if input failure occurs before the first receiving argument was assigned.

```C++
#include <iostream>
#include <clocale>
#include <cstdio>
 
int main()
{
    int i, j;
    float x, y;
    char str1[10], str2[4];
    wchar_t warr[2];
    std::setlocale(LC_ALL, "en_US.utf8");
 
    char input[] = u8"25 54.32E-1 Thompson 56789 0123 56ß水";
    // parse as follows:
    // %d: an integer 
    // %f: a floating-point value
    // %9s: a string of at most 9 non-whitespace characters
    // %2d: two-digit integer (digits 5 and 6)
    // %f: a floating-point value (digits 7, 8, 9)
    // %*d an integer which isn't stored anywhere
    // ' ': all consecutive whitespace
    // %3[0-9]: a string of at most 3 digits (digits 5 and 6)
    // %2lc: two wide characters, using multibyte to wide conversion
    int ret = std::sscanf(input, "%d%f%9s%2d%f%*d %3[0-9]%2lc",
                     &i, &x, str1, &j, &y, str2, warr);
 
    std::cout << "Converted " << ret << " fields:\n"
              << "i = " << i << "\nx = " << x << '\n'
              << "str1 = " << str1 << "\nj = " << j << '\n'
              << "y = " << y << "\nstr2 = " << str2 << '\n'
              << std::hex << "warr[0] = U+" << warr[0]
              << " warr[1] = U+" << warr[1] << '\n';
}
/*
Output:
Converted 7 fields:
i = 25
x = 5.432
str1 = Thompson
j = 56
y = 789
str2 = 56
warr[0] = U+df warr[1] = U+6c34
*/
```
#### 用`istreamstring`对象 
```C++
std::istringstream in;  // could also use in("1 2")
    in.str("1 2");
    in >> n;
    std::cout << "after reading the first int from \"1 2\", the int is "
              << n << ", str() = \"" << in.str() << "\"\n";
/*
Output:
after reading the first int from "1 2", the int is 1, str() = "1 2"
*/
```
#### 用`stoi` `stof` `stod`函数
```
std::stoi, std::stol, std::stoll
Defined in header <string>
int       stoi( const std::string& str, std::size_t* pos = 0, int base = 10 );
int       stoi( const std::wstring& str, std::size_t* pos = 0, int base = 10 );  (1)	(since C++11)
long      stol( const std::string& str, std::size_t* pos = 0, int base = 10 );
long      stol( const std::wstring& str, std::size_t* pos = 0, int base = 10 );  (2)	(since C++11)
long long stoll( const std::string& str, std::size_t* pos = 0, int base = 10 );
long long stoll( const std::wstring& str, std::size_t* pos = 0, int base = 10 );  (3)	(since C++11)

std::stoul, std::stoull
Defined in header <string>
unsigned long      stoul( const std::string& str, std::size_t* pos = 0, int base = 10 );
unsigned long      stoul( const std::wstring& str, std::size_t* pos = 0, int base = 10 );  (1)	(since C++11)
unsigned long long stoull( const std::string& str, std::size_t* pos = 0, int base = 10 );
unsigned long long stoull( const std::wstring& str, std::size_t* pos = 0, int base = 10 );  (2)	(since C++11)

std::stof, std::stod, std::stold
Defined in header <string>
float       stof( const std::string& str, std::size_t* pos = 0 );
float       stof( const std::wstring& str, std::size_t* pos = 0 );  (1)	(since C++11)
double      stod( const std::string& str, std::size_t* pos = 0 );
double      stod( const std::wstring& str, std::size_t* pos = 0 );  (2)	(since C++11)
long double stold( const std::string& str, std::size_t* pos = 0 );
long double stold( const std::wstring& str, std::size_t* pos = 0 );  (3)	(since C++11)
```

> **Parameters**
str	-	the string to convert
pos	-	address of an integer to store the number of characters processed
base	-	the number base
**Return value**
The string converted to the specified signed integer type.
**Exceptions**
`std::invalid_argument` if no conversion could be performed
`std::out_of_range` if the converted value would fall out of the range of the result type or if the underlying function (std::strtol or std::strtoll) sets errno to ERANGE.

例子：
```C++
#include <iostream>
#include <string>
 
int main()
{
    std::string str1 = "45";
    std::string str2 = "3.14159";
    std::string str3 = "32 with words";
    std::string str4 = "words and 2";
    
    std::size_t num = 0;
    
    int myint1 = std::stoi(str1);
    float myfloat = std::stof(str2);
    int myint2 = std::stoi(str3, &num, 16);
    // error: 'std::invalid_argument'
    // int myint4 = std::stoi(str4);
 
    std::cout << "std::stoi(\"" << str1 << "\") is " << myint1 << '\n';
    std::cout << "std::stof(\"" << str2 << "\") is " << myfloat << '\n';
    std::cout << "std::stoi(\"" << str3 << "\"" << " ,&num" << " ,16" << ") is " << myint2 << '\n';
    std::cout << "num = " << num << std::endl;
    //std::cout << "std::stoi(\"" << str4 << "\") is " << myint4 << '\n';
}
/*
Output:
std::stoi("45") is 45
std::stof("3.14159") is 3.14159
std::stoi("32 with words" ,&num ,16) is 50
num = 2
*/
```
#### 用`atoi` `atof` `strtof`函数
这里的一系列函数的输入字符串都是`char*`，对于C类型的字符串可以用下面这些函数。需要注意的是`atoi`函数是按10进制解释字符串所表示的整数，如果没有可转换进行时（比如所解析的字符串首字符不是数字或正负号），返回`0`。
`strtol`函数可以按照指定的进制解释字符串所表示的整数，`strtol` `strtof` `strtod`函数的第二个参数是`char**`。
```C++
std::atoi, std::atol, std::atoll
Defined in header <cstdlib>
int       atoi( const char *str );
long      atol( const char *str );
long long atoll( const char *str );

std::atof
Defined in header <cstdlib>
double atof( const char *str );

std::strtof, std::strtod, std::strtold
Defined in header <cstdlib>
float       strtof( const char* str, char** str_end );  (since C++11)
double      strtod( const char* str, char** str_end );
long double strtold( const char* str, char** str_end );  (since C++11)

std::strtol, std::strtoll
Defined in header <cstdlib>
long      strtol( const char *str, char **str_end, int base );
long long strtoll( const char *str, char **str_end, int base );  (since C++11)
```
例子：
```C++
#include <iostream>
#include <cstdlib>
 
int main()
{
    const char *str1 = "42";
    const char *str2 = "3.14159";
    const char *str3 = "31337 with words";
    const char *str4 = "words and 2";
 
    int num1 = std::atoi(str1);
    int num2 = std::atoi(str2);
    int num3 = std::atoi(str3);
    int num4 = std::atoi(str4);
 
    std::cout << "std::atoi(\"" << str1 << "\") is " << num1 << '\n';
    std::cout << "std::atoi(\"" << str2 << "\") is " << num2 << '\n';
    std::cout << "std::atoi(\"" << str3 << "\") is " << num3 << '\n';
    std::cout << "std::atoi(\"" << str4 << "\") is " << num4 << '\n';
}
/*
Output:
std::atoi("42") is 42
std::atoi("3.14159") is 3
std::atoi("31337 with words") is 31337
std::atoi("words and 2") is 0
*/
```
```C++
#include <cstdlib>
#include <iostream>
 
int main()
{
    std::cout << std::atof("0.0000000123") << "\n"
              << std::atof("0.012") << "\n"
              << std::atof("15e16") << "\n"
              << std::atof("-0x1afp-2") << "\n"
              << std::atof("inF") << "\n"
              << std::atof("Nan") << "\n";
}
/*
Output:
1.23e-08
0.012
1.5e+17
-107.75
inf
nan
*/
```
```C++
#include <iostream>
#include <string>
#include <cerrno>
#include <cstdlib>
 
int main()
{
    const char* p = "10 200000000000000000000000000000 30 -40";
    char *end;
    std::cout << "Parsing '" << p << "':\n";
    for (long i = std::strtol(p, &end, 10);
         p != end;
         i = std::strtol(p, &end, 10))
    {
        std::cout << "'" << std::string(p, end-p) << "' -> ";
        p = end;
        if (errno == ERANGE){
            std::cout << "range error, got ";
            errno = 0;
        }
        std::cout << i << '\n';
    }
}
/*
Output:
Parsing '10 200000000000000000000000000000 30 -40':
'10' -> 10
' 200000000000000000000000000000' -> range error, got 9223372036854775807
' 30' -> 30
' -40' -> -40
*/
```
```C++
#include <iostream>
#include <string>
#include <cerrno>
#include <cstdlib>
 
int main()
{
    const char* p = "111.11 -2.22 0X1.BC70A3D70A3D7P+6  1.18973e+4932zzz";
    char* end;
    std::cout << "Parsing \"" << p << "\":\n";
    for (double f = std::strtod(p, &end); p != end; f = std::strtod(p, &end))
    {
        std::cout << "'" << std::string(p, end-p) << "' -> ";
        p = end;
        if (errno == ERANGE){
            std::cout << "range error, got ";
            errno = 0;
        }
        std::cout << f << '\n';
    }
}
/*
Output:
Parsing "111.11 -2.22 0X1.BC70A3D70A3D7P+6  1.18973e+4932zzz":
'111.11' -> 111.11
' -2.22' -> -2.22
' 0X1.BC70A3D70A3D7P+6' -> 111.11
'  1.18973e+4932' -> range error, got inf
*/
```
### Java中如何把数字转换为字符串
函数原型：
```Java
public static String toString(int i,
                              int radix)
```
该函数是类`Integer`中的一个静态方法，该函数作用是把整数按某一进制转换成字符串并返回。具体的描述如下（摘自Java SE 8参考文档）。

> Returns a string representation of the first argument in the radix specified by the second argument.  If the radix is smaller than `Character.MIN_RADIX` or larger than `Character.MAX_RADIX`, then the radix 10 is used instead. 
> 
> If the first argument is negative, the first element of the result is the ASCII minus character '-' ('\u002D'). If the first argument is not negative, no sign character appears in the result.
> 
> The remaining characters of the result represent the magnitude of the first argument. If the magnitude is zero, it is represented by a single zero character '0' ('\u0030'); otherwise, the first character of the representation of the magnitude will not be the zero character.
> The following ASCII characters are used as digits: 
```
0123456789abcdefghijklmnopqrstuvwxyz 
```
> These are '\u0030' through '\u0039' and '\u0061' through '\u007A'. If radix is N, then the first N of these characters are used as radix-N digits in the order shown. Thus, the digits for hexadecimal (radix 16) are 0123456789abcdef. If uppercase letters are desired, the `String.toUpperCase()` method may be called on the result:  `Integer.toString(n, 16).toUpperCase()`

相关函数：
```Java
public static String toUnsignedString(int i, int radix)
public static String toHexString(int i)
public static String toOctalString(int i)
public static String toBinaryString(int i)
public static String toString(int i)
```
对于Float、Double、Character向字符串的转换：
```Java
public class ToString {
    public static void main(String[] args) {
        int a = 10;
        float f = -12.70f;
        double d = 13.54;
        char c = 'A';
        String s1 = Integer.toString(a, 10);
        String s2 = Float.toString(f);
        String s3 = Float.toHexString(f);
        String s4 = Double.toString(d);
        String s5 = Character.toString(c);
        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
        System.out.println(s4);
        System.out.println(s5);
        /*
        10
        -12.7
        -0x1.966666p3
        13.54
        A
         */
    }
}
```
利用String类中的静态方法把int，float，double，char转换为字符串：
```Java
public class ToString {
    public static void main(String[] args) {
        int a = 10;
        float f = -12.70f;
        double d = 13.54;
        char c = 'A';
        char[] arr = {'h', 'e', 'l', 'l', 'o'};

        String s1 = String.valueOf(a);
        String s2 = String.valueOf(f);
        String s3 = String.valueOf(d);
        String s4 = String.valueOf(c);
        String s5 = String.valueOf(arr);
        System.out.println(s1);
        System.out.println(s2);
        System.out.println(s3);
        System.out.println(s4);
        System.out.println(s5);
        /*
        10
        -12.7
        13.54
        A
        hello
         */
    }
}
```
### Java中如何把字符串转换为数字
函数原型：
```Java
public static int parseInt(String s,
                           int radix)
                    throws NumberFormatException
```
该函数是类`Integer`中的一个静态方法，该函数作用是把字符串按某一进制转换成整数并返回。具体的描述如下（摘自Java SE 8参考文档）。

> Parses the string argument as a signed integer in the radix specified by the second argument. The characters in the string must all be digits of the specified radix (as determined by whether `Character.digit(char, int)` returns a nonnegative value), except that the first character may be an ASCII minus sign '-' ('\u002D') to indicate a negative value or an ASCII plus sign '+' ('\u002B') to indicate a positive value. The resulting integer value is returned. 

An exception of type `NumberFormatException` is thrown if any of the following situations occurs: 

- The first argument is null or is a string of length zero. 
- The radix is either smaller than Character.MIN_RADIX or larger than Character.MAX_RADIX. 
- Any character of the string is not a digit of the specified radix, except that the first character may be a minus sign '-' ('\u002D') or plus sign '+' ('\u002B') provided that the string is longer than length 1. 
- The value represented by the string is not a value of type int. 

```Java
Examples: 
 parseInt("0", 10) returns 0
 parseInt("473", 10) returns 473
 parseInt("+42", 10) returns 42
 parseInt("-0", 10) returns 0
 parseInt("-FF", 16) returns -255
 parseInt("1100110", 2) returns 102
 parseInt("2147483647", 10) returns 2147483647
 parseInt("-2147483648", 10) returns -2147483648
 parseInt("2147483648", 10) throws a NumberFormatException
 parseInt("99", 8) throws a NumberFormatException
 parseInt("Kona", 10) throws a NumberFormatException
 parseInt("Kona", 27) returns 411787
```
和它对应的还有一个函数
```Java
public static int parseInt(String s)
                    throws NumberFormatException
```
该函数是默认按照十进制进行转换的。
字符串转换到Double、Float：
```Java
public class ToString {
    public static void main(String[] args) {
        String s1 = new String("-0100");
        String s2 = "0.0050";
        String s3 = "23.98f";

        int i = Integer.parseInt(s1, 10);
        float f = Float.parseFloat(s2);
        double d = Double.parseDouble(s3);

        System.out.println(i);
        System.out.println(f);
        System.out.println(d);
        /*
        -100
        0.005
        23.98
         */
    }
}
```
