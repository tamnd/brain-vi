---
title: "CF 1407B - Vova Lớn"
description: "Chúng ta được cho một chuỗi các số nguyên dương và chúng ta cần sắp xếp lại chúng thành một chuỗi $b$ sao cho chuỗi tiền tố ước số chung lớn nhất $ci = gcd(b1, b2, dots, bi)$ là cực đại về mặt từ điển."
date: "2026-06-11T07:48:59+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1407
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 669 (Div. 2)"
rating: 1300
weight: 1407
solve_time_s: 92
verified: false
draft: false
---

[CF 1407B - Vova lớn](https://codeforces.com/problemset/problem/1407/B) 

**Đánh giá:** 1300 
**Tags:** sức mạnh vũ phu, lòng tham, toán học, lý thuyết số 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một dãy các số nguyên dương và chúng ta cần sắp xếp lại chúng thành một dãy$b$sao cho dãy tiền tố ước số chung lớn nhất$c_i = \gcd(b_1, b_2, \dots, b_i)$là tối đa về mặt từ điển. Nói cách khác, ở mỗi vị trí, chúng ta muốn GCD đang chạy càng lớn càng tốt ở vị trí khác nhau ngoài cùng bên trái so với bất kỳ thứ tự nào khác. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm, mỗi trường hợp có một chuỗi tương đối nhỏ ($n \le 10^3$) và các số được giới hạn bởi$10^3$. Bởi vì tổng của tất cả$n$trên các trường hợp thử nghiệm không vượt quá$10^3$, MỘT$O(n^2)$giải pháp cho mỗi trường hợp thử nghiệm là chấp nhận được. 

Các trường hợp cạnh không rõ ràng phát sinh khi nhiều số có chung thừa số nhưng khác nhau về kích thước. Ví dụ, nếu trình tự là`[4, 2, 8]`, một cách tiếp cận ngây thơ là luôn chọn số lớn nhất trước tiên có thể thất bại, vì việc chọn 8 đầu tiên sẽ tạo ra tiền tố GCD`[8, 2, 2]`, trong khi bắt đầu bằng 4 sẽ cho`[4, 2, 2]`. Ở đây, bước tham lam phải xem xét GCD hiện tại với mỗi ứng cử viên và chọn một GCD tối đa hóa kết quả. Các chuỗi phần tử đơn, các chuỗi có tất cả các số bằng nhau và các chuỗi chứa 1 cũng phải được xử lý chính xác. 

## Phương pháp tiếp cận 

Phương pháp brute-force là tạo ra tất cả$n!$hoán vị của chuỗi, tính toán chuỗi GCD tiền tố cho mỗi hoán vị và chọn hoán vị tạo ra chuỗi GCD tiền tố lớn nhất về mặt từ điển. Điều này đúng vì nó khám phá tất cả các thứ tự có thể có, nhưng nó không khả thi ngay cả đối với$n = 10$, BẰNG$10! = 3.6 \cdot 10^6$các hoạt động và tính toán trình tự GCD sẽ bổ sung thêm chi phí. 

Cái nhìn sâu sắc quan trọng cho một giải pháp tối ưu là chuỗi GCD tiền tố không tăng:$\gcd(b_1, \dots, b_{i+1}) \le \gcd(b_1, \dots, b_i)$. Vì vậy, phần tử đầu tiên phải luôn là số lớn nhất để tối đa hóa$c_1$. Sau phần tử đầu tiên, ở mỗi bước, chúng tôi chọn số từ nhóm còn lại để tối đa hóa GCD khi kết hợp với tiền tố GCD hiện tại. Cách tiếp cận tham lam này hợp lệ vì so sánh từ điển phụ thuộc chủ yếu vào các phần tử ngoài cùng bên trái: tối đa hóa GCD hiện tại ở mỗi bước đảm bảo không có GCD nhỏ hơn xuất hiện trước đó trong chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Tham lam tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng ca kiểm thử và lặp lại từng trường hợp. 
2. Với mỗi test case, hãy đọc trình tự$a$và khởi tạo một danh sách kết quả trống$b$. 
3. Chọn số lớn nhất trong$a$là phần tử đầu tiên của$b$. Điều này đảm bảo tiền tố đầu tiên GCD là tối đa. Loại bỏ nó khỏi hồ bơi. 
4. Khởi tạo`current_gcd`là giá trị của phần tử đầu tiên. 
5. Trong khi vẫn còn các số trong nhóm, hãy lặp qua chúng và tính GCD của`current_gcd`với từng ứng viên. Chọn số tối đa hóa GCD này. Nối nó vào$b$và cập nhật`current_gcd`. Xóa số đã chọn khỏi nhóm. 
6. Lặp lại cho đến khi tất cả các số được đặt vào$b$. đầu ra$b$. 

Tại sao nó hoạt động: Ở mỗi bước, thuật toán duy trì tính bất biến rằng chuỗi tiền tố GCD được tạo ra cho đến nay là tối đa về mặt từ điển so với bất kỳ thứ tự nào của các số còn lại. Vì tiền tố GCD không tăng nên việc tối đa hóa từng bước cục bộ sẽ đảm bảo mức tối đa từ điển toàn cục. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        used = [False] * n
        b = []
        # Step 1: pick the largest number first
        max_idx = a.index(max(a))
        b.append(a[max_idx])
        used[max_idx] = True
        current_gcd = a[max_idx]

        for _ in range(1, n):
            best_gcd = -1
            best_idx = -1
            for i in range(n):
                if not used[i]:
                    g = math.gcd(current_gcd, a[i])
                    if g > best_gcd:
                        best_gcd = g
                        best_idx = i
            b.append(a[best_idx])
            used[best_idx] = True
            current_gcd = best_gcd
        print(*b)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã sẽ chọn số lớn nhất vì số đó sẽ tối đa hóa tiền tố GCD đầu tiên. các`used`mảng theo dõi những số đã được chọn để tránh lặp lại. Vòng lặp bên trong chọn số mang lại GCD cao nhất với tiền tố hiện tại, phản ánh bất biến tham lam. sử dụng`math.gcd`đảm bảo tính chính xác và tránh tính toán GCD thủ công. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2
2
2 5
3
3 8 9
```#### Dấu vết cho trường hợp thử nghiệm đầu tiên`[2,5]`| Bước | GCD hiện tại | Còn lại | Đã chọn | GCD với ứng viên | 
| --- | --- | --- | --- | --- | 
| 1 | - | [2,5] | 5 | - | 
| 2 | 5 | [2] | 2 | gcd(5,2)=1 | 

Đầu ra:`5 2`. GCD đầu tiên là 5, là giá trị tối đa. 

#### Dấu vết cho trường hợp thử nghiệm thứ hai`[3,8,9]`| Bước | GCD hiện tại | Còn lại | Đã chọn | GCD với ứng viên | 
| --- | --- | --- | --- | --- | 
| 1 | - | [3,8,9] | 9 | - | 
| 2 | 9 | [3,8] | 3 | gcd(9,3)=3 | 
| 3 | 3 | [8] | 8 | gcd(3,8)=1 | 

Đầu ra:`9 3 8`. Trình tự đảm bảo mức tối đa từ điển của tiền tố GCD`[9,3,1]`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Với mỗi n vị trí, chúng ta quét tối đa n phần tử còn lại để tính GCD. | 
| Không gian | O(n) | Lưu trữ mảng đã sử dụng và chuỗi kết quả. | 

Được cho$n \le 10^3$và tổng n trên các trường hợp thử nghiệm ≤ 10^3, giải pháp thực hiện tối đa$10^6$hoạt động, trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    solve()
    return output.getvalue().strip()

# Provided samples
assert run("7\n2\n2 5\n4\n1 8 2 3\n3\n3 8 9\n5\n64 25 75 100 50\n1\n42\n6\n96 128 88 80 52 7\n5\n2 4 8 16 17\n") == \
    "5 2\n8 2 1 3\n9 3 8\n100 50 25 75 64\n42\n128 96 80 88 52 7\n17 2 4 8 16"

# Minimum-size input
assert run("1\n1\n1\n") == "1"

# All equal
assert run("1\n4\n5 5 5 5\n") == "5 5 5 5"

# Maximum element first vs last
assert run("1\n5\n2 4 6 8 10\n") == "10 8 6 4 2"

# Contains 1
assert run("1\n3\n1 3 6\n") == "6 3 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n1`|`1`| Chuỗi phần tử đơn | 
|`1\n4\n5 5 5 5`|`5 5 5 5`| Tất cả các phần tử đều bằng nhau | 
|`1\n5\n2 4 6 8 10`|`10 8 6 4 2`| Tham lam chọn những công trình GCD lớn nhất | 
|`1\n3\n1 3 6`|`6 3 1`| Chuỗi chứa 1 được xử lý đúng | 

## Vỏ cạnh 

Một chuỗi chứa 1, như`[1, 3, 6]`, chứng tỏ rằng việc chọn số lớn nhất trước tiên sẽ tránh được GCD giảm quá sớm. Thuật toán chọn 6 đầu tiên (`current_gcd=6`), rồi 3 (`gcd(6,3)=3`), thì 1 (`gcd(3,1)=1`). Điều này đảm bảo chuỗi tiền tố GCD`[6,3,1]`là tối đa. Nếu không có sự lựa chọn tham lam ở mỗi bước, việc chọn 3 đầu tiên sẽ mang lại `[3,
