---
title: "CF 106458F - \u0420\u0430\u0441\u0441\u0442\u043e\u044f\u043d\u0438\u0435 \u043c\u0435\u0436\u0434\u0443 \u0442\u043e\u0447\u043a\u0430\u043c\u0438"
description: "Nhiệm vụ là tính khoảng cách đường thẳng giữa hai điểm trên mặt phẳng tọa độ. Đầu vào mô tả tọa độ của hai điểm, mỗi điểm được biểu thị bằng tọa độ x và tọa độ y. Đầu ra là độ dài của đoạn nối hai điểm này."
date: "2026-06-25T09:12:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106458
codeforces_index: "F"
codeforces_contest_name: "\u041a\u043e\u0433\u043d\u0438\u0442\u0438\u0432\u043d\u044b\u0435 \u0442\u0435\u0445\u043d\u043e\u043b\u043e\u0433\u0438\u0438 2023-2024. \u041f\u0435\u0440\u0432\u044b\u0439 \u043e\u0442\u0431\u043e\u0440"
rating: 0
weight: 106458
solve_time_s: 27
verified: true
draft: false
---

[CF 106458F - \u0420\u0430\u0441\u0441\u0442\u043e\u044f\u043d\u0438\u0435 \u043c\u0435\u0436\u0434\u0443 \u0442\u043e\u0447\u043a\u0430\u043c\u0438](https://codeforces.com/problemset/problem/106458/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là tính khoảng cách đường thẳng giữa hai điểm trên mặt phẳng tọa độ. Đầu vào mô tả tọa độ của hai điểm, mỗi điểm được biểu thị bằng tọa độ x và tọa độ y. Đầu ra là độ dài của đoạn nối hai điểm này. 

Đối với điểm`(x1, y1)`Và`(x2, y2)`, sự khác biệt theo chiều ngang là`x2 - x1`và sự khác biệt theo chiều dọc là`y2 - y1`. Hai điểm khác biệt này tạo thành hai chân của một tam giác vuông, do đó khoảng cách cần thiết là cạnh huyền:$$\sqrt{(x_2-x_1)^2+(y_2-y_1)^2}$$Các ràng buộc tuy nhỏ nhưng vẫn xác định được các chi tiết triển khai chính xác. Không cần bất kỳ cấu trúc dữ liệu tìm kiếm, sắp xếp hoặc hình học nào vì câu trả lời có được từ một số phép tính số học không đổi. Mối quan tâm chính là độ chính xác về mặt số học, vì kết quả thường không phải là số nguyên. 

Các trường hợp đặc biệt rất đơn giản nhưng dễ xử lý sai nếu công thức được triển khai không chính xác. 

Cho hai điểm giống nhau:```
0 0
0 0
```câu trả lời là:```
0.0
```Một giải pháp chỉ tính toán một sai số tọa độ cũng sẽ vượt qua nhiều thử nghiệm nhưng có thể thất bại ở các đầu vào chung. 

Đối với các điểm nằm trên đường thẳng đứng:```
5 1
5 6
```câu trả lời là:```
5.0
```Hiệu x bằng 0, nhưng khoảng cách vẫn được xác định bởi hiệu y. 

Đối với các điểm nằm chéo:```
0 0
3 4
```câu trả lời là:```
5.0
```Việc triển khai bất cẩn làm tăng thêm sự khác biệt về tọa độ thay vì sử dụng công thức Euclide sẽ tạo ra`7`, đó là khoảng cách Manhattan chứ không phải khoảng cách được yêu cầu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực thường có nghĩa là cố gắng tìm kiếm một số khuôn mẫu hoặc liệt kê các khả năng. Đối với bài toán này, bản năng đó là không cần thiết vì chỉ có một khoảng cách duy nhất giữa hai điểm đã cho. Bất kỳ phép liệt kê nào cũng sẽ chỉ lặp lại công việc mà công thức hình học đã giải trực tiếp. 

Quan sát toán học là sự khác biệt tọa độ tạo ra các cạnh của một tam giác vuông. Định lý Pythagore quy đổi hai chuyển động độc lập này thành độ dài của đoạn đường chéo. Vấn đề chuyển từ một nhiệm vụ hình học sang việc đánh giá một công thức cố định. 

Ý tưởng brute-force không có ý nghĩa ở đây, bởi vì ngay cả một vòng lặp không cần thiết cũng sẽ tăng thêm độ phức tạp mà không cung cấp thông tin mới. Việc tính toán trực tiếp là thời gian không đổi và là giải pháp mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) tùy thuộc vào việc liệt kê không cần thiết | O(1) | Quá chậm về mặt khái niệm và không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ của hai điểm. Chúng tôi lưu trữ bốn giá trị vì câu trả lời phụ thuộc vào sự khác biệt giữa các tọa độ tương ứng. 
2. Tính chuyển vị ngang bằng cách sử dụng`x2 - x1`và chuyển vị dọc bằng cách sử dụng`y2 - y1`. Dấu hiệu không quan trọng vì cả hai giá trị sẽ bình phương. 
3. Bình phương cả hai hiệu và cộng chúng lại. Điều này cho chiều dài bình phương của đoạn. 
4. Lấy căn bậc hai của tổng. Giá trị kết quả là khoảng cách Euclide giữa các điểm. 

