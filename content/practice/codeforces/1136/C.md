---
title: "CF 1136C - Nastya đang chuyển đổi ma trận"
description: "Chúng ta có hai ma trận có cùng kích thước, A và B. Phép toán được phép có hiệu quả đáng ngạc nhiên: chúng ta có thể chọn bất kỳ ma trận con vuông nào bên trong A và hoán vị nó. Chuyển vị hoán đổi vị trí tương ứng với đường chéo chính của hình vuông."
date: "2026-06-12T04:00:41+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1136
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 546 (Div. 2)"
rating: 1500
weight: 1136
solve_time_s: 109
verified: true
draft: false
---

[CF 1136C - Nastya đang chuyển ma trận](https://codeforces.com/problemset/problem/1136/C) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho hai ma trận có cùng kích thước,`A`Và`B`. 

Phép toán được phép có hiệu quả đáng ngạc nhiên: chúng ta có thể chọn bất kỳ ma trận con vuông nào bên trong`A`và chuyển vị nó. Chuyển vị hoán đổi vị trí tương ứng với đường chéo chính của hình vuông. Chúng tôi có thể thực hiện thao tác này nhiều lần và trên bất kỳ kích thước hình vuông nào. 

Nhiệm vụ là xác định xem ma trận`A`có thể chuyển đổi thành ma trận`B`. 

Kích thước ma trận nhiều nhất là`500 × 500`, như vậy có thể có tới 250.000 tế bào. Bất kỳ giải pháp nào mô phỏng rõ ràng các chuỗi chuyển vị đều bị nghi ngờ ngay lập tức vì số lượng ma trận con bình phương có thể có là rất lớn. Ngay cả việc kiểm tra tất cả các hoạt động có thể xảy ra một lần cũng đã quá tốn kém. 

Thách thức chính là hiểu được thuộc tính nào của ma trận không thay đổi trong mọi thao tác được phép. 

Một lỗi phổ biến là so sánh các hàng hoặc cột một cách độc lập. Chuyển vị di chuyển các phần tử giữa các hàng và cột, do đó cả nội dung hàng và nội dung cột đều không được giữ nguyên. 

Một bẫy dễ dàng khác là giả định rằng toàn bộ ma trận có thể được sắp xếp lại một cách tùy ý. Hoạt động linh hoạt nhưng vẫn bảo tồn những hạn chế về cấu trúc nhất định. 

Coi như:```
A:
1 2
3 4

B:
1 3
2 4
```Đầu ra:```
YES
```Toàn bộ ma trận có thể được chuyển đổi một lần. 

Bây giờ hãy xem xét:```
A:
1 2
3 4

B:
1 4
3 2
```Đầu ra:```
NO
```Nhiều tập hợp phần tử ma trận giống hệt nhau, nhưng không có chuỗi chuyển vị nào được phép có thể di chuyển các phần tử giữa các đường chéo tùy ý. Giải pháp chỉ so sánh tần số toàn cầu sẽ trả lời sai CÓ. 

Một ví dụ tinh tế hơn là:```
A:
1 2 3
4 5 6

B:
1 2 3
4 6 5
```Đầu ra:```
NO
```Chỉ có hai yếu tố cuối cùng là khác nhau. Vì chúng thuộc các nhóm đường chéo được bảo toàn khác nhau nên chúng không thể trao đổi được. 

Hiểu chính xác những gì được bảo tồn là toàn bộ vấn đề. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô hình hóa các chuyển vị và tìm kiếm một chuỗi các hoạt động biến đổi`A`vào trong`B`. 

Điều này đúng về mặt lý thuyết vì mọi phép biến đổi hợp lệ cuối cùng đều có thể được phát hiện. Thật không may, không gian trạng thái mang tính thiên văn. Một ma trận có thể chứa tới 250.000 phần tử và có khoảng`O(nm · min(n,m))`ma trận con vuông khác nhau. Việc khám phá trình tự hoạt động là hoàn toàn không khả thi. 

Bước đột phá đến từ việc nghiên cứu chức năng thực sự của phép chuyển vị đối với tọa độ ma trận. 

Lấy một ma trận con vuông có góc trên bên trái là`(r,c)`. Bên trong hình vuông đó, một ô ở tọa độ địa phương`(x,y)`di chuyển đến`(y,x)`sau khi chuyển vị. 

Trước khi di chuyển:```
global row    = r + x
global column = c + y
```Sau khi di chuyển:```
global row    = r + y
global column = c + x
```Bây giờ hãy kiểm tra giá trị:```
(row - column)
```Trước:```
(r + x) - (c + y)
= (r - c) + (x - y)
```Sau đó:```
(r + y) - (c + x)
= (r - c) + (y - x)
```Thay vì nhìn vào`row - column`, dễ nhìn hơn:```
row + column
```Trước:```
(r + x) + (c + y)
= r + c + x + y
```Sau đó:```
(r + y) + (c + x)
= r + c + x + y
```tổng`row + column`không bao giờ thay đổi. 

Điều đó có nghĩa là mọi phần tử luôn nằm trên cùng một đường chéo. 

Mọi thao tác được phép chỉ đơn thuần là hoán vị các phần tử bên trong một đường chéo. Nó không bao giờ có thể di chuyển một phần tử sang một đường chéo khác. 

Bây giờ chúng ta cần thêm một quan sát nữa. Chuyển vị của một`2 × 2`hình vuông hoán đổi hai phần tử ngoài đường chéo. Chuyển vị cục bộ lặp đi lặp lại cho phép chúng ta nhận ra các hoán vị tùy ý trong một đường chéo cố định. Do đó, điều duy nhất quan trọng đối với mỗi đường chéo là tập hợp nhiều giá trị mà nó chứa. 

Vì vậy, vấn đề trở thành: 

Đối với mọi đường chéo`i + j`, thu thập tất cả các giá trị từ`A`và tất cả các giá trị từ`B`. Nếu nhiều bộ khớp với mọi đường chéo, câu trả lời là CÓ. Nếu không thì là KHÔNG. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Tìm kiếm theo cấp số nhân/không gian trạng thái | Rất lớn | Quá chậm | 
| Tối ưu | O(nm log(nm)) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi ô`(i,j)`trong ma trận`A`, đặt giá trị của nó vào một thùng chứa tương ứng với đường chéo`i + j`. 
2. Đối với mỗi ô`(i,j)`trong ma trận`B`, đặt giá trị của nó vào một thùng chứa khác tương ứng với đường chéo`i + j`. 
3. Đối với mỗi chỉ số chống đường chéo từ`0`ĐẾN`n + m - 2`, sắp xếp các giá trị được thu thập từ`A`. 
4. Sắp xếp các giá trị được thu thập từ`B`cho cùng một đường chéo. 
5. So sánh hai danh sách đã được sắp xếp. 
6. Nếu có bất kỳ đường chéo nào khác, xuất ra`"NO"`ngay lập tức vì một số giá trị sẽ cần phải rời khỏi đường chéo của nó, điều này là không thể. 
7. Nếu tất cả các đường chéo đều khớp nhau, xuất ra`"YES"`. 

### Tại sao nó hoạt động 

Mỗi chuyển vị đều bảo toàn số lượng`row + column`, do đó một phần tử không bao giờ rời khỏi đường chéo của nó. Do đó, tập hợp nhiều giá trị trên mỗi đường chéo là bất biến trong mọi phép toán được phép. 

Câu hỏi còn lại là liệu các tập hợp đa đường chéo phù hợp có đủ hay không. Đúng vậy. Chuyển vị của các ma trận con vuông tạo ra sự hoán đổi giữa các vị trí lân cận trên cùng một đường chéo và các hoán đổi lân cận tạo ra các hoán vị tùy ý. Có thể đạt được bất kỳ sự sắp xếp nào của các giá trị trong một đường chéo cố định. 

Do đó, hai ma trận có thể tiếp cận lẫn nhau một cách chính xác khi mọi đường chéo đều chứa cùng nhiều tập giá trị. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    da = [[] for _ in range(n + m - 1)]
    db = [[] for _ in range(n + m - 1)]

    for i in range(n):
        row = list(map(int, input().split()))
        for j, x in enumerate(row):
            da[i + j].append(x)

    for i in range(n):
        row = list(map(int, input().split()))
        for j, x in enumerate(row):
            db[i + j].append(x)

    for k in range(n + m - 1):
        da[k].sort()
        db[k].sort()
        if da[k] != db[k]:
            print("NO")
            return

    print("YES")

solve()
```Việc thực hiện phản ánh bằng chứng trực tiếp. 

Chỉ số mảng`i + j`xác định một đường chéo. Vì giá trị lớn nhất có thể là```
(n - 1) + (m - 1) = n + m - 2
```chúng tôi cần chính xác`n + m - 1`thùng chứa. 

Trong khi đọc từng ma trận, mọi phần tử sẽ được thêm vào danh sách tương ứng với đường chéo của nó. Không cần lưu trữ ma trận sau khi chèn, điều này giúp việc triển khai trở nên đơn giản. 

Việc sắp xếp sẽ chuyển đổi từng tập hợp đa đường chéo thành một biểu diễn chuẩn. Hai tập hợp bằng nhau khi và chỉ khi thứ tự sắp xếp của chúng bằng nhau. 

Việc trả lại sớm sau khi phát hiện sự không phù hợp sẽ tránh được những công việc không cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
1 1
6 1
1 6
1 1
```Các đường chéo của`A`: 

| Tế bào | tôi+j | Giá trị | 
| --- | --- | --- | 
| (0,0) | 0 | 1 | 
| (0,1) | 1 | 1 | 
| (1,0) | 1 | 6 | 
| (1,1) | 2 | 1 | 

Các đường chéo của`B`: 

| Tế bào | tôi+j | Giá trị | 
| --- | --- | --- | 
| (0,0) | 0 | 1 | 
| (0,1) | 1 | 6 | 
| (1,0) | 1 | 1 | 
| (1,1) | 2 | 1 | 

Sau khi sắp xếp: 

| Đường chéo | A | B | 
| --- | --- | --- | 
| 0 | [1] | [1] | 
| 1 | [1,6] | [1,6] | 
| 2 | [1] | [1] | 

Tất cả đều trùng khớp, vì vậy câu trả lời là:```
YES
```Ví dụ này cho thấy các giá trị có thể di chuyển trong cùng một đường chéo. 

### Ví dụ 2```
3 3
1 2 3
4 5 6
7 8 9

1 2 3
4 6 5
7 8 9
```Các đường chéo được thu thập: 

| Đường chéo | A | B | 
| --- | --- | --- | 
| 0 | [1] | [1] | 
| 1 | [2,4] | [2,4] | 
| 2 | [3,5,7] | [3,6,7] | 
| 3 | [6,8] | [5,8] | 
| 4 | [9] | [9] | 

Đường chéo`2`đã khác rồi. 

Đầu ra:```
NO
```Điều này chứng tỏ rằng việc có cùng một tập hợp các giá trị ma trận là không đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log(nm)) | Việc sắp xếp tất cả các đường chéo với nhau tốn nhiều nhất mức này | 
| Không gian | O(nm) | Mỗi phần tử ma trận được lưu trữ một lần trong danh sách đường chéo | 

Có nhiều nhất là 250.000 phần tử. Việc sắp xếp nhiều giá trị trải đều trên các đường chéo dễ dàng nằm gọn trong giới hạn 1 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    da = [[] for _ in range(n + m - 1)]
    db = [[] for _ in range(n + m - 1)]

    for i in range(n):
        row = list(map(int, input().split()))
        for j, x in enumerate(row):
            da[i + j].append(x)

    for i in range(n):
        row = list(map(int, input().split()))
        for j, x in enumerate(row):
            db[i + j].append(x)

    ans = "YES"
    for k in range(n + m - 1):
        if sorted(da[k]) != sorted(db[k]):
            ans = "NO"
            break

    return ans + "\n"

# provided sample
assert run(
"""2 2
1 1
6 1
1 6
1 1
"""
) == "YES\n", "sample 1"

# minimum size
assert run(
"""1 1
5
5
"""
) == "YES\n", "1x1 matrix"

# minimum size mismatch
assert run(
"""1 1
5
7
"""
) == "NO\n", "single cell differs"

# same values globally but wrong diagonal
assert run(
"""2 2
1 2
3 4
1 4
3 2
"""
) == "NO\n", "global multiset equal"

# all values equal
assert run(
"""3 3
7 7 7
7 7 7
7 7 7
7 7 7
7 7 7
7 7 7
"""
) == "YES\n", "all equal"

# rectangular matrix
assert run(
"""2 3
1 2 3
4 5 6
1 4 3
2 5 6
"""
) == "YES\n", "anti-diagonals preserved"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ma trận giống nhau 1×1 | CÓ | Đầu vào nhỏ nhất có thể | 
| 1×1 ma trận khác nhau | KHÔNG | Không khớp trực tiếp | 
| Cùng nhiều tập hợp toàn cầu, các đường chéo khác nhau | KHÔNG | Bắt các giải pháp chỉ có tần số không chính xác | 
| Tất cả các giá trị bằng nhau | CÓ | Xử lý trùng lặp | 
| Ma trận hình chữ nhật | CÓ | Lập chỉ mục chính xác của các đường chéo | 

## Vỏ cạnh 

### Ma trận ô đơn 

đầu vào:```
1 1
42
42
```Chỉ có một đường chéo chéo chứa`[42]`trong cả hai ma trận. 

Thuật toán xây dựng:```
diag[0] = [42]
```cho cả ma trận và trả về`YES`. 

### Tần số toàn cầu bằng nhau nhưng không thể chuyển đổi 

đầu vào:```
2 2
1 2
3 4

1 4
3 2
```Các đường chéo là:```
A:
0 -> [1]
1 -> [2,3]
2 -> [4]

B:
0 -> [1]
1 -> [4,3]
2 -> [2]
```Sau khi sắp xếp:```
[2,3] != [3,4]
```Thuật toán trả về`NO`. 

Một giải pháp chỉ so sánh bốn giá trị`{1,2,3,4}`sẽ quay lại không chính xác`YES`. 

### Ma trận hình chữ nhật 

đầu vào:```
2 3
1 2 3
4 5 6

1 4 3
2 5 6
```Chống đường chéo:```
0 -> [1]
1 -> [2,4]
2 -> [3,5]
3 -> [6]
```Các bộ đa tương ứng khớp chính xác. 

Đầu ra của thuật toán`YES`, cho thấy đối số chỉ phụ thuộc vào các đường chéo và không yêu cầu ma trận phải vuông. 

### Nhiều giá trị trùng lặp 

đầu vào:```
3 3
1 1 1
1 2 1
1 1 1

1 1 1
1 2 1
1 1 1
```Sắp xếp bảo toàn tính đa dạng. Thuật toán so sánh các danh sách được sắp xếp đầy đủ thay vì các tập hợp, do đó các giá trị lặp lại được tính chính xác và câu trả lời là`YES`.
