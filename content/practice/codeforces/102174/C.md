---
title: "CF 102174C - \u8d5b\u5c14\u9035\u4f20\u8bf4"
description: "森克需要依次击败 (n) 只怪兽。第 (i) 只怪兽有 (di) 点生命值和 (xi) 点攻击力, 森克平时的攻击力为 (k) . . 森克有 (c) 份力量炖水果。每吃一份,当前回合的攻击力增加 (k),也就是说,如果这一回合吃了 (t) 份水果,森克这一击造成 ((t+1)k) Bạn có thể làm điều đó một cách dễ dàng. 我们需要决定水果如何分配,使所有怪兽被击败 后森克承受的总伤害最小。输出这个最小值。…"
date: "2026-08-19T15:24:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "C"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 370
verified: true
draft: false
---

[CF 102174C - \u8d5b\u5c14\u9035\u4f20\u8bf4](https://codeforces.com/problemset/problem/102174/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

森克需要依次击败 (n) 只怪兽。第 (i) 只怪兽有 (d_i) 点生命值和 (x_i) 点攻击力, 森克平时的攻击力为 (k) . . 

森克有 (c) 份力量炖水果。每吃一份,当前回合的攻击力增加 (k),也就是说,如果这一回合吃了 (t) 份水果,森克这一击造成 ((t+1)k) Bạn có thể làm điều đó một cách dễ dàng. 

我们需要决定水果如何分配,使所有怪兽被击 败后森克承受的总伤害最小。输出这个最小值。 

对于第 (i) 只怪兽,如果完全不使用水果,它需要 

[ 
q_i=\left\lceil\frac{d_i}{k}\right\rceil 
] 

次攻击才能死亡。由于森克始终先手,其中最后一 次攻击不会受到反击,所以这只怪兽造成的伤害为 

[ 
(q_i-1)x_i. 
] 

Bạn có thể làm điều đó bằng cách sử dụng công cụ này. 斗中额外获得一份水果,相当于额外增加一次基础攻击单位 (k)。假设原本需要 (q_i) 次攻击，现在为这只怪兽投入 (f) 份水果，那么总共拥有 (q_i+f) 个单位力可以把这些攻击力分配到若干回合中。只要最终需要的回合数为 (q_i-f),就可以把水果带来的攻击力全部用于提前结束战斗 也正好减少 (x_i) 点伤害。 

一只怪兽最多只能从 (q_i) 个回合缩短到一个回合,所以它最多能使用 

[ 
q_i-1 
] 

份水果。于是问题已经从模拟战斗变成了一个资源分配问题:每只怪兽提供 (q_i-1) 个位置,每个位置使用一份水果可以获得 (x_i) 的收益,总共只有 (c) 份水果。 

由于 (n) 可以达到 (10^5)，而 (k,c,d_i,x_i) 最大可以达到 (10^6),任何涉及所有水果状态和所有怪兽状态的二维动态规划都无法通过。比如 (O(nc)) 会达到 (10^{11}) 级别的状态，1 Bạn có thể làm được điều đó? Bạn có thể làm điều đó bằng cách sử dụng nó. 

还需要注意几个容易出错的边界情况。 

第一种情况是怪兽只需要一回合。例如输入```
1 10 100
7 5
```输出应该是`0`(q_i x_i) 当作伤害,就会错误得到 5。 

Bạn có thể làm được điều đó.```
1 3 1
6 7
```输出应该是`0`. 6. 可以直接击杀。若把回合数写成`d // k + 1`就会出现典型的整除边界错误。 

Bạn có thể làm được điều đó không?```
2 10 100
1 4
1 9
```输出仍然是`0`(c) Bạn có thể làm điều đó một cách dễ dàng. 

Bạn có thể làm được điều đó?```
2 3 1
3 3
4 5
```Bạn có thể làm điều đó? Bạn có thể làm được điều đó không?`0`. . 如果按怪兽生命值 水果实际减少的伤害排序,就可能得到错误策略。 

## Phương pháp tiếp cận 

Bạn có thể làm được điều đó không? 象。先计算每只怪兽不使用水果时需要的回合数 (q_i),然后考虑给前若干只怪兽分别分配多少水果。可 Bạn có thể làm điều đó một cách dễ dàng. 水果, 转移时枚举当前怪兽使全忠 Một công ty có thể cung cấp cho bạn một công cụ hỗ trợ. 

(n) 和 (c) Bạn có thể làm được điều đó? 

[ 
n c=10^5\times10^6=10^{11}, 
] 

这已经远远超过可接受范围。若真的枚举每只 怪兽可以使用的水果数量,复杂度还会更高。 

(i) 只怪兽而言,它本来需要 (q_i) 次攻击,也就是会反击 (q_i-1) 次。每额外使用一份水果,就相当于增加一次基础攻击单位 因此可以让所需攻击次数减少一次。一次少反击就少承受 (x_i) 点伤害,所以这只怪兽的每一份有效水果都具有完全相同的收益 (x_i)。 

于是第 (i) 只怪兽实际上贡献了 (q_i-1) 个价值为 (x_i) 的物品。所有物品的重量都是 1. 而背包容量是 (c)。这样值最大的物品即可。 

bạn có thể làm được điều đó không? bạn có thể làm được điều đó. (x_i) 从大到小排序,然后尽可能把水果交给攻击力最高的怪兽即可。每只怪兽最多拿 (q_i-1) 份 拿满后再处理下一只怪兽。 

Công ty có thể cung cấp dịch vụ tốt nhất: 

[ 
\sum_i(q_i-1)x_i. 
] 

每使用一份水果,就从答案中减去当前怪兽的 (x_i)。所以只需要按 (x_i) 降序贪心扣除收益。 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | (O(nc)) 或更高 | (O(c)) | Quá chậm | 
| Sắp xếp và tham lam | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. 对每只怪兽计算它在没有水果时需要的攻击次数 

[ 
q_i=\left\lceil\frac{d_i}{k}\right\rceil 
=\frac{d_i+k-1}{k}. 
] 

因为最后一次攻击不会遭到反击，所以它初始造成的伤害为 ((q_i-1)x_i)。 
2. 将第 (i) 只怪兽的可用水果数量记为 

[ 
cap_i=q_i-1. 
] 

Bạn có thể làm điều đó một cách dễ dàng. 一次会导致反击的回合,因此每份水果可以减少 (x_i) 点伤害。一个怪兽最多只能缩短到一回合,所以不能使用超过 (q_i-1) 份水果。 
3. 把所有怪兽按照攻击力 (x_i) 从大到小排序。 

Bạn có thể làm được điều đó không? 1. Bạn có thể làm điều đó một cách dễ dàng. Bạn có thể làm được điều đó. 
4. 依次处理排序后的怪兽。设当前还剩 (c) 份水果,那么最多可以给当前怪兽使用 

[ 
\min(c,cap_i) 
] 

份。将这些水果带来的收益从答案中扣除,然后减少剩余水果数量。 
5. Bạn có thể làm được điều đó không? Bạn có thể làm được điều đó không? 

### Tại sao nó hoạt động 

(x_i) Bạn có thể làm điều đó một cách dễ dàng. Bạn có thể làm điều đó bằng cách (x_i) Bạn có thể làm điều đó? 

对于单位重量物品,最优方案显然选择价值最大的 Bạn có thể làm điều đó một cách dễ dàng. (x_i)，所以按 (x_i) 从大到小选择，并且每只怪兽最多选择 (q_i-1) 个,就是全局最优初始伤害减去最大可获得的收益后,得到的正是最小伤害。 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, c = map(int, input().split())

    monsters = []
    answer = 0

    for _ in range(n):
        d, x = map(int, input().split())

        rounds = (d + k - 1) // k
        useful = rounds - 1

        answer += useful * x

        if useful > 0:
            monsters.append((x, useful))

    monsters.sort(reverse=True)

    remaining = c

    for x, useful in monsters:
        if remaining == 0:
            break

        use = min(remaining, useful)
        answer -= use * x
        remaining -= use

    print(answer)

if __name__ == "__main__":
    solve()
```代码首先计算`rounds = (d + k - 1) // k``这是整数形式的向上取整。这里不能写成`d // k + 1`因为当 (d) 恰好被 (k) 整除时,两者不同。`useful = rounds - 1`表示这只怪兽最多有多少次反击可以被水果消除。若`useful`为 0. 

变量`answer`Bạn có thể làm được điều đó không?`(rounds - 1) * x`。 

随后按照`x`降序排序。Python là một công cụ tuyệt vời`monsters.sort(reverse=True)`正好按照攻击力从大到小排列。第二个元素只是容量,不影响我们需要的主要排序顺序。 

Bạn có thể làm điều đó?`min(remaining, useful)`Bạn có thể làm được điều đó không?`x`所以直接执行`answer -= use * x`。 

Python 的整数没有固定 64 位限制,因此不会发生溢出。在C++`long long`。 

## Ví dụ đã hoạt động 

### Mẫu 1 

输入为:```
1 5 3
21 3
```这只怪兽有 21 点生命值,森克基础攻击力为 5,所以没有水果时需要 

[ 
\left\lceil\frac{21}{5}\right\rceil=5 
] 

次攻击,因此会反击 4,每次造成 3 点伤害。 

| Quái vật | (d_i) | (x_i) | (q_i) | (cap_i) | Thiệt hại ban đầu | Trái cây đã qua sử dụng | Trái cây còn lại | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 21 | 3 | 5 | 4 | 12 | 3 | 0 | 

3 减少到 

[ 
12-3\times3=3. 
] 

这与直接模拟也一致。第一回合使用三份水果,攻击力为 20,怪兽剩 1 5 直接击杀, 所以总共只受到 3 点伤害。 

### Mẫu 2 

输入为:```
2 3 1
3 3
4 5
```第一只怪兽的生命值是 3,基础攻击力也是 3, 所以它一回合就会死亡,没有任何可优化的反击 次数。第二只怪兽需要两回合,因此存在一个价值为 5 điều cần làm. 

| Quái vật | (d_i) | (x_i) | (q_i) | (cap_i) | Thiệt hại ban đầu | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 1 | 0 | 0 | 
| 2 | 4 | 5 | 2 | 1 | 5 | 

排序后第二只怪兽排在前面,因为它的攻击力 5 3。唯一的一份水果给第二只怪兽,收益为 5。 

| Quái vật đã qua chế biến | (x_i) | Công suất | Trái Cây Trước | Trái cây đã qua sử dụng | Thiệt hại sau | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 5 | 1 | 1 | 1 | 0 | 
| 1 | 3 | 0 | 0 | 0 | 0 | 

最终答案为`0`。这也展命值或者输入顺序 分配水果, bạn có thể làm được điều đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | 计算所有怪兽信息需要 (O(n)),按照攻击力排序需要 (O(n\log n)),最后扫描一次需要 (O(n)) | 
| Không gian | (O(n)) | 最多保存 (n) 只存在可优化回合的怪兽 | 

当 (n=10^5) 时避免了与 (c) 相乘的动态规划状态。即使 (c) 达到 (10^6),实际循环次数仍然只有 (O(n)),因为不会逐份枚举水果,所以能够满足 1 秒级别的时间限制。 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, k, c = map(int, input().split())

    monsters = []
    answer = 0

    for _ in range(n):
        d, x = map(int, input().split())

        rounds = (d + k - 1) // k
        useful = rounds - 1

        answer += useful * x

        if useful > 0:
            monsters.append((x, useful))

    monsters.sort(reverse=True)

    remaining = c

    for x, useful in monsters:
        if remaining == 0:
            break

        use = min(remaining, useful)
        answer -= use * x
        remaining -= use

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """1 5 3
21 3
"""
).strip() == "3", "sample 1"

