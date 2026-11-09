---
title: "CF 1475E - Cơ quan quảng cáo"
description: "Masha có một danh sách các blogger, mỗi blogger có số lượng người theo dõi nhất định và cô ấy muốn thuê chính xác k người trong số họ để tối đa hóa tổng lượng khán giả tiếp cận. Dữ liệu đầu vào cung cấp n, tổng số blogger, k, số lượng cô ấy có thể thuê và một mảng a có độ dài n với số lượng người theo dõi của mỗi blogger."
date: "2026-06-11T00:09:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1475
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 697 (Div. 3)"
rating: 1600
weight: 1475
solve_time_s: 106
verified: true
draft: false
---

[CF 1475E - Đại lý quảng cáo](https://codeforces.com/problemset/problem/1475/E) 

**Đánh giá:** 1600 
**Tags:** tổ hợp, toán, sắp xếp 
**Thời gian giải:** 1 phút 46 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Masha có một danh sách các blogger, mỗi blogger có số lượng người theo dõi nhất định và cô ấy muốn thuê chính xác`k`trong số đó để tối đa hóa tổng số khán giả tiếp cận. Đầu vào mang lại`n`, tổng số blogger,`k`, số lượng cô ấy có thể thuê, và một mảng`a`chiều dài`n`với số lượng người theo dõi của mỗi blogger. Với mỗi test case, kết quả đầu ra là số cách chọn`k`các blogger sao cho tổng số người theo dõi họ là cao nhất có thể. Hai lựa chọn được coi là khác nhau nếu chúng khác nhau bởi ít nhất một blogger. 

Với những hạn chế, với`n`lên tới 1000 và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng được giới hạn bởi 1000, chúng tôi biết rằng một giải pháp có độ phức tạp về thời gian khoảng`O(n log n)`mỗi trường hợp thử nghiệm sẽ dễ dàng phù hợp. Chúng ta cũng cần cẩn thận khi đếm tổ hợp vì`k`có thể lớn như`n`và việc đếm có thể được lặp lại, khiến cho việc đếm ngây thơ dễ mắc phải sai lầm. 

Một trường hợp phức tạp phát sinh khi nhiều blogger có cùng số lượng người theo dõi. Ví dụ: nếu tất cả các blogger đều có cùng người theo dõi và`k=2`, mọi cặp đều hợp lệ. Một cách tiếp cận ngây thơ chỉ cần thực hiện`k`số lớn nhất mà không xem xét tần số sẽ tính thiếu các khả năng. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả`n choose k`các tập hợp con, tính tổng số người theo dõi của chúng và đếm những người đạt được số tiền tối đa. Điều này có tác dụng vì tính chính xác được đảm bảo bằng cách kiểm tra tất cả các tập hợp con, nhưng nó trở nên không khả thi về mặt tính toán ngay cả đối với`n=30`vì số lượng tập hợp con tăng theo cấp số nhân. Với`n`lên tới 1000, điều này là không thể. 

Quan sát quan trọng là tổng số người theo dõi được tối đa hóa bằng cách lấy`k`các blogger có lượng người theo dõi lớn nhất. Sắp xếp mảng theo thứ tự giảm dần sẽ xác định ngay tổng số tối đa. Việc giảm độ phức tạp xuất phát từ việc nhận thấy rằng một khi lớn nhất`k`những người theo dõi đã biết thì số cách chọn họ chỉ phụ thuộc vào tần suất của giá trị nhỏ nhất trong số những người theo dõi đó.`k`người theo dõi trong toàn bộ mảng. Cụ thể, nếu`k`-số lượng người theo dõi lớn nhất thứ xuất hiện`c`lần trong toàn bộ mảng và`m`số lần ở trên cùng`k`, thì số cách chọn đỉnh`k`các blogger là`C(c, m)`, hệ số nhị thức. 

Việc này nhanh hơn đáng kể vì việc sắp xếp`O(n log n)`và tính toán`C(c, m)`có thể được thực hiện trong`O(k)`với modulo giai thừa được tính toán trước`10^9+7`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n chọn k * k) | O(k) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`Và`k`, sau đó đọc mảng theo dõi`a`. 
3. Sắp xếp`a`theo thứ tự giảm dần. đầu tiên`k`các phần tử bây giờ cho tổng số tiền tối đa có thể. 
4. Xác định`x`, số người theo dõi nhỏ nhất trong top`k`các blogger. 
5. Đếm`m`, số lần xuất hiện của`x`trong top`k`. 
6. Đếm`c`, tổng số lần xuất hiện của`x`trong toàn bộ mảng. 
7. Tính hệ số nhị thức`C(c, m)`modulo`10^9+7`để có được số lượng lựa chọn hợp lệ. 
8. In kết quả cho từng test. 

Thuật toán hoạt động vì tính bất biến là bất kỳ tập hợp tối ưu nào cũng phải bao gồm tất cả các phần tử lớn hơn`x`và chính xác`m`của các phần tử bằng`x`. Điều này đảm bảo rằng tổng là tối đa và đếm chính xác mọi kết hợp riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 10**9 + 7

# precompute factorials and inverse factorials
N = 1000
fact = [1] * (N+1)
invfact = [1] * (N+1)

for i in range(2, N+1):
    fact[i] = fact[i-1] * i % MOD

def modinv(x):
    return pow(x, MOD-2, MOD)

for i in range(2, N+1):
    invfact[i] = modinv(fact[i])

def comb(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * invfact[k] % MOD * invfact[n-k] % MOD

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    a.sort(reverse=True)
    x = a[k-1]
    m = a[:k].count(x)
    c = a.count(x)
    print(comb(c, m))
```Việc tính toán trước các giai thừa và nghịch đảo mô-đun cho phép chúng ta tính toán bất kỳ hệ số nhị thức nào một cách hiệu quả. Sắp xếp đảm bảo chúng tôi chọn các phần tử lớn nhất. Đếm số lần xuất hiện của phần tử nhỏ nhất ở trên cùng`k`và tổng đảm bảo tính toán tổ hợp chính xác. Một lỗi phổ biến là đếm sai`m`hoặc`c`hoặc quên phép tính modulo. 

