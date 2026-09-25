---
title: "CF 104820C - \u041e\u0446\u0435\u043d\u043e\u0447\u043d\u043e\u0435"
description: "Chúng ta có hai hệ thống chấm điểm: một hệ thống sử dụng điểm từ 1 đến n, hệ thống kia sử dụng điểm từ 1 đến m. Một số điểm trong hệ thống thứ nhất được coi là tương đương với một số điểm trong hệ thống thứ hai, nhưng bài toán không cho ta các cặp điểm tùy ý."
date: "2026-06-28T12:54:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104820
codeforces_index: "C"
codeforces_contest_name: "\u0420\u0421\u041e-\u0410\u043b\u0430\u043d\u0438\u044f 2018-2023. \u0418\u0437\u0431\u0440\u0430\u043d\u043d\u043e\u0435"
rating: 0
weight: 104820
solve_time_s: 65
verified: true
draft: false
---

[CF 104820C - \u041e\u0446\u0435\u043d\u043e\u0447\u043d\u043e\u0435](https://codeforces.com/problemset/problem/104820/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai hệ thống chấm điểm: một hệ thống sử dụng điểm từ 1 đến n, hệ thống kia sử dụng điểm từ 1 đến m. Một số điểm trong hệ thống thứ nhất được coi là tương đương với một số điểm trong hệ thống thứ hai, nhưng bài toán không cho ta các cặp điểm tùy ý. Thay vào đó, cấu trúc mà các mẫu ngụ ý là sự tương đương hoạt động giống như một tỷ lệ thống nhất giữa hai hệ thống: các giá trị tương ứng với cùng một “mức chất lượng” tương đối sẽ căn chỉnh bất cứ khi nào chúng đại diện cho cùng một phần của mức tối đa. 

Từ quan điểm này, điểm x trong thang đo m tương ứng với một số điểm y trong thang đo n nếu cả hai đều đại diện cho cùng một vị trí chuẩn hóa, nghĩa là x / m = y / n. Một cặp như vậy tồn tại chính xác khi x là bội số của m / gcd(n, m), bởi vì lưới phân số chung giữa hai thang đo được xác định bởi ước chung lớn nhất của chúng. 

Nhiệm vụ giảm xuống việc đếm có bao nhiêu số nguyên x trong phạm vi [1, m] thực sự xuất hiện trong lưới chia sẻ này. 

Các ràng buộc lên tới 10^18 cho cả n và m, do đó, bất kỳ giải pháp nào lặp lại các giá trị hoặc xây dựng mảng đều không thể thực hiện được. Ngay cả O(min(n, m)) cũng quá lớn. Lời giải phải rút gọn bài toán thành một số không đổi các phép tính số học trên các số nguyên lớn. 

Trường hợp cạnh tinh tế xuất hiện khi n và m là số nguyên tố cùng nhau. Khi đó, các điểm chuẩn hóa được chia sẻ duy nhất là điểm cuối 0 và 1 ở dạng phân số, chỉ tương ứng với giá trị m của chính nó trong thang đo m. Ví dụ: với n = 3 và m = 4, chỉ có một giá trị khớp. 

Một trường hợp cạnh khác là khi n = m. Trong trường hợp đó, mọi điểm đều khớp với chính nó, vì vậy câu trả lời là m. Một cách giải thích ngây thơ chỉ tính “các điểm nội bộ được ánh xạ chính xác” sẽ bỏ sót các điểm cuối một cách không chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng mô phỏng trực tiếp các giá trị tương đương, chúng ta sẽ cần kiểm tra mọi x trong [1, m] xem có tồn tại y trong [1, n] sao cho x / m = y / n hay không. Việc kiểm tra điều kiện này yêu cầu giảm hợp lý hoặc quét tất cả các ứng cử viên. Ngay cả một lần vượt qua m lên tới 10^18 cũng là không thể. 

Quan sát chính là sự bằng nhau của các phân số x / m và y / n bao hàm phép nhân chéo: x * n = y * m. Bất kỳ cặp hợp lệ nào cũng tương ứng với một điểm hợp lý được chia sẻ trên cả hai lưới. Tập hợp tất cả các điểm như vậy chính xác là mạng được tạo bởi cấu trúc bội chung nhỏ nhất của n và m. 

Khoảng cách giữa các giá trị x khớp liên tiếp là m / gcd(n, m). Điều này xuất phát từ việc viết lại đẳng thức dưới dạng x = k * (m / gcd(n, m)), trong đó k nằm trên tất cả các số nguyên sao cho y tương ứng nằm trong [1, n]. Vì k có thể nằm trong khoảng từ 1 đến gcd(n, m), nên có chính xác gcd(n, m) giá trị hợp lệ của x. 

Do đó, vấn đề quy về việc tính toán gcd(n, m), tính toán trực tiếp có bao nhiêu điểm tương đương tồn tại trong thang đo m. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bảng liệt kê Brute Force của x trong [1, m] | O(m) | O(1) | Quá chậm | 
| Tối ưu sử dụng cấu trúc gcd | O(log min(n, m)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc hai số nguyên n và m. Chúng xác định hai thang đo riêng biệt mà chúng tôi so sánh thông qua căn chỉnh tỷ lệ. 
2. Tính g = gcd(n,m). Điều này nắm bắt kích thước bước tối đa để duy trì sự cân bằng tỷ lệ giữa hai thang đo. 
3. Nhận biết rằng mỗi điểm so khớp hợp lệ trong thang m phải có dạng k* (m/g), trong đó k là số nguyên từ 1 đến g. 
4. Kết luận số điểm hợp lệ đúng là g. 

Lý do phép liệt kê này hoạt động là vì việc chia cả n và m cho gcd của chúng sẽ làm giảm phân số n/m xuống số hạng thấp nhất. Các điểm căn chỉnh giữa hai chuỗi xảy ra chính xác ở bội số của bước rút gọn này. 

### Tại sao nó hoạt động

Mọi sự tương ứng hợp lệ đều thỏa mãn x/m = y/n. Sắp xếp lại sẽ có x * n = y * m. Cho g = gcd(n, m), và viết n = g * n' và m = g * m' trong đó n' và m' là nguyên tố cùng nhau. Phương trình trở thành x * n' = y * m'. Vì n' và m' không có thừa số chung nên x phải chia hết cho m' và y phải chia hết cho n'. Điều này buộc x = k * m' và y = k * n' đối với một số nguyên k. Các ràng buộc 1 ∼ x ∼ m và 1 ∼ y ₫ n ngụ ý k nằm trong khoảng chính xác từ 1 đến g, tạo ra g giá trị hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import gcd

n, m = map(int, input().split())
print(gcd(n, m))
```Toàn bộ lời giải dựa trên việc tính ước chung lớn nhất. Việc triển khai mô-đun toán học sử dụng thuật toán Euclide hiệu quả, phù hợp với các giá trị lên tới 10^18. 

Điều tinh tế duy nhất là không cần chuẩn hóa bổ sung hoặc xử lý ranh giới. Cả hai điểm cuối đều được bao gồm ngầm vì việc tính gcd đã tính đến các điểm căn chỉnh đầy đủ, bao gồm cả điểm tối đa m khi nó tương ứng với n. 

## Ví dụ đã hoạt động 

### Ví dụ 1: n = 5, m = 10 

Chúng ta tính g = gcd(5, 10) = 5. 

| Bước | Giá trị | 
| --- | --- | 
| n | 5 | 
| m | 10 | 
| gcd(n, m) | 5 | 
| trả lời | 5 | 

Năm điểm trùng khớp tương ứng với các phân số 1/5, 2/5, 3/5, 4/5 và 5/5, tương ứng với 2, 4, 6, 8 và 10 trong thang đo 10. Điều này xác nhận rằng tất cả các mức tỷ lệ cách đều nhau đều được tính. 

### Ví dụ 2: n = 3, m = 4 

Chúng ta tính g = gcd(3, 4) = 1. 

| Bước | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 4 | 
| gcd(n, m) | 1 | 
| trả lời | 1 | 

Chỉ điểm cuối 4 tương ứng với phân số chia sẻ hợp lệ (1), do đó chỉ tồn tại một điểm trong thang đo m. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log min(n, m)) | Tính toán gcd Euclide làm giảm bài toán theo các bước logarit | 
| Không gian | O(1) | chỉ một số số nguyên được lưu trữ | 

Giới hạn đầu vào lên tới 10^18 làm cho các phép tính số học trở thành phương pháp khả thi duy nhất và tính toán gcd phù hợp một cách thoải mái trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io
from math import gcd

def solve():
    n, m = map(int, sys.stdin.readline().split())
    print(gcd(n, m))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    sys.stdin = old_stdin
    return out.getvalue().strip()

assert run("5 10") == "5"
assert run("1 1") == "1"
assert run("3 4") == "1"
assert run("6 9") == "3"
assert run("10 1000000000000000000") == "10"
assert run("7 13") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 10 | 5 | căn chỉnh tỷ lệ cơ bản | 
| 1 1 | 1 | trường hợp tối thiểu | 
| 3 4 | 1 | trường hợp đồng nguyên tố | 
| 6 9 | 3 | gcd không tầm thường | 
| 10 10^18 | 10 | căng thẳng mất cân bằng lớn | 

## Vỏ cạnh 

Khi n = m, gcd bằng n, nên đáp án là n. Ví dụ: đầu vào 7 7 tạo ra gcd(7, 7) = 7, nghĩa là tất cả bảy điểm trong thang đo m tương ứng trực tiếp với các vị trí giống hệt nhau trong thang đo n. 

Khi n và m là nguyên tố cùng nhau, chẳng hạn như 7 và 13, gcd là 1. Thuật toán trả về 1, phản ánh rằng chỉ các điểm cuối thẳng hàng trong không gian chuẩn hóa. Việc tính toán không cần bất kỳ xử lý đặc biệt nào vì thuật toán Euclide giảm cặp này một cách tự nhiên. 

Khi một giá trị cực kỳ lớn và giá trị còn lại nhỏ, chẳng hạn như 10 và 10^18, gcd vẫn nắm bắt được cấu trúc đầy đủ. Thuật toán Euclide giảm nhanh 10^18 modulo 10, để lại gcd 10.
