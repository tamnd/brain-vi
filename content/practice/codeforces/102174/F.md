---
title: "CF 102174F - \u98ce\u738b\u4e4b\u77b3"
description: "我们面对的是一个由整数格点组成的 (ntimes m) 矩形网格。这里的 (n,m) ((n+1)times(m+1)) 的点集。Bạn có thể làm được điều đó không? . 这里的正方形并不局限于边平行于网格线的情况。比如在 (2times2) 网格中，除了 4 个单位正方形和 1 个边长为 2 的大正方形之外,还存在一个由四个边界中点组成的倾斜正方形，所以答案是 6。 (n,m) 都可以达到 (10^5),而测试组数最多有…"
date: "2026-08-19T07:03:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "F"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 120
verified: true
draft: false
---

[CF 102174F - \u98ce\u738b\u4e4b\u77b3](https://codeforces.com/problemset/problem/102174/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

我们面对的是一个由整数格点组成的 (n\times m) 矩形网格。这里的 (n,m) 因此格点坐标可以看成一个 ((n+1)\times(m+1)) 的点集。Bạn có thể làm được điều đó không? . 

这里的正方形并不局限于边平行于的情况。比如在 (2\times2) 网格中，除了 4 个单位正方形和 1 个边长为 2 所以歔案是 6。 

(n,m) 都可以达到 (10^5)，而测试组数最多有 100 组。这样的规模直接排除枚举大量格点、顶点 组合或者所有正方形的做法。即使单组数据只做 (O(nm)) 的工作,在最坏情况下也会达到 (10^{10}) 级别, 更不用说枚举四个顶点需要的更高复杂 Bạn có thể làm được điều đó. 

(1\times1)```
1
1 1
```答案是```
1
```如果把网格中的格子数错误地当成格点数,或者把最大的边长写成 (\min(n,m)-1),都会得到错误结果。 

其次是 (2\times2) 网格。普通的轴对齐正方形有 4 个单位正方形和 1 个 (2\times2) 正方形,而倾斜正方形还有 1 个,所以```
1
2 2
```答案是```
6
```这个例子尤其容易暴露漏掉倾斜正方形的问题。 

最后考虑非方形网格,例如```
1
1 2
```答案为```
2
```只能出现两个 (1\times1) 正方形。这里最大的有效边长是 (\min(n,m)=1),不能因为长边是 2 就继续计数。 

## Phương pháp tiếp cận 

Bạn có thể làm được điều đó không? Bạn có thể làm được điều đó. 

Một công ty có thể cung cấp cho bạn một công cụ hỗ trợ. ((a,b))。它的长度平方为 (a^2+b^2)，而垂直边可以取 ((-b,a))。当 (a,b) Một công ty có thể cung cấp cho bạn một công cụ hỗ trợ. 

但是对于固定的 (s=a+b),只要 (a,b>0),满足条件的正整数对有 (s-1) 个量平移位置。于是所有 累加起来，单组数据的复杂度达到 (\Theta(\min(n,m)^4))。当 (n=m=10^5) 时，对应的量级约为 (8.3\times10^{18})，完全不可行。 

真正有用的观察是,正方形的位置数量只与 (a+b) 有关,而不是分别与 (a,b) 有关。原因来自旋转后的两个边向量。若一条边是 ((a,b)),另一条边可以是 ((-b,a))，所以这个正方形在横坐标方向覆盖了 (a+b) 个单位长度，在纵坐标方向也覆盖了 (a+b) 个单位长度。 

bạn 

[ 
s=a+b. 
] 

对于固定的 (s),所有满足 (a,b>0) 的有序正整数对一共有 (s-1) 个。轴对齐的正方形可以看成额外的 1 种方向，因此边界尺寸为 (s) 的正方形方向总数恰好是 

[ 
1+(s-1)=s. 
] 

固定 (s) 后,一个这样的正方形可以放置在 

[ 
(n-s+1)(m-s+1) 
] 

个位置,因为它的水平和垂直包围范围都正好是 (s)。只有 (s\le\min(n,m)) 时这个数量才非零。 

所以答案直接变成 

[ 
\sum_{s=1}^{k}s(n-s+1)(m-s+1), 
] 

其中 

[ 
k=\min(n,m). 
] 

展开以后,被求和的部分只是一个关于 (s) 的三次多项式,因此可以利用平方和、立方和公式在 (O(1)) 时间内计算。 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(\min(n,m)^4)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. 令 (k=\min(n,m)),因为任何正方形的水平跨度和垂直跨度都必须至少等于它的参数 (s),所以 (s) 不可能超过较短的一边。 
2. 固定一个 (s),考虑所有边向量满足 (a,b\ge0) 且 (a+b=s) 的正方形。对于 (a,b>0),正整数解有 (s-1) 个；当其中一个为 0 时得到轴对齐正方形，这一类提供 1 个方向。因此固定 (s) 后一共有 (s) 种方向。 
3. 对于每一种方向，正方形的包围矩形宽和高都是 (s)。所以它的左上或其他固定参考顶点可以在 (n-s+1) 个横向位置和 (m-s+1) 个纵向位置中选择,总位置数为 
[ 
(n-s+1)(m-s+1). 
] 
4. 把固定(s) 的贡献写成 
[ 
s(n-s+1)(m-s+1). 
] 
从 (s=1) 到 (k) 累加,就得到全部正方形。 
5. 为了避免循环展开乘积,令 
[ 
A=(n+1)(m+1), 
\qquad 
B=n+m+2. 
] 
那么 
[ 
(n-s+1)(m-s+1) 
=A-Bs+s^2. 
] 
因而答案为 
[ 
A\sum s-B\sum s^2+\sum s^3. 
] 
6. 使用 
[ 
\sum_{s=1}^{k}s=\frac{k(k+1)}2, 
] 
[ 
\sum_{s=1}^{k}s^2=\frac{k(k+1)(2k+1)}6, 
] 
[ 
\sum_{s=1}^{k}s^3= 
\left(\frac{k(k+1)}2\right)^2. 
] 
将三个值代入即可直接得到答案。 

### Tại sao nó hoạt động 

固定 (s) 后，每个格点正方形都对应唯一的一个方向参数 ((a,b)),其中 (a,b\ge0)、(a+b=s)。当 (a,b>0) 时，交换 (a,b) 会得到不同的倾斜方向，而 (a=0) 或 (b=0) 共同对应轴对齐方向，因此恰好有 (s) 种方向。每种方向的包围范围都是 (s\times s),所以恰好有 ((n-s+1)(m-s+1)) 个平移位置。不同的 (s) 位置对应不同的四个顶点,反过来每个格点正方形 也能唯一确定这些参数,因此求和既没有遗漏,也没有重复计算。 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        k = min(n, m)

        s1 = k * (k + 1) // 2
        s2 = k * (k + 1) * (2 * k + 1) // 6
        s3 = s1 * s1

        a = (n + 1) * (m + 1)
        b = n + m + 2

        ans = a * s1 - b * s2 + s3
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```代码首先读取测试组数,然后对每组数据计算 (k=\min(n,m))。这对应算法大可能跨度。`s1`、`s2`和`s3`分别保存 (1) 到 (k) Bạn có thể làm điều đó một cách dễ dàng. Bạn có thể làm điều đó một cách dễ dàng.`a`和`b`对应展开式中的两个系数。原式 

[ 
s(n-s+1)(m-s+1) 
] 

展开为 

[ 
s\left((n+1)(m+1)-(n+m+2)s+s^2\right), 
] 

也就是 

[ 
(n+1)(m+1)s-(n+m+2)s^2+s^3. 
] 

所以最终答案就是`a * s1 - b * s2 + s3`。 

Python 不存在 C++ 中需要特别处理的 64 位溢出问题。不过结果本身可以达到 (10^{19}) 量级,因此如果用固定宽度整数实现,需要使用至少 64 位整数。 

输入输出使用`sys.stdin.readline`和一次性写出结果，避免 100 组数据下频繁输出带来的额外开销。 

## Ví dụ đã hoạt động 

### Ví dụ 1 

输入只有一个 (1\times1) 网格，此时 (k=1)。 

| s | 方向数量 | 可放置位置 | 本轮贡献 | 
| --- | --- | --- | --- | 
| 1 | 1 | ((1-1+1)(1-1+1)=1) | 1 | 

因此答案为 1。 

这个例子说明公式的起点必须是 (s=1),最小网格不能被错误地当成没有正方形。 

### Ví dụ 2 

对于 (2\times2) 网格有 (k=2)。 

| s | 方向数量 | 可放置位置 | 本轮贡献 | 
| --- | --- | --- | --- | 
| 1 | 1 | (2\times2=4) | 4 | 
| 2 | 2 | (1\times1=1) | 2 | 
| | | | **6** | 

当 (s=1) 时得到 4 个单位正方形。当 (s=2) 时，一个方向是边平行于网格的 (2\times2) 大正方形,另一个方向是倾斜的菱形正方形,因此贡献为 2. 

这也解释了为什么答案不是通常的轴对齐正方形数量 5,而是 6。 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) cho mỗi trường hợp thử nghiệm | 只计算三个幂和以及常数次乘法 | 
| Không gian | (O(1)) | 只保存常数个整数 | 

(n,m) 同时达到 (10^5),100 组测试也可以轻松满足 1 秒级别的时间要求。空间消耗同样与网格大小无关。 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        k = min(n, m)

        s1 = k * (k + 1) // 2
        s2 = k * (k + 1) * (2 * k + 1) // 6
        s3 = s1 * s1

        a = (n + 1) * (m + 1)
        b = n + m + 2

        ans = a * s1 - b * s2 + s3
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# provided samples
assert run("2\n1 1\n2 2\n") == "1\n6", "provided samples"

# minimum-size input
assert run("1\n1 1\n") == "1", "minimum grid"

# rectangular boundary case
assert run("1\n1 2\n") == "2", "one-row grid"

# catches the tilted-square counting
assert run("1\n3 3\n") == "20", "3x3 grid"

# maximum-size input
assert run("1\n100000 100000\n") == "8333666670833350000", "maximum grid"

# swapped dimensions must give the same answer
assert run("1\n2 3\n") == "10", "dimension symmetry"
assert run("1\n3 2\n") == "10", "dimension symmetry"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| 最小边界和 (s=1) 的起始条件 | 
|`1 2`|`2`| 较短边决定最大跨度防止越界计数 | 
|`3 3`|`20`| 同时包含轴对齐和倾斜正方形 | 
|`100000 100000`|`8333666670833350000`| 最大数据范围和大整数计算 | 
|`2 3`/`3 2`|`10`| 交换网格长宽后答案应保持不变 | 

## Vỏ cạnh 

对于最小输入`1 1`,算法得到 (k=1),一次方和、二次方和、三次方和都为 1。此时 (A=4)、(B=4),最终答案为 (4-4+1=1), 正好对应唯一的单位正方形。 

对于`2 2`,算法分别处理 (s=1) 和 (s=2)。第一轮贡献 (1\times2\times2=4),第二轮贡献 (2\times1\times1=2),总数为 6. 中的两个别对应轴对齐的大正方形和倾 斜正方形,这正是只枚举普通正方形时容易遗漏的部分。 

对于`1 2`，(k=\min(1,2)=1)，所以算法根本不会尝试 (s=2)。唯一可能的正方形边长只能是 1，并且有 ((1-1+1)(2-1+1)=2) 个位置,因此答案为 2。这个边界处理避免了把长边上的长度 2 错误当成正方形边长。 

对于`3 3`逐项计算得到 (s=1) 时贡献 9,(s=2) 时贡献 8,(s=3) 时贡献 3,总答案为 20。这里三个不同的跨度都会出现,其中 (s=2) 已经包含两个不同方向，能够验证方向数量确实是 (s),而不是 (s-1) 或 (2(s-1))。 

对于最大输入`100000 100000`,(k=100000)，算法不会创建网格，也不会循环 100000 次，而是直接计算三个求和公式。Python Bạn có thể làm điều đó?`8333666670833350000`所以不会发生溢出或精度损失。
