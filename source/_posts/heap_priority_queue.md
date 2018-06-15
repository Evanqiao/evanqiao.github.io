title: 堆和优先队列的算法应用
date: 2017-5-21 23:12:36
categories: 我学编程
tags: 
 - Algorithm
toc: true
comments: true
---

## 银行排队问题
有n个人去银行排队办事（n<=1000000）。银行只有一个窗口，每人有不同的优先级。每办完一个人，从等待的人中选优先级最高的人办理，同级选最先到的。
输入n以及n行数据Ai，Pi，Ti，表示第i个人的到达时间、优先级、办理用时。所有输入都是正整数，输入顺序为Ai递增顺序，输出实际办理的人的顺序。
样例输入：
5
0 1 10
2 1 10
5 3 10
7 2 5
11 3 5
样例输出：
1 3 5 4 2


### 性能一般的方法
建立一个优先队列，优先队列的优先原则是优先级大的在前，如果优先级相同，则达到时间小的在前，如果到达时间相同，则用时短的在前。设一个变量`currentTime`用来记录当前的时间，根据当前时间来提取队列中的元素，如果队顶元素不满足要求，先出队，按出队顺序保存在数组中，直到遇到符合条件的元素，就让符合条件的元素去办理业务，完后把数组中保存的元素再入队，循环。。。直到队列为空，即所有人都去办理了业务为止。
```C++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

struct Member {
    int arrTime;
    int priority;
    int conTime;
    int id;
};

void consructMember(Member &m, int arrTime, int priority, int conTime, int id) {
    m.arrTime = arrTime;
    m.priority = priority;
    m.conTime = conTime;
    m.id = id;
}

bool operator < (const Member &m1, const Member &m2) {

    if(m1.priority != m2.priority) {
        return m1.priority < m2.priority;
    } else if(m1.arrTime != m2.arrTime) {
        return  m1.arrTime > m2.arrTime;
    } else {
        return  m1.conTime > m2.conTime;
    }
}

vector<int> bankQueue(priority_queue<Member> &pq_m, int currentTime) {

    vector<int> result;
    vector<Member> vec_m;

    while(!pq_m.empty()) {
        Member m = pq_m.top();
        while(m.arrTime > currentTime) {
            vec_m.push_back(m);
            pq_m.pop();
            m = pq_m.top();
        }
        if(m.arrTime <= currentTime) {
            pq_m.pop();
            result.push_back(m.id);
            currentTime += m.conTime;
        }
        for(int i = 0; i < vec_m.size(); i++) {
            pq_m.push(vec_m[i]);
        }
        vec_m.clear();
    }
    return result;
}

/*
5
0 1 10
2 1 10
5 3 10
7 2 5
11 3 5
*/
int main() {

    int n;
    cin >> n;
    Member member[n];
    int a, p, c;
    priority_queue<Member> pq_m;
    vector<int> result;
    int currentTime;

    for(int i = 0; i < n; i++) {
        cin >> a >> p >> c;
        consructMember(member[i], a, p, c, i + 1);
        pq_m.push(member[i]);
        currentTime = i == 0 ? a : currentTime;
    }

    result = bankQueue(pq_m, currentTime);
    
    for(int i : result)
        cout << i << " ";
    cout << endl;
}
```
辅导班老师指出这个代码存在的问题：
这个算法逻辑上还有一些小bug。
首先bankQueue函数里少了一些判断，导致有些输入的时候会死循环 
另外你这个算法对某些人做了反复出堆和入堆，在某些极端情况下复杂度会到达O(n^2*logn)
下面这组数据就会同时触发上面这两件事情
	10
	0 1 1
	2 2 1
	4 3 1
	6 4 1
	8 5 1
	10 6 1
	12 7 1
	14 8 1
	16 9 1
	18 10 1

