# 位运算小trick

## 返回一个数的最小二进制位
可用于树状数组
```c++
ll lowbit(ll x){
	return x&(-x);
}
```
## 返回一个数的二进制位的个数
`__builtin_popcount()`是GCC内置的计算一个数的二进制位中1的个数的函数
可以用于优化某些状压dp
时间复杂度基本为$O(1)$
在`<stdio.h>`中
```c++
int __builtin_popcount(unsigned int u){
    u = (u & 0x55555555) + ((u >> 1) & 0x55555555);
    u = (u & 0x33333333) + ((u >> 2) & 0x33333333);
    u = (u & 0x0F0F0F0F) + ((u >> 4) & 0x0F0F0F0F);
    u = (u & 0x00FF00FF) + ((u >> 8) & 0x00FF00FF);
    u = (u & 0x0000FFFF) + ((u >> 16) & 0x0000FFFF);
	// 源码方法(?)
    return u;
}
int __builtin_popcountl(unsigned long x){
//?
}
int __builtin_popcountll(unsigned long long x){
//?
}
```

## 返回一个数的二进制位前导0个数
`__builtin_clz()`返回一个数的二进制位前导0个数，当x==0时结果未定义
时间复杂度基本为$O(1)$，据说似乎比直接调用数组里的数还要快
可以添加`l`或`ll`来使参数变为`long`或`long long`
可以用于计算以二为底的对数


