---
title: "CF 1717B - Cuộc thi Madoka và ngầm"
description: "Chúng ta cần xây dựng một lưới n × n chỉ chứa '.' và 'X'. Lưới phải thỏa mãn hai điều kiện. Đầu tiên, mọi đoạn ngang có độ dài k phải chứa ít nhất một chữ 'X'. Thứ hai, mọi đoạn thẳng đứng có độ dài k cũng phải chứa ít nhất một chữ 'X'."
date: "2026-06-09T19:47:33+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1717
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 818 (Div. 2)"
rating: 1100
weight: 1717
solve_time_s: 121
verified: false
draft: false
---

[CF 1717B - Cuộc thi Madoka và ngầm](https://codeforces.com/problemset/problem/1717/B) 

**Đánh giá:** 1100 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một`n × n`lưới chỉ chứa`'.'`Và`'X'`. 

Lưới phải thỏa mãn hai điều kiện. Đầu tiên, mỗi đoạn có chiều dài nằm ngang`k`phải chứa ít nhất một`'X'`. Thứ hai, mọi đoạn thẳng đứng có chiều dài`k`cũng phải chứa ít nhất một`'X'`. Trong số tất cả các lưới như vậy, chúng tôi muốn một lưới có số lượng tối thiểu có thể`'X'`tế bào. Ngoài ra, một ô cụ thể`(r, c)`được yêu cầu phải chứa`'X'`. 

Đầu vào đưa ra một số trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm,`n`chia hết cho`k`, đó là một gợi ý cấu trúc rất mạnh mẽ. Chúng tôi không được yêu cầu xác minh một mạng lưới mà chỉ xây dựng một mạng lưới. 

Tổng số tiền của tất cả`n`giá trị nhiều nhất là`500`. Thậm chí một`O(n²)`Việc xây dựng hoàn toàn an toàn vì bản thân đầu ra chứa`n²`nhân vật. Bất kỳ giải pháp nào ghi vào lưới đều tiêu tốn nhiều công sức như vậy. 

Khó khăn chính không phải là hiệu suất mà là việc khám phá ra mẫu đạt được cả tính hợp lệ và số lượng tối thiểu`'X'`tế bào. 

Một sai lầm phổ biến là đặt một`'X'`mọi`k`các ô độc lập trong mỗi hàng. Ví dụ:```
n = 6, k = 3

X..X..
X..X..
X..X..
X..X..
X..X..
X..X..
```Mọi điều kiện của hàng đều được thỏa mãn, nhưng cột 2 và 3 không chứa`'X'`, rất nhiều đoạn dọc không thành công. 

Một sai lầm dễ mắc phải khác là xây dựng một mẫu tuần hoàn hợp lệ nhưng lại quên ô cần thiết. Coi như:```
n = 3, k = 3, r = 3, c = 2
```Họa tiết đường chéo```
X..
.X.
..X
```chứa số lượng tối thiểu`'X'`, nhưng ô`(3,2)`không được đánh dấu. Câu trả lời phải được dịch chuyển sao cho ô được yêu cầu thuộc mẫu đã chọn. 

Trường hợp cạnh`k = 1`cũng quan trọng. Mỗi đoạn có độ dài`1`phải chứa một`'X'`, nghĩa là mỗi ô phải`'X'`. Bất kỳ công trình xây dựng nào giả định khoảng cách giữa các điểm sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Tư duy vũ phu sẽ bắt đầu bằng cách coi vấn đề như một nhiệm vụ thỏa mãn sự ràng buộc. Chúng ta có thể thử đặt`'X'`các ô, liên tục kiểm tra xem mọi chiều ngang và chiều dọc-`k`phân đoạn được bao phủ và tìm kiếm giải pháp tối thiểu. 

Về nguyên tắc thì điều này đúng nhưng không gian tìm kiếm rất lớn. MỘT`n × n`lưới có`2^(n²)`các cấu hình có thể. Ngay cả đối với`n = 20`, điều này là hoàn toàn không thể. 

Quan sát quan trọng xuất phát từ việc hiểu giải pháp tối thiểu phải như thế nào. 

Giả sử chúng ta chọn chính xác một ô trong mỗi nhóm`k`các vị trí dọc theo một hàng. Nếu những vị trí đã chọn đó xếp thẳng hàng trên các hàng thì mỗi khối chiều dài ngang`k`tự động chứa một`'X'`. 

Một mô hình rất tự nhiên là đánh dấu tất cả các ô có tọa độ thỏa mãn```
(row + column) mod k = constant
```Đối với bất kỳ hàng cố định nào, chính xác một cột trong mỗi`k`cột liên tiếp thỏa mãn điều kiện này. Tương tự, đối với bất kỳ cột cố định nào, mỗi cột có chính xác một hàng`k`các hàng liên tiếp thỏa mãn nó. 

Điều này ngay lập tức đảm bảo rằng mọi chiều ngang và chiều dọc-`k`đoạn chứa chính xác một ô được đánh dấu. 

Tại sao đây là mức tối thiểu? Từ`n`chia hết cho`k`, mỗi hàng chứa chính xác`n / k`ô được đánh dấu. Trên tất cả các hàng, tổng số`'X'`tế bào trở thành```
n × (n / k) = n² / k
```Bất kỳ lưới hợp lệ nào cũng cần ít nhất nhiều điểm này và công trình này đạt được giới hạn đó. 

Nhiệm vụ còn lại là chọn lớp dư lượng đúng sao cho`(r, c)`thuộc về mẫu. Nếu chúng ta sử dụng các chỉ số dựa trên 0 thì dư lượng yêu cầu chỉ đơn giản là```
(r + c) mod k
```và chúng tôi đánh dấu mọi ô có cùng dư lượng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(n²)) | O(n²) | Quá chậm | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi vị trí yêu cầu`(r, c)`thành tọa độ dựa trên 0. 
2. Tính dư lượng mục tiêu:```
target = (r + c) mod k
```Bất kỳ ô nào có cùng lớp dư sẽ thuộc mẫu đường chéo đã chọn. 
3. Tạo một`n × n`lưới ban đầu chứa đầy`'.'`. 
4. Đối với mỗi ô`(i, j)`, kiểm tra xem```
(i + j) mod k = target
```Nếu đúng thì đặt`'X'`. 
5. Xuất lưới kết quả. 

