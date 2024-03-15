# AbstractDFS

1.从n个数中选k个数使和为sum，有几种选法

```c++
int n,k,sum;
int ans;
int arr[1000];
void dfs(int i,int cnt,int s){//i:判断第i个，cnt：选了第cnt个，s： 当前和
	if(i==n){
		if(s==sum&&cnt==k)
			ans++;
		return;
	}
	//这个数选
	dfs(i+1,cnt+1,s+arr[i]);
	//这个数不选 
	dfs(i+1,cnt,s);
}
int main(){
	scanf("%d%d%d",&n,&k,&sum);
	for(int i=0;i<n;i++){
		scanf("%d",&arr[i]);
	}
	dfs(0,0,0);
	return 0;
}
```

![image-20240313091817183](AbstractDFS.assets/image-20240313091817183.png)

2. 八皇后问题

![image-20240313103241377](AbstractDFS.assets/image-20240313103241377.png)

![image-20240313103304978](AbstractDFS.assets/image-20240313103304978.png)

![image-20240313103323331](AbstractDFS.assets/image-20240313103323331.png)

![image-20240313103336012](AbstractDFS.assets/image-20240313103336012.png)

![image-20240313103345149](AbstractDFS.assets/image-20240313103345149.png)

```c++
int n;//n*n的棋盘，n皇后 
int ans;
bool col[100],x1[200],x2[200];//n最多取100 
bool check(int r,int i){
	return !col[i] && !x1[r+i] && !x2[r-i+n];
}
void dfs(int r){
	if(r==n){
		ans++;
		return;
	}
	for(int i=0;i<n;i++){//第i列 
		if(check(r,i)){//如果能放，那么就放
			col[i]=x1[r+i]=x2[r-i+n]=true;
			dfs(r+1); 
			col[i]=x1[r+i]=x2[r-i+n]=false;//如果要遍历所有情况，一定要有 
		}
	}
}
int main(){
	scanf("%d",&n);
	dfs(0);
	cout<<ans<<endl;
	return 0;
} 
```

<img src="AbstractDFS.assets/image-20240313183702429.png" alt="image-20240313183702429" style="zoom:67%;" />

```c++
#include<iostream>
using namespace std;
int n;
int p[25];
int visited[25];
int sum;
bool f;
void dfs(int cnt,int k,int sum1){//当前已经拼好了cnt条边，现在在看第k条小木棍，当前和为sum1
	if(f){//三条边都找到了 
		return;
	}
	if(cnt==3){//三条边都找到了 
		f=1;
		return;
	}
	if(sum1*3==sum){
		dfs(cnt+1,0,0);//这条边拼好了去拼下一条 
	}
	for(int i=k;i<n;i++){
		visited[i]=true;
		dfs(x,s+p[i],i+1);
		visited[i]=false;
	}

}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		scanf("%d",&p[i]);
		sum+=p[i];
	}
	if(sum%3!=0){
		printf("no\n");		
	}else{
		dfs();
	}	
	if(f){
		printf("yes\n");
	}else{
		printf("no\n");
	}	
	return 0;
} 
```



<img src="AbstractDFS.assets/image-20240314201620491.png" alt="image-20240314201620491" style="zoom:80%;" />

```c++
int k[5],p[5];
int n,m,cnt;
long long pow2(long long x,int p){
	long long res=1;
	for(int i=0;i<p;i++){
		res*=x;
	}
	return res;
}
void dfs(int x,int  sum){
	if(x==n){
		if(sum==0){
			cnt++;			
		}		
		return ;
	}	
	for(int i=1;i<=m;i++){
		//sum+=k[x]*pow2(i,p[x]);错误的，sum会一直加加 
		dfs(x+1,sum+k[x]*pow2(i,p[x]));
	}	
}
int main(){
	scanf("%d",&n);
	scanf("%d",&m);
	for(int i=0;i<n;i++){
		scanf("%d",&k[i]);
		scanf("%d",&p[i]);
	}
	dfs(0,0);
	printf("%d",cnt);	
	return 0;
}
```

```c++
* 2 6 * * * * * *
* * * 5 * 2 * * 4
* * * 1 * * * * 7
* 3 * * 2 * 1 8 *
* * * 3 * 9 * * *
* 5 4 * 1 * * 7 *
5 * * * * 1 * * *
6 * * * * 7 * * *
* * * * * * 7 5 *
```

