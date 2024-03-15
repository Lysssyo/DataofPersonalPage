# BFS

![image-20240315004812593](BFS.assets/image-20240315004812593.png)

例一：迷宫问题

```c++
int n,m;
int dir[4][2]={{0,1},{0,-1},{1,0},{-1,0}};
char mp[100][100];
bool visited[100][100];
bool f;
bool in(int x,int y){
	return 0<=x && x < n && 0<= y && y<m;
}
struct node{
	int x,y,d;
	node(int xx,int yy,int dd){
		x=xx;
		y=yy;
		d=dd;
	}
};
int bfs(int sx,int sy){
	queue<node> q;
	q.push(node(sx,sy,0));
	while(!q.empty()){
		node cur=q.front();
		q.pop();
		for(int i=0;i<4;i++){
			int tx=cur.x+dir[i][0];
			int ty=cur.y+dir[i][1];
			if(in(tx,ty)&&mp[tx][ty]!='*'&&!visited[tx][ty]){
				if(mp[tx][ty]=='T'){
					return cur.d+1;
				}
				else{
					visited[tx][ty]=true;
					q.push(node(tx,ty,cur.d+1));
				}
			}
		}
	}
	return -1;
}
int main(){
	cin>>n>>m;
	for(int i=0;i<n;i++){
		cin>>mp[i];
	}
	int x,y;
	for(int i=0;i<n;i++){
		for(int j=0;j<m;j++){
			if(mp[i][j]=='S'){
				bfs(i,j);
			}
		}
	}
	return 0;
}
```