Tại sao nó hoạt động: sự khác biệt về tọa độ tạo ra một tam giác vuông trong đó đoạn giữa các điểm là cạnh huyền. Định lý Pythagore phát biểu rằng bình phương cạnh huyền bằng tổng bình phương của hai cạnh huyền. Vì hai nhánh chính xác là chênh lệch x và y nên giá trị tính toán luôn là khoảng cách cần thiết. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    x1, y1 = map(float, input().split())
    x2, y2 = map(float, input().split())

    dx = x2 - x1
    dy = y2 - y1

    ans = math.sqrt(dx * dx + dy * dy)

    print(ans)

if __name__ == "__main__":
    solve()
```Chương trình đầu tiên đọc cả hai điểm. Tọa độ được lưu dưới dạng giá trị dấu phẩy động vì khoảng cách cuối cùng có thể chứa phần phân số. 

Các biến`dx`Và`dy`biểu diễn hai cạnh của tam giác vuông tạo bởi các điểm. Bình phương chúng sẽ loại bỏ bất kỳ dấu âm nào, do đó thứ tự của hai điểm không ảnh hưởng đến câu trả lời. 

biểu hiện`math.sqrt(dx * dx + dy * dy)`trực tiếp theo công thức toán học. Số dấu phẩy động của Python là đủ cho độ chính xác mà bài toán này yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
3 4
```| Bước | x1 | y1 | x2 | y2 | dx | nhuộm | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc điểm | 0 | 0 | 3 | 4 | - | - | - | 
| Tính toán sự khác biệt | 0 | 0 | 3 | 4 | 3 | 4 | - | 
| Áp dụng công thức | 0 | 0 | 3 | 4 | 3 | 4 | sqrt(9 + 16) = 5 | 

Ví dụ minh họa tam giác 3-4-5 tiêu chuẩn. Thuật toán chỉ cần hai tọa độ khác nhau để khôi phục khoảng cách. 

### Ví dụ 2 

đầu vào:```
5 1
5 6
```| Bước | x1 | y1 | x2 | y2 | dx | nhuộm | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| Đọc điểm | 5 | 1 | 5 | 6 | - | - | - | 
| Tính toán sự khác biệt | 5 | 1 | 5 | 6 | 0 | 5 | - | 
| Áp dụng công thức | 5 | 1 | 5 | 6 | 0 | 5 | sqrt(0 + 25) = 5 | 

Dấu vết này kiểm tra trường hợp đường thẳng đứng trong đó một sai lệch tọa độ biến mất hoàn toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Giải pháp thực hiện một số phép tính số học cố định. | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ. | 

Thuật toán không phụ thuộc vào kích thước của tọa độ hoặc bất kỳ phép lặp ẩn nào. Nó dễ dàng phù hợp với mọi giới hạn thời gian và bộ nhớ hợp lý. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solution(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    import math
    x1, y1 = map(float, sys.stdin.readline().split())
    x2, y2 = map(float, sys.stdin.readline().split())

    ans = math.sqrt((x2 - x1) ** 2 + (y2 - y1) ** 2)
    result = str(ans)

    sys.stdin = old_stdin
    return result

# provided-style samples
assert solution("0 0\n3 4\n") == "5.0", "sample 1"
assert solution("1 1\n1 1\n") == "0.0", "sample 2"

# custom cases
assert solution("5 1\n5 6\n") == "5.0", "vertical line"
assert solution("-1 -1\n2 3\n") == "5.0", "negative coordinates"
assert solution("1000000 1000000\n1000000 999999\n") == "1.0", "large boundary values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`Và`3 4`|`5.0`| Khoảng cách đường chéo cơ bản | 
|`1 1`Và`1 1`|`0.0`| Xử lý điểm giống nhau | 
|`5 1`Và`5 6`|`5.0`| Không có sự khác biệt theo chiều ngang | 
|`-1 -1`Và`2 3`|`5.0`| Tọa độ âm | 
|`1000000 1000000`Và`1000000 999999`|`1.0`| Giá trị tọa độ lớn | 

## Vỏ cạnh 

Đối với các điểm giống nhau:```
0 0
0 0
```thuật toán tính toán`dx = 0`Và`dy = 0`. Công thức trở thành`sqrt(0 + 0)`, sản xuất`0.0`. Không cần xử lý đặc biệt vì công thức chung đã bao gồm trường hợp này. 

Đối với đoạn thẳng đứng:```
5 1
5 6
```thuật toán tính toán`dx = 0`Và`dy = 5`. Bình phương khoảng cách là`25`, vậy câu trả lời là`sqrt(25) = 5.0`. Ở đây, giải pháp chia cho một chênh lệch tọa độ hoặc giả sử cả hai tọa độ đều thay đổi sẽ không thành công. 

Đối với đoạn chéo:```
0 0
3 4
```thuật toán tính toán`dx = 3`Và`dy = 4`. Bình phương khoảng cách là`9 + 16 = 25`, đưa ra câu trả lời của`5.0`. Điều này xác nhận rằng giải pháp sử dụng khoảng cách Euclide thay vì chỉ thêm các thay đổi tọa độ.