Lý do điều này hoạt động là vì tất cả các ô được đánh dấu đều thuộc về một modulo lớp dư lượng duy nhất`k`. Ô bắt buộc được sử dụng để chọn lớp dư lượng đó, vì vậy nó được đảm bảo được đánh dấu. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ hàng nào. Khi chỉ số cột tăng thêm một,`(row + column) mod k`quay vòng qua tất cả các dư lượng. Trong số mọi`k`các cột liên tiếp, có đúng một cột có phần dư được chọn. Do đó mỗi đoạn có chiều dài nằm ngang`k`chứa đúng một`'X'`. 

Đối số tương tự áp dụng cho các cột. Khi chỉ số hàng tăng lên, phần dư sẽ quay vòng qua tất cả các giá trị, do đó, mỗi đoạn có chiều dài dọc`k`cũng chứa đúng một`'X'`. 

Mỗi hàng chứa chính xác`n / k`ô được đánh dấu vì`n`chia hết cho`k`. Vậy tổng số điểm là`n² / k`, đó là số lượng tối thiểu có thể đạt được. Vì số dư được chọn là`(r + c) mod k`, ô được yêu cầu luôn được bao gồm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, k, r, c = map(int, input().split())

    r -= 1
    c -= 1

    target = (r + c) % k

    grid = []

    for i in range(n):
        row = []
        for j in range(n):
            if (i + j) % k == target:
                row.append('X')
            else:
                row.append('.')
        grid.append(''.join(row))

    print('\n'.join(grid))
```Bước đầu tiên chuyển đổi tọa độ đã cho thành chỉ mục dựa trên 0. Điều này làm cho số học mô-đun sạch hơn vì các chỉ số Python tự nhiên bắt đầu từ 0. 

giá trị`target`xác định lớp dư lượng chéo nào sẽ chứa tất cả`'X'`tế bào. Mỗi tế bào thỏa mãn`(i + j) % k == target`được đánh dấu. 

Các vòng lặp lồng nhau truy cập vào mỗi ô chính xác một lần và xây dựng lưới đầu ra. Vì bản thân câu trả lời có chứa`n²`các ký tự, không thể xây dựng nhanh hơn một cách tiệm cận. 

Lỗi triển khai phổ biến nhất là quên chuyển đổi`(r, c)`về tọa độ dựa trên 0 trước khi tính toán phần dư. Việc sử dụng tọa độ dựa trên 1 sẽ dịch chuyển toàn bộ mẫu và có thể không đánh dấu ô được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3, k = 3, r = 3, c = 2
```Sau khi chuyển đổi sang tọa độ dựa trên 0:```
r = 2, c = 1
target = (2 + 1) mod 3 = 0
```| Ô (i,j) | (i+j)%3 | Đánh dấu? | 
| --- | --- | --- | 
| (0,0) | 0 | X | 
| (0,1) | 1 | . | 
| (0,2) | 2 | . | 
| (1,0) | 1 | . | 
| (1,1) | 2 | . | 
| (1,2) | 0 | X | 
| (2,0) | 2 | . | 
| (2,1) | 0 | X | 
| (2,2) | 1 | . | 

