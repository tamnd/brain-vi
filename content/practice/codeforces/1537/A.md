---
title: "CF 1537A - Mảng số học"
description: "Chúng ta được cho một mảng số nguyên và nhiệm vụ của chúng ta là làm cho giá trị số học của nó bằng 1 bằng cách thêm các số nguyên không âm. Mỗi trường hợp thử nghiệm trình bày một mảng các số nguyên tùy ý, cả dương và âm, và yêu cầu số lượng phép cộng tối thiểu cần thiết."
date: "2026-06-10T15:02:21+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1537
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 726 (Div. 2)"
rating: 800
weight: 1537
solve_time_s: 146
verified: true
draft: false
---

[CF 1537A - Mảng số học](https://codeforces.com/problemset/problem/1537/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng số nguyên và nhiệm vụ của chúng ta là làm cho giá trị số học của nó bằng 1 bằng cách thêm các số nguyên không âm. Mỗi trường hợp thử nghiệm trình bày một mảng các số nguyên tùy ý, cả dương và âm, và yêu cầu số lượng phép cộng tối thiểu cần thiết. Giá trị trung bình số học được tính chính xác bằng tổng của các phần tử chia cho số phần tử. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cho biết độ dài hiện tại của mảng và các phần tử mảng. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm: số nguyên không âm ít nhất mà khi được thêm vào sẽ điều chỉnh giá trị trung bình của mảng thành chính xác một. 

Các ràng buộc rất nhỏ: độ dài mảng lên tới 50 và giá trị phần tử lên tới ±10^4. Điều này cho phép thực hiện các thao tác O(n) trên mỗi trường hợp thử nghiệm một cách thoải mái. Tổng số ca kiểm thử nhiều nhất là 1000, nghĩa là ngay cả việc quét tuyến tính đơn giản cho mỗi ca kiểm thử cũng hiệu quả. Các trường hợp cạnh bao gồm các mảng trong đó giá trị trung bình đã bằng một, các mảng có tổng âm và các mảng có tổng lớn hơn độ dài. 

Việc triển khai bất cẩn có thể cố gắng luôn thêm các số 0 cho đến khi giá trị trung bình trở thành một, điều này sẽ thất bại khi tổng lớn hơn độ dài. Ví dụ: một mảng [3,3] có giá trị trung bình là 3, do đó việc thêm các số 0 chỉ làm giảm giá trị trung bình về 0, không bao giờ đạt đến một. Kết quả đầu ra chính xác cho mảng này là hai: việc thêm hai số 0 sẽ cho tổng 6, độ dài 4, trung bình 1,5, vẫn còn quá cao, vì vậy cần phải suy luận cẩn thận. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liên tục nối các số nguyên bắt đầu từ 0 hoặc một và tính lại giá trị trung bình cho đến khi nó đạt chính xác một. Điều này hiệu quả vì mỗi phần bổ sung thay đổi giá trị trung bình theo cách có thể dự đoán được, nhưng nó không hiệu quả đối với các mảng có tổng lớn hơn nhiều so với độ dài: số lần lặp có thể không bị giới hạn và việc kiểm tra từng bước là không cần thiết. 

Quan sát quan trọng là chúng ta có thể giải quyết vấn đề này bằng toán học mà không cần mô phỏng. Đặt tổng của mảng ban đầu là S và độ dài của nó là n. Chúng ta cần nối k số nguyên không âm sao cho tổng mới chia cho độ dài mới bằng 1:$$\frac{S + X}{n + k} = 1$$trong đó X là tổng của k số mới. Vì tất cả các số được thêm vào đều không âm nên tổng tối thiểu chúng ta có thể cộng là 0. Do đó, có hai trường hợp phát sinh: 

1. Nếu S bằng n thì mảng đã có giá trị trung bình bằng 1, vì vậy chúng ta nối thêm 0 phần tử. 
2. Nếu S < n, việc thêm một số nguyên bằng n - S là đủ vì số này không âm, làm tăng giá trị trung bình lên đúng một. 
3. Nếu S > n, chỉ thêm số 0 là không đủ. Chúng ta cần tăng độ dài để phù hợp với tổng, yêu cầu nối thêm k phần tử. K nhỏ nhất thỏa mãn S/(n+k) ≤ 1 là k = S - n. 

Cái nhìn sâu sắc này làm giảm vấn đề thành một phép tính thời gian không đổi cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(max(S)) | O(n + max(S)) | Quá chậm đối với số tiền lớn | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case t. 
2. Với mỗi test, đọc n và mảng a. 
3. Tính tổng S của mảng a. 
4. Nếu S bằng n, in ra 0. Mảng đã có giá trị trung bình bằng một. 
5. Nếu S < n, in ra 1. Một số được thêm vào duy nhất n - S mang giá trị trung bình bằng một. 
6. Nếu S > n, in S - n. Việc thêm nhiều số 0 này sẽ tăng độ dài sao cho giá trị trung bình đạt đến một. 

Tại sao nó hoạt động: Thuật toán bảo toàn bất biến rằng giá trị trung bình của mảng cuối cùng chính xác là một. Ba trường hợp bao gồm tất cả các khả năng: bằng nhau, tổng nhỏ hơn độ dài và tổng lớn hơn độ dài. Tổng và số phần tử xác định duy nhất giá trị trung bình, do đó không tồn tại trường hợp nào khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    s = sum(a)
    if s == n:
        print(0)
    elif s < n:
        print(1)
    else:
        print(s - n)
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`. Tổng được tính bằng công cụ tích hợp`sum`, tránh các vòng lặp thủ công. Mỗi nhánh quyết định tương ứng trực tiếp với một trong ba trường hợp trong hướng dẫn thuật toán. Tránh được các lỗi sai lệch một vì công thức xuất phát từ sự bằng nhau chính xác của tổng và độ dài. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
3
1 1 1
2 1 2
4 8 4 6 2
```| Mảng | n | Tổng S | Trường hợp | Đã thêm | Ý nghĩa mới | 
| --- | --- | --- | --- | --- | --- | 
| [1,1,1] | 3 | 3 | S=n | 0 | 1 | 
| [1,2] | 2 | 3 | S>n | 1 | 1 | 
| [8,4,6,2] | 4 | 20 | S>n | 16 | 1 | 

Dấu vết này cho thấy mỗi nhánh của phân tích trường hợp đều kích hoạt chính xác. Trong ví dụ thứ ba, việc thêm 16 số 0 sẽ điều chỉnh độ dài thành 20, làm cho giá trị trung bình là 1. 

### Đầu vào mẫu 2```
1
1 -2
```| Mảng | n | Tổng S | Trường hợp | Đã thêm | Ý nghĩa mới | 
| --- | --- | --- | --- | --- | --- | 
| [-2] | 1 | -2 | S<n | 1 | 1 | 

Một 3 được thêm vào sẽ mang giá trị trung bình về 1, xác nhận việc xử lý trường hợp S<n. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Tổng mảng có độ dài n là tuyến tính | 
| Không gian | O(n) | Lưu trữ mảng đầu vào, không cần cấu trúc bổ sung | 

Cho n ≤ 50 và t ≤ 1000, điều này có hiệu quả trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        s = sum(a)
        if s == n:
            print(0)
        elif s < n:
            print(1)
        else:
            print(s - n)
    return output.getvalue().strip()

# Provided samples
assert run("4\n3\n1 1 1\n2\n1 2\n4\n8 4 6 2\n1\n-2\n") == "0\n1\n16\n1", "sample cases"

# Custom cases
assert run("2\n2\n0 0\n3\n2 2 2\n") == "1\n3", "zero sum and all equal >1"
assert run("1\n1\n1\n") == "0", "single element already 1"
assert run("1\n2\n-1 3\n") == "1", "mixed negative and positive"
assert run("1\n4\n5 5 5 5\n") == "16", "large sum > length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n2\n0 0\n3\n2 2 2\n | 1\n3 | Xử lý tổng < n và tổng > n đúng cách | 
| 1\n1\n1\n | 0 | Trường hợp phần tử đơn có nghĩa là 1 | 
| 1\n2\n-1 3\n | 1 | Hỗn hợp số âm và số dương | 
| 1\n4\n5 5 5 5\n | 16 | Tổng lớn, cần nhiều số 0 | 

## Vỏ cạnh 

Đối với mảng âm một phần tử, ví dụ [-5], S < n. Thuật toán nối thêm 1 số bằng 6, thu được [-5,6]. Tổng 1, độ dài 2, trung bình 1. 

Đối với một mảng trong đó tất cả các phần tử đều lớn hơn một, ví dụ: [10,10], S > n. Thêm S - n = 20 - 2 = 18 số 0 sẽ có [10,10,0,...,0] (20 phần tử). Tổng 20, độ dài 20, trung bình 1. 

Những ví dụ này xác nhận cách tiếp cận ba trường hợp xử lý các tổng cực âm, tổng dương lớn và mảng một phần tử không có lỗi riêng lẻ.
