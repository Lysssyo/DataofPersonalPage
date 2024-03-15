# DFS

![image-20240306083836042](DFS.assets/image-20240306083836042.png)

![image-20240306083900668](DFS.assets/image-20240306083900668.png)

```cpp
int n,m;
char maze[110][110];
bool visited[110][110];
bool in(int x,int y){
	if(0<=x&&x<n&&0<=y&&y<m){
		return true;
	}
	return false;
}
bool dfs(int x,int y){
	if(maze[x][y] == 'T'){
		cout<<"Here is T"<<"  "<<x<<"  "<<y<<endl;
		return true;
	}
	visited[x][y] = true;//记录已经访问过了 
	maze[x][y] = 'm';//标记走过的路径 
	int tx = x-1,ty=y;//向上走
	if(in(tx,ty)&&visited[tx][ty]==false&&maze[tx][ty]!='*'){//能走且没走过 
		if(dfs(tx,ty)){
			return true;
		}
	}
	tx = x;
	ty=y-1;//向左走
	if(in(tx,ty)&&visited[tx][ty]==false&&maze[tx][ty]!='*'){//能走且没走过 
		if(dfs(tx,ty)){
			return true;
		}
	}
	tx = x+1;
	ty=y;//向下走
	if(in(tx,ty)&&visited[tx][ty]==false&&maze[tx][ty]!='*'){//能走且没走过 
		if(dfs(tx,ty)){
			return true;
		}
	}
	tx = x;
	ty=y+1;//向右走
	if(in(tx,ty)&&visited[tx][ty]==false&&maze[tx][ty]!='*'){//能走且没走过 
		if(dfs(tx,ty)){
			return true;
		}
	}
	//如果都没有
	maze[x][y]='.';//是为了找到来时路 
    //如果需要找多条路的话，需要取消访问标记：
    visited[x][y]=0;//如果只有一条路也不会因为这个而陷入循环
	return false;
}
int main(){
	//输入地图
	cin>>n>>m;//n行m列 
	 for(int i=0;i<n;i++){
	 	cin>>maze[i];
	 }
	 int x,y;
	 for(int i=0;i<n;i++){
	 	for(int j=0;j<m;j++){
	 		if(maze[i][j]=='S'){
	 			x=i;
	 			y=j;	 			
			 }
		 }
	 }//找到起点
	 if(dfs(x,y)){
	 	for(int i=0;i<n;i++){
	 		cout<<maze[i]<<endl;
		 }
	 } 	
	return 0;
}
```

改进：

![image-20240306093904470](DFS.assets/image-20240306093904470.png)

![image-20240306093724202](DFS.assets/image-20240306093724202.png)



![image-20240306111525773](DFS.assets/image-20240306111525773.png)

![image-20240306111534120](DFS.assets/image-20240306111534120.png)

```cpp
#include<iostream>
using namespace std;
char mp[105][105];
bool visited[105][105];
int dir[4][2]={{-1,0},{0,-1},{1,0},{0,1}};
int count;
int n,m;
bool in(int x,int y){
	return 0<=x && x < n && 0<= y && y<m;
}

void dfs(int x,int y){	
	visited[x][y]=true;	
	for(int i=0;i<4;i++){
		int tx=x+dir[i][0];
		int ty=y+dir[i][1];
		if(in(tx,ty)&&mp[tx][ty]=='#'&&!visited[tx][ty]){
			dfs(tx,ty);
		}
	}
}

int main(){
	scanf("%d%d",&n,&m);
	for(int i=0;i<n;i++){
		scanf("%s",mp[i]);
	}//数据输入
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if(mp[i][j]=='#'&& !visited[i][j]){
				count++;
				dfs(i,j);
			}
		}
	} 
	cout<<"总数："<<count<<endl;
	return 0;
}
```

![image-20240307085718525](DFS.assets/image-20240307085718525.png)

