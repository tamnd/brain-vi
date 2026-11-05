---
title: "CF 1203C - Ước chung"
description: "Chúng ta được yêu cầu tìm số số nguyên dương chia đều cho mọi số trong một mảng nhất định. Mảng có thể chứa tối đa 400.000 phần tử, mỗi phần tử có thể lớn tới $10^{12}$. Về mặt khái niệm, chúng ta đang tìm tập hợp ước chung của tất cả các phần tử mảng."
date: "2026-06-11T23:42:03+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1203
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 579 (Div. 3)"
rating: 1300
weight: 1203
solve_time_s: 99
verified: true
draft: false
---

[CF 1203C - Ước chung](https://codeforces.com/problemset/problem/1203/C) 

**Đánh giá:** 1300 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tìm số số nguyên dương chia đều cho mọi số trong một mảng nhất định. Mảng có thể chứa tới 400.000 phần tử, mỗi phần tử có thể lớn bằng$10^{12}$. Về mặt khái niệm, chúng ta đang tìm tập hợp ước chung của tất cả các phần tử mảng. 

Đầu ra là một số nguyên duy nhất: số lượng các ước số này. Chẳng hạn, trong một mảng như`[2, 4, 6, 2, 10]`, những con số`1`Và`2`chia tất cả các phần tử, vì vậy câu trả lời là`2`. 

Các ràng buộc ngay lập tức thông báo cho cách tiếp cận của chúng tôi. Từ`n`có thể là 4·10^5 và mỗi số lên tới 10^12, chúng tôi không thể lặp lại tất cả các số cho đến phần tử tối đa và kiểm tra tính chia hết. Một giải pháp đơn giản sẽ yêu cầu lặp lại tới`10^12`cho từng phần tử mảng, điều này là không thể thực hiện được. Chúng ta cần một cách tiếp cận có độ phức tạp phụ thuộc vào số ước của một số chứ không phải chính số đó. 

Một trường hợp cạnh tinh tế phát sinh khi mảng chứa`1`. Nếu bất kỳ phần tử nào`1`, ước chung duy nhất là`1`. Tương tự, nếu tất cả các phần tử đều giống nhau thì đáp án là số ước của số đó. Mảng có số nguyên tố lớn cũng đặt ra một thách thức vì ước số của chúng bị hạn chế. Các giải pháp bất cẩn có thể lặp lại trên một phạm vi dựa trên kích thước mảng thay vì ước số chung lớn nhất, tạo ra hành vi không chính xác hoặc không hiệu quả. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ cố gắng kiểm tra mọi số nguyên từ`1`lên đến phần tử tối thiểu trong mảng, đếm những phần tử chia hết tất cả các phần tử. Điều này đúng vì bất kỳ ước số chung nào cũng không thể vượt quá số nhỏ nhất. Tuy nhiên, trong trường hợp xấu nhất, số nhỏ nhất có thể lên tới$10^{12}$và việc kiểm tra khả năng chia hết cho mỗi số quá chậm. Số lượng hoạt động có thể là khoảng$10^{12} \cdot n$, sẽ không bao giờ phù hợp trong 2 giây. 

Quan sát quan trọng là tập hợp các ước số chung của mảng chính xác là tập hợp các ước số của ước số chung lớn nhất (GCD) của tất cả các phần tử. Điều này là do bất kỳ số nào chia GCD sẽ tự động chia từng phần tử của mảng và ngược lại, bất kỳ số nào chia tất cả các phần tử mảng sẽ chia GCD của chúng. Việc tính toán GCD của tất cả các số có thể được thực hiện một cách hiệu quả bằng thuật toán Euclide, ngay cả đối với những số rất lớn. Khi đã biết GCD, chúng ta chỉ cần đếm các ước của nó. Đếm các ước số đến$\sqrt{g}$đảm bảo rằng chúng ta xem xét tất cả các cặp số chia mà không lặp lại quá căn bậc hai. 

Quan sát này làm giảm vấn đề từ một không gian tìm kiếm tiềm năng rất lớn thành một thứ có thể quản lý được, trong đó các hoạt động gần như tỷ lệ thuận với$n \log(\text{max element})$để tính toán GCD và$O(\sqrt{g})$để đếm các ước số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * phút(a)) | O(1) | Quá chậm | 
| GCD + Số chia | O(n log max(a) + √g) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng từ đầu vào và lưu trữ các phần tử trong danh sách. Điều này là cần thiết để tính toán GCD trên tất cả các phần tử. 
2. Khởi tạo một biến`g`với phần tử đầu tiên. Điều này sẽ lưu trữ GCD đang chạy. 
3. Lặp qua các phần tử còn lại của mảng. Đối với mỗi phần tử`a[i]`, cập nhật`g`như GCD của`g`Và`a[i]`sử dụng thuật toán Euclide. Bước này đảm bảo rằng ở cuối vòng lặp,`g`là GCD của toàn bộ mảng. 
4. Khởi tạo bộ đếm`count`về không. Điều này sẽ theo dõi số ước của`g`. 
5. Lặp lại tất cả các số nguyên`d`từ`1`đến căn bậc hai số nguyên của`g`. Đối với mỗi`d`, kiểm tra xem`g % d == 0`. Nếu đúng thì tăng`count`bằng 1 vì`d`là một số chia. 
6. Với mỗi ước số`d`cái đó không bằng`g // d`, tăng`count`bằng 1 để tính số chia ghép đôi. Bước này đảm bảo chúng ta không đếm gấp đôi các ô vuông hoàn hảo. 
7. In giá trị của`count`, là số số nguyên dương chia hết tất cả các phần tử của mảng. 

