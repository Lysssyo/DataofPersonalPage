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

![image-20240229085710856](STL.assets/image-20240229085710856.png)



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

**cout()函数**

​		count函数可以用来统计集合中某个特定元素的个数。在C++中，set是一种有序的容器，其中的元素都是唯一的，因此count函数返回的结果要么是0（表示集合中没有该元素），要么是1（表示集合中有该元素）。

示例代码如下：

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> mySet = {1, 2, 3, 4, 5};

    int element = 3;
    int count = mySet.count(element);

    if (count == 1) {
        cout << "Element " << element << " is in the set" << endl;
    } else {
        cout << "Element " << element << " is not in the set" << endl;
    }
    return 0;
}
```

在上面的示例中，我们创建了一个包含1到5的set，并使用count函数来检查元素3是否在set中。最后输出的结果应该是"Element 3 is in the set"。



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



![image-20240228090350110](STL.assets/image-20240228090350110.png)

![image-20240228090358341](STL.assets/image-20240228090358341.png)

> 注意：输出map的key是`it -> first`而不是`(*it) -> first`，输出value同理。

## 运用

![image-20240228091143903](STL.assets/image-20240228091143903.png)

![image-20240228091413990](STL.assets/image-20240228091413990.png)

![image-20240228093341806](STL.assets/image-20240228093341806.png)

![image-20240228093316681](STL.assets/image-20240228093316681.png)

```c++
#include<iostream>
#include<vector>
using namespace std;
vector<int> arr[10005];
int main(){
	int n,m,x,y;
	cin>>n>>m;
	for(int i=0;i<m;i++){
		cin>>x>>y;
		arr[x].push_back(y);		
	}
	for(int i=1;i<n;i++){
		for(int j=0;j<arr[n].size();j++){
			if(j!=arr[i].size()-1){
				cout<<arr[i][j]<<" ";
			}
			else{
				cout<<arr[i][j];
			}
		}
		cout<<endl;
	}
	return 0;
}
```



![image-20240228104717005](STL.assets/image-20240228104717005.png)

```c++
//省略头
map<string,int> books;
int main(){
	int n;
	cin>>n;
	string name;
	for(int i=0;i<n;i++){
		cin>>name;
//		if(books.count(name)){
//			books[name]++;
//		}
//		else books.insert(make_pair(name,1));
//或者这样：
		books[name]++; 
	}
    cout<<books.size()<<endl;
	for(map<string,int>::iterator it=books.begin();it!=books.end();it++){
		cout<<it->first<<" "<<it->second<<endl;
	}
	return 0;
}
```

//运行超时，因为数据量比较大，所以改用c风格的输入输出

```c
#include<iostream>
#include<cstdio>
#include<map>
using namespace std;
map<string, int> books;
char name[105];
int main() {
	int n;
	scanf("%d", &n);
	for (int i = 0; i < n; i++) {
		scanf("%s", name);
		books[name]++;//自动把char字符数组转为string
	}
	printf("%d\n", books.size());
	for (map<string, int>::iterator it = books.begin(); it != books.end(); it++) {
		printf("%s %d\n", (it->first).c_str(), it->second);//c_str()是为了将string转为c风格的字符串
	}
	return 0;
}
```



<img src="STL.assets/image-20240229090129451.png" alt="image-20240229090129451" style="zoom:80%;" />

<img src="STL.assets/image-20240229090157996.png" alt="image-20240229090157996" style="zoom:80%;" />

```c++
#include<iostream>
#include<vector>
using namespace std;
vector<int> arr[10005];
int main(){
	int n,m,a,b;
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;i++){
		arr[i].push_back(i);
	}
	for(int i=0;i<m;i++){
		scanf("%d%d",&a,&b);
		if(a==b)continue;
		else{
			for(int j=0;j<arr[b].size();j++){
				arr[a].push_back(arr[b][j]);
			}
			vector<int>().swap(arr[b]);//清内存 
		}
	}
	for(int i=0;i<n;i++){
		for(int j=0;j<arr[i].size();j++){
			if(j==arr[i].size()-1)
				//cout<<arr[i][j];
				printf("%d",arr[i][j]);
			//else cout<<arr[i][j]<<" ";
			else printf("%d ",arr[i][j]);
		}
		cout<<endl;
	}	
	return 0;
}
```

<img src="STL.assets/image-20240229092003792.png" alt="image-20240229092003792" style="zoom:80%;" />

```c++
#include<iostream>
#include<set>
using namespace std;
set<int> setA;
int main(){
	int n,m,temp,cnt;
	scanf("%d%d",&n,&m);
	for(int i=0;i<n+m;i++){
		scanf("%d",&temp);
		setA.insert(temp);
	}
	for(set<int>::iterator it=setA.begin();it!=setA.end();it++){
		if(cnt==setA.size()-1)
			printf("%d\n",(*it));
		else{
			printf("%d ",(*it));
			cnt++;
		} 
	}
	return 0;
}
```





<img src="STL.assets/image-20240229193833846.png" alt="image-20240229193833846" style="zoom:80%;" />

```c++
#include<iostream>
#include<map>
#include<set>
using namespace std;
map<int,int> m;
int n,num;
int main(){
	scanf("%d",&n);
	//输入 
	for(int i=0;i<n;i++){
		scanf("%d",&num);
		if(!m.count(num))
			m.insert(make_pair(num,1));
		else m[num]++;
	}
	//输出
	int max1=0;
	int max2;
	 for(map<int,int>::iterator it=m.begin();it!=m.end();it++){
	 	if(it->second >= max1){
	 		max1=it->second;
	 		max2=it->first;
		 }
	 }
	 cout<<max2<<" "<<max1;
	return 0;
}
```



<img src="STL.assets/image-20240229194324958.png" alt="image-20240229194324958" style="zoom:80%;" />

<img src="STL.assets/image-20240229194344039.png" alt="image-20240229194344039" style="zoom:80%;" />
