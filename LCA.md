```
vector<int> son[N];
int f[N][21],dp[N];

void dfs(int u,int fa,int dep)
{
    f[u][0] = fa;dp[u] = dep;
    for(int i = 1;i<21;i++)
    {
        f[u][i] = f[f[u][i-1]][i-1];
    }
    for(int i = 0;i<son[u].size();i++)
    {
        int v = son[u][i];
        if(v != fa)
        {
            dfs(v,u,dep+1);
        }
    }

}


int lca(int a,int b)
{
    if(dp[a] < dp[b]) swap(a,b);

    for(int i = 20;i>=0;i--)
    {
        if(dp[f[a][i]]>= dp[b]) a = f[a][i];
    }

    if(a == b) return a;

    for(int i = 20;i>=0;i--)
    {
        if(f[a][i] != f[b][i]) a = f[a][i],b = f[b][i];
    }

    return f[a][0];
}
```
