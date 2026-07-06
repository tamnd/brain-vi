---
title: "CF 102944F - Đá lửa"
description: "Chúng ta được cho một tập hợp nhỏ các số nguyên dương. Từ bộ sưu tập này, chúng tôi xem xét mọi tập hợp con không trống có thể có và tính ước số chung lớn nhất của các số bên trong tập hợp con đó. Một tập hợp con được coi là “hợp lệ” nếu gcd này bằng chính xác 1."
date: "2026-07-04T07:36:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102944
codeforces_index: "F"
codeforces_contest_name: "UMPT 2020-2021 Team Tryout Contest"
rating: 0
weight: 102944
solve_time_s: 40
verified: true
draft: false
---

[CF 102944F - Flint](https://codeforces.com/problemset/problem/102944/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp nhỏ các số nguyên dương. Từ bộ sưu tập này, chúng tôi xem xét mọi tập hợp con không trống có thể có và tính ước số chung lớn nhất của các số bên trong tập hợp con đó. Một tập hợp con được coi là "hợp lệ" nếu gcd này bằng chính xác 1. Nhiệm vụ là đếm xem có bao nhiêu tập hợp con hợp lệ như vậy tồn tại. 

Kích thước đầu vào nhỏ, tối đa là 100 số, nhưng mỗi số có thể lớn tới 10^9. Ràng buộc kích thước là tín hiệu chính: về nguyên tắc có thể liệt kê tất cả các tập hợp con vì 2^100 là quá lớn, nhưng 100 đủ nhỏ để cho phép các phương pháp hàm mũ có nén hoặc loại trừ bao gồm các giá trị xuất phát từ các số, đặc biệt là các ước số. 

Một cách giải thích ngây thơ sẽ cố gắng lặp lại tất cả các tập hợp con và tính toán trực tiếp gcd. Điều đó đã gợi ý khó khăn cốt lõi: không gian tập hợp con là theo cấp số nhân, nhưng hoạt động gcd có cấu trúc có thể được khai thác. 

Trường hợp cạnh tinh vi xuất hiện khi tất cả các số có chung một thừa số lớn hơn 1. Ví dụ: nếu tất cả các số là số chẵn thì mọi tập hợp con đều có gcd ít nhất là 2, vì vậy câu trả lời phải là 0. Bất kỳ cách tiếp cận nào không thực thi đúng cách hành vi thu gọn gcd sẽ đếm không chính xác các tập hợp con không bao giờ thực sự đạt tới 1. 

Một trường hợp góc khác xảy ra khi mảng chứa một phần tử duy nhất bằng 1. Tập hợp con duy nhất là {1}, có gcd là 1, vì vậy câu trả lời là 1. Đây là kiểm tra độ chính xác tối thiểu mà bất kỳ phương thức nào cũng phải đáp ứng. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi lặp lại mọi tập hợp con không trống của mảng, tính gcd của nó bằng cách gấp qua các phần tử của nó và tăng câu trả lời nếu kết quả bằng 1. Điều này đúng vì nó tuân theo định nghĩa của vấn đề. 

Tuy nhiên, số lượng tập hợp con là 2^N. Với N = 100, đây là khoảng 10^30 tập hợp con, vượt xa mọi giới hạn tính toán. Ngay cả khi tính toán gcd nhanh, số lượng tập hợp con khổng lồ khiến phương pháp này không thể thực hiện được. 

Quan sát chính là cấu trúc gcd tương tác tốt với các ước số hơn là các tập hợp con. Thay vì nhóm các tập hợp con theo giá trị gcd của chúng sau khi xây dựng, chúng ta đảo ngược quan điểm: cố định một giá trị d và đếm xem có bao nhiêu tập hợp con có tất cả các phần tử chia hết cho d. Bất kỳ tập hợp con nào như vậy đều có gcd chia hết cho d và nếu chúng ta tính đến việc đếm thừa một cách cẩn thận, chúng ta có thể khôi phục số lượng chính xác các tập hợp con có gcd chính xác bằng d. Đây là một ý tưởng bao gồm loại trừ ước số tiêu chuẩn, nhưng ở đây nó trở nên đặc biệt rõ ràng vì N nhỏ và giá trị lớn. 

Chúng ta tiến hành đếm, với mỗi số nguyên d, có bao nhiêu phần tử mảng chia hết cho d. Nếu k phần tử chia hết cho d thì có 2^k - 1 tập con không trống chỉ bao gồm bội số của d. Đặt f(d) biểu thị số đếm này. Tuy nhiên, f(d) đếm tất cả các tập con có gcd là bội số của d, không chính xác bằng d. Sau đó, chúng tôi trừ phần đóng góp của bội số của d bằng cách sử dụng sàng giảm dần trên các ước số, tạo ra số lượng chính xác các tập hợp con có gcd chính xác là d. 

Cuối cùng, chúng ta tính tổng giá trị tương ứng với d = 1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N · 2^N) | O(1) | Quá chậm | 
| Số chia DP + Bao gồm-Loại trừ | O(N √A + D log D) | O(D) | Đã chấp nhận | 

