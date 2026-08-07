---
title: "CF 102535R - Cấp 3 duy nhất"
description: "Chúng ta cần đếm các cặp số (k, b) trong đó k là số nhân và b là cơ số. Một cặp được coi là hợp lệ khi nhân k với mọi vị trí chữ số từ 0 đến b-1 tạo ra các nghiệm số chứa mọi chữ số có thể có của cơ số đó đúng một lần."
date: "2026-08-06T20:04:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102535
codeforces_index: "R"
codeforces_contest_name: "2020 UP ACM Algolympics Elimination Round"
rating: 0
weight: 102535
solve_time_s: 206
verified: true
draft: false
---

[CF 102535R - Cấp 3 duy nhất](https://codeforces.com/problemset/problem/102535/R) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm các cặp số`(k, b)`Ở đâu`k`là một số nhân và`b`là một cơ sở. Một cặp được coi là hợp lệ khi nhân`k`theo từng vị trí chữ số từ`0`ĐẾN`b-1`tạo ra các gốc số chứa mọi chữ số có thể có của cơ số đó đúng một lần. 

Đầu vào cho hệ số nhân lớn nhất được phép`k'`và cơ sở cho phép lớn nhất`b'`. Câu trả lời là số cặp hợp lệ với`1 <= k <= k'`Và`2 <= b <= b'`, lấy modulo`10^6`. 

Các giới hạn vượt xa mọi thứ cho phép lặp lại trên tất cả các cặp. Có thể có tới`5 * 10^9`các giá trị có thể có cho cả hai biến, vì vậy việc kiểm tra mọi cơ số và mọi số nhân sẽ cần khoảng`2.5 * 10^19`hoạt động. Lời giải phải đưa bài toán về số học trên các ước số và sau đó khai thác thực tế là các giá trị chia sàn chỉ thay đổi một số lần nhỏ. 

Trường hợp cạnh ẩn đầu tiên là cơ sở`2`. Gốc kỹ thuật số chỉ chứa các chữ số`0`Và`1`và mọi số dương đều có nghiệm số`1`trong căn cứ`2`. Một công thức giả định một mô đun của`b-1`lớn hơn một người có thể phá vỡ ở đây. Ví dụ, đầu vào`1 2`có câu trả lời`1`, bởi vì cặp duy nhất`(1,2)`là hợp lệ. 

Một trường hợp cạnh khác là khi`k`Và`b-1`không phải là nguyên tố cùng nhau. Mô phỏng trực tiếp có thể vô tình vượt qua các ví dụ nhỏ vì chỉ một số sản phẩm được kiểm tra. Ví dụ, đầu vào`2 3`có câu trả lời`3`. Đối với cơ sở`3`, chúng ta cần các giá trị cho`0, k, 2k`trở thành`0,1,2`. Với`k=2`, các giá trị không phải là một hoán vị, bởi vì`2*2`có gốc kỹ thuật số giống như`1*2`. 

Trường hợp cạnh cuối cùng là ranh giới lớn. đầu vào`5000000000 5000000000`không thể tiếp cận bằng cách lưu trữ các mảng có kích thước này hoặc lặp qua mọi cơ sở. Thuật toán chỉ được phép hoạt động với các nhóm giá trị có kích thước bằng căn bậc hai. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lặp lại trên mọi cơ sở`b`, thì mọi`k`và mô phỏng nguồn gốc kỹ thuật số của`0, k, 2k, ..., (b-1)k`. Mô phỏng là đúng vì nó trực tiếp kiểm tra định nghĩa. Tuy nhiên, nó không thể sử dụng được: chỉ riêng số lượng cặp có thể đạt tới`25 * 10^18`. 

Quan sát quan trọng đến từ công thức gốc số. Đối với số nguyên dương`x`trong căn cứ`b`:`f_b(x) = 1 + ((x - 1) mod (b - 1))`. 

Cho phép`m = b - 1`. Các giá trị cho`k, 2k, ..., mk`trở thành một hoán vị của`1, 2, ..., m`chính xác khi nhân với`k`hoán vị mọi dư lượng modulo`m`. Điều đó xảy ra chính xác khi`gcd(k, m) = 1`. 

Vấn đề ban đầu bây giờ là vấn đề đếm số chia. Chúng tôi cần:`sum over m = 1 to b'-1 of count of k <= k' with gcd(k,m)=1`. 

Sử dụng hàm Mobius:`count(k <= K, gcd(k,m)=1) = sum over d|m of mu(d) * floor(K/d)`. 

Hoán đổi các tổng cho:`answer = sum over d <= min(K, B-1) of mu(d) * floor(K/d) * floor((B-1)/d)`. 

Thách thức còn lại là đánh giá điều này một cách nhanh chóng. Các giá trị sàn không đổi trong khoảng thời gian. Chúng ta có thể chuyển đổi giữa các khoảng đó và chỉ cần tổng tiền tố của hàm Möbius. Các giá trị tiền tố lớn của hàm Möbius thu được bằng đệ quy kiểu Du Jiao. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k' * b' * b') | O(1) | Quá chậm | 
| Tối ưu | O(N^(2/3)) xấp xỉ | O(N^(1/2)) | Đã chấp nhận | 

Đây`N = min(k', b'-1)`. 

## Hướng dẫn thuật toán 

1. Hãy để`n = b' - 1`. Chuyển đổi câu trả lời thành tổng Möbius:`sum(mu(d) * floor(k'/d) * floor(n/d))`. 

Việc chuyển đổi loại bỏ nhu cầu kiểm tra từng cặp riêng lẻ. 
2. Tính hàm Mertens`M(x) = mu(1) + ... + mu(x)`cho các giá trị cần thiết cho việc phân chia tầng. 

Các giá trị nhỏ được tính toán trước bằng sàng tuyến tính. Các giá trị lớn hơn được tính toán đệ quy bằng cách sử dụng:`M(x) = 1 - sum(M(x / i))`cho tất cả các phạm vi nơi`x / i`là không đổi. 
3. Lặp lại biến số chia`d`trong các khối. Đối với một dòng điện`d`, tính giá trị lớn nhất`r`như vậy:`k'/d`Và`n/d`không đổi với mọi giá trị trong`[d, r]`. 

Phần đóng góp của cả khối là:`(M(r) - M(d-1)) * floor(k'/d) * floor(n/d)`. 
4. Thêm mỗi modulo đóng góp khối`10^6`. 

Bất biến đằng sau thuật toán là mọi ước số`d`đóng góp chính xác`mu(d) * floor(k'/d) * floor(n/d)`. Việc nhóm các phân chia tầng bằng nhau chỉ thay đổi thứ tự phép cộng chứ không bao giờ thay đổi giá trị. Hàm Mertens đưa ra tổng của tất cả các giá trị Möbius bên trong một khối, do đó mỗi ước số được bao gồm chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**6
LIM = 2000000

mu = [1] * (LIM + 1)
prime = []
vis = [False] * (LIM + 1)
mu[0] = 0

for i in range(2, LIM + 1):
    if not vis[i]:
        prime.append(i)
        mu[i] = -1
    for p in prime:
        if i * p > LIM:
            break
        vis[i * p] = True
        if i % p == 0:
            mu[i * p] = 0
            break
        mu[i * p] = -mu[i]

pref = [0] * (LIM + 1)
for i in range(1, LIM + 1):
    pref[i] = pref[i - 1] + mu[i]

cache = {}

def mertens(n):
    if n <= LIM:
        return pref[n]
    if n in cache:
        return cache[n]
    res = 1
    i = 2
    while i <= n:
        q = n // i
        j = n // q
        res -= (j - i + 1) * mertens(q)
        i = j + 1
    cache[n] = res
    return res

def solve():
    k, b = map(int, input().split())
    n = b - 1
    limit = min(k, n)

    ans = 0
    d = 1
    while d <= limit:
        qk = k // d
        qn = n // d
        r = min(k // qk, n // qn)
        mob = mertens(r) - mertens(d - 1)
        ans = (ans + mob * qk * qn) % MOD
        d = r + 1

    print(ans % MOD)

solve()
```Sàng chỉ tính toán các giá trị Möbius tối đa`LIM`, bởi vì tất cả các yêu cầu lớn hơn đều được xử lý thông qua hàm Mertens đệ quy. Phép đệ quy lưu trữ kết quả cho các đối số chia tầng lặp đi lặp lại, đó là lý do khiến nó luôn nhanh. 

Vòng lặp chính không bao giờ tiến lên một ước số tại một thời điểm trên toàn bộ phạm vi. Khi`k//d`hoặc`(b-1)//d`là hằng số, tất cả các ước số trong khoảng đó có cùng hệ số nhân, do đó toàn bộ khoảng được xử lý cùng nhau. 

Số nguyên Python tránh tràn, nhưng câu trả lời là giảm modulo`10^6`sau mỗi lần đóng góp khối. Việc sử dụng`min(k, b-1)`cũng tránh được những công việc không cần thiết vì các ước số lớn hơn không đóng góp gì. 

## Ví dụ đã hoạt động 

Đối với đầu vào:```
3 5
```chúng tôi có`k'=3`Và`b'-1=4`. 

| phạm vi d | M(r)-M(d-1) | tầng(3/d) | tầng(4/ngày) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 3 | 4 | 12 | 
| 2 | -1 | 1 | 2 | -2 | 
| 3 | -1 | 1 | 1 | -1 | 
| 4 | 0 | 0 | 1 | 0 | 

Tổng số tiền là`9`, phù hợp với mẫu 

Đối với đầu vào:```
2 3
```chúng tôi có căn cứ`2`Và`3`. 

| d | mu(d) | tầng(2/d) | tầng(2/d) | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 4 | 
| 2 | -1 | 1 | 1 | -1 | 

Câu trả lời là`3`. Trường hợp này chứng tỏ rằng không phải mọi số nhân đều có tác dụng với mọi cơ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | Khoảng O(N^(2/3)) | Sàng xử lý các giá trị nhỏ và giới hạn nhóm phân chia sàn trạng thái đệ quy | 
| Không gian | O(N^(1/2)) | Các giá trị Möbius được lưu trữ và các truy vấn Mertens được ghi nhớ | 

Đầu vào lớn nhất chỉ tạo ra khoảng vài trăm nghìn trạng thái phân chia tầng riêng biệt, do đó thuật toán vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out.strip()

assert run("3 5\n") == "9", "sample 1"
assert run("1 2\n") == "1", "minimum values"
assert run("2 3\n") == "3", "small non-coprime case"
assert run("5 2\n") == "5", "only base two"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 5`|`9`| Cung cấp mẫu và đếm chung | 
|`1 2`|`1`| Cơ số và số nhân nhỏ nhất có thể | 
|`2 3`|`3`| Điều kiện đồng nguyên | 
|`5 2`|`5`| Xử lý`b-1 = 1`| 

## Vỏ cạnh 

cho`1 2`, bộ thuật toán`n = 1`. Tổng hợp chỉ chứa`d = 1`, với`mu(1)=1`,`floor(1/1)=1`, Và`floor(1/1)=1`, đưa ra câu trả lời`1`. 

Vì`2 3`, thuật toán xem xét`n = 2`. Việc mở rộng Mobius mang lại:`floor(2/1)*floor(2/1) - floor(2/2)*floor(2/2) = 4 - 1 = 3`. 

Điều này đếm các cặp hợp lệ`(1,2)`,`(2,2)`, Và`(1,3)`trong khi từ chối`(2,3)`. 

Đối với các giá trị tối đa, vòng chia số không bao giờ đạt tới hàng tỷ lần lặp. Nó nhảy giữa các phạm vi trong đó cả hai phân chia tầng đều bằng nhau và các truy vấn Mertens lớn được sử dụng lại thông qua quá trình ghi nhớ. Điều này giữ cho việc thực thi không phụ thuộc vào kích thước thô của các giá trị đầu vào.
