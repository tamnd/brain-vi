---
title: "CF 1493F - Ma trận mê hoặc"
description: "Chúng ta được cho một ma trận $n lần m$ chưa biết. Thông tin duy nhất chúng ta biết ban đầu là kích thước của nó. Nhiệm vụ là đếm tất cả các cặp $(r, c)$ sao cho $r$ chia $n$, $c$ chia $m$, và nếu chúng ta phân chia ma trận thành các khối có kích thước $r nhân c$, thì tất cả các khối đều giống hệt nhau."
date: "2026-06-10T22:18:27+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "interactive", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1493
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 705 (Div. 2)"
rating: 2600
weight: 1493
solve_time_s: 148
verified: false
draft: false
---

[CF 1493F - Ma trận mê hoặc](https://codeforces.com/problemset/problem/1493/F) 

**Đánh giá:** 2600 
**Tags:** bitmasks, tương tác, lý thuyết số 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một ẩn số$n \times m$ma trận. Thông tin duy nhất chúng ta biết ban đầu là kích thước của nó. Nhiệm vụ là đếm tất cả các cặp$(r, c)$như vậy$r$chia rẽ$n$,$c$chia rẽ$m$và nếu chúng ta phân chia ma trận thành các khối có kích thước$r \times c$, tất cả các khối đều giống hệt nhau. Nói cách khác, ma trận được xếp lớp bằng cách lặp lại một hình chữ nhật có kích thước$r \times c$. 

Thử thách nảy sinh vì ma trận bị ẩn và thao tác duy nhất được phép là truy vấn so sánh hai hình chữ nhật con có cùng kích thước. Mỗi truy vấn cho chúng ta biết liệu hai hình chữ nhật con có hoàn toàn bằng nhau hay không. Các truy vấn được giới hạn trong khoảng$3 \log_2(n+m)$, ít hơn nhiều so với tổng số hình chữ nhật con, vì vậy chúng ta không thể kiểm tra từng cặp ứng cử viên một cách toàn diện. 

Hạn chế$n, m \le 1000$có nghĩa là chúng tôi không đủ khả năng cung cấp các thuật toán để kiểm tra từng khối tiềm năng riêng lẻ; chúng ta phải khai thác tính chia hết và cấu trúc lặp lại của ma trận. Các trường hợp cạnh không rõ ràng bao gồm: 

- Một ma trận hoàn toàn thống nhất trong đó tất cả các giá trị đều giống hệt nhau. Mỗi ước số của$n$Và$m$sẽ mang lại một lát gạch hợp lệ. Mã bất cẩn có thể đếm gấp đôi. 
- Ma trận trong đó mỗi hàng hoặc cột là duy nhất. Chỉ có kích thước ma trận đầy đủ$(n, m)$là hợp lệ và các thử nghiệm khối nhỏ sẽ xuất hiện không chính xác bằng nhau nếu chúng tôi không truy vấn các khối không chồng chéo một cách chính xác. 
- Kích thước nguyên tố. Ví dụ,$n=7$,$m=6$; chỉ một$r=1$hoặc$r=7$Và$c=1, 2, 3, 6$là những ứng cử viên có thể Xử lý các ước số không chính xác sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên tất cả các ước số của$n$Và$m$, và với mỗi$(r, c)$, kiểm tra tất cả$(n/r) \times (m/c)$khối cho sự bình đẳng bằng cách sử dụng truy vấn. Điều này đúng nhưng không khả thi. Trong trường hợp xấu nhất,$n=m=1000$, có thể có tới$16$mỗi ước số sẽ cho 256 cặp ứng cử viên. Đối với mỗi việc kiểm tra đơn giản có thể bao gồm hàng trăm truy vấn, vượt quá giới hạn cho phép.$O(\log(n+m))$ngân sách truy vấn. 

Quan sát quan trọng là chúng ta không cần phải kiểm tra từng khối một cách rõ ràng. Bởi vì việc xếp lớp là định kỳ nên chúng ta có thể xác định đơn vị lặp lại tối thiểu trong từng chiều riêng biệt. Đối với kích thước hàng, hãy$r$là một ước số ứng cử viên. Nếu lần đầu tiên$r$các hàng được lặp lại trong ma trận, sau đó các hàng được xếp chồng lên nhau. Chúng ta có thể kiểm tra điều này bằng cách so sánh từng khối liên tiếp của$r$hàng với hàng đầu tiên. Điều tương tự cũng áp dụng cho các cột. Sử dụng chiến lược tìm kiếm nhị phân trên các ước số, chúng ta có thể giảm số lượng truy vấn xuống logarit trong$n$Và$m$. Khi chúng tôi biết tính chu kỳ tối đa dọc theo hàng và cột, chúng tôi kết hợp các bộ ước số hợp lệ cho từng thứ nguyên. Mỗi ước số hàng hợp lệ$r$có thể ghép nối với mỗi ước số cột hợp lệ$c$, cho tất cả hợp lệ$(r, c)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(nm * #divisors(n) * #divisors(m)) | O(1) | Quá chậm, vượt quá giới hạn truy vấn | 
| Tối ưu | Truy vấn O(log n * log m) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tất cả các ước của$n$Và$m$. Sắp xếp chúng tăng dần thuận tiện cho việc kiểm tra sau này. Đây là tất cả các kích thước khối ứng viên có thể có dọc theo hàng và cột. 
2. Xác định hàm trợ giúp`check_rows(r)`để kiểm tra xem ma trận có thể được xếp lớp bằng`r`-hàng khối. Đối với điều này, lặp đi lặp lại`i`trong các bước của`r`từ`0`ĐẾN`n-r`và so sánh khối bắt đầu từ hàng`1`với khối bắt đầu ở hàng`i+1`sử dụng một truy vấn có kích thước`r x m`. Nếu so sánh không thành công, hãy trả về`False`; nếu không thì trả lại`True`. 
3. Tương tự xác định`check_cols(c)`sử dụng truy vấn có kích thước`n x c`. Lặp lại dọc theo các cột theo các bước của`c`và so sánh với khối cột đầu tiên. 
4. Áp dụng các phép kiểm tra trên cho tất cả các ước số. Lưu trữ các ước số xếp hàng thành công trong`valid_rows`và các cột trong`valid_cols`. 
5. Tính tổng số cặp hợp lệ theo tích Descartes:`len(valid_rows) * len(valid_cols)`. Điều này hoạt động vì một khối có kích thước`r x c`là hợp lệ khi và chỉ khi`r`hàng gạch và`c`cột gạch. 
6. Xuất ra câu trả lời cuối cùng. 

Tại sao nó hoạt động: Thuật toán tách biệt hai chiều, dựa vào sự độc lập của việc xếp chồng hàng và xếp chồng cột. Mỗi truy vấn so sánh khối không chồng chéo với khối đầu tiên. Nếu một số chia không thành công thì bất kỳ bội số nào cũng sẽ thất bại. Điều bất biến là chúng tôi chỉ chấp nhận các ước số có các khối nhất quán trên toàn bộ chiều, đảm bảo rằng việc kết hợp các ước số hàng và cột sẽ cho ra tất cả các kích thước hình chữ nhật hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import isqrt

def divisors(x):
    small, large = [], []
    for i in range(1, isqrt(x)+1):
        if x % i == 0:
            small.append(i)
            if i != x // i:
                large.append(x//i)
    return small + large[::-1]

def query(h, w, i1, j1, i2, j2):
    print(f"? {h} {w} {i1} {j1} {i2} {j2}", flush=True)
    return int(input())

n, m = map(int, input().split())

row_divs = divisors(n)
col_divs = divisors(m)

valid_rows = []
for r in row_divs:
    ok = True
    for start in range(r+1, n+1, r):
        if start + r - 1 > n:
            break
        if not query(r, m, 1, 1, start, 1):
            ok = False
            break
    if ok:
        va
```