Ở đây D là số ước số riêng biệt xuất hiện giữa các giá trị đầu vào, số này nhỏ vì chúng tôi chỉ tạo ước số của mỗi số. 

## Hướng dẫn thuật toán 

Chúng tôi tránh liệt kê toàn bộ các tập hợp con và thay vào đó tính theo các lớp chia.

1. Với mỗi số trong mảng, hãy liệt kê tất cả các ước của nó. Với mỗi ước số d, hãy tăng bộ đếm tần số cnt[d] lên 1. Bước này xây dựng ánh xạ từ một ước số tới số phần tử mảng có thể chia hết cho nó. Lý do điều này có tác dụng là vì bất kỳ tập hợp con nào có các phần tử chia hết cho d chỉ được hình thành từ các phần tử cnt[d] này. 
2. Với mỗi ước số d xuất hiện trong cnt, hãy tính số tập con không trống được hình thành từ các phần tử đó, đó là f(d) = 2^{cnt[d]} - 1. Điều này thể hiện tất cả các tập hợp con có các phần tử đều là bội số của d, nghĩa là gcd của chúng ít nhất là d. 
3. Sắp xếp tất cả các ước số theo thứ tự giảm dần. Chúng tôi muốn loại bỏ việc đếm thừa khỏi bội số. Nếu một tập hợp con có gcd chính xác bằng bội số lớn hơn của d thì nó đã được đưa vào f(d) không chính xác, vì vậy chúng tôi trừ đi những đóng góp đó. 
4. Xử lý các ước số theo thứ tự giảm dần. Duy trì một mảng g[d] ban đầu được đặt thành f(d). Với mỗi d, lặp lại tất cả các bội số m = 2d, 3d, ... và trừ g[m] từ g[d]. Điều này đảm bảo rằng sau khi xử lý, g[d] đại diện cho các tập con có gcd chính xác là d. 
5. Đáp án cuối cùng là g[1]. 

### Tại sao nó hoạt động 

Mỗi tập hợp con không trống có một giá trị gcd duy nhất. Với mọi d cố định, f(d) tính tất cả các tập con có gcd là bội số của d. Các tập con này được phân chia theo các giá trị gcd chính xác của chúng, tất cả đều là bội số của d. Bước trừ thực chất là phép đảo ngược Möbius trên mạng chia, đảm bảo mỗi tập hợp con được gán chính xác một lần cho giá trị gcd thực của nó. Vì quan hệ số chia tạo thành một thứ tự từng phần nên việc xử lý từ lớn đến nhỏ đảm bảo rằng khi chúng ta trừ đi, tất cả các phần đóng góp lớn hơn đều đã được hoàn tất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def get_divisors(x):
    divs = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            divs.append(i)
            if i * i != x:
                divs.append(x // i)
        i += 1
    return divs

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = (res * a) % MOD
        a = (a * a) % MOD
        e >>= 1
    return res

n = int(input())
a = list(map(int, input().split()))

cnt = {}
for x in a:
    for d in get_divisors(x):
        cnt[d] = cnt.get(d, 0) + 1

vals = sorted(cnt.keys(), reverse=True)

f = {}
g = {}

for d in vals:
    c = cnt[d]
    f[d] = (modpow(2, c) - 1) % MOD
    g[d] = f[d]

for d in vals:
    for m in vals:
        if m > d and m % d == 0:
            g[d] = (g[d] - g[m]) % MOD

print(g.get(1, 0) % MOD)
```Giải pháp bắt đầu bằng cách xây dựng tần số chia. Đối với mỗi số, chúng tôi liệt kê các ước của nó theo dạng O(√aᵢ), đủ cho aᵢ lên đến 10^9. Đây là bước duy nhất chạm trực tiếp vào các giá trị đầu vào thô. 

Tiếp theo, chúng tôi tính 2^k - 1 cho mỗi số chia. Điều này đại diện cho tất cả các tập hợp con chỉ bao gồm các số chia hết cho ước số đó. Việc lũy thừa mô đun là cần thiết vì k có thể lên tới 100. 

Cuối cùng, chúng tôi sửa lỗi đếm quá mức bằng cách trừ đi phần đóng góp của các giá trị gcd lớn hơn. Vòng lặp lồng nhau trên các ước số có thể chấp nhận được vì số lượng ước số riêng biệt trên tất cả các số vẫn còn nhỏ trong thực tế dưới những ràng buộc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
2 3 5
```Mọi số đều là số nguyên tố và phân biệt, vì vậy chỉ các tập hợp con đơn lẻ mới quan trọng đối với gcd 1. 

| Bước | cnt | f(d) | g(d) | 
| --- | --- | --- | --- | 
| d=5 | 1 | 1 | 1 | 
| d=3 | 1 | 1 | 1 | 
| d=2 | 1 | 1 | 1 | 
| d=1 | 3 | 7 | 4 | 

Giá trị cuối cùng g(1) = 4 tương ứng với tất cả các tập hợp con ngoại trừ những tập hợp con có gcd không bằng 1. Tất cả các tập hợp con hợp lệ đều ngoại trừ tập hợp trống và bất kỳ tập hợp con nào có gcd >1. 

Dấu vết này cho thấy cách loại trừ bao gồm phân tách các lớp gcd ngay cả khi tất cả các số đều là cặp số nguyên tố cùng nhau. 

### Ví dụ 2 

đầu vào:```
3
2 4 8
```Tất cả các số đều là lũy thừa của 2 nên không tập hợp con nào có thể có gcd 1. 

| Bước | cnt | f(d) | g(d) | 
| --- | --- | --- | --- | 
| d=8 | 1 | 1 | 1 | 
| d=4 | 2 | 3 | 2 | 
| d=2 | 3 | 7 | 4 | 
| d=1 | 3 | 7 | 0 | 

Ở đây, mọi tập hợp con đều được tính dưới một lớp gcd lũy thừa 2 nào đó và mọi thứ đều bị loại bỏ ở d = 1, xác nhận rằng không có tập hợp con nào đạt đến gcd 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N √A + D2) | phép liệt kê số chia cộng với hiệu chỉnh bội số chia | 
| Không gian | O(D) | lưu trữ số chia và bảng DP | 

