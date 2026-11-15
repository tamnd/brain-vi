---
title: "CF 1869A - Làm cho nó bằng không"
description: "Đối với Codeforces 1869A, toàn bộ giải pháp là sự quan sát mang tính xây dựng. Nếu n chẵn, thực hiện: Sau thao tác đầu tiên, mọi phần tử trở thành X, trong đó X là XOR của toàn mảng."
date: "2026-06-09T00:55:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1869
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 896 (Div. 2)"
rating: 900
weight: 1869
solve_time_s: 158
verified: false
draft: false
---

[CF 1869A - Make It Zero](https://codeforces.com/problemset/problem/1869/A) 

**Xếp hạng:** 900 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
Đối với Codeforces 1869A, toàn bộ giải pháp là sự quan sát mang tính xây dựng. 

Nếu như`n`chẵn, thực hiện:```
(1, n)
(1, n)
```Sau thao tác đầu tiên, mọi phần tử sẽ trở thành`X`, Ở đâu`X`là XOR của toàn bộ mảng. 

Vì độ dài phân đoạn là chẵn nên XOR của toàn bộ mảng bây giờ là:```
X xor X xor ... xor X = 0
```Áp dụng lại thao tác tương tự sẽ biến mọi phần tử thành`0`. 

Nếu như`n`thật kỳ lạ, hãy sử dụng cái đầu tiên`n - 1`các phần tử có độ dài chẵn:```
(1, n - 1)
(1, n - 1)
(n - 1, n)
(n - 1, n)
```Hai thao tác đầu tiên tạo nên vị trí`1..n-1`bằng không. Sau đó, hai thao tác cuối cùng tạo nên vị trí`n-1`Và`n`cũng bằng 0. 

Điều này sử dụng tối đa 4 thao tác, thấp hơn giới hạn 8.```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))  # values are irrelevant

        if n % 2 == 0:
            print(2)
            print(1, n)
            print(1, n)
        else:
            print(4)
            print(1, n - 1)
            print(1, n - 1)
            print(n - 1, n)
            print(n - 1, n)

if __name__ == "__main__":
    solve()
```Tại sao nó hoạt động: 

Đối với đoạn có độ dài chẵn, sau khi thay thế đoạn đó bằng giá trị XOR của nó`x`, mọi vị trí đều chứa`x`. XOR của toàn bộ phân khúc đó trở thành`x`lặp lại một số lần chẵn, tương đương`0`. Áp dụng lại thao tác trên cùng một đoạn sẽ thay thế tất cả các vị trí bằng`0`. 

Khi`n`thật kỳ lạ, đoạn`[1, n-1]`có độ dài chẵn, vì vậy hai thao tác sẽ loại bỏ nó. Sau đó, vị trí`n-1`đã bằng không rồi. Áp dụng thao tác hai lần trên`[n-1, n]`bằng 0 ở hai vị trí cuối cùng, để lại toàn bộ mảng bằng 0.
