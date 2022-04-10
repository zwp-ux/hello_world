```

const int N=210,M=5e3+10;
const ll INF=0x7f7f7f7f;
struct Edge
{
	int to,nxt; ll val;
}e[M<<1];
int n,m,S,T,tot=1,head[N],pre[N],mp[N][N];
ll mxflow,dis[N];
bool vis[N];

void Add( int u,int v,int ll w )
{
	e[++tot].to=v; e[tot].nxt=head[u]; head[u]=tot; e[tot].val=w;
	e[++tot].to=u; e[tot].nxt=head[v]; head[v]=tot; e[tot].val=0;	//反向边
}

bool BFS()
{
	for ( int i=1; i<=n; i++ )
		vis[i]=0;
	queue<int> q; q.push( S ); vis[S]=1; dis[S]=INF;
	while ( !q.empty() )
	{
		int u=q.front(); q.pop();
		for ( int i=head[u]; i; i=e[i].nxt )
		{
			if ( e[i].val==0 ) continue;	//流没了
			int v=e[i].to;
			if ( vis[v] ) continue; 
			dis[v]=min( dis[u],e[i].val );	//取路径上的最小剩余容量
			pre[v]=i; 		//记录从哪条边过来
			q.push( v ); vis[v]=1;
			if ( v==T ) return 1;	//走到汇点了
		}
	}
	return 0;
}

void Update()		//每次找到一条增广路，并更新路径上的值
{
	int u=T;
	while ( u^S )
	{
		int v=pre[u];
		e[v].val-=dis[T]; e[v^1].val+=dis[T];		
		//这条增广路的流量，用来更新路径上所有边和反向边
		u=e[v^1].to;		//本来记录的是过来的边，回去要反向
	}
	mxflow+=dis[T];		//总流量增加
}

int main()
{
	n=read(); m=read(); S=read(); T=read();
	for ( int i=1; i<=m; i++ )
	{
		int u,v; ll w; u=read(); v=read(); w=read();
		if ( mp[u][v]==0 ) Add( u,v,w ),mp[u][v]=tot;
		else e[mp[u][v]-1].val+=w;
		//重边容量合并
	}

	while ( BFS() ) Update();

	printf( "%lld\n",mxflow ); 

	return 0;
}

```