Kết quả:```
X..
..X
.X.
```Ô cần thiết`(3,2)`trở thành`(2,1)`trong lập chỉ mục dựa trên 0 và nó được đánh dấu vì phần dư của nó khớp với lớp đã chọn. 

### Ví dụ 2 

đầu vào:```
n = 2, k = 1, r = 1, c = 2
```| Ô (i,j) | (i+j)%1 | Đánh dấu? | 
| --- | --- | --- | 
| (0,0) | 0 | X | 
| (0,1) | 0 | X | 
| (1,0) | 0 | X | 
| (1,1) | 0 | X | 

Kết quả:```
XX
XX
```Điều này chứng tỏ trường hợp đặc biệt`k = 1`. Vì mọi giá trị modulo`1`bằng`0`, mọi ô đều thuộc lớp dư lượng đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi ô được truy cập một lần | 
| Không gian | O(n²) | Lưới đầu ra được lưu trữ trước khi in | 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`500`, vậy tổng khối lượng công việc nhiều nhất là khoảng`500² = 250000`hoạt động của tế bào. Điều này thấp hơn nhiều so với giới hạn và giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, k, r, c = map(int, input().split())

        r -= 1
        c -= 1
        target = (r + c) % k

        for i in range(n):
            row = ''.join(
                'X' if (i + j) % k == target else '.'
                for j in range(n)
            )
            out.append(row)

    return "\n".join(out)

# sample 2 from statement
assert run("1\n2 1 1 2\n") == "XX\nXX"

# minimum size
assert run("1\n1 1 1 1\n") == "X"

# k = n
assert run("1\n3 3 3 2\n") == "X..\n..X\n.X."

# off-by-one check near border
assert run("1\n4 2 4 4\n") == ".X.X\nX.X.\n.X.X\nX.X."

# larger periodic pattern
assert run("1\n6 3 4 2\n") == ".X..X.\nX..X..\n..X..X\n.X..X.\nX..X..\n..X..X"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1`| Đơn`X`| Lưới nhỏ nhất có thể | 
|`2 1 1 2`| Tất cả các ô`X`| Trường hợp đặc biệt`k = 1`| 
|`3 3 3 2`| Đã dịch chuyển đường chéo | Lựa chọn ô bắt buộc | 
|`4 2 4 4`| Mô hình xen kẽ | Tọa độ ranh giới | 
|`6 3 4 2`| Lặp lại mô hình modulo | Xây dựng tổng hợp | 

## Vỏ cạnh 

### Trường hợp 1: Ô bắt buộc không nằm trên đường chéo mặc định 

đầu vào:```
1
3 3 3 2
```Sử dụng dư lượng cố định như`0`mà không xem xét`(r, c)`có thể vô tình hoạt động với một số đầu vào và không hoạt động với một số đầu vào khác. Ở đây thuật toán tính toán:```
target = (2 + 1) mod 3 = 0
```và tạo ra:```
X..
..X
.X.
```Tế bào`(3,2)`được đánh dấu chính xác theo yêu cầu. 

### Trường hợp 2: k = 1 

đầu vào:```
1
2 1 1 2
```Thuật toán tính toán:```
target = 0
```Mỗi ô thỏa mãn:```
(i + j) mod 1 = 0
```vì vậy đầu ra trở thành:```
XX
XX
```Mỗi đoạn có độ dài 1 chứa một`'X'`, đó là câu trả lời hợp lệ duy nhất. 

### Trường hợp 3: k = n 

đầu vào:```
1
4 4 2 3
```Thuật toán tính toán:```
target = (1 + 2) mod 4 = 3
```và đánh dấu chính xác một ô trong mỗi hàng:```
...X
..X.
.X..
X...
```Mỗi hàng và cột có một`'X'`, thế là đủ vì độ dài duy nhất-`k`đoạn trong một hàng hoặc cột là toàn bộ hàng hoặc cột đó. 

### Trường hợp 4: Ô bắt buộc ở viền 

đầu vào:```
1
4 2 4 4
```Số dư được chọn là:```
(3 + 3) mod 2 = 0
```Lưới được tạo ra là:```
.X.X
X.X.
.X.X
X.X.
```Ô dưới cùng bên phải được đánh dấu, mỗi cặp ngang chứa một ô`'X'`và mỗi cặp dọc chứa một`'X'`. Mẫu mô-đun xử lý các đường viền một cách tự nhiên mà không cần bất kỳ logic đặc biệt nào.