**修复bug后的代码：** [bankqueue2.cpp](https://github.com/Evanqiao/CSTutorship/blob/master/Algorithm/bankQueue/bankqueue2.cpp)


### 性能更好的方法

思路：
维护两个数据结构，一个是`vector`，按先来后到的顺序存放每个`member`，一个是优先队列`priority_queue`，优先队列的优先原则是优先级大的在前，如果优先级相同，则达到时间小的在前，如果到达时间相同，则用时短的在前。设一个变量`currentTime`用来记录当前的时间。
首先，去访问队列中的成员，如果该成员已经服务过，直接删除，如果没有，则判断该成员是否已经到来，如果到来则访问，并记录他的编号，然后删除。如果队列头元素没有到来，就去`vector`数组中访问当前游标指向的成员，访问后游标加1，并置访问过的成员的标志为`true`。
要注意的是:
1. 两个数据结构里面存放的都是指针;
2. 相比方法一，`Member`中添加了一个成员变量`isVisited`，用来标记当前成员是否已经服务过了。
3. 循环的退出条件是队列为空或者数组已遍历一遍。

该算法的时间复杂度相比方法一提高了不少，为`O(nlogn)`。


```C++
#include <iostream>
#include <vector>
#include <queue>

using namespace std;

struct Member {
	// 依次对应每个成员的到达时间、优先级、消耗时间、编号、是否已经服务过
	int arrTime;
	int priority;
	int conTime;
	int id;
	bool isVisited;
	// 构造函数
	Member(int arrT, int p, int conT, int i, bool f) :
		arrTime(arrT), priority(p), conTime(conT), id(i), isVisited(f) {}
};

// 定义自己的比较函数
struct mycmp {
	bool operator () (const Member* m1, const Member* m2) {
		if(m1->priority != m2->priority) {
			return m1->priority < m2->priority;
		} else if(m1->arrTime != m2->arrTime) {
			return  m1->arrTime > m2->arrTime;
		} else {
			return  m1->conTime > m2->conTime;
		}
	}
};

vector<int> bankQueue(priority_queue<Member*, vector<Member*>, mycmp> &pq_m, Member *pmem[], int n) {

	vector<int> result;
	if(n == 0)
		return result;
	int currentTime = pmem[0]->arrTime;
	int i = 0;
	while(i < n && pq_m.size() > 0) {
		Member *m = pq_m.top();
		if(m->isVisited) {
			pq_m.pop();
			continue;
		}
		if(m->arrTime <= currentTime) {
			m->isVisited = true;
			result.push_back(m->id);
			currentTime += m->conTime;
			pq_m.pop();
		} else {
			if(pmem[i]->isVisited == false) {
				result.push_back(pmem[i]->id);
				pmem[i]->isVisited = true;
				currentTime += pmem[i]->conTime;
			}
			i++;
		}
	}
	return result;
}

/*
   5
   0 1 10
   2 1 10
   5 3 10
   7 2 5
   11 3 5
 */
int main() {

	int n;
	cin >> n;
	Member *pmember[n];
	int a, p, c;
	// 定义优先队列
	priority_queue<Member*, vector<Member*>, mycmp> pq_m;
	vector<int> result;

	for(int i = 0; i < n; i++) {
		cin >> a >> p >> c;
		pmember[i] = new Member(a, p, c, i + 1, false);
		pq_m.push(pmember[i]);
	}
	result = bankQueue(pq_m, pmember, n);

	for(int i : result)
		cout << i << " ";
	cout << endl;
}
```
## 堆排序的原理与特性

这部分我之前总结过。详见[典型排序算法 - 堆排序][1]
堆排序动态示例：
![堆排序升序排列][2]

[1]: http://blog.xiaojn.cn/2016/08/25/%E5%85%B8%E5%9E%8B%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E5%9B%9E%E9%A1%BE/#7-堆排序
[2]: https://upload.wikimedia.org/wikipedia/commons/4/4d/Heapsort-example.gif

## 堆排序实现
### 方法一
1. 在原数组上建立最小堆；
2. 交换数组的第一个元素和最后一个元素；
3. 维护最小堆；
4. 重复2-3步骤直到完成数组中所有元素的排序。  

此方法的时间复杂度为O(nlogn)，空间复杂度为O(1)。
**提示：**建堆的方法和过程参考上面给出的堆排序的链接。
```c++
#include <iostream>
#include <algorithm>
using std::cout;
using std::endl;
class HeapSort {
public:
    void heapSort(int A[], int n) {
        if(n < 2)
            return;
        buildMinHeap(A, n);
        for(int i = n - 1; i > 0; i--) {
            std::swap(A[0], A[i]);
            heap_size--;
            minHeapify(A, 0);
        }
    }
private:
    void minHeapify(int A[],int index) {
    	int left = 2 * index + 1;
    	int right = 2 * index + 2;
    	int lowest = 0;
    	if(left < heap_size && A[left] < A[index]) {
            lowest = left;
        } else {
            lowest = index;
        }
        if(right < heap_size && A[right] < A[lowest])
            lowest = right;
        if(lowest != index) {
        	std::swap(A[index], A[lowest]);
        	minHeapify(A, lowest);
        }
    }
    void buildMinHeap(int A[], int n) {
    	heap_size = n;
    	for(int i = (n - 2) / 2; i >= 0; i--) {
            minHeapify(A, i);
        }
    }
private:
    int heap_size = 0;
};

int main()
{
    int A[] = {9, 2, 7, 5, 3, 19, 8, 0};
    int n = sizeof(A) / sizeof(int);
    HeapSort h;
    h.heapSort(A, n);
    for(int e : A) {
        cout << e << "  ";
    }
    cout << endl;
    
    return 0;
}
```

### 方法二
1. 在原数组上建立最大堆；
2. 把数组的第一个元素放入新的数组中，把数组最后一个元素赋值给第一个元素；
3. 维护最大堆；
4. 重复2-3步骤直到完成数组中所有元素的排序。  

此方法的时间复杂度为O(nlogn)，空间复杂度为O(n)。

```C++
#include <iostream>
#include <algorithm>
#include <vector>
using std::cout;
using std::endl;
using std::vector;

class HeapSort {
public:
    void heapSort(int A[], vector<int> &B, int n) {
        if(n < 2)
            return;
        buildMaxHeap(A, n);
        for(int i = n - 1; i > 0; i--) {
            B.push_back(A[0]);
            A[0] = A[i];
            heap_size--;
            maxHeapify(A, 0);
        }
    }
private:
    void maxHeapify(int A[],int index) {
    	int left = 2 * index + 1;
    	int right = 2 * index + 2;
    	int largest = 0;
    	if(left < heap_size && A[left] > A[index]) {
            largest = left;
        } else {
            largest = index;
        }
        if(right < heap_size && A[right] > A[largest])
            largest = right;
        if(largest != index) {
        	std::swap(A[index], A[largest]);
        	maxHeapify(A, largest);
        }
    }
    void buildMaxHeap(int A[], int n) {
    	heap_size = n;
    	for(int i = (n - 2) / 2; i >= 0; i--) {
            maxHeapify(A, i);
        }
    }
private:
    int heap_size = 0;
};
int main()
{
    int A[] = {7, 9, 2, 5, 3, 19, 8, 0};
    int n = sizeof(A) / sizeof(int);
    vector<int> B;
    HeapSort h;
    h.heapSort(A, B, n);
    for(int e : B) {
        cout << e << "  ";
    }
    cout << endl;
    return 0;
}
```
### 方法三
按照课堂上老师的代码进行修改，先建立一个最大堆，然后依次打印出堆顶元素。

**提示：**这里的代码不一定是最好的，也不一定非要建立一个最大堆。

```C++
#include <iostream>
#include <vector>
using namespace std;

class MaxHeap{
private:  // private表示只有在class内部可访问
    vector<int> data;   // 数据，因为是满二叉树，用数组存就可以
    int capacity,size;  // 最大容量，实际数量
    
    void swap(int &a,int &b){   // 交换整数辅助函数
        a=a+b;
        b=a-b;
        a=a-b;
    }
    
    // 从节点i开始恢复堆的特性
    void recover(int i){
        if (i>1 && data[i/2]<data[i]){
            swap(data[i/2],data[i]);
            recover(i/2);
        }
        if (i*2+1<=size && data[i*2+1]>data[i] && data[i*2+1]>=data[i*2]){ // 右子树最大
            swap(data[i*2+1],data[i]);
            recover(i*2+1);
        }
        if (i*2<=size && data[i*2]>data[i] && (i*2+1>size || data[i*2]>data[i*2+1])){ // 左子树最大
            swap(data[i*2],data[i]);
            recover(i*2);
        }
    }
    
public:
    
    MaxHeap(const int& n){ // 初始化
        data.empty();
        capacity=1<<n;
        size=0;
        data.reserve(capacity);
    }
    
    int removeRoot() {  // 删除根节点并返回其值
        int root= data[1];
        data[1]=data[size--];
        recover(1);
        return root;
    }
    
    void insert(int value){ // 新增一个元素
        data[++size]=value;
        recover(size);
    }
    
    void print(){   // 用友好的方式输出堆的内容
        for (int i=1;i<=size;i++){
            int left=i*2<=size?data[i*2]:-1;
            int right=i*2+1<=size?data[i*2+1]:-1;
            cout<<data[i]<<"("<<left<<","<<right<<")"<<endl;
        }
        cout<<endl;
    }
};

void heapSort(int data[],int n){
    int level=0;
    for(int i=n; i>0; i>>=1,level++); // 计算能容纳n个节点的堆的高度
    MaxHeap heap(level);    // 初始化堆
    for (int i=0;i<n;i++){  // 入堆
        heap.insert(data[i]);
    }
    for (int i=1; i<=n; i++){
        int max = heap.removeRoot(); // 取出堆顶元素，即最大的元素
        cout << max << " ";
    }
}

int main(){
    /*9
     4 3 5 7 11 90 7 29 3*/
    int n;
    scanf("%d", &n);
    int data[n];
    for (int i=0;i<n;i++){
        scanf("%d", &data[i]);
    }

    heapSort(data, n);
    cout << endl;
}
```

