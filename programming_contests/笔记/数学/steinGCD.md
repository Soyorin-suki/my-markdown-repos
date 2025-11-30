# 求GCD的Stein算法
>可以用于__int128的gcd计算

当素数很大（大于64bit）时cpu在除法和取余上会花费大量的时间，而RSA加密算法中存在大量的500+bit的素数，在这样的情况下需要一种不使用除法和取余的GCD算法
而**Stein算法**就很好的解决了这个情况，该算法中只包含+-min和位运算
它的步骤如下
- 如果这a和b都是偶数，那就将其同时右移一位，并在答案上左移一位
- 如果a是偶数而b不是偶数，那就将a向右移一位，因为此时2肯定不是答案
- 如果两者都不是偶数，则$a=|a-b|,b=min(a,b)$再重复上一步

```c++
ll SteinGCD(ll a,ll b){
	ll ans=1,k=0;
	while(((a&1LL)==0)&&((b&1LL)==0)){
		a>>=1;
		b>>=1;
		k++;
	}
	while((a&1LL)==0)a>>=1;
	while((b&1LL)==0)b>>=1;
	if(a<b)swap(a,b);
	while((a=a-b)!=0){
		while((a&1LL)==0)a>>=1;
		if(a<b)swap(a,b);
	}
	return b<<k;
}
```
