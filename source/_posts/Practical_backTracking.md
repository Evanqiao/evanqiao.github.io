title: 实践回溯法
date: 2017-6-4 11:12:36
categories: 我学编程
tags: 
 - Algorithm
toc: true
comments: true
mathjax: true
---

## 素数环问题

输入一个整数n，输出所有由1~n排成的环，满足任意两个相邻数之和都是素数。  
样例输入：  
6   
样例输出：  
1 4 3 2 5 6  
1 6 5 2 3 4  
（注：输出都以1开头，其他循环的不用输出，如4 3 2 5 6 1）

思路：
```
输入n
状态选择：以环的前i个数为状态
void solve(int a[], int i) {
    if(i == n) {
        判断a是否符合素数环条件，符合就输出
    }
    for(a[i] = 1, i <= n; a[i]++) {
        solve(a, i + 1)
    }
}
调用solve(0)
```
solve和search是竞赛中最常出现的函数名称，意为用暴力方法解决问题。

题目的代码为：[primering.cpp](https://github.com/Evanqiao/CSTutorship/blob/master/Algorithm/%E5%9B%9E%E6%BA%AF%E7%AE%97%E6%B3%95/%E7%B4%A0%E6%95%B0%E7%8E%AF%E9%97%AE%E9%A2%98/primering.cpp)

本题目在判断某个数是否是素数时，存在重复的判断，可以进行优化，预先把`1 ~ 2n-1`的数是否是素数存到一个表中，在使用的时候，不需要再计算，直接查询即可。代码如下所示。
```C++
#include <iostream>
#include <cmath>
#include <vector>
#include <ctime>

using namespace std;

vector<int> table;

int isPrime(int n) {    // 判断质数
    if (n==1 || n==2 || (n>2 && n%2==0)){
        return 0;
    }
    for (int i=3;i<=sqrt(n);i+=2){
        if (n%i==0){
            return 0;
        }
    }
    return 1;
}


// 回溯法遍历
void search(int data[],int n,int i) {
    /*
    cout<<"Seaching: "<<data[1]; // 日志
    for (int j=2;j<=i;j++){
        cout<<" "<<data[j];
    }
    cout<<endl;
     */
    
    if (i==n){    // 递归边界，n个数都确定了位置
        if (table[data[n]+data[1]]) {
            for (int j=1;j<=n;j++){  // 最后判断首尾和是不是素数，是的话就找到一个解
                cout<<data[j]<<" ";
            }
            cout<<endl;
        }
        return;
    }
    
    int tmp=data[i];
    for (int j=i;j<=n;j++){  // 枚举尚未确定的数字
        if (table[data[j]+data[i-1]]){ // 剪枝
            data[i]=data[j];    // 交换i,j位的数字
            data[j]=tmp;
            search(data,n,i+1);   // 递归搜索下一个数
            data[j]=data[i];    // 恢复第j位的数字
        }
    }
    data[i]=tmp;    // 恢复第i位的数字
}


int main(){
    int n;
    cin>>n;
    if (n%2==1) {   // 预判，也是一种优化
        cout<<"Impossible!"<<endl;
        return 0;
    }
    clock_t start, end;
    start = clock();
    table.reserve(2 * n);
    for(int i = 1; i < 2 * n; i++) {
        table[i] = isPrime(i);
    }
    int data[n+1];
    for (int j=1;j<=n;j++) {    // 初始化排列
        data[j]=j;
    }
    search(data,n,2);   // 为什么从2开始？因为循环对称性，第1位固定为1}
    end = clock();
    // 用来统计算法的执行时间
    cout << (end - start) / CLOCKS_PER_SEC << " s" << endl;
    return 0;
}
```

## 再谈小球下落问题

$2^D-1$个开关排列成深度为D的二叉树，最初都为关闭状态。有n个小球从顶端依次落下，并遵循如下规则：

如果经过一个关闭的开关，则开关打开，小球落向左侧；
如果经过一个打开的开关，则开关关闭，小球落向右侧；
输入D，n，输出最后一个小球最终落到的位置。
样例输入:
4 3
样例输出：
10

原来的方法是模拟小球下落的过程，具体代码见 [初探二叉树的应用](http://blog.xiaojn.cn/2017/05/14/Practical_binaryTree/) 一文。

&emsp;&emsp;原模拟算法的时间复杂度很高。注意到有这样的规律，节点`k`左右子节点编号分别为`2k`和`2k+1`，每个小球都会落在根结点上，因此前两个小球必然是一个在左子树，一个在右子树。一般地，只需看小球编号的奇偶性，就能知道它是最终在哪棵子树中。对于那些落入根结点左子树的小球来说，只需知道该小球是第几个落在根的左子树里的，就可以知道它下一步往左还是往右了。依此类推，直到小球落到叶子上。
&emsp;&emsp;对于编号为`id`的小球，如果`id`为奇数，它是往左走的第`(id+1)/2`个小球，如果`id`为偶数，它是往右走的第`id/2`个小球。这样就可以直接模拟出最后一个小球的路线：
```C++
int k = 1;
for(int i = 0; i < D - 1; i++) {
    if(n % 2) {
        k = k * 2;
        n = (n + １) / 2;
    } else {
        k = k * 2 + 1;
        n /= 2;
    }
}
```

## 八皇后问题

&nbsp;&nbsp;This one is a classic in computer science. The goal is to assign eight queens to eight positions on an 8x8 chessboard so that no queen, according to the rules of normal chess play, can attack any other queen on the board. 
In pseudocode, our strategy will be:
```
Start in the leftmost columm
If all queens are placed, return true
for (every possible choice among the rows in this column)
    if the queen can be placed safely there,
        make that choice and then recursively try to place the rest of the queens
        if recursion successful, return true
        if !successful, remove queen and try another row in this column
if all rows have been tried and nothing worked, return false to trigger backtracking
```
![八皇后问题棋盘状态树](/images/algorithm/backtracking/8queens.png)
```C++
#include <iostream>
#include <vector>
#include <cstdlib>
using namespace std;

const int NUM_QUEENS = 8;
template <typename T>
class Grid {
private:
    T v[NUM_QUEENS][NUM_QUEENS] = { {0} };
public:
    /*
     * 皇后是按列进行放置的，每列只有一个皇后，本函数是检查前col-1个皇后和现在放置在
     * (col, row) 上的皇后是否冲突
     */
    bool isSafe(int col, int row) {
        for (int i = 0; i < NUM_QUEENS; ++i) {
            for (int j = 0; j < col; ++j) {
                if (v[i][j] == 1) {
                    if ( i == row || abs(i - row) == abs(j - col) )
                        return false;
                }
            }
        }
        return true;
    }
    bool solve(int col) {
        // 进入该函数时，前col-1个皇后已经正确的放置在了棋盘上
        if (col >= NUM_QUEENS) {
            print();
            return true;
        }
        for (int rowToTry = 0; rowToTry < NUM_QUEENS; rowToTry++) {
            v[rowToTry][col] = 1;
            if (isSafe(col, rowToTry)) {
                solve(col + 1);     // recur to place rest
            }
            v[rowToTry][col] = 0;   // failed, remove, try again
        }
        return false;
    }
    void print() {
        static int count = 1;
        cout << "Case " << count++ << endl;
        for (int i = 0; i < NUM_QUEENS; i++) {
            for (int j = 0; j < NUM_QUEENS; j++) {
                v[i][j] == 1 ? printf("%c ", 2) : printf(". ");
            }
            cout << endl;
        }
        cout << endl;
    }

    void printSimply() {
        for (int i = 0; i < NUM_QUEENS; i++) {
            for (int j = 0; j < NUM_QUEENS; j++) {
                if(v[i][j] == 1)
                    cout << j + 1 << " ";
            }
        }
        cout << endl;
    }
};

int main() {
    Grid<bool> board;
    board.solve(0);
    return 0;
}
```

该算法可进行空间上的优化，初始化一个一维数组`matrix[9]`，对应的是棋盘中的8行，数组中元素的值代表放置皇后的列，比如如果`matrix[i]`的值不为0，就表示棋盘第`i`行，第`matrix[i]`列放置了一个皇后。代码如下：
```C++
#include <iostream>
using namespace std;

#define N 8

int matrix[N + 1] = {0};

bool isSafe(const int &row, const int &col) {
	for (int i = 1; i <= row - 1; ++i) {
		int j = matrix[i];
		if (  j == col || abs(i - row) == abs(j - col) )
			return false;
	}
	return true;
}

void print(void) {
	for (int i = 1; i <= N; i++) {
        cout << matrix[i] << " ";
	}
	cout << endl;
}

void solve(const int &row) {
	//	进入本函数时，在N*N的棋盘前row-1行已放置了互不攻击的row-1个棋子
	//	现从第row行起继续为后续棋子选择合适位置

	if (row > N)	//	输出当前的合法布局
		print();
	else {
		for (int j = 1; j <= N; ++j) {
			matrix[row] = j;
			if (isSafe(row, j))
				solve(row + 1);
			matrix[row] = 0;
		}
	}
}

int main(void) {
	solve(1);
	return 0;
}
```
