---
title: 数据结构-树状数组
published: 2024-10-10
description: "树状数组常用于区间求和，能被线段树完全上位替代"
tags: ["ds","algrithm"]
category: ds
draft: false
---

```cpp
int lowbit(int x)
{
  return x & (-x);//表示求数组下标二进制的非0最低位所表示的值
}

//查找1~x的和
int find_sum(int x)
{
  int ans = 0;
  while(x)
  {
    ans += c[x];//从右往左累加求和
    x -= lowbit(x);
  }
  return x;
}

//单点修改
void gexi(int x,int v)
{
  a[x] += v;
  while(x <= n)
  {
    c[x] += v;
    x += lowbit(x);//由叶子节点向上更新树状数组C，从左往右更新
  }
}
```