## Ví dụ đã hoạt động 

Mẫu 1:```
n=4, k=3, a=[1,3,1,2]
```| Bước | Mảng a | Đầu k | x | m | c | lược(c,m) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,3,1,2] | [3,2,1] | 1 | 1 | 2 | 2 | 

Điều này cho thấy phần tử nhỏ nhất trong top 3 là 1. Nó xuất hiện một lần trong top 3 và hai lần trong tổng thể. Có 2 cách chọn một`1`trong số hai có sẵn, phù hợp với sản lượng dự kiến. 

Mẫu 2:```
n=4, k=2, a=[1,1,1,1]
```| Bước | Mảng a | Đầu k | x | m | c | lược(c,m) | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | [1,1,1,1] | [1,1] | 1 | 2 | 4 | 6 | 

Tất cả các giá trị đều giống nhau. Có 4 chọn 2 cách chọn ra 2 cao nhất, được 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) cho mỗi trường hợp thử nghiệm | Việc sắp xếp chiếm ưu thế, số lần đếm là O(n) | 
| Không gian | O(n) | Lưu trữ mảng và giai thừa lên tới 1000 | 

Giải pháp dễ dàng phù hợp trong vòng 2 giây và 256 MB vì`n`và tổng của tất cả các trường hợp thử nghiệm là 1000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    sys.stdin = io.StringIO(inp)
    MOD = 10**9 + 7
    N = 1000
    fact = [1]*(N+1)
    invfact = [1]*(N+1)
    for i in range(2,N+1):
        fact[i] = fact[i-1]*i % MOD
    def modinv(x):
        return pow(x,MOD-2,MOD)
    for i in range(2,N+1):
        invfact[i] = modinv(fact[i])
    def comb(n,k):
        if k<0 or k>n: return 0
        return fact[n]*invfact[k]%MOD*invfact[n-k]%MOD
    t=int(input())
    res=[]
    for _ in range(t):
        n,k=map(int,input().split())
        a=list(map(int,input().split()))
        a.sort(reverse=True)
        x=a[k-1]
        m=a[:k].count(x)
        c=a.count(x)
        res.append(str(comb(c,m)))
    return "\n".join(res)

# Provided samples
assert run("3\n4 3\n1 3 1 2\n4 2\n1 1 1 1\n2 1\n1 2\n") == "2\n6\n1", "sample 1"

# Custom cases
assert run("1\n5 3\n5 5 5 5 5\n") == "10", "all equal"
assert run("1\n3 2\n1 2 3\n") == "1", "simple increasing"
assert run("1\n6 3\n1 2 2 3 3 3\n") == "1", "multiple repeats, largest freq"
assert run("1\n6 3\n3 3 2 2 1 1\n") == "1", "sorted descending with ties"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 3 5 5 5 5 5 | 10 | Tất cả các giá trị bằng nhau, nhiều kết hợp | 
| 3 2 1 2 3 | 1 | Tăng đơn giản, các phần tử lớn nhất độc đáo | 
| 6 3 1 2 2 3 3 3 | 1 | Nhiều lần lặp lại, chọn trong số tối đa thường xuyên nhất | 
| 6 3 3 3 2 2 1 1 | 1 | Sắp xếp giảm dần, mối quan hệ được xử lý chính xác | 

## Vỏ cạnh 

Ví dụ: nếu tất cả các blogger đều có cùng một người theo dõi`a=[1,1,1,1]`Và`k=2`, thuật toán xác định chính xác `x
