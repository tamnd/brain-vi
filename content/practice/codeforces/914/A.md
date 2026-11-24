---
title: "CF 914A - Hình vuông hoàn hảo"
description: "Chúng ta được cho một mảng các số nguyên và cần tìm phần tử lớn nhất không phải là số chính phương hoàn hảo. Số bình phương hoàn hảo là số có thể biểu diễn dưới dạng bình phương của một số nguyên. Đầu vào bao gồm một số n, kích thước của mảng, theo sau là các phần tử mảng."
date: "2026-06-13T01:24:24+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "A"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 900
weight: 914
solve_time_s: 278
verified: true
draft: false
---

[CF 914A - Hình vuông hoàn hảo](https://codeforces.com/problemset/problem/914/A) 

**Xếp hạng:** 900 
**Tags:** vũ lực, thực hiện, toán học 
**Thời gian giải:** 4 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và cần tìm phần tử lớn nhất **không phải là hình vuông hoàn hảo**. Số bình phương hoàn hảo là số có thể biểu diễn dưới dạng bình phương của một số nguyên. Đầu vào bao gồm một số _n_, kích thước của mảng, theo sau là các phần tử mảng. Đầu ra là một số nguyên duy nhất: phần tử lớn nhất trong mảng không phải là hình vuông hoàn hảo. 

Các ràng buộc rất khiêm tốn: _n_ có thể lên tới 1000 và các phần tử có thể nằm trong khoảng từ -10^6 đến 10^6. Các số âm không bao giờ có thể là số chính phương hoàn hảo nên chúng ta biết ngay chúng là ứng cử viên. Giới hạn trên của _n_ có nghĩa là chúng ta có thể kiểm tra từng số riêng lẻ theo thời gian tuyến tính mà không gặp vấn đề về hiệu suất. Phạm vi số cũng cho phép chúng ta tính căn bậc hai một cách an toàn bằng số học số nguyên. 

Trường hợp cạnh phát sinh khi mảng chứa hỗn hợp các số bình phương lớn và các số không phải là số chính phương nhỏ hơn. Ví dụ, đầu vào`5\n1 4 9 16 2`chỉ có 2 là số không vuông lớn nhất. Một cách tiếp cận ngây thơ chỉ đơn giản là chọn số lớn nhất mà không kiểm tra các ô vuông sẽ trả về 16, điều này không chính xác. Số âm và số 0 cần được cẩn thận: số 0 là số chính phương và số âm không bao giờ là số chính phương hoàn hảo. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Chúng tôi lặp qua từng phần tử của mảng và kiểm tra xem nó có phải là một hình vuông hoàn hảo hay không. Nếu không, chúng tôi sẽ theo dõi giá trị tối đa được thấy cho đến nay. Việc kiểm tra xem một số có phải là số chính phương hay không có thể được thực hiện bằng cách lấy căn bậc hai của số nguyên và bình phương lại để xem nó có bằng số ban đầu hay không. Vì có tối đa 1000 phần tử, mỗi phần tử yêu cầu kiểm tra căn bậc hai theo thời gian không đổi nên tổng số thao tác là khoảng 1000, không đáng kể. Do đó, vũ lực có thể được chấp nhận trong thực tế, mặc dù đối với các mảng lớn hơn, nó có thể trở nên không hiệu quả. 

Thông tin chi tiết quan trọng là chúng ta không cần sắp xếp mảng hoặc lưu trữ các cấu trúc dữ liệu bổ sung. Một đường chuyền tuyến tính duy nhất là đủ. Với mỗi phần tử, chúng ta tính căn bậc hai số nguyên bằng cách sử dụng`math.isqrt`chức năng, bình phương lại và so sánh với số ban đầu. Nếu chúng không bằng nhau thì số đó không phải là số chính phương hoàn hảo và chúng ta so sánh nó với số lớn nhất hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Vì cách tiếp cận tối ưu và bạo lực thực sự giống nhau trong bài toán này nên trọng tâm là xác định chính xác các ô không bình phương. 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`best`đến âm vô cùng. Điều này sẽ lưu trữ không vuông lớn nhất được tìm thấy cho đến nay. 
2. Lặp qua từng phần tử`x`của mảng. 
3. Nếu`x`là âm, nó không thể là một hình vuông hoàn hảo. So sánh nó với`best`và cập nhật`best`nếu như`x`lớn hơn. 
4. Nếu`x`không âm, hãy tính`y = int(sqrt(x))`sử dụng`math.isqrt(x)`. Kiểm tra xem`y * y == x`. Nếu không,`x`không phải là một hình vuông hoàn hảo. 
5. Nếu`x`không phải là một hình vuông hoàn hảo, hãy so sánh nó với`best`và cập nhật`best`nếu như`x`lớn hơn. 
6. Sau khi xử lý tất cả các phần tử, xuất ra`best`. 

**Tại sao nó hoạt động:** Ở mỗi bước, chúng tôi duy trì tính bất biến`best`là số lớn nhất được thấy cho đến nay và không phải là một hình vuông hoàn hảo. Bởi vì chúng tôi kiểm tra mọi phần tử và xác định chính xác các hình vuông hoàn hảo, giá trị cuối cùng của`best`được đảm bảo là số không vuông lớn nhất. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

best = -float('inf')
for x in a:
    if x < 0:
        best = max(best, x)
    else:
        y = math.isqrt(x)
        if y * y != x:
            best = max(best, x)

print(best)
```Giải pháp đầu tiên đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Chúng tôi khởi tạo`best`đến âm vô cùng để xử lý số âm một cách chính xác. Đối với mỗi phần tử, số âm tự động là ứng cử viên. Các số không âm được kiểm tra xem có phải là số bình phương hoàn hảo bằng cách sử dụng căn bậc hai số nguyên để tránh điểm không chính xác của dấu phẩy động. Cuối cùng,`best`chứa số không vuông lớn nhất. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
2
4 2
```| Bước | x | là hình vuông hoàn hảo? | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 4 | vâng | -inf | 
| 2 | 2 | không | 2 | 

Đầu ra:`2`. Điều này xác nhận thuật toán bỏ qua chính xác các ô vuông hoàn hảo và chọn ô vuông lớn nhất. 

**Ví dụ 2** 

đầu vào:```
5
1 4 9 16 2
```| Bước | x | là hình vuông hoàn hảo? | tốt nhất | 
| --- | --- | --- | --- | 
| 1 | 1 | vâng | -inf | 
| 2 | 4 | vâng | -inf | 
| 3 | 9 | vâng | -inf | 
| 4 | 16 | vâng | -inf | 
| 5 | 2 | không | 2 | 

Đầu ra:`2`. Điều này chứng tỏ thuật toán xử lý chính xác nhiều ô vuông và xác định ô vuông tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phần tử trong số n phần tử được kiểm tra một lần;`math.isqrt`là O(1) | 
| Không gian | O(1) | Chỉ có một biến duy nhất`best`được sử dụng bên cạnh việc lưu trữ đầu vào | 

Với n 1000, thuật toán này chạy trong khoảng dưới 1 mili giây, thấp hơn nhiều so với giới hạn 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    a = list(map(int, input().split()))
    best = -float('inf')
    for x in a:
        if x < 0:
            best = max(best, x)
        else:
            y = math.isqrt(x)
            if y * y != x:
                best = max(best, x)
    return str(best)

# provided samples
assert run("2\n4 2\n") == "2", "sample 1"

# custom cases
assert run("5\n1 4 9 16 2\n") == "2", "all squares except one"
assert run("3\n-1 0 1\n") == "-1", "negative numbers handled"
assert run("4\n2 3 5 7\n") == "7", "all non-squares"
assert run("1\n0\n") == "-inf", "single element, square zero"  # would never happen due to guarantee
assert run("6\n-100 -36 0 1 2 3\n") == "3", "mixed negative and positive"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5\n1 4 9 16 2`| 2 | bỏ qua nhiều ô vuông hoàn hảo một cách chính xác | 
|`3\n-1 0 1`| -1 | xử lý số âm chính xác | 
|`4\n2 3 5 7`| 7 | không phải là hình vuông lớn nhất được chọn trong số tất cả các hình không phải là hình vuông | 
|`6\n-100 -36 0 1 2 3`| 3 | hỗn hợp âm, số không và hình vuông | 

## Vỏ cạnh 

Đối với đầu vào`-1 0 1`,`best`bắt đầu ở âm vô cực.`-1`là âm, vì vậy`best`trở thành -1.`0`là một hình vuông hoàn hảo, bị bỏ qua.`1`là một hình vuông hoàn hảo, bị bỏ qua. Đầu ra là -1, đúng. Thuật toán không bao giờ phân loại sai số âm hoặc số 0. 

Đối với đầu vào`2 3 5 7`, tất cả các số đều không phải là số chính phương. Thuật toán cập nhật`best`ở mỗi bước, kết thúc bằng 7. Điều này xác nhận rằng các chuỗi không có hình vuông được xử lý một cách tự nhiên. 

Điều này xác nhận rằng giải pháp xác định chính xác hình không vuông lớn nhất trong tất cả các kịch bản cạnh.
