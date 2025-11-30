# 01BFS
01BFS是一种特殊的最短路算法，当边权只有0或1时可以使用，时间复杂度为$O(V+E)$
由堆优化dijkstra可得我们每次更新最短的边，而边权只有0或1，因此我们可以分类讨论：
- 当w=0时，将边插入到最前方
- 当w=1时，将边插入到后方

因此我们使用一个双端队列即可维护

e.g.
2023ICPC澳门J
> https://codeforces.com/gym/104891/problem/J
> 我们可以将建一排辅助点，当我们进行传送时可以加点ADD(i,n+(i+a[i]-1)%n+1,1)，即在第i个点和它传送到的点+n的点（即辅助点）建权值为1的边
> 然后再建边使第i+n个点可以以代价0回到第i个点
> 同时把钟表+1可以视为在辅助点i+n和i+n+1间建一条边权为1的边
> 之后跑一遍01BFS即可

code:
```c++
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll N=1e6+10;
const ll INF=1e11+10;
ll n,x;
ll a[N];
struct EDGE{
	ll to,nxt,w;
}e[N<<2];
ll head[N<<1],cnt;
ll vis[N<<1],dist[N<<1];
void add(ll u,ll v,ll w){
	e[++cnt].to=v;
	e[cnt].w=w;
	e[cnt].nxt=head[u];
	head[u]=cnt;
}
void solve(void){
	cin>>n>>x;
	for(ll i=1;i<=n;++i)cin>>a[i];
	x++;
	for(ll i=1;i<=n;++i){
		add(i,n+(i+a[i]-1+n)%n+1,1);
		add(i+n,i,0);
		add(n+i-1,n+i,1);
		dist[i]=dist[i+n]=INF;
	}
	deque<ll>q;
	q.push_back(1);
	dist[1]=0;
	while(!q.empty()){
		ll u=q.front();
		q.pop_front();
		for(ll i=head[u];i;i=e[i].nxt){
			ll v=e[i].to,w=e[i].w;
			if(dist[v]>dist[u]+w){
				dist[v]=dist[u]+w;
				if(w==0)q.push_front(v);
				else q.push_back(v);
			}
		}
	}
	cout<<dist[x]<<endl;
}
int main(void){
	ios::sync_with_stdio(0);
	cin.tie(0);cout.tie(0);
	ll times=1;
	// cin>>times;
	while(times--)solve();
	return 0;
}
```