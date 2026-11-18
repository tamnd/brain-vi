---
title: "CF 2031F - Penchick và số trung vị chẵn"
description: "Vấn đề ban đầu mang tính tương tác: có một hoán vị ẩn và chúng ta phải xác định vị trí của hai giá trị ở giữa, cụ thể là $frac n2$ và $frac n2 + 1$, bằng cách sử dụng truy vấn trung vị. Đối với phiên bản hack được sử dụng trong kho lưu trữ, tính tương tác sẽ bị loại bỏ."
date: "2026-06-08T11:53:50+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "interactive", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 2031
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 987 (Div. 2)"
rating: 2800
weight: 2031
solve_time_s: 153
verified: false
draft: false
---

[CF 2031F - Penchick và Trung vị chẵn](https://codeforces.com/problemset/problem/2031/F) 

**Đánh giá:** 2800 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, tương tác, xác suất 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề ban đầu mang tính tương tác: có một hoán vị ẩn và chúng ta phải xác định vị trí của hai giá trị ở giữa, cụ thể là$\frac n2$Và$\frac n2 + 1$, sử dụng truy vấn trung vị. 

Đối với phiên bản hack được sử dụng trong kho lưu trữ, tính tương tác sẽ bị loại bỏ. Toàn bộ hoán vị được đưa ra trực tiếp trong đầu vào. 

Mỗi test chứa một số nguyên chẵn$n$và một hoán vị của các số từ$1$ĐẾN$n$. Chúng ta phải xuất ra hai chỉ số có giá trị chính xác$\frac n2$Và$\frac n2 + 1$. Thứ tự của hai chỉ số không quan trọng. 

Những hạn chế là rất nhỏ. Tổng của tất cả$n$nhiều nhất là các giá trị trên tệp thử nghiệm$10^4$. Thậm chí một$O(n^2)$giải pháp sẽ vượt qua một cách thoải mái, nhưng cấu trúc của đầu vào cho phép quét tuyến tính trực tiếp. 

Một lỗi phổ biến là xuất ra các giá trị thay vì vị trí của chúng. 

Ví dụ, với```
n = 6
p = [6, 2, 3, 5, 1, 4]
```các giá trị ở giữa là$3$Và$4$. Vị trí của họ là$3$Và$6$, vì vậy kết quả đúng là:```
3 6
```không```
3 4
```Một lỗi dễ mắc phải khác là quên rằng hoán vị được lập chỉ mục từ 1 trong câu lệnh, trong khi mảng Python được lập chỉ mục từ 0. Giá trị if$3$xuất hiện tại`p[2]`, đáp án là vị trí`3`. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là kiểm tra mọi vị trí và kiểm tra xem giá trị có bằng không$\frac n2$hoặc$\frac n2 + 1$. Vì chỉ có hai giá trị đích nên chúng ta có thể lưu trữ vị trí của chúng khi gặp chúng. 

Vì mảng đã là một hoán vị nên mỗi giá trị đích xuất hiện chính xác một lần. Một lần quét là đủ. 

Phiên bản tương tác ban đầu yêu cầu một chiến lược phức tạp liên quan đến các truy vấn trung bình, nhưng không có truy vấn nào tồn tại ở định dạng hack. Khi hoán vị được đưa ra một cách rõ ràng, toàn bộ nhiệm vụ sẽ giảm xuống việc định vị hai giá trị đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

Trong trường hợp này, quá trình quét brute-force đã là tối ưu. 

## Hướng dẫn thuật toán 

1. Đọc$n$. 
2. Đọc hoán vị$p$. 
3. Hãy để`a = n // 2`Và`b = a + 1`. 
4. Quét hoán vị từ trái sang phải. 
5. Khi giá trị`a`được tìm thấy, lưu trữ vị trí dựa trên 1 của nó. 
6. Khi giá trị`b`được tìm thấy, lưu trữ vị trí dựa trên 1 của nó. 
7. Xuất ra hai vị trí đã lưu. 

### Tại sao nó hoạt động 

Hoán vị chứa mọi số nguyên từ$1$ĐẾN$n$đúng một lần. 

Các giá trị$\frac n2$Và$\frac n2 + 1$mỗi cái xảy ra ở đúng một vị trí. Trong quá trình quét, chúng tôi ghi lại những vị trí duy nhất đó. Vì mỗi phần tử được kiểm tra một lần nên cả hai vị trí đều được tìm thấy và kết quả đầu ra chính xác là cặp chỉ số mà bài toán yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))
        
        x = n // 2
        y = x + 1
        
        pos_x = pos_y = -1
        
        for i, v in enumerate(p, start=1):
            if v == x:
                pos_x = i
            elif v == y:
                pos_y = i
        
        print(pos_x, pos_y)