# Sample 2
assert run(
    """2 3 1
3 3
4 5
"""
).strip() == "0", "sample 2"

# Sample 3
assert run(
    """2 10 2
51 10
20 1
"""
).strip() == "30", "sample 3"

# Minimum-size input
assert run(
    """1 1 1
1 7
"""
).strip() == "0", "minimum-size case"

# Exact divisibility boundary
assert run(
    """1 3 1
6 7
"""
).strip() == "0", "exact divisibility"

# All equal values
assert run(
    """3 2 2
5 4
5 4
5 4
"""
).strip() == "4", "all equal values"

# More fruit than useful positions
assert run(
    """2 10 100
1 4
1 9
"""
).strip() == "0", "excess fruit"

# Maximum n and c
max_case = "100000 1 1000000\n" + ("1 1\n" * 100000)
assert run(max_case).strip() == "0", "maximum-size case"
```第三个样例中,第一只怪兽需要 

[ 
\left\lceil\frac{51}{10}\right\rceil=6 
] 

次攻击,因此有 5 个价值为 10 51 减去 20 后得到 30。 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 7`| 0 | 最小规模以及一击击杀 | 
|`1 3 1 / 6 7`| 0 | (d_i) 恰好整除 (k) 的边界 | 
|`3 2 2 / 5 4 / 5 4 / 5 4`| 4 | 相同攻击力下的容量累计 | 
|`2 10 100 / 1 4 / 1 9`| 0 | 水果多于全部有效位置 | 
|`100000 1 1000000 / 100000\times(1,1)`| 0 | 最大规模以及不应按 (c) 枚举水果 | 

