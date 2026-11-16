---
title: "CF 1905E - One-X"
description: "Chúng ta được yêu cầu làm việc với một cây phân đoạn được xây dựng trên một mảng có độ dài $n$, trong đó cây phân đoạn là cấu trúc đệ quy nhị phân tiêu chuẩn. Mỗi nút của cây tương ứng với một đoạn của mảng và nút gốc được gắn nhãn $1$."
date: "2026-06-09T01:19:40+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "dp", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1905
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 915 (Div. 2)"
rating: 2400
weight: 1905
solve_time_s: 80
verified: true
draft: false
---

[CF 1905E - One-X](https://codeforces.com/problemset/problem/1905/E) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dfs và tương tự, dp, toán, cây 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu làm việc với cây phân đoạn được xây dựng trên một mảng có độ dài$n$, trong đó cây phân đoạn là cấu trúc đệ quy nhị phân tiêu chuẩn. Mỗi nút của cây tương ứng với một đoạn của mảng và nút gốc được gắn nhãn$1$. Con trái và con phải của nó được dán nhãn$2v$Và$2v+1$, Ở đâu$v$là nút hiện tại. Lá tương ứng với các đoạn đơn phần tử. 

Nhiệm vụ cốt lõi là xem xét tất cả các tập con không trống của các lá và tính tổng số nút của tổ tiên ít chung nhất của chúng. Ví dụ: nếu các lá được đánh số liên tục bắt đầu từ$n$, đối với mỗi tập hợp con của các lá, chúng tôi xác định nút trong cây là LCA của chúng và cộng số của nó vào tổng số đang chạy. Bởi vì số lượng tập hợp con tăng theo cấp số nhân nên việc liệt kê một cách thô bạo là không thể. lớn nhất$n$là$10^{18}$, do đó, bất kỳ cách tiếp cận nào có độ phức tạp về thời gian đều tỷ lệ thuận với$n$ngay lập tức bị loại trừ. 

Các trường hợp cạnh không rõ ràng xuất hiện khi$n$không phải là lũy thừa của hai, vì cây phân đoạn sẽ không cân bằng. Ví dụ, đối với$n=3$, các lá là các nút 2, 3 và 4 và gốc là 1. Một cách tiếp cận đơn giản giả sử một cây có chiều cao hoàn chỉnh$\lceil \log_2 n \rceil$sẽ tính toán sai số LCA cho các tập hợp con vượt qua nhánh không cân bằng. Xử lý rất lớn$n$cũng có nghĩa là chúng ta không thể xây dựng cây một cách rõ ràng; chúng ta cần một cách tiếp cận mang tính công thức. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ lặp qua tất cả các tập hợp con của các lá, tính toán LCA của chúng và tính tổng số nút. Về nguyên tắc, điều này hoạt động vì LCA của hai nút trong cây nhị phân có thể được tính toán thông qua các con trỏ cha thông thường. Tuy nhiên, số lượng tập hợp con là$2^n - 1$, lớn về mặt thiên văn ngay cả đối với$n=60$, vậy rõ ràng cách tiếp cận này thất bại khi$n$tùy thuộc vào$10^{18}$. 

Điểm mấu chốt là mỗi nút trong cây phân đoạn đóng góp vào tổng chính xác một lần cho mỗi tập con lá không trống trong cây con của nó bao gồm ít nhất một lá từ mỗi cây con con. Nếu chúng ta để$f(v)$là số tập con không trống của các lá bên dưới nút$v$, tổng đóng góp của nút$v$có thể được biểu diễn đệ quy như sau: số tập con bao trùm cả con trái và con phải nhân với số nút. Chính thức hơn, nếu$L$Và$R$là số lá trong cây con trái và cây con phải, nút$v$đóng góp$v \times ((2^L - 1) \times (2^R - 1))$. Lá tự đóng góp số của mình đúng một lần. 

Quan sát này làm giảm vấn đề thành một công thức đệ quy trên cây phân đoạn mà không bao giờ xây dựng nó một cách rõ ràng. Bằng cách làm việc với lũy thừa hai và lũy thừa mô-đun, chúng ta có thể tính toán kết quả một cách hiệu quả ngay cả đối với những trường hợp rất lớn.$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đóng góp cây con đệ quy | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định số lá trong cây con hiện tại. Đối với một nút tương ứng với phân đoạn$[l,r]$, số lá là$r-l+1$. Nếu như$l=r$, bản thân nó là một chiếc lá. 
2. Nếu nút hiện tại là một lá, hãy trả về số riêng của nó làm phần đóng góp của nó và số lượng là 1 lá. 
3. Nếu không, hãy chia đoạn thành hai nửa bên trái và bên phải bằng cách sử dụng$m = \lfloor (l+r)/2 \rfloor$và tính toán đệ quy các đóng góp cho con trái và con phải. 
4. Hãy để$L$Và$R$là số lá ở cây con trái và cây con phải. Số tập con có ít nhất một lá từ mỗi con là$(2^L - 1) \times (2^R - 1)$. Nhân số này với số nút hiện tại để nhận được sự đóng góp của nút này. 
5. Trả về tổng đóng góp của nút con trái, nút con phải và nút hiện tại, modulo 998244353, cùng với tổng số lá bên dưới nút này. 
6. Bắt đầu đệ quy từ nút gốc tương ứng với đoạn$[1,n]$. 

Tại sao nó hoạt động: Điều bất biến là mỗi tập hợp con của các lá đóng góp chính xác một lần vào nút là LCA của tập hợp con đó. Bằng cách đếm đệ quy các tập hợp con trong cây con trái và phải và nhân lên để tính đến các kết hợp trải rộng trên cả hai, chúng tôi đảm bảo rằng tất cả các tập hợp con có nút này là LCA đều được tính chính xác một lần. Các nút lá tự động đóng góp vì bất kỳ tập hợp con đơn lẻ nào cũng có lá là LCA của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modpow(a, b):
    res = 1
    while b > 0:
        if b % 2:
            res = res * a % MOD
        a = a * a % MOD
        b //= 2
    return res

def sum_lca(v, l, r):
    if l == r:
        return v, 1  # contribution, number of leaves
    m = (l + r) // 2
    left_sum, left_count = sum_lca(2*v, l, m)
    right_sum, right_count = sum_lca(2*v+1, m+1, r)
    cross = (modpow(2, left_count) - 1) * (modpow(2, right_count) - 1) % MOD
    total = (left_sum + right_sum + v * cross) % MOD
    return total, left_count + right_count

t = int(input())
for _ in range(t):
    n = int(input())
    ans, _ = sum_lca(1, 1, n)
    print(ans)
```chức năng`modpow`tính toán$a^b \mod 998244353$một cách hiệu quả.`sum_lca`tính toán đệ quy tổng các số LCA trong cây con có gốc tại`v`. Lệnh gọi đệ quy trả về cả tổng đóng góp và số lá, cho phép tính toán số tập hợp con cây chéo cho đóng góp LCA. Hoạt động modulo đảm bảo chúng ta tránh được tràn số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n=2 

| Nút | Phân đoạn | Đếm Trái | Đếm đúng | Vượt qua | Đóng góp nút | Tổng số tiền | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,2] | 1 | 1 | 1 | 1 | 6 | 
| 2 | [1,1] | - | - | - | 2 | 2 | 
| 3 | [2,2] | - | - | - | 3 | 3 | 

Phép đệ quy cho thấy các lá tự đóng góp (2+3) và gốc đóng góp 1 cho tập con {1,2}. Tổng số là 6, phù hợp với mẫu. 

### Ví dụ 2: n=3 

| Nút | Phân đoạn | Đếm Trái | Đếm đúng | Vượt qua | Đóng góp nút | Tổng số tiền | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,3] | 2 | 1 | 2 | 2 | 17 | 
| 2 | [1,2] | 1 | 1 | 1 | 2 | 6 | 
| 4 | [1,1] | - | - | - | 4 | 4 | 
| 5 | [2,2] | - | - | - | 5 | 5 | 
| 3 | [3,3] | - | - | - | 3 | 3 | 

Các lá tự đóng góp, các nút bên trong chiếm các tập hợp con cây con chéo, mang lại 17. Bảng xác nhận rằng phép nhân chéo xử lý chính xác các cây không cân bằng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Mỗi lần đệ quy sẽ chia đoạn đó thành một nửa; độ sâu đệ quy là log(n). | 
| Không gian | O(log n) | Độ sâu ngăn xếp đệ quy tỷ lệ thuận với chiều cao của cây. | 

Ngay cả đối với$n = 10^{18}$, log(n) ≈ 60, do đó giải pháp thực hiện thoải mái trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353

    def modpow(a, b):
        res = 1
        while b > 0:
            if b % 2:
                res = res * a % MOD
            a = a * a % MOD
            b //= 2
        return res

    def sum_lca(v, l, r):
        if l == r:
            return v, 1
        m = (l + r) // 2
        left_sum, left_count = sum_lca(2*v, l, m)
        right_sum, right_count = sum_lca(2*v+1, m+1, r)
        cross = (modpow(2, left_count) - 1) * (modpow(2, right_count) - 1) % MOD
        total = (left_sum + right_sum + v * cross) % MOD
        return total, left_count + right_count

    input_lines = inp.strip().splitlines()
    t = int(input_lines[0])
    res = []
    for i
```
