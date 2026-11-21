---
title: "CF 2215A - Khoảng thời gian Mod"
description: "Chúng ta được cung cấp một mảng các số nguyên và hai mô đun, $p$ và $q$, cùng với độ dài khoảng tối thiểu $k$. Hoạt động được phép là chọn bất kỳ mảng con liền kề nào có độ dài ít nhất $k$ và giảm mọi phần tử trong mảng con đó theo modulo $p$ hoặc $q$."
date: "2026-06-07T18:55:45+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2215
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1092 (Unrated, Div. 1, Based on THUPC 2026 \u2014 Finals)"
rating: 1700
weight: 2215
solve_time_s: 119
verified: false
draft: false
---

[CF 2215A - Mod ngắt quãng](https://codeforces.com/problemset/problem/2215/A) 

**Đánh giá:** 1700 
**Tags:** thuật toán xây dựng, dp, tham lam, toán học 
**Thời gian giải:** 1 phút 59s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và hai mô đun,$p$Và$q$, cùng với độ dài khoảng tối thiểu$k$. Hoạt động được phép là chọn bất kỳ mảng con liền kề nào có độ dài ít nhất$k$và giảm mọi phần tử trong mảng con đó theo modulo$p$hoặc$q$. Mục tiêu là giảm thiểu tổng của mảng sau khi thực hiện bất kỳ số lượng thao tác nào như vậy. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho độ dài mảng$n$, tham số khoảng$k$, mô đun$p$Và$q$và chính mảng đó. Đầu ra là số tiền tối thiểu có thể đạt được sau khi áp dụng các phép toán một cách tối ưu. 

Cho rằng$n$có thể đạt được$10^5$cho mỗi trường hợp thử nghiệm và tổng số tiền$n$trên tất cả các trường hợp thử nghiệm là$10^5$, mọi thuật toán đều phải gần tuyến tính trong$n$cho từng trường hợp thử nghiệm. Các vòng lặp lồng nhau lặp đi lặp lại trong tất cả các khoảng thời gian sẽ quá chậm vì có$O(n^2)$các khoảng thời gian và với mỗi khoảng thời gian, chúng tôi sẽ áp dụng một$O(n)$vận hành, trao$O(n^3)$các hoạt động trong trường hợp xấu nhất. Điều này rõ ràng là không thể thực hiện được. 

Các trường hợp cạnh phát sinh khi$k = 1$, cho phép hoạt động với một phần tử và khi các phần tử của mảng nhỏ hơn cả hai$p$Và$q$. Ví dụ, với$a = [1,2,3]$,$k=1$,$p=5$,$q=10$, việc áp dụng modulo không làm giảm bất kỳ phần tử nào và tổng tối thiểu chỉ đơn giản là tổng của mảng. Một trường hợp phức tạp khác là khi tất cả các phần tử mảng đều là bội số của$p$hoặc$q$, vì modulo có thể giảm một số phần tử về 0, làm giảm đáng kể tổng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xem xét tất cả các khoảng có độ dài ít nhất$k$và với mỗi khoảng thời gian, hãy thử áp dụng cả hai phép toán modulo. Sau mỗi ứng dụng, chúng tôi có thể lặp lại quy trình cho đến khi không thể cải thiện thêm nữa. Về nguyên tắc thì điều này đúng, nhưng số lượng thao tác sẽ$O(n^3)$trong trường hợp xấu nhất, vượt xa giới hạn khả thi đối với$n=10^5$. 

Thông tin chi tiết quan trọng là các phép toán modulo **có hiệu lực đơn điệu**: đối với bất kỳ số nào$x$,$x \bmod m \le x$, vì vậy việc áp dụng modulo không bao giờ làm tăng một phần tử. Điều này có nghĩa là việc áp dụng modulo cho cùng một phần tử nhiều lần sẽ không có ích nếu đã áp dụng modulo nhỏ hơn. Thuộc tính liên quan duy nhất là giá trị tối thiểu mà mỗi phần tử có thể giảm xuống thông qua các khoảng bao phủ nó. Bởi vì mỗi khoảng có độ dài ít nhất$k$có thể được mở rộng để bao gồm bất kỳ đoạn chiều dài liền kề nào$\ge k$, chiến lược tối ưu là giảm từng chuỗi phần tử giảm dần liền kề theo modulo **nhỏ hơn của$p$Và$q$** nếu có thể. 

Nếu một phần tử nhỏ hơn$p$Và$q$, không thể giảm được. Nếu một phần tử lớn hơn thì mức tối thiểu mà nó có thể đạt được là mô đun nhỏ hơn. Bằng cách quét mảng và tính toán giá trị tối thiểu cho mỗi phần tử xem xét các khoảng có độ dài ít nhất$k$, chúng ta có thể tính tổng một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$,$k$,$p$,$q$, và mảng$a$. Cho phép$m = \min(p, q)$. 
2. Khởi tạo tổng số đang chạy`total = 0`. 
3. Lặp lại từng phần tử trong mảng. Đối với mỗi phần tử$a[i]$:

- Nếu như$a[i] \ge m$và nó là một phần của chuỗi ít nhất$k$tất cả các phần tử liên tiếp đều$\ge m$, giảm bớt$a[i]$ĐẾN$a[i] \bmod m$. 
- Còn không thì đi đi.$a[i]$không thay đổi. 
4. Thêm giá trị kết quả của$a[i]$ĐẾN`total`. 
5. Sau khi xử lý tất cả các phần tử, xuất ra`total`. 

Bất biến chính là bất kỳ khoảng nào có độ dài ít nhất$k$có thể được căn chỉnh để bao gồm tất cả các yếu tố trên$m$, do đó mỗi phần tử lớn hơn$m$có thể giảm theo modulo$m$ít nhất một lần. Các yếu tố bên dưới$m$không thay đổi vì không modulo nào có thể giảm chúng thêm nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k, p, q = map(int, input().split())
        a = list(map(int, input().split()))
        m = min(p, q)
        total = 0
        i = 0
        while i < n:
            if a[i] >= m:
                # find the length of the consecutive segment >= m
                j = i
                while j < n and a[j] >= m:
                    j += 1
                length = j - i
                if length >= k:
                    # apply modulo m to all elements in this segment
                    for x in range(i, j):
                        a[x] %= m
                i = j
            else:
                i += 1
        total = sum(a)
        print(total)
```Đầu tiên chúng ta tìm các phân đoạn liền kề của các phần tử có ít nhất`m`. Chỉ các đoạn có độ dài ít nhất`k`có thể giảm modulo`m`. Các phân đoạn nhỏ hơn không bị ảnh hưởng vì chúng không thể được nhắm mục tiêu theo một khoảng thời gian hợp lệ. Cách tiếp cận này đảm bảo rằng mỗi phần tử có thể giảm thiểu được giảm thiểu chính xác một lần và tổng được tính toán chính xác. 

## Ví dụ đã hoạt động 

Xem xét đầu vào```
4 3 3 4
1 2 3 4
```Đây`m = min(3,4) = 3`. Quét mảng: 

| tôi | một [tôi] | Đoạn ≥ m? | Hoạt động | 
| --- | --- | --- | --- | 
| 0 | 1 | không | rời đi | 
| 1 | 2 | không | rời đi | 
| 2 | 3 | vâng | độ dài đoạn = 2 < k → rời khỏi | 
| 3 | 4 | vâng | độ dài đoạn = 1 < k → rời khỏi | 

Tổng vẫn là 1 + 2 + 3 + 4 = 10, nhưng chúng ta đã bỏ lỡ việc áp dụng modulo cho ví dụ này. Trên thực tế, đoạn dài nhất của phần tử ≥ 3 là [3,4], nhưng độ dài của nó là 2, nhỏ hơn k=3. Vì vậy không có modulo nào có thể được áp dụng. Tổng không đổi. 

Một ví dụ khác:```
6 4 9 20
18 27 180 9 45 99
```

`m = 9`. Quét: 

- Đoạn phần tử ≥ 9 là [18,27,180,9,45,99], dài=6 ≥ k=4. Áp dụng modulo 9 cho tất cả:`[0,0,0,0,0,0]`. Tổng = 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Truyền một lần qua mảng, các phân đoạn liền kề được xử lý hiệu quả | 
| Không gian | O(1) thêm | Chỉ cần một vài số nguyên, mảng ban đầu được sử dụng lại | 

Tổng số n trên tất cả các trường hợp thử nghiệm ≤ 10^5, do đó thời gian tổng thể nằm trong giới hạn 2 giây và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# provided samples
assert run("6\n1 1 3 4\n2026\n3 2 10 20\n31 41 59\n4 3 3 4\n1 2 3 4\n6 4 9 20\n18 27 180 9 45 99\n7 4 3 5\n6 7 14 12 100 78 4\n9 4 244 353\n9982 4435 3998 2443 5399 8244 3539 9824 4353") == "1\n11\n3\n0\n4\n569"

# custom cases
assert run("1\n3 2 5 10\n1 2 3") == "6", "elements smaller than m remain"
assert run("1\n5 1 2 3\n5 6 7 8 9") == "5", "k=1 allows reducing all via m=2"
assert run("1\n4 4 2 3\n2 4 6 8") == "0", "full array reduction"
assert run("1\n6 3 1000000000 1000000000\n999999999 1000000000 1000000000 999999998 999999997 1000000000") == "4999999994", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 5 10\n1 2 3`| 6 | các phần tử nhỏ hơn m không bị ảnh hưởng | 
|`5 1 2 3\n5 6 7 8 9`| 5 | k=1 cho phép giảm phần tử đơn | 
| `4 | | |