Các ràng buộc N ≤ 100 giữ cho việc liệt kê ước số là tầm thường và D vẫn có thể quản lý được vì mỗi số đóng góp tối đa khoảng 100 ước số. Điều này làm cho giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    def get_divisors(x):
        divs = []
        i = 1
        while i * i <= x:
            if x % i == 0:
                divs.append(i)
                if i * i != x:
                    divs.append(x // i)
            i += 1
        return divs

    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = (res * a) % MOD
            a = (a * a) % MOD
            e >>= 1
        return res

    n = int(input())
    a = list(map(int, input().split()))

    cnt = {}
    for x in a:
        for d in get_divisors(x):
            cnt[d] = cnt.get(d, 0) + 1

    vals = sorted(cnt.keys(), reverse=True)

    f = {}
    g = {}

    for d in vals:
        f[d] = (modpow(2, cnt[d]) - 1) % MOD
        g[d] = f[d]

    for d in vals:
        for m in vals:
            if m > d and m % d == 0:
                g[d] = (g[d] - g[m]) % MOD

    return str(g.get(1, 0) % MOD)

# sample-like
assert run("3\n2 3 5\n") == "4"

# single element 1
assert run("1\n1\n") == "1"

# all same even number
assert run("3\n2 2 2\n") == "0"

# powers of two
assert run("3\n2 4 8\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 2 3 5 | 4 | tính chính xác bao gồm-loại trừ cơ bản | 
| 1 1 | 1 | trường hợp cạnh tối thiểu | 
| 3 2 2 2 | 0 | gcd được chia sẻ > 1 loại bỏ các tập hợp con hợp lệ | 
| 3 2 4 8 | 0 | không có tập hợp con nào có thể đạt tới gcd 1 | 

## Vỏ cạnh 

Khi tất cả các giá trị có chung một ước số lớn hơn 1 thì gcd của mỗi tập hợp con ít nhất cũng bằng ước số đó. Đối với đầu vào`2 2 2`, số lượng ước số tạo ra f(2)=7 và f(1)=7, nhưng phép trừ sẽ loại bỏ tất cả các đóng góp tại d=1, để lại 0 tập hợp con hợp lệ. Thuật toán thu gọn chính xác mọi thứ thành các lớp gcd cao hơn trước khi đạt tới 1. 

Khi mảng chứa một phần tử bằng 1, cnt[1]=1 và f(1)=1. Không có ước số lớn hơn nào tồn tại để trừ nó, do đó g(1)=1 trực tiếp. Thuật toán xử lý việc này mà không cần bất kỳ cách viết hoa đặc biệt nào vì phép liệt kê số chia tự nhiên bao gồm 1 cho mỗi số.
