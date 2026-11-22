---
title: "CF 330A - Máy làm bánh"
description: "Chúng ta được cấp một lưới hình chữ nhật nhỏ tượng trưng cho một chiếc bánh. Mỗi ô là một ô bánh thông thường (.) hoặc chứa một quả dâu tây (S)."
date: "2026-06-06T09:29:47+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation"]
categories: ["algorithms"]
codeforces_contest: 330
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 192 (Div. 2)"
rating: 800
weight: 330
solve_time_s: 93
verified: true
draft: false
---

[CF 330A - Cakeminator](https://codeforces.com/problemset/problem/330/A) 

**Đánh giá:** 800 
**Tags:** bạo lực, thực hiện 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một lưới hình chữ nhật nhỏ tượng trưng cho một chiếc bánh. Mỗi ô là một ô bánh thông thường (`.`) hoặc chứa dâu tây (`S`). 

Người làm bánh có thể liên tục chọn toàn bộ một hàng hoặc toàn bộ cột và ăn tất cả các ô trong đó, nhưng chỉ khi hàng hoặc cột đó không chứa dâu tây nào cả. Các tế bào đã được ăn không thành vấn đề. Một hàng hoặc cột vẫn có thể được chọn sau này miễn là nó chứa ít nhất một ô chưa được sử dụng. 

Mục tiêu là tìm ra số lượng tế bào tối đa có thể ăn được. 

Kích thước lưới tối đa là 10 × 10. Điều đó có nghĩa là có tổng cộng tối đa 100 ô. Với những giới hạn nhỏ như vậy, ngay cả những cách tiếp cận tương đối kém hiệu quả cũng đủ nhanh. Thử thách không phải là hiệu suất mà là đếm chính xác các ô có thể ăn được mà không cần đếm hai lần giao điểm giữa các hàng và cột đã chọn. 

Nhận xét quan trọng là thứ tự các hoạt động ăn uống không quan trọng. Nếu một hàng không chứa dâu tây thì cuối cùng mọi ô trong hàng đó đều có thể bị ăn. Nếu một cột không chứa dâu tây thì cuối cùng mọi tế bào trong cột đó đều có thể bị ăn hết. Một ô có thể ăn được nếu nó thuộc ít nhất một hàng không có dâu tây hoặc một cột không có dâu tây. 

Một số trường hợp đặc biệt có thể gây ra sai sót. 

Hãy xem xét một lưới trong đó một ô thuộc cả hàng an toàn và cột an toàn:```
2 2
..
..
```Câu trả lời đúng là:```
4
```Một giải pháp bất cẩn có thể đếm tất cả các ô trong các hàng an toàn và sau đó là tất cả các ô trong các cột an toàn, thu được 8 thay vì 4 vì mỗi ô được tính hai lần. 

Một trường hợp quan trọng khác là khi không có hàng và không có cột nào là an toàn:```
2 2
S.
.S
```Câu trả lời đúng là:```
0
```Mỗi hàng chứa một quả dâu tây và mỗi cột chứa một quả dâu tây nên không thể di chuyển. 

Trường hợp thứ ba là khi chỉ có hàng là an toàn:```
2 3
...
S.S
```Câu trả lời đúng là:```
3
```Hàng đầu tiên có thể ăn được hoàn toàn. Không có cột nào là an toàn vì mỗi cột đều chứa một quả dâu tây ở đâu đó. Câu trả lời chính xác là kích thước của hàng an toàn. 

## Phương pháp tiếp cận 

Quan điểm vũ phu là nghĩ xem hàng và cột nào có thể ăn được. Mỗi hàng không có dâu luôn đáng lấy và mỗi cột không có dâu luôn đáng lấy. Sau khi xác định được chúng, chúng tôi có thể mô phỏng tất cả các tế bào bị ăn thịt và đếm xem có bao nhiêu tế bào riêng biệt bị ăn thịt. 

Vì lưới chứa tối đa 100 ô nên thậm chí việc kiểm tra từng ô nhiều lần cũng hoàn toàn có thể chấp nhận được. Thách thức chính là tránh các ô đếm kép nằm ở giao điểm của hàng an toàn và cột an toàn. 

Quan sát quan trọng là một tế bào có thể ăn được khi và chỉ khi có ít nhất một trong những điều sau đây là đúng: 

1. Hàng của nó không chứa dâu tây. 
2. Cột của nó không chứa dâu tây. 

Nếu một trong hai điều kiện được thỏa, hàng hoặc cột tương ứng có thể được chọn tại một thời điểm nào đó, khiến ô đó bị ăn. Nếu không có điều kiện nào xảy ra thì cả hàng và cột của nó đều chứa dâu tây, do đó không có hoạt động hợp pháp nào có thể tiếp cận ô đó. 

Điều này biến vấn đề thành một nhiệm vụ đánh dấu đơn giản. Đầu tiên hãy xác định hàng nào an toàn và cột nào an toàn. Sau đó kiểm tra từng tế bào. Đếm nó nếu hàng của nó an toàn hoặc cột của nó an toàn. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force với các ô được truy cập | O(r·c) | O(r·c) | Đã chấp nhận | 
| Đánh dấu hàng/cột tối ưu | O(r·c) | O(r + c) | Đã chấp nhận | 

Vì lưới quá nhỏ nên cả hai cách tiếp cận đều dễ dàng nằm trong giới hạn. Cách tiếp cận thứ hai đơn giản hơn và thể hiện trực tiếp quan sát chính. 

## Hướng dẫn thuật toán 

1. Đọc lưới. 
2. Đối với mỗi hàng, hãy kiểm tra xem nó có chứa dâu tây hay không. 

Nếu một hàng không chứa`S`, đánh dấu nó là an toàn. 
3. Đối với mỗi cột, hãy kiểm tra xem nó có chứa quả dâu tây hay không. 

Nếu một cột không chứa`S`, đánh dấu nó là an toàn. 
4. Lặp lại qua từng ô của lưới. 
5. Nếu hàng của ô an toàn hoặc cột của ô an toàn, hãy thêm một giá trị vào câu trả lời. 

Một tế bào như vậy cuối cùng có thể bị ăn thịt thông qua ít nhất một hoạt động hợp pháp. 
6. Xuất số đếm cuối cùng. 

### Tại sao nó hoạt động 

Một hàng không có dâu tây luôn có thể được chọn, vì vậy mọi ô trong hàng đó đều có thể ăn được. Tương tự, một cột không có dâu tây luôn có thể được chọn, vì vậy mọi ô trong cột đó đều có thể ăn được. 

Nếu một ô không thuộc hàng an toàn hay cột an toàn thì cả hàng và cột của nó đều chứa dâu tây. Vì chỉ có thể chọn các hàng và cột hoàn toàn không có dâu tây nên không hoạt động hợp pháp nào có thể ăn được ô đó. 

Do đó, các ô ăn được chính xác là những ô có hàng an toàn hoặc cột an toàn. Đếm chính xác những tế bào đó sẽ cho ra số lượng tế bào bị ăn tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

r, c = map(int, input().split())
grid = [input().strip() for _ in range(r)]

safe_row = [False] * r
safe_col = [False] * c

for i in range(r):
    if 'S' not in grid[i]:
        safe_row[i] = True

for j in range(c):
    ok = True
    for i in range(r):
        if grid[i][j] == 'S':
            ok = False
            break
    safe_col[j] = ok

answer = 0

for i in range(r):
    for j in range(c):
        if safe_row[i] or safe_col[j]:
            answer += 1

print(answer)
```Phần đầu tiên xác định tất cả các hàng không có dâu tây. Việc sử dụng tính năng kiểm tra tư cách thành viên chuỗi của Python khiến việc này trở nên rất ngắn gọn. 

Phần thứ hai quét từng cột một cách độc lập. Một cột chỉ được đánh dấu an toàn nếu không có hàng nào chứa`S`trong cột đó. 

Vòng lặp lồng nhau cuối cùng đếm các ô. Điều kiện sử dụng logic OR vì một ô có thể ăn được khi hàng hoặc cột của nó có thể bị ăn. 

Một lỗi phổ biến là đếm tất cả các ô từ các hàng an toàn và sau đó đếm tất cả các ô từ các cột an toàn một cách riêng biệt. Các giao lộ sau đó sẽ được tính hai lần. Kiểm tra từng ô chính xác một lần sẽ tránh được hoàn toàn vấn đề đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
S...
....
..S.
```Hàng an toàn: 

- Hàng 0: không an toàn 
- Hàng 1: an toàn 
- Hàng 2: không an toàn 

Cột an toàn: 

- Cột 0: không an toàn 
- Cột 1: an toàn 
- Cột 2: không an toàn 
- Cột 3: an toàn 

| Tế bào | Hàng an toàn? | Cột an toàn? | Được tính? | 
| --- | --- | --- | --- | 
| (0,0) | Không | Không | Không | 
| (0,1) | Không | Có | Có | 
| (0,2) | Không | Không | Không | 
| (0,3) | Không | Có | Có | 
| (1,0) | Có | Không | Có | 
| (1,1) | Có | Có | Có | 
| (1,2) | Có | Không | Có | 
| (1,3) | Có | Có | Có | 
| (2,0) | Không | Không | Không | 
| (2,1) | Không | Có | Có | 
| (2,2) | Không | Không | Không | 
| (2,3) | Không | Có | Có | 

Tổng số ô được đếm = 8. 

Ví dụ này giải thích tại sao các giao lộ chỉ được tính một lần. Tế bào`(1,1)`thuộc về cả hàng an toàn và cột an toàn nhưng chỉ đóng góp một ô cho câu trả lời. 

### Ví dụ 2 

đầu vào:```
2 2
S.
.S
```Hàng an toàn: 

- Hàng 0: không an toàn 
- Hàng 1: không an toàn 

Cột an toàn: 

- Cột 0: không an toàn 
- Cột 1: không an toàn 

| Tế bào | Hàng an toàn? | Cột an toàn? | Được tính? | 
| --- | --- | --- | --- | 
| (0,0) | Không | Không | Không | 
| (0,1) | Không | Không | Không | 
| (1,0) | Không | Không | Không | 
| (1,1) | Không | Không | Không | 

Tổng số ô được đếm = 0. 

Ví dụ này cho thấy rằng việc có các ô trống là không đủ. Một ô chỉ có thể ăn được nếu toàn bộ hàng hoặc cột chứa nó không có dâu tây. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(r·c) | Mỗi ô được kiểm tra một số lần không đổi | 
| Không gian | O(r + c) | Lưu trữ thông tin an toàn cho hàng và cột | 

Từ`r, c ≤ 10`, lưới lớn nhất có thể chỉ chứa 100 ô. Thuật toán chỉ thực hiện một vài lần truyền qua lưới và dễ dàng đáp ứng cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    r, c = map(int, input().split())
    grid = [input().strip() for _ in range(r)]

    safe_row = [False] * r
    safe_col = [False] * c

    for i in range(r):
        if 'S' not in grid[i]:
            safe_row[i] = True

    for j in range(c):
        safe_col[j] = True
        for i in range(r):
            if grid[i][j] == 'S':
                safe_col[j] = False
                break

    ans = 0
    for i in range(r):
        for j in range(c):
            if safe_row[i] or safe_col[j]:
                ans += 1

    return str(ans)

# provided sample
assert run(
"""3 4
S...
....
..S.
"""
) == "8", "sample 1"

# minimum size, all cells edible
assert run(
"""2 2
..
..
"""
) == "4", "all safe"

# no legal move
assert run(
"""2 2
S.
.S
"""
) == "0", "no safe row or column"

# only one safe row
assert run(
"""2 3
...
S.S
"""
) == "3", "safe row only"

# only safe columns contribute
assert run(
"""3 3
S.S
S.S
S.S
"""
) == "3", "middle column safe"
```### Tóm tắt trường hợp kiểm thử 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2×2 tất cả các dấu chấm | 4 | Mỗi tế bào ăn được | 
| Dâu tây chéo | 0 | Không có hàng an toàn và không có cột an toàn | 
| Một hàng an toàn | 3 | Chỉ ăn theo hàng | 
| Ba hàng`S.S`| 3 | Chỉ ăn theo cột | 
| Mẫu chính thức | 8 | Hàng và cột an toàn hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét chiếc bánh hoàn toàn trống rỗng:```
2 2
..
..
```Mọi hàng đều an toàn và mọi cột đều an toàn. Thuật toán đánh dấu tất cả các hàng và cột là an toàn, sau đó đếm tất cả bốn ô chính xác một lần. Đầu ra là:```
4
```Điều này tránh được lỗi đếm kép phổ biến có thể tạo ra 8. 

Hãy xem xét một chiếc bánh trong đó mỗi hàng và mỗi cột đều có một quả dâu tây:```
2 2
S.
.S
```Không có hàng nào được đánh dấu an toàn và không có cột nào được đánh dấu an toàn. Trong lần đếm cuối cùng, mọi ô đều không đạt điều kiện`safe_row[i] or safe_col[j]`. Câu trả lời là:```
0
```Hãy xem xét một trường hợp chỉ ăn theo hàng:```
2 3
...
S.S
```Hàng đầu tiên an toàn. Không có cột nào là an toàn vì mỗi cột đều chứa ít nhất một quả dâu tây ở đâu đó trong lưới. Thuật toán đếm chính xác ba ô của hàng đầu tiên và trả về:```
3
```Điều này xác nhận rằng các hàng và cột được xử lý độc lập và một ô chỉ cần một trong hai điều kiện là có thể sử dụng được.
