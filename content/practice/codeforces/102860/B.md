---
title: "CF 102860B - Hình tam giác và hình tròn"
description: "Chúng ta được cho một số điểm được đánh dấu trên chu vi của một vòng tròn. Vòng tròn có chu vi L và mỗi điểm được biểu thị bằng khoảng cách theo chiều kim đồng hồ tính từ điểm bắt đầu đã chọn."
date: "2026-07-25T14:18:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "B"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 35
verified: true
draft: false
---

[CF 102860B - Hình tam giác và hình tròn](https://codeforces.com/problemset/problem/102860/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 35s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta được cho một số điểm được đánh dấu trên chu vi của một vòng tròn. Hình tròn có chu vi`L`và mọi điểm được biểu thị bằng khoảng cách theo chiều kim đồng hồ tính từ điểm bắt đầu đã chọn. Chúng ta cần đếm có bao nhiêu lựa chọn khác nhau về ba điểm được đánh dấu tạo thành một tam giác chứa tâm của đường tròn hoặc nằm hoàn toàn bên trong tam giác hoặc chính xác ở một trong các cạnh của nó. 

Một thực tế hình học quan trọng là ba đỉnh của một tam giác nội tiếp trong một đường tròn chỉ không chứa tâm khi cả ba đỉnh đều nằm bên trong một hình bán nguyệt mở nào đó. Nếu các điểm trải rộng trên hơn một nửa đường tròn thì tâm phải nằm trong tam giác. Nếu hai điểm hoàn toàn đối diện nhau thì tâm nằm trên đường viền của tam giác nên các tam giác đó cũng phải được tính. 

Đầu vào cho đến`300000`điểm. Với kích thước đó, việc kiểm tra từng bộ ba là không thể vì có khoảng`n^3 / 6`các hình tam giác có thể. Thậm chí`O(n^2)`hoạt động quá lớn khi`n`đang ở gần giới hạn. Giải pháp phải xử lý các điểm theo thứ tự được sắp xếp và sử dụng quét tuyến tính hoặc gần tuyến tính. 

Những trường hợp phức tạp xuất phát từ sự khác biệt giữa hình bán nguyệt mở và hình bán nguyệt đóng. Một tam giác có các đỉnh cách nhau đúng nửa đường tròn là hợp lệ vì tâm nằm trên đường viền của nó. Một giải pháp sử dụng`<= L / 2`khi tìm kiếm các hình tam giác không hợp lệ sẽ loại bỏ những trường hợp này không chính xác. 

Ví dụ:```
3 10
0 5 6
```Câu trả lời là:```
1
```Các điểm tại`0`Và`5`là hai đầu đối diện của một đường kính. Tam giác có tâm ở cạnh nối hai điểm đó. Xử lý khoảng cách`5`là một phần của hình bán nguyệt xấu sẽ tính tam giác này là không hợp lệ một cách không chính xác. 

Một trường hợp cạnh khác là khi tất cả các điểm tập trung bên trong một cung nhỏ.```
4 20
0 1 2 3
```Câu trả lời là:```
0
```Mọi tam giác có thể đều nằm trong một hình bán nguyệt mở, vì vậy không có tam giác nào chứa tâm. Việc thực hiện bất cẩn mà chỉ kiểm tra các điểm liền kề hoặc chỉ một hướng xung quanh vòng tròn có thể bỏ sót những trường hợp này. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi bộ ba điểm có thể có. Đối với mỗi bộ ba, chúng ta có thể tính toán xem tâm có nằm trong tam giác hay không bằng cách sử dụng hình học hoặc kiểm tra tương tự xem các điểm có khớp với hình bán nguyệt nào đó hay không. Điều này đúng vì mọi tam giác có thể đều được xem xét, nhưng có`C(n,3)`gấp ba lần. Với`n = 300000`, đại khái là vậy`4.5 * 10^15`kiểm tra, vượt xa mọi giới hạn thực tế. 

Quan sát quan trọng là việc đếm các hình tam giác chứa tâm trực tiếp khó hơn việc đếm nhóm đối diện. Một tam giác không hợp lệ khi tất cả các đỉnh của nó nằm trong một hình bán nguyệt mở. Sau khi sắp xếp các điểm xung quanh vòng tròn, chúng ta có thể đếm những hình tam giác không hợp lệ đó một cách hiệu quả. 

Sửa điểm đầu tiên của hình bán nguyệt như vậy. Giả sử chúng ta đang đứng ở điểm`i`và nhìn theo chiều kim đồng hồ. Nếu có`k`điểm sau hoàn toàn ít hơn`L / 2`đi, sau đó mỗi cặp trong số đó`k`điểm tạo ra một tam giác không hợp lệ với điểm`i`. Điều này mang lại`k * (k - 1) / 2`hình tam giác không hợp lệ bắt đầu từ`i`. 

Câu hỏi duy nhất còn lại là liệu cùng một tam giác không hợp lệ có được tính nhiều lần hay không. Đối với một tam giác bên trong hình bán nguyệt mở, điểm đầu tiên của hình bán nguyệt đó là duy nhất. Nếu ba điểm là`a < b < c`theo thứ tự vòng tròn và`c - a < L / 2`, sau đó bắt đầu từ`b`sẽ yêu cầu quấn quanh vòng tròn và khoảng cách được bao bọc đó lớn hơn`L / 2`. Vì vậy, mỗi tam giác không hợp lệ được tính chính xác một lần. 

Câu trả lời là tổng số hình tam giác trừ đi những hình không hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Sắp xếp tất cả tọa độ điểm theo chiều kim đồng hồ. Nhân đôi mảng đã sắp xếp bằng cách nối thêm mọi tọa độ cộng`L`. Phần trùng lặp thể hiện việc đi vòng quanh vòng tròn thêm một lần nữa, điều này cho phép chúng ta sử dụng chuyển động hai con trỏ thông thường thay vì xử lý các trường hợp bao quanh theo cách thủ công. 
2. Với mọi điểm gốc`i`, di chuyển con trỏ sang phải cho đến khi đạt khoảng cách từ`i`đạt ít nhất`L / 2`. Các điểm giữa`i + 1`Và`right - 1`chính xác là những điểm có thể tham gia`i`bên trong một hình bán nguyệt mở. 
3. Nếu có`k`những điểm như vậy, thêm`k * (k - 1) / 2`đến số lượng tam giác không hợp lệ. Đây là tất cả các cặp điểm kết hợp với`i`để tạo ra một hình tam giác không thể chứa tâm. 
4. Tính tổng số hình tam giác có thể có,`n * (n - 1) * (n - 2) / 6`và trừ số lượng không hợp lệ. 

Bất biến đằng sau quá trình quét hai con trỏ là đối với mọi điểm bắt đầu`i`, con trỏ bên phải luôn đánh dấu vị trí đầu tiên nằm ngoài hình bán nguyệt mở được phép. Vì cả hai con trỏ chỉ di chuyển về phía trước nên mỗi cặp điểm được xem xét theo thứ tự sắp xếp đúng một lần. Đối số đếm chứng minh rằng mọi tam giác không hợp lệ đều có một đỉnh bắt đầu duy nhất, do đó phép trừ sẽ loại bỏ chính xác các tam giác không mong muốn. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, L = map(int, input().split())
    x = list(map(int, input().split()))
    x.sort()

    a = x + [v + L for v in x]

    bad = 0
    r = 0

    for i in range(n):
        if r < i + 1:
            r = i + 1
        while r < i + n and a[r] - a[i] < L / 2:
            r += 1
        cnt = r - i - 1
        bad += cnt * (cnt - 1) // 2

    total = n * (n - 1) * (n - 2) // 6
    print(total - bad)

if __name__ == "__main__":
    solve()
```Đầu vào được sắp xếp trước vì thứ tự vòng tròn là thông tin duy nhất cần thiết cho bài kiểm tra hình bán nguyệt. Mảng trùng lặp cho phép mọi tìm kiếm được viết dưới dạng khoảng tăng dần đơn giản. 

Con trỏ`r`không bao giờ bị dịch chuyển về phía sau. Đối với mỗi vị trí bắt đầu, nó sẽ tìm điểm đầu tiên không nằm hoàn toàn bên trong hình bán nguyệt. Việc so sánh chặt chẽ là cần thiết vì điểm chính xác`L / 2`ngoài việc tạo các hình tam giác hợp lệ với tâm trên đường viền. 

giá trị`cnt`là số cách lựa chọn sau`i`vẫn có thể vừa với hình bán nguyệt mở tương tự. Việc chọn bất kỳ hai trong số chúng sẽ cho một tam giác không hợp lệ, đó là lý do tại sao phần đóng góp là`cnt * (cnt - 1) // 2`. 

Số nguyên Python không bị tràn, do đó số lượng lớn các hình tam giác có thể được tính toán trực tiếp một cách an toàn. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3 10
0 1 2
```Các tọa độ trùng lặp được sắp xếp là:`[0, 1, 2, 10, 11, 12]`| tôi | Điểm hiện tại | Điểm bên trong hình bán nguyệt mở | Đếm | Đã thêm hình tam giác không hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1, 2 | 2 | 1 | 
| 1 | 1 | 2 | 1 | 0 | 
| 2 | 2 | không | 0 | 0 | 

Tổng số hình tam giác là`1`. Tam giác duy nhất không hợp lệ vì tất cả các điểm đều nằm trong hình bán nguyệt ngắn hơn nửa đường tròn nên kết quả là`1 - 1 = 0`. 

Đối với mẫu thứ hai:```
10 10
0 1 2 3 4 5 6 7 8 9
```Đối với mỗi điểm bắt đầu, có đúng bốn điểm nằm trong hình bán nguyệt mở sau đây. 

| tôi | Điểm hiện tại | Số điểm theo sau trong khoảng cách < 5 | Đóng góp không hợp lệ | 
| --- | --- | --- | --- | 
| 0 | 0 | 4 | 6 | 
| 1 | 1 | 4 | 6 | 
| 2 | 2 | 4 | 6 | 
| 3 | 3 | 4 | 6 | 
| 4 | 4 | 4 | 6 | 
| 5 | 5 | 4 | 6 | 
| 6 | 6 | 4 | 6 | 
| 7 | 7 | 4 | 6 | 
| 8 | 8 | 4 | 6 | 
| 9 | 9 | 4 | 6 | 

Số đếm không hợp lệ là`60`. Tổng số hình tam giác là`120`, vậy câu trả lời là`120 - 60 = 60`. Ví dụ này cũng cho thấy tại sao các cặp đường kính phải có giá trị: các điểm đối diện có khoảng cách chính xác`5`, không nằm trong hình bán nguyệt mở. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp chiếm ưu thế và quá trình quét hai con trỏ là tuyến tính vì mỗi con trỏ chỉ di chuyển về phía trước. | 
| Không gian | O(n) | Mảng tọa độ trùng lặp lưu trữ hai bản sao của các điểm. | 

Thuật toán phù hợp với ràng buộc của`300000`điểm vì phần đắt tiền chỉ là phân loại. Giai đoạn đếm thực hiện một lượng công việc không đổi trên mỗi điểm. 

# Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("3 10\n0 1 2\n") == "0\n", "sample 1"
assert run("10 10\n0 1 2 3 4 5 6 7 8 9\n") == "60\n", "sample 2"

assert run("3 10\n0 5 6\n") == "1\n", "diameter edge case"
assert run("4 20\n0 1 2 3\n") == "0\n", "all points in one arc"
assert run("5 10\n0 2 4 6 8\n") == "5\n", "even spacing boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 10 / 0 1 2`|`0`| Tam giác cơ bản hoàn toàn bên trong hình bán nguyệt | 
|`10 10 / 0 1 2 3 4 5 6 7 8 9`|`60`| Vỏ đối xứng có nhiều cặp đường kính | 
|`3 10 / 0 5 6`|`1`| Xử lý đúng các điểm cách nhau đúng nửa vòng tròn | 
|`4 20 / 0 1 2 3`|`0`| Tất cả các điểm tập trung thành một cung nhỏ | 
|`5 10 / 0 2 4 6 8`|`5`| Nhiều trường hợp biên xung quanh vòng tròn | 

# Vỏ cạnh 

Khi hai đỉnh đối diện nhau thì phải tính tam giác đó. Đối với đầu vào:```
3 10
0 5 6
```tìm kiếm con trỏ từ`0`dừng lại trước`5`bởi vì khoảng cách chính xác là`L / 2`. Hình tam giác không được đánh dấu là không hợp lệ và tổng số vẫn còn`1`. 

Khi tất cả các điểm nằm gần nhau thì mọi tam giác đều không hợp lệ. Vì:```
4 20
0 1 2 3
```bắt đầu từ`0`tìm ba điểm còn lại trong khoảng cách`10`, tạo ra ba hình tam giác không hợp lệ. Điều tương tự cũng xảy ra với các điểm bắt đầu khác khi thích hợp và mọi tam giác có thể sẽ bị xóa chính xác một lần. 

Lỗi triển khai phổ biến nhất là sử dụng`<= L / 2`thay vì`< L / 2`. Thuật toán dựa trên hình bán nguyệt mở vì hình bán nguyệt khép kín bao gồm các trường hợp đường kính trong đó tâm nằm trên đường viền tam giác và phải được tính.
