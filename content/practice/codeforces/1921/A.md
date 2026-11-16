---
title: "CF 1921A - Vuông"
description: "Chúng ta có bốn đỉnh của một hình vuông trên mặt phẳng tọa độ 2D. Các điểm được trình bày theo thứ tự tùy ý nên chúng ta không biết điểm nào là góc dưới bên trái, góc trên bên phải, v.v. Hình vuông có hai tính chất đặc biệt."
date: "2026-06-08T19:21:57+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1921
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 920 (Div. 3)"
rating: 800
weight: 1921
solve_time_s: 117
verified: true
draft: false
---

[CF 1921A - Hình vuông](https://codeforces.com/problemset/problem/1921/A) 

**Đánh giá:** 800 
**Tags:** tham lam, toán học 
**Thời gian giải:** 1 phút 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bốn đỉnh của một hình vuông trên mặt phẳng tọa độ 2D. Các điểm được trình bày theo thứ tự tùy ý nên chúng ta không biết điểm nào là góc dưới bên trái, góc trên bên phải, v.v. 

Hình vuông có hai tính chất đặc biệt. Các cạnh của nó song song với các trục tọa độ và diện tích của nó hoàn toàn dương. Vì các cạnh được căn chỉnh theo trục nên các đỉnh của hình vuông phải sử dụng chính xác hai tọa độ x phân biệt và chính xác hai tọa độ y phân biệt. Độ dài cạnh đơn giản là khoảng cách giữa hai giá trị x, bằng khoảng cách giữa hai giá trị y. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải tính diện tích hình vuông. 

Những hạn chế là rất nhỏ. Có tối đa 100 ca kiểm thử và mỗi ca kiểm thử chỉ chứa 4 điểm. Bất kỳ thuật toán nào thực hiện vài chục thao tác cho mỗi trường hợp thử nghiệm sẽ chạy ngay lập tức. Hiệu quả không phải là thách thức ở đây. Thách thức là nhận ra cấu trúc hình học bị ẩn bởi thứ tự ngẫu nhiên của các đỉnh. 

Một lỗi phổ biến là cho rằng các điểm đến theo thứ tự hữu ích. Coi như:```
1 2
4 5
1 5
4 2
```Hai điểm đầu tiên là các góc đối diện theo đường chéo chứ không phải các góc liền kề. Việc tính toán khoảng cách giữa các điểm đầu vào liên tiếp sẽ tạo ra độ dài cạnh sai. 

Một trường hợp tinh tế khác là khi tọa độ âm:```
-1 1
1 -1
1 1
-1 -1
```Độ dài cạnh không thu được từ giá trị tọa độ lớn nhất. Chúng ta phải tính toán sự khác biệt giữa giá trị x tối đa và tối thiểu, đưa ra`1 - (-1) = 2`. 

Việc triển khai bất cẩn cũng có thể cố gắng sử dụng khoảng cách Euclide giữa các cặp điểm tùy ý. Ví dụ:```
17 11
17 39
45 11
45 39
```Độ dài đường chéo lớn hơn nhiều so với độ dài cạnh. Bình phương đường chéo sẽ không cho diện tích. Độ dài cạnh đúng là`45 - 17 = 28`, vậy diện tích là`28² = 784`. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ kiểm tra tất cả các cặp điểm và xác định cặp nào tạo thành các cạnh của hình vuông. Vì chỉ có bốn điểm nên có sáu cặp. Chúng ta có thể tìm khoảng cách ngang hoặc dọc nhỏ nhất khác 0 và sử dụng nó làm độ dài cạnh. Điều này hoạt động vì đầu vào được đảm bảo mô tả một hình vuông thẳng hàng theo trục hợp lệ. 

Mặc dù cách tiếp cận này đã đủ nhanh nhưng nó vẫn phức tạp hơn mức cần thiết. Chúng tôi đang giải một bài toán hình học có cấu trúc đơn giản hơn nhiều so với bài toán tìm bình phương tổng quát. 

Quan sát quan trọng là một hình vuông thẳng hàng có chính xác hai tọa độ x và hai tọa độ y riêng biệt. Độ dài cạnh đơn giản là:```
max(x) - min(x)
```Vì hình được đảm bảo là hình vuông nên giá trị tương tự cũng bằng:```
max(y) - min(y)
```Một khi chúng ta biết chiều dài cạnh, diện tích là hình vuông của nó. 

Điều này biến vấn đề thành trích xuất giá trị x tối thiểu và tối đa từ bốn điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Đã chấp nhận | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

Độ phức tạp tiệm cận là như nhau vì kích thước đầu vào cho mỗi trường hợp thử nghiệm là cố định, nhưng cách tiếp cận tối ưu rõ ràng hơn nhiều và sử dụng trực tiếp hình học của các ô vuông thẳng hàng với trục. 

## Hướng dẫn thuật toán 

1. Đọc bốn điểm của test case hiện tại. 
2. Lưu trữ tất cả tọa độ x trong danh sách. 
3. Tính giá trị x tối thiểu và tối đa. 
4. Tính độ dài cạnh như sau`max_x - min_x`. 

Vì hình vuông được căn chỉnh theo trục nên mỗi đỉnh nằm trên một trong đúng hai đường thẳng đứng. Sự phân tách của chúng là chiều dài cạnh. 
5. Tính diện tích là`side_length * side_length`. 
6. Xuất diện tích. 

### Tại sao nó hoạt động 

Đối với hình vuông thẳng hàng với trục, cạnh trái và cạnh phải nằm trên hai giá trị x riêng biệt. Mỗi đỉnh thuộc về một trong hai đường thẳng đó. Khoảng cách theo chiều ngang giữa chúng chính xác là độ dài cạnh của hình vuông. 

Bởi vì đầu vào được đảm bảo tạo thành một hình vuông hợp lệ,`max(x) - min(x)`luôn dương và bằng độ dài cạnh hình vuông. Bình phương giá trị đó sẽ cho diện tích. Không cần tính toán hình học khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    xs = []

    for _ in range(4):
        x, y = map(int, input().split())
        xs.append(x)

    side = max(xs) - min(xs)
    print(side * side)
```Giải pháp đọc bốn điểm cho mỗi trường hợp thử nghiệm và chỉ giữ lại tọa độ x của chúng. Vì hình vuông được đảm bảo thẳng hàng theo trục nên độ dài cạnh được xác định bởi khoảng cách theo chiều ngang giữa đỉnh ngoài cùng bên trái và đỉnh ngoài cùng bên phải. 

sử dụng`max(xs) - min(xs)`tránh mọi sự phụ thuộc vào thứ tự đầu vào. Cho dù các đỉnh được liệt kê theo chiều kim đồng hồ, ngược chiều kim đồng hồ hay xáo trộn hoàn toàn thì các giá trị x cực trị vẫn giữ nguyên. 

Tất cả số học phù hợp thoải mái bên trong các loại số nguyên tiêu chuẩn. Phạm vi tọa độ tối đa là 1000 độ lớn, do đó chiều dài cạnh lớn nhất có thể là 2000 và diện tích lớn nhất có thể là 4.000.000. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Điểm đầu vào:```
(1,2)
(4,5)
(1,5)
(4,2)
```| Bước | xs | phút_x | max_x | bên | khu vực | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi đọc điểm | [1, 4, 1, 4] | 1 | 4 | 3 | 9 | 

Đầu ra:```
9
```Ví dụ này cho thấy tại sao thứ tự điểm không quan trọng. Các điểm không được cho xung quanh chu vi của hình vuông, tuy nhiên các giá trị x cực trị ngay lập tức cho biết chiều dài cạnh. 

### Ví dụ 2 

Điểm đầu vào:```
(-1,1)
(1,-1)
(1,1)
(-1,-1)
```| Bước | xs | phút_x | max_x | bên | khu vực | 
| --- | --- | --- | --- | --- | --- | 
| Sau khi đọc điểm | [-1, 1, 1, -1] | -1 | 1 | 2 | 4 | 

Đầu ra:```
4
```Ví dụ này chứng minh rằng tọa độ âm không yêu cầu xử lý đặc biệt. Sự khác biệt giữa giá trị x tối đa và tối thiểu vẫn tạo ra độ dài cạnh chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Bốn điểm được xử lý cho mỗi trường hợp thử nghiệm | 
| Không gian | O(1) | Chỉ một số số nguyên được lưu trữ | 

Số lượng điểm là cố định nên mỗi trường hợp kiểm thử yêu cầu một lượng công việc và bộ nhớ không đổi. Với tối đa 100 trường hợp thử nghiệm, giải pháp sẽ chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        xs = []
        for _ in range(4):
            x, y = map(int, input().split())
            xs.append(x)

        side = max(xs) - min(xs)
        ans.append(str(side * side))

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""3
1 2
4 5
1 5
4 2
-1 1
1 -1
1 1
-1 -1
45 11
45 39
17 11
17 39
"""
) == "9\n4\n784"

# side length 1
assert run(
"""1
0 0
1 0
0 1
1 1
"""
) == "1"

# negative coordinates
assert run(
"""1
-3 -3
-1 -3
-3 -1
-1 -1
"""
) == "4"

# large coordinate range
assert run(
"""1
-1000 -1000
1000 -1000
-1000 1000
1000 1000
"""
) == "4000000"

# shuffled order
assert run(
"""1
5 2
2 2
5 5
2 5
"""
) == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn vị hình vuông có cạnh 1 | 1 | Diện tích dương nhỏ nhất | 
| Hình vuông tọa độ âm | 4 | Xử lý đúng các giá trị âm | 
| Tọa độ tại giới hạn | 4000000 | Giá trị biên và diện tích lớn nhất | 
| Thứ tự đỉnh ngẫu nhiên | 9 | Độc lập với thứ tự đầu vào | 

## Vỏ cạnh 

Xét hình vuông:```
1
0 0
1 0
0 1
1 1
```Các giá trị x là`[0, 1, 0, 1]`. Thuật toán tính toán`max_x = 1`,`min_x = 0`, cho độ dài cạnh là`1`. Diện tích là`1`. Đây là hình vuông hợp lệ nhỏ nhất có thể vì diện tích phải dương. 

Hãy xem xét một hình vuông hoàn toàn ở tọa độ âm:```
1
-3 -3
-1 -3
-3 -1
-1 -1
```Các giá trị x là`[-3, -1, -3, -1]`. Độ dài cạnh trở thành`-1 - (-3) = 2`, và diện tích là`4`. Phép trừ tự động xử lý các giá trị âm một cách chính xác. 

Hãy xem xét một thứ tự đầu vào được xáo trộn:```
1
4 5
1 2
4 2
1 5
```Các điểm không được liệt kê xung quanh ranh giới. Thuật toán bỏ qua thứ tự và chỉ sử dụng các giá trị x cực trị. Nó tìm thấy`max_x = 4`Và`min_x = 1`, tạo ra chiều dài cạnh`3`và diện tích`9`. 

Hãy xem xét trải rộng tọa độ lớn nhất được phép:```
1
-1000 -1000
1000 -1000
-1000 1000
1000 1000
```Độ dài cạnh là`1000 - (-1000) = 2000`, và diện tích là`4,000,000`. Việc tính toán vẫn là phép trừ và nhân số nguyên đơn giản, trong giới hạn.
