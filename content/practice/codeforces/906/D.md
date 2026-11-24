---
title: "CF 906D - Tháp Điện"
description: "Chúng ta được yêu cầu tính toán \"tháp điện\" modulo một số cho trước. Về mặt khái niệm, hãy tưởng tượng một chuỗi các khối đá, mỗi khối có một số nguyên dương."
date: "2026-06-12T23:21:08+07:00"
tags: ["codeforces", "competitive-programming", "chinese-remainder-theorem", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 906
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 454 (Div. 1, based on Technocup 2018 Elimination Round 4)"
rating: 2700
weight: 906
solve_time_s: 887
verified: true
draft: false
---

[CF 906D - Tháp điện](https://codeforces.com/problemset/problem/906/D) 

**Đánh giá:** 2700 
**Tas:** định lý số dư Trung Hoa, toán, lý thuyết số 
**Thời gian giải:** 14 phút 47 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tính modulo "tháp điện" của một số cho trước. Về mặt khái niệm, hãy tưởng tượng một chuỗi các khối đá, mỗi khối có một số nguyên dương. Để xây dựng tòa tháp từ những tảng đá được đánh số từ _l_ đến _r_, chúng ta bắt đầu từ tảng đá trên cùng _r_ và tiến dần xuống _l_, liên tục nâng sức mạnh của tảng đá hiện tại lên bằng sức mạnh của tòa tháp phía trên nó. Về mặt hình thức, nếu chúng ta có đá$w_l, w_{l+1}, \dots, w_r$, công suất tích lũy là$$w_l^{w_{l+1}^{\dots^{w_r}}} \mod m$$Đầu vào cho biết số lượng đá$n$, mô đun$m$, sức mạnh của từng tảng đá theo thứ tự, và$q$các truy vấn yêu cầu sức mạnh tích lũy của một nhóm nhỏ các loại đá. 

Những hạn chế cho chúng ta biết$n$Và$q$có thể đạt được$10^5$và công suất đá và mô đun có thể lên tới$10^9$. Một giải pháp ngây thơ đánh giá trực tiếp phép lũy thừa sẽ tạo ra những con số lớn về mặt thiên văn và không khả thi. Ngay cả số học có độ chính xác tùy ý của Python cũng sẽ quá chậm để thực hiện các phép tính lặp đi lặp lại.$10^5$truy vấn. Điều này ngụ ý rằng chúng ta cần một giải pháp tránh tính toán toàn bộ lũy thừa và thay vào đó sử dụng các thuộc tính của số học mô-đun. 

Các trường hợp cạnh không rõ ràng bao gồm khi$m = 1$, trong đó kết quả luôn là 0 và các chuỗi có lũy thừa là 1, có thể làm sập tháp nhanh chóng. Một trường hợp phức tạp khác phát sinh khi các lũy thừa liên tiếp rất lớn, chẳng hạn$2^{2^{30}}$, trong đó tính toán trực tiếp sẽ tràn cả số nguyên 64 bit. Việc xử lý những trường hợp này một cách chính xác đòi hỏi phải sử dụng cẩn thận phép rút gọn mô đun và định lý Euler. 

## Phương pháp tiếp cận 

Phương pháp brute-force lặp lại trên tháp từ trên xuống dưới, tính toán modulo lũy thừa lồng nhau$m$. Cụ thể, với mỗi truy vấn, người ta có thể tính toán:```
result = w_r
for i in range(r-1, l-1, -1):
    result = w_i ** result % m
```Điều này đúng về mặt toán học nhưng không khả thi trong thực tế. Ngay cả một truy vấn cũng có thể liên quan đến việc lũy thừa các số lớn tới 10^9^{10^9}, điều này rất tốn kém về mặt thiên văn. Với tối đa$10^5$các truy vấn, độ phức tạp sẽ tăng theo cấp số nhân theo độ dài của mảng con, điều này là không thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là phép lũy thừa mô-đun có tính chất đệ quy được điều chỉnh bởi định lý Euler. Nếu như$a$Và$m$là nguyên tố cùng nhau,$a^{\phi(m)} \equiv 1 \mod m$, Ở đâu$\phi$là hàm tổng Euler. Điều này cho phép chúng ta giảm modulo số mũ$\phi(m)$ở mỗi bước, giảm đáng kể kích thước của lũy thừa trung gian trong khi vẫn giữ được tính chính xác. Nếu như$a$Và$m$không nguyên tố cùng nhau, chúng ta phải xử lý cẩn thận việc rút gọn để tránh mất đi hành vi modulo chính xác, thường bằng cách giới hạn số mũ và cộng$\phi(m)$khi cần thiết. 

Điều này biến bài toán thành một phép tính đệ quy đi xuống tháp, giảm số mũ ở mỗi bước bằng cách sử dụng chuỗi tổng cho đến khi chạm đáy. Cách tiếp cận này hiệu quả ngay cả đối với các cường quốc lớn và$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * (r-l+1) * lũy thừa) | O(1) | Quá chậm | 
| Tối ưu | O(n log m + q log m) | Độ sâu đệ quy O(log m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước chuỗi tổng cho mô đun. Bắt đầu từ$m$, tính toán nhiều lần$\phi(m_i)$cho đến khi đạt 1. Điều này cho phép giảm số mũ ở mỗi cấp độ đệ quy. 
2. Định nghĩa hàm đệ quy`tower(l, r, mod)`tính toán đó$w_l^{w_{l+1}^{\dots^{w_r}}} \mod mod$. Nếu như`l == r`, trở lại$w_l \mod mod$. Ngược lại hãy tính`next_exp = tower(l+1, r, phi(mod))`. 
3. Sử dụng lũy ​​thừa mô đun với số mũ`next_exp`. Nếu số mũ vượt quá`phi(mod)`hoặc lớn hơn mức cần thiết do các tình huống không nguyên tố cùng nhau, hãy điều chỉnh nó bằng cách thêm`phi(mod)`để xử lý tràn một cách chính xác. 
4. Với mỗi truy vấn, hãy gọi`tower(l, r, m)`và xuất kết quả. 

Tại sao nó hoạt động: Việc giảm tổng lượng đảm bảo rằng đối với mỗi cấp độ, chúng tôi thay thế một số mũ có khả năng rất lớn bằng một modulo tương đương nhỏ hơn nhiều$\phi(m)$, trong khi định lý Euler đảm bảo kết quả theo modulo$m$vẫn đúng. Áp dụng đệ quy điều này xuống tháp sẽ duy trì tính bất biến về tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(200000)

def phi(n):
    result = n
    i = 2
    while i * i <= n:
        if n % i == 0:
            while n % i == 0:
                n //= i
            result -= result // i
        i += 1
    if n > 1:
        result -= result // n
    return result

def mod_pow(a, b, mod):
    result = 1
    a %= mod
    while b > 0:
        if b & 1:
            result = result * a % mod
        a = a * a % mod
        b >>= 1
    return result

def tower(l, r, mod):
    if mod == 1:
        return 0
    if l == r:
        return w[l] % mod
    next_exp = tower(l+1, r, phis[mod])
    if next_exp >= phis[mod]:
        next_exp = next_exp % phis[mod] + phis[mod]
    return mod_pow(w[l], next_exp, mod)

n, m = map(int, input().split())
w = [0] + list(map(int, input().split()))
phis = {}

def build_phi_chain(x):
    if x in phis:
        return phis[x]
    if x == 1:
        phis[x] = 1
    else:
        phis[x] = phi(x)
    return phis[x]

def prepare_phis(x):
    build_phi_chain(x)
    current = x
    while current != 1:
        current = phis[current]
        build_phi_chain(current)

prepare_phis(m)

q = int(input())
for _ in range(q):
    l, r = map(int, input().split())
    print(tower(l, r, m))
```Giải pháp này bắt đầu bằng cách tính toán trước chuỗi tổng cho mô-đun để cho phép giảm số mũ đệ quy hiệu quả.`mod_pow`thực hiện lũy thừa mô-đun nhanh. Đệ quy`tower`hàm tính modulo tháp điện`mod`, điều chỉnh số mũ nếu cần để tính các giá trị vượt quá tổng. Mỗi truy vấn gọi`tower`trực tiếp. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
6 1000000000
1 2 2 3 3 3
8
1 1
1 6
2 2
2 3
2 4
4 4
4 5
4 6
```| Truy vấn | tôi | r | Tháp tính toán | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 
| 2 | 1 | 6 | 1^{2^{2^{3^{3^3}}}} mod 1e9 | 1 | 
| 3 | 2 | 2 | 2 | 2 | 
| 4 | 2 | 3 | 2^2 = 4 | 4 | 
| 5 | 2 | 4 | 2^{2^3} = 2^8 = 256 | 256 | 
| 6 | 4 | 4 | 3 | 3 | 
| 7 | 4 | 5 | 3^3 = 27 | 27 | 
| 8 | 4 | 6 | 3^{3^3} mod 1e9 = 597484987 | 597484987 | 

Dấu vết này cho thấy chuỗi tổng và đệ quy giảm số mũ lớn thành các phép tính có thể quản lý được như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log m * log max w_i) | Mỗi truy vấn đi xuống tháp, giảm modulo tổng chuỗi, với lũy thừa mô-đun nhanh cho từng cấp độ | 
| Không gian | O(n + log m) | Mảng quyền hạn cộng với ngăn xếp đệ quy bị giới hạn bởi tổng độ dài chuỗi | 

Điều này phù hợp trong giới hạn, vì$log m \le 30$vì$m \le 10^9$, Và$q \le 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # Call the solution function
    exec(open('power_tower.py').read())
    return output.getvalue().strip()

# Provided sample
assert run("""6 1000000000
1 2 2 3 3 3
8
1 1
1 6
2 2
2 3
2 4
4 4
4 5
4 6""") == """1
1
2
4
256
3
27
597484987"""

# Custom cases
assert run("1
```
