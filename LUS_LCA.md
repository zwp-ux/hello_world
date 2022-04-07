```
vector<int> son[N];

int head[N],e[N<<1],ne[N<<1],tp[N<<1],tot;

void add(int u,int v,int type)
{
    ++tot;
    e[tot] = v;
    ne[tot] = head[u];
    tp[tot] = type;
    head[u] = tot;
}

int fa[N],ans[N];

int find(int u)
{
    if(fa[u] == u) return u;
    return fa[u] = find(fa[u]);
}

void dfs(int u,int f)
{
    fa[u] = u;

    for(int i = 0;i<son[u].size();i++)
    {
        int v = son[u][i];
        if(v != f)
        {
            dfs(v,u);
        }
    }

    for(int i = head[u];i;i = ne[i])
    {
        int v = e[i];
        if(fa[v])
        {
            ans[tp[i]] = find(v);
        }
    }

    fa[u] = f;

}
```
