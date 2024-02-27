# 常用STL

## vecotr

![image-20240227183619660](STL.assets/image-20240227183619660.png)

```c++
#include<iostream>
#include<vector>
using namespace std;
int main() {
	vector<int> arr;
	arr.push_back(1);
	arr.push_back(2);
	arr.push_back(3);
	for (int i = 0; i < arr.size(); i++)
		cout << arr[i];
	cout << endl;
	arr.pop_back();//删除尾部元素
	arr.clear();//清空vector
}
```

![image-20240227183828969](STL.assets/image-20240227183828969.png)



![image-20240227184135777](STL.assets/image-20240227184135777.png)

![image-20240227184158362](STL.assets/image-20240227184158362.png)

![image-20240227184522476](STL.assets/image-20240227184522476.png)

> `vector<int> vec(m,0) `表示初始化vec为m个0



## 集合

```c++
#include<set>
using namespace std;
```

![image-20240227185341067](STL.assets/image-20240227185341067.png)

![image-20240227185358051](STL.assets/image-20240227185358051.png)

**遍历元素**

![image-20240227185638761](STL.assets/image-20240227185638761.png)

![image-20240227185705011](STL.assets/image-20240227185705011.png)



例：![image-20240227190013770](STL.assets/image-20240227190013770.png)



![image-20240227190139782](STL.assets/image-20240227190139782.png)

> vector则不会清空内存

**与结构体配合使用要重载运算符**

```c++
struct Node
{
	int x, y;
	bool operator<(const Node& rhs)const {
		if (x == rhs.x) {
			return y < rhs.y;
		}
		else
		{
			return x < rhs.x;
		}
	}
};
```

![image-20240227191129605](STL.assets/image-20240227191129605.png)

## 映射

![image-20240227191810008](STL.assets/image-20240227191810008.png)

```c++
#include<map>
using namespace std;
```

![image-20240227192445108](STL.assets/image-20240227192445108.png)

![image-20240227192505121](STL.assets/image-20240227192505121.png)



![image-20240227194634841](STL.assets/image-20240227194634841.png)

![image-20240227195044745](STL.assets/image-20240227195044745.png)

![image-20240227195215525](STL.assets/image-20240227195215525.png)