Tại sao nó hoạt động: Điều bất biến ở đây là bất kỳ ước số nào của GCD đều chia hết tất cả các số trong mảng. Ngược lại, bất kỳ số nào chia hết tất cả các phần tử đều chia GCD. Bằng cách lặp lại đến căn bậc hai của`g`và đếm từng cặp ước số, chúng tôi đảm bảo rằng mọi ước số chung đều được tính chính xác một lần. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

g = a[0]
for num in a[1:]:
    g = math.gcd(g, num)

count = 0
sqrt_g = int(g**0.5)
for d in range(1, sqrt_g + 1):
    if g % d == 0:
        count += 1
        if d != g // d:
            count += 1

print(count)
```Mã đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`và tính toán GCD bằng cách sử dụng công cụ tích hợp sẵn của Python`math.gcd`. Chúng ta lặp qua các ước số đến căn bậc hai để đếm tất cả các ước số một cách hiệu quả mà không bị trùng lặp. Séc`d != g // d`đảm bảo chúng tôi xử lý các ô vuông hoàn hảo một cách chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
1 2 3 4 5
```| Bước | g | Số chia | Ghi chú | 
| --- | --- | --- | --- | 
| Ban đầu | 1 | 0 | Bắt đầu với g = a[0] = 1 | 
| Vòng GCD | 1 | 0 | gcd(1,2)=1, gcd(1,3)=1, gcd(1,4)=1, gcd(1,5)=1 | 
| Vòng chia số | 1 | 1 | Ước số duy nhất là 1 | 

Đầu ra:`1`. Đúng, vì chỉ có 1 chia hết các số. 

### Ví dụ tùy chỉnh 2 

đầu vào:```
4
2 4 6 8
```| Bước | g | Số chia | Ghi chú | 
| --- | --- | --- | --- | 
| Ban đầu | 2 | 0 | g = a[0] = 2 | 
| Vòng GCD | 2 | 0 | gcd(2,4)=2, gcd(2,6)=2, gcd(2,8)=2 | 
| Vòng chia số | 2 | 2 | ước số là 1 và 2 | 

Đầu ra:`2`. Đúng, ước của 2 chia hết các phần tử. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log max(a) + √g) | Tính toán GCD là O(n log max(a)), ước số đếm là O(√g) | 
| Không gian | O(n) | Lưu trữ mảng | 

Thuật toán dễ dàng phù hợp với các ràng buộc: n 4·10^5 và các số đến 10^12. Vòng đếm số chia được giới hạn bởi √(10^12) ≈ 10^6, có thể chấp nhận được trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    g = a[0]
    for num in a[1:]:
        g = math.gcd(g, num)
    count = 0
    sqrt_g = int(g**0.5)
    for d in range(1, sqrt_g + 1):
        if g % d == 0:
            count += 1
            if d != g // d:
                count += 1
    return str(count)

# Provided sample
assert run("5\n1 2 3 4 5\n") == "1", "sample 1"

# All equal numbers
assert run("4\n6 6 6 6\n") == "4", "all equal"

# Single element
assert run("1\n10\n") == "4", "single element divisors"

# Large prime numbers
assert run("3\n17 34 51\n") == "2", "common divisors of 17,34,51"

# Perfect squares
assert run("3\n16 32 48\n") == "5", "divisors of 16"

# Mixed small and large numbers
assert run("5\n1 1000000000000 500000000000 250000000000 125000000000\n") == "1", "only 1 divides all"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 6 6 6 6 | 4 | Xử lý tất cả các phần tử bằng nhau | 
| 1 10 | 4 | Trường hợp phần tử đơn | 
| 17 34 51 | 2 | GCD với số nguyên tố | 
| 16 32 48 | 5 | Xử lý hình vuông hoàn hảo | 
| 1 1000000000000 500000000000 250000000000 125000000000 | 1 | Số lớn chỉ có 1 ước chung | 

## Vỏ cạnh 

Khi mảng chứa`1`bên cạnh các số lớn hơn, như `[1, 2, 3,
