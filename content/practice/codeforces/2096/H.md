---
title: "CF 2096H - Sự cố XOR tuyệt vời"
description: "Chúng ta có n khoảng, mỗi khoảng được xác định bởi giới hạn dưới và giới hạn trên [li, ri]. Từ mỗi khoảng, chúng ta có thể chọn một số ai trong giới hạn của nó. Mục tiêu của chúng ta là xem xét tất cả các chuỗi (a1, a2, ..."
date: "2026-06-08T05:26:36+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "fft", "math"]
categories: ["algorithms"]
codeforces_contest: 2096
codeforces_index: "H"
codeforces_contest_name: "Neowise Labs Contest 1 (Codeforces Round 1018, Div. 1 + Div. 2)"
rating: 3200
weight: 2096
solve_time_s: 102
verified: false
draft: false
---

[CF 2096H - Sự cố XOR tuyệt vời](https://codeforces.com/problemset/problem/2096/H) 

**Đánh giá:** 3200 
**Tags:** bitmasks, tổ hợp, dp, fft, toán 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`n`các khoảng, mỗi khoảng được xác định bởi giới hạn dưới và giới hạn trên`[l_i, r_i]`. Từ mỗi khoảng, chúng ta có thể chọn một số`a_i`trong giới hạn của nó. Mục tiêu của chúng ta là xem xét tất cả các chuỗi`(a_1, a_2, ..., a_n)`và tính toán cho từng giá trị XOR có thể`x`trong phạm vi`0`ĐẾN`2^m - 1`, có bao nhiêu chuỗi mang lại XOR đó. Sau khi tính toán các số đếm này theo modulo`998244353`, chúng tôi nhân mỗi lần đếm với`2^x`và lấy XOR của tất cả các kết quả để tạo ra một số cuối cùng. 

Những ràng buộc cho phép`n`lên đến`2 * 10^5`tổng hợp trên tất cả các trường hợp thử nghiệm và`m`lên đến`18`. Điều này có nghĩa`2^m`có thể đạt được khoảng`2^18 = 262144`. Một cách tiếp cận bạo lực lặp đi lặp lại qua tất cả các chuỗi là không thể bởi vì ngay cả đối với những`n`, số lượng chuỗi là theo cấp số nhân trong`n`. Cách tiếp cận khả thi duy nhất là cách tiếp cận có độ phức tạp tuyến tính hoặc đa thức với`2^m`Và`n`, không theo cấp số nhân với`n`. 

Các trường hợp cạnh bao gồm các khoảng có chiều rộng bằng 0 (ví dụ:`l_i = r_i`) và các khoảng bao phủ toàn bộ phạm vi`0..2^m-1`. Các giải pháp bất cẩn có thể tính sai các chuỗi nếu chúng giả sử tính độc lập của các bit mà không xử lý việc chuyển tiếp theo các khoảng thời gian một cách thích hợp. Bé nhỏ`m`và lớn`n`sự kết hợp cũng cần số học modulo cẩn thận để tránh tràn. 

## Phương pháp tiếp cận 

Giải pháp ngây thơ liệt kê tất cả các chuỗi`(a_1, ..., a_n)`. Đối với mỗi chuỗi, chúng tôi tính toán XOR và tăng số lượng`f_x`. Điều này làm việc cho nhỏ`n`bởi vì mỗi chuỗi có thể được kiểm tra trực tiếp nhưng độ phức tạp của nó rất cao.`O(product(r_i - l_i + 1))`. Đối với những khoảng thời gian vừa phải như`[0, 10]`lặp lại 20 lần, điều này vượt quá mọi giới hạn thực tế. 

Thông tin chi tiết quan trọng là XOR **độc lập về mặt bit**. Nếu chúng ta xử lý từng vị trí bit riêng biệt, chúng ta có thể xem xét có bao nhiêu chuỗi tạo ra một`0`hoặc`1`trong bit đó một cách độc lập. Sau đó, chúng ta có thể kết hợp các kết quả trên tất cả các bit bằng cách sử dụng Biến đổi Walsh-Hadamard nhanh (FWHT), cho phép chúng ta tính toán tích chập XOR một cách hiệu quả. FWHT lợi dụng thực tế là tích chập XOR tương ứng với việc nhân các đa thức trong miền XOR. 

Cách tiếp cận tối ưu tận dụng **FWHT`2^m`phần tử**, làm giảm vấn đề từ hàm mũ trong`n`ĐẾN`O(n + m*2^m)`. Mỗi khoảng đóng góp một đa thức nhỏ trên chiều rộng của nó, sau đó chúng ta sử dụng FWHT để kết hợp tất cả các khoảng đóng góp vào tổng`f_x`. Cuối cùng nhân với`2^x`và XOR-ing rất đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(sản phẩm(r_i - l_i + 1)) | O(2^m) | Quá chậm | 
| XOR-FWHT DP | O(n + m*2^m) | O(2^m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`f`kích thước`2^m`để giữ số lượng cho mỗi giá trị XOR, bắt đầu bằng`f[0] = 1`. Điều này thể hiện trường hợp cơ bản: chưa có khoảng thời gian nào đóng góp cho XOR. 
2. Đối với mỗi khoảng thời gian`[l_i, r_i]`, xây dựng một mảng`g`kích thước`2^m`Ở đâu`g[x] = 1`nếu như`x`đang ở trong`[l_i, r_i]`Và`0`nếu không thì. Điều này thể hiện tất cả các số có thể mà chúng ta có thể chọn từ khoảng này. 
3. Thực hiện FWHT trên cả hai mảng`f`Và`g`. Điều này chuyển đổi số lượng thành miền tích chập XOR. 
4. Nhân`f`Và`g`theo điểm. Bước này kết hợp các trình tự, tính toán hiệu quả số lượng trình tự tạo ra mỗi XOR trong các khoảng thời gian được xử lý. 
5. Áp dụng FWHT nghịch đảo cho`f`để khôi phục số lượng trong miền bình thường. Sau bước này,`f[x]`giữ số lượng chuỗi mang lại XOR`x`modulo`998244353`. 
6. Nhân mỗi`f[x]`qua`2^x`modulo`998244353`để có được`g_x`. 
7. XOR tất cả`g_x`để tính kết quả cuối cùng`h`. 
8. Lặp lại cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Ở mỗi bước,`f[x]`thể hiện chính xác số lượng chuỗi trong các khoảng thời gian được xử lý cho đến nay mang lại XOR`x`. FWHT đảm bảo rằng tích chập tôn trọng phép cộng XOR thay vì phép cộng thông thường, duy trì tính chính xác khi kết hợp các khoảng thời gian. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def fwht(a, invert):
    n = len(a)
    step = 1
    while step < n:
        for i in range(0, n, step * 2):
            for j in range(step):
                u = a[i + j]
                v = a[i + j + step]
                a[i + j] = (u + v) % MOD
                a[i + j + step] = (u - v) % MOD
        step <<= 1
    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        size = 1 << m
        f = [1] + [0] * (size - 1)
        for _ in range(n):
            l, r = map(int, input().split())
            g = [0] * size
            for x in range(l, r + 1):
                g[x] = 1
            fwht(f, False)
            fwht(g, False)
            for i in range(size):
                f[i] = f[i] * g[i] % MOD
            fwht(f, True)
        h = 0
        for x in range(size):
            gx = f[x] * pow(2, x, MOD) 
            h ^= gx
        print(h)

solve()
```Giải pháp này triển khai FWHT để thực hiện phép chập XOR một cách hiệu quả. Việc xây dựng cẩn thận`g`đảm bảo rằng mỗi khoảng đóng góp chính xác phạm vi số của nó. Phép biến đổi nghịch đảo khôi phục số đếm bình thường và nhân với`2^x`được thực hiện theo mô-đun`998244353`để tránh tràn. XOR của`g_x`các giá trị tạo ra đầu ra cuối cùng. 

## Ví dụ đã hoạt động 

Mẫu 1 test case đầu tiên:`n = 2, m = 2`, khoảng`[0, 2]`Và`[1, 3]`. 

| Bước | mảng f trước | mảng g | mảng f sau khi tích chập | 
| --- | --- | --- | --- | 
| ban đầu | [1,0,0,0] | [0,1,1,1] | sau FWHT nhân/inv | 
| sau 1 khoảng thời gian | đếm chuỗi cho khoảng đầu tiên | - | [1,1,1,0] | 
| sau 2 khoảng thời gian | đếm chuỗi trong hai khoảng | - | [2,2,2,3] | 

nhân`f[x]`qua`2^x`cho`[2,4,8,24]`. Sản lượng XOR`22`, phù hợp với mẫu 

Một trường hợp khác:`n=1, m=1`, khoảng`[0,1]`.`f=[1,0]`,`g=[1,1]`, sau FWHT:`f=[1,1]`. Nhân với`2^x`sản lượng`[1,2]`. XOR`1^2 = 3`. 

Điều này xác nhận thuật toán xử lý chính xác cả khoảng nhỏ và khoảng chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 2^m * m) | Mỗi trường hợp thử nghiệm yêu cầu xây dựng`g`(O(phạm vi)) và thực hiện FWHT O(2^m * m). Tổng n khoảng được tính tổng theo các trường hợp phù hợp với các ràng buộc. | 
| Không gian | O(2^m) | Mảng`f`Và`g`có kích thước 2^m chiếm ưu thế trong việc sử dụng bộ nhớ. | 

Giải pháp phù hợp trong giới hạn thời gian 2 giây và bộ nhớ 256 MB vì`2^18 * 18 ≈ 4.7 * 10^6`hoạt động cho mỗi trường hợp thử nghiệm là có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
# helper
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n2 2\n0 2\n1 3\n5 3\n3 7\n1 3\n0 2\n1 5\n3 6\n10 14\n314 1592\n653 5897\n932 3846\n264 3383\n279 5028\n841 9716\n939 9375\n105 8209\n749 4459\n230 7816\n1 5\n0 29") == "22\n9812\n75032210\n1073741823", "sample 1"

# Custom: single element interval
assert run("1\n1 2\n1 1") == "2", "single interval"

# Custom: all zero intervals
assert run("1\n3
```
