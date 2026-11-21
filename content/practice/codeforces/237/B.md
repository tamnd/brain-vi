---
title: "CF 237B - Bàn trẻ"
description: "Chúng ta được cho một bảng số hình tam giác. Mỗi hàng có ít ô hơn hoặc bằng hàng phía trên nó, tạo thành một cấu trúc giống như hoạt cảnh Young. Mỗi ô chứa một số nguyên riêng biệt từ 1 đến tổng số ô."
date: "2026-06-04T16:56:30+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 237
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 147 (Div. 2)"
rating: 1500
weight: 237
solve_time_s: 183
verified: false
draft: false
---

[CF 237B - Bàn trẻ](https://codeforces.com/problemset/problem/237/B) 

**Đánh giá:** 1500 
**Tags:** triển khai, sắp xếp 
**Thời gian giải:** 3 phút 3 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một bảng số hình tam giác. Mỗi hàng có ít ô hơn hoặc bằng hàng phía trên nó, tạo thành một cấu trúc giống như hoạt cảnh Young. Mỗi ô chứa một số nguyên riêng biệt từ 1 đến tổng số ô. Mục đích là sắp xếp lại các số này sao cho mỗi hàng được sắp xếp theo thứ tự tăng dần từ trái sang phải và mỗi cột được sắp xếp theo thứ tự tăng dần từ trên xuống dưới. Việc hoán đổi có thể xảy ra giữa hai ô bất kỳ và chúng ta có thể thực hiện nhiều lần hoán đổi bằng tổng số ô. Đầu ra phải liệt kê các giao dịch hoán đổi theo thứ tự thực hiện. 

Các ràng buộc đầu vào ở mức vừa phải: số lượng hàng`n`nhiều nhất là 50 và mỗi hàng có tối đa 50 ô. Điều đó mang lại tổng số tối đa 2500 ô, có nghĩa là ngay cả thuật toán O(s²) cũng có thể chạy trong giới hạn thời gian. Vấn đề có thể quản lý được bằng cách sắp xếp và theo dõi vị trí đơn giản. Thách thức chính không phải là tốc độ mà là chuyển đổi chính xác bảng ban đầu thành cấu trúc được sắp xếp mục tiêu trong khi xuất ra các giao dịch hoán đổi. 

Trường hợp cạnh tinh vi phát sinh khi một hàng có ít ô hơn ô phía trên nó. Nếu chúng ta cố gắng sắp xếp theo cột mà không kiểm tra độ dài hàng, chúng ta có thể truy cập vào một ô không hợp lệ. Ví dụ, một bảng như`[[4, 3, 5], [6, 1], [2]]`phải tôn trọng hình dạng: cột 3 chỉ tồn tại ở hàng đầu tiên. Việc hoán đổi số một cách ngây thơ mà không xem xét đến sự tồn tại của cột có thể dẫn đến sai sót. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là liên tục tìm bất kỳ cặp số nào vi phạm thứ tự hàng hoặc cột và hoán đổi chúng cho đến khi toàn bộ bảng được sắp xếp. Điều này được đảm bảo hội tụ vì mỗi lần hoán đổi sẽ sửa được ít nhất một lần đảo ngược. Tuy nhiên, trường hợp xấu nhất có thể thực hiện hoán đổi O(s²), điều này phù hợp với s ≤ 2500 nhưng việc thực hiện sẽ phức tạp vì chúng ta sẽ phải quét bảng nhiều lần. 

Một cách tiếp cận có cấu trúc hơn là trước tiên hãy sắp xếp tất cả các số, sau đó đặt chúng vào bảng theo đúng thứ tự cho hoạt cảnh Young. Điều này hiệu quả vì chúng ta biết chính xác bảng được sắp xếp trông như thế nào: các số nhỏ nhất điền vào hàng đầu tiên, sau đó đến hàng thứ hai, v.v. Chúng ta có thể lặp lại các vị trí trong bảng theo thứ tự chuẩn này và bất cứ khi nào một ô không chứa số cần thiết, hãy hoán đổi nó với ô hiện đang chứa số đó. Bằng cách duy trì bản đồ từ mỗi số đến vị trí hiện tại của nó, chúng ta luôn có thể xác định vị trí số cần hoán đổi trong thời gian O(1). Mỗi số được di chuyển nhiều nhất một lần nên tổng số lần hoán đổi nhiều nhất là s. Cách tiếp cận này đơn giản, trực tiếp và dễ thực hiện mà không phải lo lắng về việc truy cập cột không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(s²) | O (các) | Chấp nhận được nhưng lộn xộn | 
| Tối ưu | O(s log s) | O (các) | Sạch sẽ và hiệu quả | 

## Hướng dẫn thuật toán 

1. Làm phẳng bảng thành một danh sách các bộ chứa`(value, row, column)`. Điều này cho phép chúng tôi xử lý các vị trí độc lập với độ dài hàng. 
2. Sắp xếp danh sách theo giá trị để xác định vị trí mục tiêu cho từng số. Thứ tự sắp xếp tương ứng với việc điền vào bảng theo từng hàng, từ trái qua phải. 
3. Xây dựng từ điển ánh xạ từng số tới vị trí hiện tại của nó`(row, column)`trong bảng. Điều này cho phép tra cứu O(1) vị trí của bất kỳ số nào. 
4. Lặp lại bảng theo thứ tự chuẩn: từng hàng, từ trái sang phải, bỏ qua các ô không tồn tại (do độ dài hàng). Đối với mỗi ô, hãy xác định số sẽ chiếm ô đó dựa trên danh sách đã sắp xếp. 
5. Nếu ô hiện tại đã chứa số chính xác, hãy chuyển sang ô tiếp theo. Nếu không, hãy tìm vị trí hiện tại của số được yêu cầu qua từ điển, hoán đổi hai số và cập nhật vị trí của chúng trong từ điển. Ghi lại sự hoán đổi này vào danh sách đầu ra. 
6. Tiếp tục cho đến khi tất cả các vị trí được điền chính xác. 

Tại sao nó hoạt động: mỗi lần hoán đổi sẽ di chuyển ít nhất một số vào đúng vị trí của nó. Từ điển đảm bảo rằng chúng ta có thể tìm thấy bất kỳ số nào để hoán đổi một cách hiệu quả. Thứ tự chuẩn đảm bảo rằng thứ tự cột và hàng được tự động tôn trọng vì các số được sắp xếp được gán từ trên cùng bên trái đến dưới cùng bên phải, tôn trọng hình dạng bảng tam giác. Không có trao đổi nào sẽ vi phạm thuộc tính tableau vì chúng tôi chỉ đặt các số vào vị trí được sắp xếp của chúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
c = list(map(int, input().split()))
table = [list(map(int, input().split())) for _ in range(n)]

# Flatten and sort
flattened = []
for i in range(n):
    for j in range(c[i]):
        flattened.append((table[i][j], i, j))
flattened.sort()  # sort by value

# Build current position map
pos = {}
for i in range(n):
    for j in range(c[i]):
        pos[table[i][j]] = (i, j)

swaps = []
index = 0
for i in range(n):
    for j in range(c[i]):
        correct_value = flattened[index][0]
        if table[i][j] != correct_value:
            ci, cj = pos[correct_value]
            # swap in table
            table[i][j], table[ci][cj] = table[ci][cj], table[i][j]
            # update positions
            pos[table[ci][cj]] = (ci, cj)
            pos[table[i][j]] = (i, j)
            swaps.append((i+1, j+1, ci+1, cj+1))
        index += 1

print(len(swaps))
for a, b, c_, d in swaps:
    print(a, b, c_, d)
```Đầu tiên chúng ta làm phẳng bảng để có thể sắp xếp và ánh xạ các con số một cách dễ dàng. Từ điển vị trí rất quan trọng: nó cho phép chúng tôi xác định vị trí bất kỳ số nào ngay lập tức, tránh việc quét không cần thiết. Chúng tôi lặp lại theo thứ tự chuẩn, đảm bảo rằng cả hàng và cột đều phát triển chính xác. Mỗi lần hoán đổi sẽ cập nhật cả bảng và ánh xạ, duy trì tính chính xác. các`+1`điều chỉnh là cần thiết vì vấn đề sử dụng chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Bảng đầu vào:```
Row 1: 4 3 5
Row 2: 6 1
Row 3: 2
```Làm phẳng và sắp xếp:`[1, 2, 3, 4, 5, 6]`Vị trí kinh điển:```
Row 1: 1 2 3
Row 2: 4 5
Row 3: 6
```Bảng theo dõi: 

| Bước | Ô (i,j) | Giá trị hiện tại | Giá trị đúng | Trao đổi với | Bảng sau khi trao đổi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | (0,0) | 4 | 1 | (1,1) | 1 3 5/6 4/2 | 
| 2 | (0,1) | 3 | 2 | (2,0) | 1 2 5/6 4/3 | 
| 3 | (0,2) | 5 | 3 | (2,1) | ... | 

Chỉ có hai hoán đổi thực sự cần thiết trong ví dụ tối thiểu: (1,1)-(2,2) và (2,1)-(3,1). Điều này chứng tỏ rằng thuật toán tạo ra một chuỗi hợp lệ để sắp xếp bảng trong khi vẫn tôn trọng các ràng buộc về hàng và cột. 

### Ví dụ tùy chỉnh 

đầu vào:```
2
2 1
2 1
3
```Làm phẳng và sắp xếp:`[1, 2, 3]`Vị trí kinh điển:```
Row 1: 1 2
Row 2: 3
```Hoán đổi: 

- Hoán đổi (1,1) và (1,2): 1 chuyển sang (1,1), 2 chuyển sang (1,2) 
- Hoán đổi (2,1) đã đúng rồi 

Trace xác nhận rằng việc sắp xếp theo hàng và cột hoạt động ngay cả khi độ dài hàng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(s log s) | Sắp xếp danh sách phẳng chiếm ưu thế; s 2500 | 
| Không gian | O (các) | Danh sách phẳng và bản đồ vị trí lưu trữ từng ô một lần | 

Điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ, ngay cả với kích thước bảng tối đa là 50x50. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    c = list(map(int, input().split()))
    table = [list(map(int, input().split())) for _ in range(n)]
    flattened = []
    for i in range(n):
        for j in range(c[i]):
            flattened.append((table[i][j], i, j))
    flattened.sort()
    pos = {}
    for i in range(n):
        for j in range(c[i]):
            pos[table[i][j]] = (i, j)
    swaps = []
    index = 0
    for i in range(n):
        for j in range(c[i]):
            correct_value = flattened[index][0]
            if table[i][j] != correct_value:
                ci, cj = pos[correct_value]
                table[i][j], table[ci][cj] = table[ci][cj], table[i][j]
                pos[table[ci][cj]] = (ci, cj)
                pos[table[i][j]] = (i, j)
                swaps.append((i+1, j+1, ci+1, cj+1))
            index += 1
    out = [str(len(swaps))]
    for a, b,
```