```c++
#include<iostream>
using namespace std;
int n,m;
char mp[15][15];
bool visited[15][15];
int dir[4][2]={{-1,0},{0,-1},{1,0},{0,1}};
int count;
bool in(int x,int y){
	return 0<=x && x<n && 0<= y &&y<m;
}
bool dfs(int x,int y){
	if(mp[x][y]=='e'){
		count++;
		return true;
	}
	visited[x][y]=true;
	for(int i=0;i<4;i++){
		int tx=x+dir[i][0];
		int ty=y+dir[i][1];
		if(!visited[tx][ty]&&in(tx,ty)&&mp[tx][ty]!='#'){
			dfs(tx,ty);
		}
	}
	visited[x][y]=false;
	return false;
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=0;i<n;i++){
		scanf("%s",mp[i]);
	}
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if(mp[i][j]=='s'){
				dfs(i,j);
			}
		}
	}
	cout<<"总数是： "<<count<<endl;	
	return 0;
}
```



<img src="DFS.assets/image-20240307092127841.png" alt="image-20240307092127841" style="zoom:67%;" />

<img src="DFS.assets/image-20240307092152485.png" alt="image-20240307092152485" style="zoom:67%;" />

<img src="DFS.assets/image-20240307092216772.png" alt="image-20240307092216772" style="zoom:67%;" />

```c++
#include<iostream>
using namespace std;
int n,m,ans,cnt;
char mp[30][30];
int dir[4][2]={{-1,0},{0,-1},{1,0},{0,1}};
bool visited[30][30];
bool in(int x,int y){
	return 0<=x && x <n && 0<=y && y<m;
}
void dfs(int x,int y){
	visited[x][y]=true;
	cnt++;
	for(int i=0;i<4;i++){
		int tx=x+dir[i][0];
		int ty=y+dir[i][1];
		if(!visited[tx][ty]&&in(tx,ty)&&mp[tx][ty]=='#'){
			dfs(tx,ty);
		}
	}
}
int main(){
	scanf("%d%d",&n,&m);
	for(int i=0;i<n;i++){
		scanf("%s",mp[i]);
	}//以上是输入
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if(mp[i][j]=='#'&& !visited[i][j]){
				cnt=0;
				dfs(i,j);
				if(cnt>ans){
					ans=cnt;
				}	
			}
		}
	} 
	cout<<ans<<endl;
	return 0;
}
```

​		这题与峡谷草丛类似，与一般的dfs有一点点不一样。进入dfs后，不用`if(mp[x][y]=='#')`，因为进去的地方实际上就是要找的地方了，只需要看看周围是否还有要找的元素，所以进入下一层循环的条件是`if(!visited[tx][ty]&&in(tx,ty)&&mp[tx][ty]=='#')`





<img src="DFS.assets/image-20240307210358349.png" alt="image-20240307210358349" style="zoom:67%;" />

<img src="DFS.assets/image-20240307210502634.png" alt="image-20240307210502634" style="zoom:67%;" />

<img src="DFS.assets/image-20240307210515934.png" alt="image-20240307210515934" style="zoom:67%;" />

```c++
#include<iostream>
using namespace std;
int n,m,x,y,cnt;
char mp[100][100];
int dis[8][2]={{-1,-2},{-2,-1},{-2,1},{-1,2},{1,2},{2,1},{2,-1},{1,-2}};
bool in(int x,int y){
	return 0<=x && x<n && 0<=y && y<m;
}
void dfs(int x,int y,int step){
	if(step>=3){//一定要特别特别注意走的次数！！！！！
		return;
	}
	for(int i=0;i<8;i++){		
		int tx=x+dis[i][0];
		int ty=y+dis[i][1];
		if(in(tx,ty)){
			mp[tx][ty]='#';			
			dfs(tx,ty,step+1);
			//不能写成这样的形式：
			//step++;
			//dfs(tx,ty,step);
			//因为如果这样写，step会越变越大，变得很大，因为当进入下一个dfs时，出来以后，会继承原来的step 
		}
	}

}
int main(){
	scanf("%d%d",&n,&m);//输入棋盘大小 
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			mp[i][j]='.';
		}
	}
	scanf("%d%d",&x,&y); 
	dfs(x-1,y-1,0);//调整坐标 
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			cout<<mp[i][j]<<" ";
		}
		cout<<endl;
	}
	return 0;
}
```