solve()
```Mã đầu tiên tính toán hai giá trị đích. Vì thuộc tính hoán vị đảm bảo tính duy nhất nên chúng ta chỉ cần một biến cho mỗi vị trí. 

các`enumerate(..., start=1)`cuộc gọi là quan trọng. Vấn đề yêu cầu các chỉ số dựa trên 1, vì vậy sử dụng`start=1`tránh phải chuyển đổi thêm sau này. 

Quá trình quét truy cập mọi phần tử chính xác một lần. Bất cứ khi nào gặp một trong các giá trị mục tiêu, vị trí của nó sẽ được ghi lại. Sau khi vòng lặp kết thúc, cả hai vị trí đều được biết và có thể được in ngay lập tức. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hoán vị đầu vào:```
n = 6
p = [6, 2, 3, 5, 1, 4]
```Các giá trị mục tiêu là$3$Và$4$. 

| Vị trí | Giá trị | vị trí(3) | vị trí(4) | 
| --- | --- | --- | --- | 
| 1 | 6 | -1 | -1 | 
| 2 | 2 | -1 | -1 | 
| 3 | 3 | 3 | -1 | 
| 4 | 5 | 3 | -1 | 
| 5 | 1 | 3 | -1 | 
| 6 | 4 | 3 | 6 | 

Đầu ra:```
3 6
```Ví dụ này cho thấy việc giải thích trực tiếp của nhiệm vụ. Các giá trị$3$Và$4$được đặt tại các vị trí$3$Và$6$. 

### Ví dụ 2 

Hoán vị đầu vào:```
n = 10
p = [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
```Các giá trị mục tiêu là$5$Và$6$. 

| Vị trí | Giá trị | vị trí(5) | vị trí(6) | 
| --- | --- | --- | --- | 
| 1 | 10 | -1 | -1 | 
| 2 | 9 | -1 | -1 | 
| 3 | 8 | -1 | -1 | 
| 4 | 7 | -1 | -1 | 
| 5 | 6 | -1 | 5 | 
| 6 | 5 | 6 | 5 | 
| 7 | 4 | 6 | 5 | 
| 8 | 3 | 6 | 5 | 
| 9 | 2 | 6 | 5 | 
| 10 | 1 | 6 | 5 | 

Đầu ra:```
6 5
```Thứ tự không quan trọng, vì vậy`5 6`cũng sẽ được chấp nhận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét hoán vị | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Vì tổng số tiền của$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^4$, quét tuyến tính thấp hơn nhiều so với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        p = list(map(int, input().split()))

        a = n // 2
        b = a + 1

        pa = pb = -1

        for i, v in enumerate(p, start=1):
            if v == a:
                pa = i
            elif v == b:
                pb = i

        out.append(f"{pa} {pb}")

    return "\n".join(out)

# sample from statement
assert run(
"""2
6
6 2 3 5 1 4
10
10 9 8 7 6 5 4 3 2 1
"""
) == "3 6\n6 5"

# minimum n
assert run(
"""1
6
1 2 3 4 5 6
"""
) == "3 4"

# reversed minimum n
assert run(
"""1
6
6 5 4 3 2 1
"""
) == "4 3"

# middle values at ends
assert run(
"""1
8
4 1 2 3 5 6 7 8
"""
) == "1 5"

# larger case
assert run(
"""1
12
12 1 2 3 4 5 7 8 9 10 11 6
"""
) == "12 7"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`6 / 1 2 3 4 5 6`|`3 4`| Kích thước hợp lệ nhỏ nhất | 
|`6 / 6 5 4 3 2 1`|`4 3`| Hoán vị ngược thứ tự | 
|`8 / 4 1 2 3 5 6 7 8`|`1 5`| Mục tiêu ở vị trí cực đoan | 
|`12 / 12 1 2 3 4 5 7 8 9 10 11 6`|`12 7`| Hoán vị lớn hơn | 

## Vỏ cạnh 

Hãy xem xét hoán vị```
6
6 5 4 3 2 1
```Các giá trị mục tiêu là$3$Và$4$. Chúng xuất hiện ở những vị trí$4$Và$3$. Một giải pháp tự xuất ra các giá trị sẽ in`3 4`, điều đó là sai. Quá trình quét trả về chính xác các vị trí`4 3`. 

Coi như```
8
4 1 2 3 5 6 7 8
```Các giá trị mục tiêu là$4$Và$5$. Một cái xuất hiện ở vị trí đầu tiên và cái còn lại ở vị trí thứ năm. Việc sử dụng nhầm các chỉ số dựa trên số 0 sẽ tạo ra`0 4`. Thuật toán sử dụng`enumerate(..., start=1)`, do đó nó đưa ra câu trả lời đúng`1 5`. 

Coi như```
10
10 9 8 7 6 5 4 3 2 1
```Hai giá trị mục tiêu liền kề nhau về giá trị nhưng không ở vị trí. Thuật toán không dựa vào bất kỳ giả định thứ tự nào. Nó chỉ đơn giản ghi lại nơi mỗi giá trị đích xảy ra và trả về`6 5`, điều đó đúng.