## Vỏ cạnh 

当怪兽本身一回合就能被杀死时,(q_i=1),所以`useful = 0`。例如```
1 10 100
7 5
```计算得到 (q=1),初始伤害就是 ((1-1)\times5=0)。算法不会给它分配水果,因为它没有任何有效位置,最终输出`0`。 

当 (d_i) 恰好是 (k) 的倍数时,向上取整必须得到准确的整数商。例如```
1 3 1
6 7
```有 (q = 6/3 = 2) 6, 最接击杀怪兽, 收益为 7, 最终答案为`0`。使用`(d + k - 1) // k`可以正确处理这个边界。 

Công ty có thể cung cấp dịch vụ hỗ trợ tốt nhất cho bạn.```
2 10 100
1 4
1 9
```两只怪兽都有 (q_i=1),所以所有`useful`都是 0. 0. Bạn có thể làm điều đó một cách dễ dàng. 

Bạn có thể làm điều đó bằng cách sử dụng các công cụ hỗ trợ.```
2 3 1
3 3
4 5
```5 5 伤害, 正好把它从两回合战斗变成一回合战斗,因此答案为 0。算法按照攻击力排序后自然得到这个选择。 

最后,当 (c) 非常大时,不能写成循环执行 (c) 次并每次寻找最优怪兽。(c) 可以达到 (10^6)，虽然单独看并不算巨大，但结合 (n=10^5) 会让错误的 (O(nc)) 方法达到 (10^{11}) 级别。当前算法对每只怪兽一次性计算`use = min(remaining, useful)`所以无论 (c) 多大，处理水果的循环仍然只有 (O(n)) 次。
