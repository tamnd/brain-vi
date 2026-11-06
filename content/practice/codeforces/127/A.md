---
title: "CF 127A - Lãng phí thời gian"
description: "Scrooge ký giấy tờ bằng cách di chuyển bút dọc theo đường polyline. Chữ ký bắt đầu ở điểm đầu tiên, sau đó vẽ các đoạn thẳng giữa các điểm liên tiếp cho đến điểm cuối cùng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 127
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 93 (Div. 2 Only)"
rating: 900
weight: 127
solve_time_s: 99
verified: true
draft: false
---

[CF 127A - Lãng phí thời gian](https://codeforces.com/problemset/problem/127/A) 

**Xếp hạng:** 900 
**Thẻ:** hình học 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Scrooge ký giấy tờ bằng cách di chuyển bút dọc theo đường polyline. Chữ ký bắt đầu ở điểm đầu tiên, sau đó vẽ các đoạn thẳng giữa các điểm liên tiếp cho đến điểm cuối cùng. Tốc độ bút của anh ấy được cố định ở mức 50 mm mỗi giây và anh ấy lặp lại chính xác chữ ký đó.`k`lần. 

Nhiệm vụ là tính tổng thời gian dành cho việc ký tất cả các giấy tờ. 

Đầu vào đưa ra các điểm của đa tuyến theo thứ tự. Mỗi cặp điểm liên tiếp tạo thành một đoạn chữ ký. Tổng quãng đường mà cây bút đi được là tổng độ dài Euclide của tất cả các đoạn đó. Khi chúng tôi biết tổng khoảng cách cho một chữ ký, chúng tôi nhân nó với`k`và chia cho tốc độ viết. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 điểm nên có nhiều nhất là 99 đoạn. Ngay cả những tính toán đắt tiền cũng dễ dàng nằm gọn trong giới hạn. Chỉ cần quét tuyến tính đơn giản trên các phân đoạn là đủ. Các bài toán hình học đôi khi yêu cầu tối ưu hóa cẩn thận hoặc các thủ thuật chính xác, nhưng ở đây các con số nhỏ và công thức là trực tiếp. 

Rủi ro thực hiện chính là xử lý chính xác. Khoảng cách giữa các điểm thường không phải là số nguyên vì căn bậc hai. Việc sử dụng số học số nguyên sẽ âm thầm cắt bớt các giá trị và tạo ra các câu trả lời sai. 

Hãy xem xét đầu vào này:```
2 1
0 0
1 1
```Độ dài đoạn là`sqrt(2) ≈ 1.41421356`. Thời gian đúng là:```
1.41421356 / 50 ≈ 0.028284271
```Nếu một giải pháp vô tình sử dụng phép chia số nguyên hoặc chuyển đổi khoảng cách thành số nguyên trước khi chia, nó sẽ in`0`, điều đó không đúng. 

Một lỗi dễ mắc phải khác là quên nhân với`k`. 

Ví dụ:```
2 5
0 0
10 0
```Một chữ ký có độ dài`10`, do đó cần một chữ ký`10 / 50 = 0.2`giây. Vì có 5 bài nên đáp án đúng là:```
1.0
```Việc triển khai bất cẩn chỉ tính toán một chữ ký sẽ tạo ra`0.2`. 

Trường hợp tinh vi thứ ba là tọa độ âm. Khoảng cách phụ thuộc vào sự khác biệt tọa độ, không phụ thuộc vào vị trí tuyệt đối. 

Ví dụ:```
2 1
-3 -4
0 0
```Khoảng cách vẫn còn`5`, bởi vì:```
sqrt((0 - (-3))^2 + (0 - (-4))^2) = sqrt(9 + 16) = 5
```Sử dụng trực tiếp các giá trị tọa độ tuyệt đối thay vì khác biệt sẽ phá vỡ các trường hợp như vậy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu đã đủ nhanh ở đây. Chúng ta có thể xử lý từng đoạn một cách độc lập, tính độ dài Euclide của nó bằng công thức khoảng cách và tính tổng tất cả các độ dài lại với nhau. 

Cho hai điểm liên tiếp`(x1, y1)`Và`(x2, y2)`, độ dài đoạn là:$d=\sqrt{(x_2-x_1)^2+(y_2-y_1)^2}$Kéo các điểm để cập nhật khoảng cách giữa hai điểm.-10-8-6-4-2246810-10-5510A(6.0, 6.0)B(-6.0, -6.0)d = 16.97Delta x = 12Delta y = 12 

Sau khi tổng hợp tất cả các độ dài đoạn, chúng tôi nhân với`k`vì chữ ký giống nhau được lặp lại`k`lần. Cuối cùng, chúng tôi chia cho tốc độ viết`50`. 

Phương pháp brute-force hoạt động vì đường dẫn chữ ký được đưa ra rõ ràng dưới dạng các phân đoạn liên tiếp. Không có cấu trúc ẩn nào để xây dựng lại và không cần tối ưu hóa các phép tính lặp đi lặp lại. Mỗi đoạn đóng góp độc lập vào tổng khoảng cách. 

Đôi khi các vấn đề về hình học khiến mọi người suy nghĩ quá nhiều về các giao lộ hoặc các con đường chồng chéo nhau. Không có vấn đề gì ở đây. Ngay cả khi đường polyline cắt chính nó hoặc vẽ lại các đoạn trước đó, Scrooge vẫn di chuyển bút dọc theo toàn bộ đường dẫn, do đó mọi đoạn phải được tính chính xác như đã vẽ. 

Cách tiếp cận tối ưu giống hệt với cách tiếp cận bạo lực vì các ràng buộc quá nhỏ nên một đường truyền tuyến tính duy nhất đã là tối ưu. Chúng ta chỉ cần duyệt qua đường đa tuyến một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`k`. 
2. Đọc tất cả`n`các điểm của đa tuyến theo thứ tự. 
3. Khởi tạo một biến`total_distance = 0.0`. 
4. Lặp lại các cặp điểm liên tiếp. 

Đối với mọi`i`từ`0`ĐẾN`n - 2`, lấy điểm:```
(x[i], y[i]) and (x[i+1], y[i+1])
```5. Tính khoảng cách Euclide giữa hai điểm. 

Công thức khoảng cách đo độ dài chính xác của đoạn thẳng nối chúng. 
6. Thêm độ dài đoạn này vào`total_distance`. 

Sau khi xử lý tất cả các cặp,`total_distance`bằng toàn bộ chuyển động của bút cho một chữ ký. 
7. Nhân lên`total_distance`qua`k`. 

Chữ ký ở mọi thời điểm đều giống nhau nên tổng quãng đường di chuyển tỷ lệ tuyến tính với số lượng giấy tờ. 
8. Chia kết quả cho`50`. 

Tốc độ bút là 50 mm mỗi giây, vì vậy:```
time = distance / speed
```9. In câu trả lời dưới dạng số dấu phẩy động. 

### Tại sao nó hoạt động 

Mỗi chữ ký bao gồm chính xác các đoạn giữa các điểm liên tiếp. Thuật toán xử lý từng đoạn đó một lần và tính toán độ dài hình học chính xác của nó bằng công thức khoảng cách Euclide. 

Bất biến trong suốt vòng lặp là sau khi xử lý dữ liệu đầu tiên`i`phân đoạn,`total_distance`bằng tổng chuyển động của bút dọc theo các đoạn đó. Vì mỗi phân đoạn đóng góp độc lập và không có phân đoạn nào bị bỏ qua hoặc được tính hai lần nên tổng cuối cùng chính xác là độ dài của một chữ ký hoàn chỉnh. 

nhân với`k`đưa ra tổng khoảng cách cho tất cả các chữ ký và chia cho tốc độ không đổi sẽ chuyển khoảng cách thành thời gian. Bởi vì tính toán khớp trực tiếp với quá trình vật lý được mô tả trong bài toán nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

# solution

def solve():
    n, k = map(int, input().split())

    points = [tuple(map(int, input().split())) for _ in range(n)]

    total_distance = 0.0

    for i in range(n - 1):
        x1, y1 = points[i]
        x2, y2 = points[i + 1]

        dx = x2 - x1
        dy = y2 - y1

        total_distance += math.hypot(dx, dy)

    total_time = (total_distance * k) / 50.0

    print(f"{total_time:.9f}")

solve()
```Giải pháp tuân theo thuật toán trực tiếp. Đầu tiên, nó đọc tất cả các điểm vào một danh sách để có thể truy cập dễ dàng các điểm liên tiếp. 

Bên trong vòng lặp,`dx`Và`dy`lưu trữ sự khác biệt tọa độ giữa các điểm lân cận. chức năng`math.hypot(dx, dy)`tính toán:```
sqrt(dx^2 + dy^2)
```Điều này an toàn và sạch sẽ hơn so với việc viết công thức căn bậc hai theo cách thủ công. 

Bộ tích lũy`total_distance`là giá trị dấu phẩy động vì độ dài đoạn thường không phải là số nguyên. 

Phép nhân với`k`xảy ra sau khi toàn bộ chiều dài chữ ký được tính toán. Điều này tránh việc lặp lại các phép tính tương tự một cách không cần thiết. 

Bản in cuối cùng sử dụng 9 chữ số sau dấu thập phân, đáp ứng thoải mái độ chính xác cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
0 0
10 0
```Dấu vết: 

| Bước | Phân khúc hiện tại | Độ dài đoạn | tổng_khoảng cách | 
| --- | --- | --- | --- | 
| Ban đầu | không | 0 | 0 | 
| 1 | (0,0) → (10,0) | 10 | 10 | 

Tính toán cuối cùng: 

| Giá trị | Kết quả | 
| --- | --- | 
| tổng_khoảng cách | 10 | 
| k | 1 | 
| tổng_thời gian | 10/50 = 0,2 | 

Đầu ra:```
0.200000000
```Ví dụ này xác nhận trường hợp đơn giản nhất có thể, một đoạn ngang. 

### Ví dụ 2 

đầu vào:```
3 2
0 0
3 4
6 8
```Dấu vết: 

| Bước | Phân khúc hiện tại | Độ dài đoạn | tổng_khoảng cách | 
| --- | --- | --- | --- | 
| Ban đầu | không | 0 | 0 | 
| 1 | (0,0) → (3,4) | 5 | 5 | 
| 2 | (3,4) → (6,8) | 5 | 10 | 

Tính toán cuối cùng: 

| Giá trị | Kết quả | 
| --- | --- | 
| tổng_khoảng cách | 10 | 
| k | 2 | 
| tổng_thời gian | 20/50 = 0,4 | 

Đầu ra:```
0.400000000
```Dấu vết này chứng tỏ rằng thuật toán tích lũy chính xác nhiều phân đoạn trước khi chia tỷ lệ theo`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi phân đoạn được xử lý chính xác một lần | 
| Không gian | O(1) | Chỉ một số biến được sử dụng ngoài bộ lưu trữ đầu vào | 

Với tối đa 100 điểm, thuật toán thực hiện ít hơn 100 phép tính khoảng cách. Điều này là không đáng kể đối với giới hạn thời gian và việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k = map(int, input().split())

    points = [tuple(map(int, input().split())) for _ in range(n)]

    total_distance = 0.0

    for i in range(n - 1):
        x1, y1 = points[i]
        x2, y2 = points[i + 1]

        total_distance += math.hypot(x2 - x1, y2 - y1)

    total_time = (total_distance * k) / 50.0

    return f"{total_time:.9f}"

# provided sample
assert run(
"""2 1
0 0
10 0
"""
) == "0.200000000", "sample 1"

# minimum-size input
assert run(
"""2 1
0 0
0 1
"""
) == "0.020000000", "minimum case"

# diagonal distance
assert run(
"""2 1
0 0
3 4
"""
) == "0.100000000", "3-4-5 triangle"

# multiple segments and repeated signatures
assert run(
"""3 2
0 0
3 4
6 8
"""
) == "0.400000000", "multiple segments"

# negative coordinates
assert run(
"""2 1
-3 -4
0 0
"""
) == "0.100000000", "negative coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 0 0 / 0 1`|`0.020000000`| Đầu vào hợp lệ tối thiểu | 
|`2 1 / 0 0 / 3 4`|`0.100000000`| Khoảng cách Euclide chính xác | 
|`3 2 / 0 0 / 3 4 / 6 8`|`0.400000000`| Nhiều phân đoạn và nhân với`k`| 
|`2 1 / -3 -4 / 0 0`|`0.100000000`| Xử lý đúng tọa độ âm | 

## Vỏ cạnh 

Hãy xem xét trường hợp độ dài đoạn là không hợp lý. 

đầu vào:```
2 1
0 0
1 1
```Thuật toán tính toán:```
dx = 1
dy = 1
distance = sqrt(2)
```Sau đó:```
time = sqrt(2) / 50
```Bởi vì việc triển khai sử dụng số học dấu phẩy động và`math.hypot`, không có độ chính xác nào bị mất khi cắt bớt số nguyên. 

Bây giờ hãy xem xét các chữ ký lặp lại. 

đầu vào:```
2 5
0 0
10 0
```Vòng lặp tính toán một khoảng cách chữ ký duy nhất của`10`. Sau vòng lặp:```
total_distance = 10
```Sau đó:```
total_time = (10 * 5) / 50 = 1
```Phép nhân xảy ra sau khi tính toán độ dài đường dẫn đầy đủ, do đó mọi chữ ký đều được tính chính xác. 

Cuối cùng, hãy xem xét tọa độ âm. 

đầu vào:```
2 1
-3 -4
0 0
```Thuật toán tính toán:```
dx = 3
dy = 4
distance = 5
```Chỉ có sự khác biệt về tọa độ mới quan trọng, vì vậy vị trí của đường đa tuyến trong mặt phẳng không ảnh hưởng đến độ chính xác. Câu trả lời cuối cùng là:```
5 / 50 = 0.1
```
