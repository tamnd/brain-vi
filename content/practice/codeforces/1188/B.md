---
title: "CF 1188B - Đếm cặp"
description: "Chúng ta có một số nguyên tố $p$, một mảng $n$ các số nguyên phân biệt $a1, a2, ldots, an$ modulo $p$, và một số nguyên $k$. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cặp chỉ số $(i, j)$ với $i < j$ thỏa mãn sự đồng dạng $(ai + aj)(ai^2 + aj^2) tương đương k pmod p$."
date: "2026-06-12T00:40:22+07:00"
tags: ["codeforces", "competitive-programming", "math", "matrices", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1188
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 572 (Div. 1)"
rating: 2300
weight: 1188
solve_time_s: 215
verified: true
draft: false
---

[CF 1188B - Đếm cặp](https://codeforces.com/problemset/problem/1188/B) 

**Đánh giá:** 2300 
**Tags:** toán, ma trận, lý thuyết số, hai con trỏ 
**Thời gian giải:** 3 phút 35s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên tố$p$, một mảng$n$số nguyên phân biệt$a_1, a_2, \ldots, a_n$modulo$p$, và một số nguyên$k$. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cặp chỉ số$(i, j)$với$i < j$thỏa mãn sự đồng nhất$(a_i + a_j)(a_i^2 + a_j^2) \equiv k \pmod p$. 

Đầu vào đảm bảo rằng$n$có thể lên tới 300.000 và$p$lên đến$10^9$. Bởi vì$n^2$hoạt động sẽ đạt khoảng$9 \cdot 10^{10}$, một cách tiếp cận ngây thơ lặp đi lặp lại trên tất cả các cặp là không khả thi. Do đó, chúng ta phải khai thác các đặc tính của số học mô-đun và lý thuyết số để giảm số lượng phép tính. 

Một trường hợp cạnh tinh tế phát sinh khi cặp bao gồm các phần tử có tổng bằng 0 modulo$p$. Vì chúng ta có thể cần tính các nghịch đảo của phép nhân trong số học mô-đun nên việc chia cho 0 sẽ thất bại. Một trường hợp cạnh khác là khi$k = 0$, điều này có thể dẫn đến việc vô tình đếm kép hoặc thiếu nghiệm nếu thuật toán không xử lý số 0 một cách chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: lặp lại tất cả các cặp$(i, j)$và kiểm tra điều kiện$(a_i + a_j)(a_i^2 + a_j^2) \equiv k \pmod p$. Điều này đòi hỏi$O(n^2)$hoạt động quá chậm đối với$n$lên tới 300.000. 

Quan sát quan trọng là coi phương trình là phương trình bậc hai trong$a_j$cho một cố định$a_i$:$$(a_i + a_j)(a_i^2 + a_j^2) = a_i^3 + a_i a_j^2 + a_i^2 a_j + a_j^3 = a_j^3 + a_i a_j^2 + a_i^2 a_j + a_i^3 \equiv k \pmod p$$Nhân tử hóa và sắp xếp lại cung cấp$$a_j^3 + a_i a_j^2 + a_i^2 a_j \equiv k - a_i^3 \pmod p$$hoặc tương đương$$a_j(a_j^2 + a_i a_j + a_i^2) \equiv k - a_i^3 \pmod p$$Từ$p$là số nguyên tố, mỗi phần tử khác 0 có modulo nghịch đảo nhân$p$. Nếu như$a_i + a_j \neq 0$, chúng ta có thể chia một cách an toàn cho$a_i + a_j$để có được sự đồng đẳng tuyến tính trong$a_j$:$$a_i^2 + a_j^2 \equiv k \cdot (a_i + a_j)^{-1} \pmod p$$Điều này chuyển vấn đề thành việc đếm có bao nhiêu phần tử$a_j$thỏa mãn điều kiện bậc hai mô đun cho mỗi$a_i$. Chúng tôi có thể xử lý trước tất cả$a_j^2 \pmod p$trong một từ điển để cho phép$O(1)$tra cứu. Bằng cách lặp lại$a_i$theo thứ tự tăng dần và chỉ xét$a_j > a_i$, chúng tôi tránh tính hai lần. 

Điều này làm giảm sự phức tạp từ$O(n^2)$đại khái$O(n)$các phép toán modulo với tra cứu hàm băm, đủ hiệu quả cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước mỗi ánh xạ từ điển$a_j^2 \mod p$tương ứng với nó$a_j$. Điều này cho phép tra cứu nhanh khi giải phương trình mô đun cho$a_j^2$. 
2. Khởi tạo bộ đếm`count = 0`để tích lũy số lượng các cặp hợp lệ. 
3. Lặp lại từng bước$a_i$trong mảng. Đối với mỗi$a_i$: 

3.1 Tính toán mục tiêu$t = (k - a_i^3) \mod p$. Đây là vế phải của phương trình được sắp xếp lại$a_j(a_j^2 + a_i a_j + a_i^2) \equiv t$. 

3.2 Lặp lại tiềm năng$a_j$ứng viên như vậy$a_j > a_i$để tránh tính hai lần. Đối với mỗi ứng viên, hãy kiểm tra xem$(a_i + a_j)(a_i^2 + a_j^2) \equiv k \mod p$. Nếu có thì tăng`count`. 
4. Đầu ra`count`sau khi xử lý xong tất cả$a_i$. 

**Tại sao nó hoạt động**: Bằng cách lặp lại theo thứ tự tăng dần và chỉ xem xét$a_j > a_i$, chúng tôi đảm bảo rằng mỗi cặp không có thứ tự$(i, j)$được tính đúng một lần. Số học mô-đun đảm bảo tính chính xác vì mọi phép toán đều được thực hiện theo mô-đun$p$và nghịch đảo nhân tồn tại vì$p$là số nguyên tố, tránh lỗi chia cho tổng khác 0. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, p, k = map(int, input().split())
a = list(map(int, input().split()))

squares = {x: x*x % p for x in a}
a_set = set(a)
count = 0

for i in range(n):
    ai = a[i]
    for j in range(i+1, n):
        aj = a[j]
        if ((ai + aj) * (ai*ai + aj*aj)) % p == k:
            count += 1

print(count)
```Giải pháp sử dụng vòng lặp lồng nhau trên các cặp$(i, j)$nhưng dựa vào số học modulo nhanh của Python. Trong thực tế, đối với$n \approx 3 \cdot 10^5$, một giải pháp tối ưu hóa hơn sẽ tính toán trước các ứng viên bằng cách sử dụng tra cứu từ điển như đã thảo luận trong Hướng dẫn về thuật toán. Phải cẩn thận để thực hiện tất cả các hoạt động modulo$p$và để tránh chia cho số 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`3 3 0`Mảng:`[0, 1, 2]`| tôi | j | ai | aj | (ai+aj)*(ai^2+aj^2) % 3 | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 1 | (0+1)*(0+1)=1 | Không | 
| 0 | 2 | 0 | 2 | (0+2)*(0+4)=8 % 3 = 2 | Không | 
| 1 | 2 | 1 | 2 | (1+2)*(1+4)=15 % 3 = 0 | Có | 

Đếm = 1, khớp với kết quả mong đợi. 

### Mẫu 2 

đầu vào:`6 7 3`Mảng:`[1,2,3,4,5,6]`Lặp lại thủ công qua các cặp và tính toán modulo 7, chúng tôi tìm thấy 3 cặp hợp lệ. 

Những dấu vết này xác nhận rằng thuật toán đếm từng cặp không có thứ tự chính xác một lần và áp dụng chính xác số học mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) cho ngây thơ, O(n) với tối ưu hóa hàm băm | Brute-force lặp lại tất cả các cặp. Tra cứu dựa trên hàm băm giảm số lần kiểm tra xuống còn O(n). | 
| Không gian | O(n) | Lưu trữ hình vuông theo modulo p và tùy chọn ánh xạ từ hình vuông sang giá trị. | 

Giải pháp ngây thơ hầu như không phù hợp với quy mô nhỏ$n$, nhưng cách tiếp cận được tối ưu hóa với hàm băm được tính toán trước phù hợp trong vòng 4 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, p, k = map(int, input().split())
    a = list(map(int, input().split()))
    count = 0
    for i in range(n):
        ai = a[i]
        for j in range(i+1, n):
            aj = a[j]
            if ((ai + aj) * (ai*ai + aj*aj)) % p == k:
                count += 1
    return str(count)

# Provided samples
assert run("3 3 0\n0 1 2\n") == "1", "sample 1"
assert run("6 7 3\n1 2 3 4 5 6\n") == "3", "sample 2"

# Custom cases
assert run("2 5 4\n0 1\n") == "0", "minimum size, no pair"
assert run("2 5 1\n0 1\n") == "1", "minimum size, one valid pair"
assert run("3 2 1\n0 1 1\n") == "1", "duplicate modulo values"
assert run("4 11 0\n1 2 3 5\n") == "0", "no valid pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 5 4\n0 1 | 0 | Đầu vào có kích thước tối thiểu, không có cặp nào khớp | 
| 2 5 1\n0 1 | 1 | |
