```
#include <bits/stdc++.h>
#define PII pair<int,int>
using namespace std;

const int N = 1e6+3000;
const int inf = 0x7f7f7f7f;

int head[N],e[N<<1],ne[N<<1],c[N<<1],tot;

void add(int u,int v,int cost)
{
    ++tot;
    e[tot] = v;
    ne[tot] = head[u];
    c[tot] = cost;
    head[u] = tot;
}

int dis[N];
bool vis[N];

priority_queue<PII > que;


int n,m,k;

int main()
{
    cin >> n >> m >> k;

    for(int i = 0;i<m;i++)
    {
        int u,v,cost;
        cin >> u >> v >> cost;
        add(u,v,cost),add(v,u,cost);
        for(int j = 1;j<=k;j++)
        {
            add(u+(j-1)*n,v+j*n,0);
            add(v+(j-1)*n,u+j*n,0);
            add(u+j*n,v+j*n,cost),add(v+j*n,u+j*n,cost);
        }
    }

    for(int i = 1;i<=k;i++) add(n+(i-1)*n,n+i*n,0);

    for(int i = 2;i<=n*(k+1);i++) dis[i] = inf;

    que.push({0,1});

    while(!que.empty())
    {
        int u = que.top().second;
        que.pop();
        if(vis[u]) continue;

        vis[u] = 1;

        for(int i = head[u];i;i = ne[i])
        {
            int v = e[i];
            if(!vis[v]&&dis[v] > max(dis[u],c[i]))
            {
                dis[v] = max(dis[u],c[i]);
                que.push({-dis[v],v});
            }
        }
    }

    if(dis[n+n*k] == inf) cout << -1 << endl;

    else cout << dis[n+n*k] << endl;
```
