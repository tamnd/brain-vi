---
title: "CF 1525A - Pha chế thuốc"
description: "Chúng tôi được giao nhiệm vụ tạo ra một lọ thuốc chứa chính xác $k%$ tinh chất ma thuật và $(100-k)%$ nước. Cái vạc bắt đầu trống rỗng và chúng ta chỉ có thể thêm một lít mỗi lần một trong hai thành phần."
date: "2026-06-10T17:32:26+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1525
codeforces_index: "A"
codeforces_contest_name: "Educational Codeforces Round 109 (Rated for Div. 2)"
rating: 800
weight: 1525
solve_time_s: 587
verified: true
draft: false
---

[CF 1525A - Chế tạo thuốc](https://codeforces.com/problemset/problem/1525/A) 

**Đánh giá:** 800 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 9 phút 47 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao nhiệm vụ tạo ra một loại thuốc có chứa chính xác$k\%$bản chất ma thuật và$(100-k)\%$Nước. Cái vạc bắt đầu trống rỗng và chúng ta chỉ có thể thêm một lít mỗi lần một trong hai thành phần. Mục đích là xác định số lít tối thiểu chúng ta phải đổ để đạt được tỷ lệ chính xác. Đầu vào cung cấp một số trường hợp thử nghiệm$t$, mỗi cái chỉ định tỷ lệ phần trăm mong muốn$k$và đầu ra phải cung cấp tổng số lít tối thiểu cho mỗi loại. 

Quan sát quan trọng là tỷ lệ của lọ thuốc chỉ phụ thuộc vào số lượng tương đối của tinh chất và nước chứ không phụ thuộc vào thể tích tuyệt đối. Do đó, bài toán quy về việc tìm các số nguyên nhỏ nhất$e$Và$w$như vậy$$\frac{e}{e + w} = \frac{k}{100}.$$Chúng ta có thể viết lại điều này như$100 \cdot e = k \cdot (e + w)$, dẫn đến$100e - k(e + w) = 0$, hoặc tương đương$e \cdot (100 - k) = w \cdot k$. Đây là phương trình Diophantine tuyến tính theo số nguyên$e$Và$w$. 

Những hạn chế là nhỏ,$1 \le k \le 100$Và$t \le 100$, vì vậy một$O(1)$giải pháp số học cho mỗi trường hợp thử nghiệm là đủ. Trường hợp cạnh xảy ra khi$k$chia đều cho 100 (ví dụ:$k = 25, 50, 100$) và khi nào$k$là nguyên tố cùng nhau với 100 (ví dụ:$k = 3, 7$). Một giải pháp đơn giản để kiểm tra tất cả các tập lên tới 100 sẽ có hiệu quả nhưng không cần thiết do sự tồn tại của một công thức lý thuyết số đơn giản. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại qua tất cả các tập có thể$e + w$bắt đầu từ 1, kiểm tra xem phân số có$e/(e+w)$bằng$k/100$. Vì$k = 1$hoặc$k = 99$, việc này có thể yêu cầu tới 100 bước, có thể chấp nhận được nhưng không thanh lịch. 

Cách tiếp cận tối ưu sử dụng lý luận về ước số chung lớn nhất. Nếu như$g = \gcd(k, 100)$, sau đó$k = g \cdot k'$Và$100 = g \cdot 100'$. Tổng số lít tối thiểu thỏa mãn tỷ lệ chính xác là$100/g$, từ$$\frac{e}{e+w} = \frac{k}{100} = \frac{k'}{100'}$$và các số nguyên nhỏ nhất trong tỉ số đó là$e = k'$Và$w = 100' - k'$, cho$e + w = k' + (100' - k') = 100' = 100/g$. Điều này mang lại một công thức ngay lập tức mà không cần lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
|---|---|---|---| 
| Lực lượng vũ phu | O(100) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng không cần thiết | 
| Tối ưu | O(log 100) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận và thanh lịch | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc tỷ lệ phần trăm$k$. 
3. Tính ước chung lớn nhất$g = \gcd(k, 100)$. Điều này xác định yếu tố lớn nhất có thể làm giảm cả tử số và mẫu số. 
4. Tính tổng số lít tối thiểu là$100/g$. Điều này tương ứng với các số nguyên nhỏ nhất$e$Và$w$thỏa mãn tỉ số đó. 
5. In kết quả cho test case. 

Tại sao nó hoạt động: Bằng cách giảm phần$k/100$đối với các thuật ngữ thấp nhất, chúng tôi tìm thấy tỷ lệ số nguyên nhỏ nhất duy trì tỷ lệ phần trăm chính xác. Tổng tử số và mẫu số của phân số rút gọn này cho tổng thể tích nhỏ nhất. Không có tổ hợp số nguyên nhỏ hơn nào có thể thỏa mãn tỷ lệ chính xác vì bất kỳ tổng nhỏ hơn nào cũng sẽ vi phạm thuộc tính phân số rút gọn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    k = int(input())
    g = math.gcd(k, 100)
    print(100 // g)
```Giải pháp đọc từng trường hợp kiểm thử, tính ước số chung lớn nhất và đưa ra tổng khối lượng tối thiểu. Việc lựa chọn phép chia số nguyên đảm bảo chúng ta thu được số lít chính xác. Tích hợp sẵn của Python`math.gcd`tự động xử lý tất cả các trường hợp cạnh, bao gồm$k = 100$Và$k = 1$. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**$k = 3$| Biến | Giá trị | 
|---|---| 
| k | 3 | 
| gcd(k,100) | 1 | 
| 100/gcd(k,100) | 100 | 

Chúng ta cần 3 lít tinh chất và 97 lít nước, tổng cộng là 100 lít. Điều này xác nhận tổng số tối thiểu là 100. 

**Ví dụ 2:**$k = 25$| Biến | Giá trị | 
|---|---| 
| k | 25 | 
| gcd(k,100) | 25 | 
| 100/gcd(k,100) | 4 | 

Sự kết hợp tối thiểu là 1 lít tinh chất và 3 lít nước, tổng cộng là 4 lít. Thuật toán tính toán chính xác khối lượng nhỏ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
|---|---|---| 
| Thời gian | O(t * log 100) | tính toán gcd cho mỗi trường hợp thử nghiệm mất O(log 100) | 
| Không gian | O(1) | chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm | 

Được cho$t \le 100$, điều này rất phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        k = int(input())
        g = math.gcd(k, 100)
        output.append(str(100 // g))
    return "\n".join(output)

# Provided samples
assert run("3\n3\n100\n25\n") == "100\n1\n4", "sample 1"

# Custom tests
assert run("3\n1\n50\n99\n") == "100\n2\n100", "edge and half values"
assert run("2\n2\n10\n") == "50\n10", "small divisors"
assert run("2\n33\n66\n") == "100\n50", "nontrivial gcd"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
|---|---|---| 
| 1,50,99 | 100,2,100 | nhỏ nhất, một nửa, đồng nguyên tố lớn | 
| 2,10 | 50,10 | ước số nhỏ | 
| 33,66 | 100,50 | gcd không tầm thường | 

## Vỏ cạnh 

Khi nào$k = 100$, gcd là 100 và 100/100 = 1, xác nhận rằng đổ một lít là đủ. Khi$k = 1$, gcd(1.100) = 1 và thuật toán đưa ra chính xác 100 lít. Công thức tự động xử lý tỷ lệ phần trăm chia đều cho 100 và tỷ lệ nguyên tố cùng nhau với 100, đảm bảo tính chính xác trong mọi điều kiện biên.
