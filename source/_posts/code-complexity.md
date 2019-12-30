---
title: 复习度分析
date: 2019-10-20 10:09:54
copyright: false
rating: false
related_posts: false
tags:
categories:
---

代码写出来就是交由机器去执行的，不同的人，可能会写出不同的代码。评价一个人的代码，不仅要从代码的可读性。还得从代码的扩展性，封装性等去看待。当然，还有一点，那就是代码执行的效率。

那么，如何去评估一段代码的执行效率呢？今天的这篇文章，将会是一个开端。

<!-- more -->

## 事后统计法

{% note info %}
通过统计、监控，就能得到算法执行的时间和占用的内存大小。
{% endnote%}

缺点：

* 测试结果非常依赖测试环境
* 测试结果受数据规模的影响很大

## 大 O 复杂度表示法

算法的执行效率，粗略地讲，就是算法代码执行的时间。

```java 大 O 表示法
T(n)=O(f(n))

``` 

T(n) 它表示代码执行的时间； n 表示数据规模的大小； f(n) 表示每行代码执行的次数总和。

大 O 时间复杂度实际上并不具体表示代码真正的执行
时间，而是表示代码执行时间随数据规模增长的变化趋势，所以，也叫作渐进时间复杂度（asymptotic time complexity），简称**时间复杂度。**

```java
int cal(int n) {
  int sum = 0;
  int i = 1;
  int j = 1;
  for (; i <= n; ++i) {
    j = 1;
    for (; j <= n; ++j) {
      sum = sum + i * j;
  }
}
```

`T(n) = O(2n 2 +2n+3)` , 我们只需要记录一个最大量级就可以
了，如果用大 O 表示法表示刚讲的那两段代码的时间复杂度，就可以记为： `T(n) = O(n)； T(n) = O(n 2 )` 。

### 时间复杂度分析

1. 只关注循环执行次数最多的一段代码
2. 加法法则：总复杂度等于量级最大的那段代码的复杂度
3. 乘法法则：嵌套代码的复杂度等于嵌套内外代码复杂度的乘积

``` java
int cal(int n) {
int ret = 0;
int i = 1;
for (; i < n; ++i) {
ret = ret + f(i);
}
int f(int n) {
int sum = 0;
int i = 1;
for (; i < n; ++i) {
sum = sum + i;
}
return sum;
}
```

我们单独看 cal() 函数。假设 f() 只是一个普通的操作，那第 4 ～ 6 行的时间复杂度就是， ` T1(n) = O(n) ` 。但 f() 函数本身不是一个简单的操作，它的时间复杂度是 `T2(n) =
O(n) `，所以，整个cal()函数的时间复杂度就是，` T(n) = T1(n) * T2(n) = O(n*n) = O(n 2 )。`

#### 常见时间复杂度

![20191230163353](http://q3bgvsie8.bkt.clouddn.com/zoro/20191230163353.png?imageView/2/w/300)

2. O(logn)、O(nlogn)

``` java
i=1;
while (i <= n) {
  i = i * 2;
}
```

3. O(m+n)、O(m*n)

<div class="reference-linking">参考链接</div>

