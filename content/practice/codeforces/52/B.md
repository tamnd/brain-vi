---
title: "CF 52B - Tam giác vuông"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô trống hoặc được đánh dấu bằng . Mỗi ô đại diện cho một điểm nằm ở trung tâm của ô đó. Chúng ta cần đếm có thể tạo được bao nhiêu tam giác vuông sao cho:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics"]
categories: ["algorithms"]
codeforces_contest: 52
codeforces_index: "B"
codeforces_contest_name: "Codeforces Testing Round 1"
rating: 1600
weight: 52
solve_time_s: 98
verified: true
draft: false
---
[CF 52B - Tam giác vuông](https://codeforces.com/problemset/problem/52/B) 

**Đánh giá:** 1600 
**Thẻ:** tổ hợp 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô trống hoặc được đánh dấu bằng`*`. Mọi`*`ô đại diện cho một điểm nằm ở trung tâm của ô đó. Chúng ta cần đếm có thể tạo được bao nhiêu tam giác vuông sao cho: 

- cả ba đỉnh đều`*`tế bào, 
- góc vuông được tạo bởi một cạnh ngang và một cạnh thẳng đứng, 
- các chân của tam giác song song với các trục lưới. 

Một tam giác được xác định đầy đủ khi chúng ta chọn đỉnh chứa góc vuông. Nếu một`*`ô có cái khác`*`ở đâu đó trong cùng một hàng và một hàng khác`*`ở đâu đó trong cùng một cột thì hai lựa chọn đó sẽ tạo thành một tam giác vuông hợp lệ. 

Kích thước lưới tối đa là`1000 × 1000`, nên tổng số ô có thể lên tới một triệu. Bất kỳ giải pháp nào kiểm tra tất cả các bộ ba của`*`tế bào là hoàn toàn không thể thực hiện được. Ngay cả việc kiểm tra từng ô với tất cả các ô trong hàng và cột của nó một cách đơn giản cũng có thể trở nên quá chậm nếu thực hiện nhiều lần. 

Quan sát hữu ích là đối với một đỉnh góc vuông cố định, các lựa chọn theo chiều ngang và chiều dọc là độc lập. Nếu một ô có`r`các ngôi sao khác trong hàng của nó và`c`các ngôi sao khác trong cột của nó thì nó sẽ đóng góp`r * c`hình tam giác. 

Một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai không chính xác. 

Hãy xem xét một lưới chỉ có một hàng:```
1 5
*****
```Câu trả lời đúng là`0`. Mặc dù có nhiều cặp tồn tại theo chiều ngang nhưng không thể hình thành được chân thẳng đứng. 

Hãy xem xét một lưới trong đó đỉnh góc vuông vô tình được tính là một trong các lựa chọn:```
2 2
**
**
```Câu trả lời đúng là`4`. 

Mỗi góc đóng góp chính xác một hình tam giác: 

- trên cùng bên trái sử dụng trên cùng bên phải và dưới cùng bên trái, 
- trên cùng bên phải sử dụng trên cùng bên trái và dưới cùng bên phải, 
- dưới cùng bên trái sử dụng trên cùng bên trái và dưới cùng bên phải, 
- dưới cùng bên phải sử dụng trên cùng bên phải và dưới cùng bên trái. 

Một công thức bất cẩn khi sử dụng`row_count * col_count`thay vì`(row_count - 1) * (col_count - 1)`sẽ đếm quá mức bằng cách cho phép đỉnh ghép với chính nó. 

Một trường hợp tinh vi khác xuất hiện khi các hàng hoặc cột chứa đúng một ngôi sao:```
3 3
*..
.*.
..*
```Câu trả lời là`0`. Mỗi ngôi sao đều bị cô lập ở cả hàng và cột của nó, vì vậy không có tam giác vuông nào tồn tại mặc dù có tổng cộng ba ngôi sao. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy liệt kê từng bộ ba của`*`các ô và kiểm tra xem chúng có tạo thành một tam giác vuông thẳng hàng với trục hợp lệ hay không. 

Giả sử lưới được lấp đầy hoàn toàn. Sau đó có`10^6`các ngôi sao. Việc kiểm tra tất cả các bộ ba sẽ yêu cầu khoảng:$$\binom{10^6}{3}$$hoạt động, điều này là không thể về mặt thiên văn. 

Một cách tiếp cận bạo lực tốt hơn một chút sẽ sửa đỉnh góc vuông trước tiên. Đối với mọi`*`ô, quét toàn bộ hàng của nó để tìm các đối tác theo chiều ngang và quét toàn bộ cột của nó để tìm các đối tác theo chiều dọc. Nếu một ô có`r`lựa chọn theo chiều ngang và`c`lựa chọn theo chiều dọc, sau đó nó góp phần`r * c`. 

Cách tiếp cận này đúng vì mọi tam giác hợp lệ đều có chính xác một đỉnh góc vuông và khi đỉnh đó được cố định, điểm cuối ngang và dọc có thể được chọn độc lập. 

Vấn đề là hiệu suất. Nếu chúng ta quét liên tục các hàng và cột cho từng ô, độ phức tạp sẽ trở thành:$$O(n \cdot m \cdot (n + m))$$Với`n = m = 1000`, điều này sẽ trở thành khoảng hai tỷ phép tính trong trường hợp xấu nhất. 

Quan sát quan trọng là thông tin hàng và cột không bao giờ thay đổi. Thay vì tính toán lại số lượng nhiều lần, chúng ta có thể xử lý trước: 

- có bao nhiêu ngôi sao tồn tại trong mỗi hàng, 
- có bao nhiêu ngôi sao tồn tại trong mỗi cột. 

Sau đó mỗi`*`tế bào có thể tính toán sự đóng góp của nó trong thời gian không đổi. 

Nếu một tế bào`(i, j)`chứa một ngôi sao: 

-`row[i] - 1`cho biết số lượng các ngôi sao khác trong cùng một hàng, 
-`col[j] - 1`cho biết số lượng các ngôi sao khác trong cùng một cột. 

Tích của họ chính xác là số tam giác vuông sử dụng`(i, j)`như là đỉnh góc vuông. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · m · (n + m)) | O(1) | Quá chậm | 
| Tối ưu | O(n · m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới và lưu trữ nó. 
2. Tạo một mảng`row`Ở đâu`row[i]`lưu trữ số lượng`*`ô trong hàng`i`. 
3. Tạo một mảng`col`Ở đâu`col[j]`lưu trữ số lượng`*`ô trong cột`j`. 
4. Duyệt lưới một lần để điền vào các mảng này. 

Mỗi lần một`*`được tìm thấy tại`(i, j)`, tăng: 

-`row[i]`-`col[j]`5. Di chuyển lưới một lần nữa. 
6. Đối với mọi`*`tế bào`(i, j)`, tính:$$(row[i] - 1) \times (col[j] - 1)$$Phép trừ sẽ loại bỏ chính ô hiện tại khỏi cả hai lần đếm. 

1. Thêm tất cả đóng góp vào câu trả lời cuối cùng. 
2. In câu trả lời. 

### Tại sao nó hoạt động 

Mỗi tam giác hợp lệ đều có một đỉnh vuông góc duy nhất. Khi đỉnh đó được cố định, một đỉnh phải nằm trong cùng một hàng và một đỉnh khác phải nằm trong cùng một cột. 

Đối với một ngôi sao tại`(i, j)`: 

- chính xác là có`row[i] - 1`lựa chọn theo chiều ngang hợp lệ, 
- chính xác là có`col[j] - 1`các lựa chọn theo chiều dọc hợp lệ. 

Mỗi cặp lựa chọn như vậy tạo thành một tam giác vuông duy nhất và mỗi tam giác hợp lệ được tính chính xác một lần từ đỉnh góc vuông của nó. Không có sự trùng lặp nào xảy ra vì không có tam giác nào có thể có hai góc vuông. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

grid = [input().strip() for _ in range(n)]

row = [0] * n
col = [0] * m

for i in range(n):
    for j in range(m):
        if grid[i][j] == '*':
            row[i] += 1
            col[j] += 1

ans = 0

for i in range(n):
    for j in range(m):
        if grid[i][j] == '*':
            ans += (row[i] - 1) * (col[j] - 1)

print(ans)
```Lần duyệt đầu tiên tính toán có bao nhiêu ngôi sao xuất hiện trong mỗi hàng và cột. Quá trình tiền xử lý này là toàn bộ sự tối ưu hóa. Nếu không có nó, chúng tôi sẽ quét liên tục các hàng và cột cho từng ô. 

Lần duyệt thứ hai đánh giá từng`*`tế bào một cách độc lập. Công thức:```
(row[i] - 1) * (col[j] - 1)
```đếm tất cả các kết hợp có thể có của: 

- một ngôi sao khác ở cùng hàng, 
- một ngôi sao khác trong cùng một cột. 

Trừ`1`là cần thiết vì ô hiện tại thuộc về cả hai số đếm. Việc quên điều này sẽ tạo ra các hình tam giác không hợp lệ trong đó một điểm cuối bằng chính đỉnh góc vuông. 

Câu trả lời có thể lớn, nhưng số nguyên Python tự động xử lý kích thước tùy ý. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, loại 64 bit là cần thiết. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
**
*.
```Số lượng hàng: 

- hàng 0 → 2 
- hàng 1 → 1 

Số cột: 

- col 0 → 2 
- col 1 → 1 

| Tế bào | Là`*`| hàng[i]-1 | col[j]-1 | Đóng góp | 
| --- | --- | --- | --- | --- | 
| (0,0) | Có | 1 | 1 | 1 | 
| (0,1) | Có | 1 | 0 | 0 | 
| (1,0) | Có | 0 | 1 | 0 | 

Tổng số câu trả lời =`1`. 

Dấu vết này cho thấy ý tưởng cốt lõi một cách rõ ràng. Chỉ có ô trên cùng bên trái có cả đối tác ngang và dọc, do đó chỉ tồn tại một hình tam giác. 

### Ví dụ 2 

đầu vào:```
3 3
***
***
***
```Mỗi hàng có 3 sao và mỗi cột có 3 sao. 

| Tế bào | hàng[i]-1 | col[j]-1 | Đóng góp | 
| --- | --- | --- | --- | 
| Mỗi ô trong số 9 ô | 2 | 2 | 4 | 

Tổng số câu trả lời:$$9 \times 4 = 36$$Ví dụ này chứng minh rằng mỗi ô hoạt động độc lập như một đỉnh góc vuông. Mỗi đỉnh có thể ghép với hai lựa chọn ngang và hai lựa chọn dọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | Hai lần truyền tải đầy đủ của lưới | 
| Không gian | O(n + m) | Mảng đếm hàng và cột | 

Lưới chứa tối đa một triệu ô. Hai lần quét tuyến tính trên nhiều ô đó vừa vặn thoải mái trong giới hạn thời gian trong Python. Bộ nhớ phụ rất nhỏ so với kích thước lưới. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    grid = [input().strip() for _ in range(n)]

    row = [0] * n
    col = [0] * m

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                row[i] += 1
                col[j] += 1

    ans = 0

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                ans += (row[i] - 1) * (col[j] - 1)

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue().strip()

# provided sample
assert run(
"""2 2
**
*.
"""
) == "1", "sample 1"

# minimum grid
assert run(
"""1 1
*
"""
) == "0", "single cell"

# full 2x2 grid
assert run(
"""2 2
**
**
"""
) == "4", "all corners form triangles"

# diagonal stars only
assert run(
"""3 3
*..
.*.
..*
"""
) == "0", "no shared row or column"

# full 3x3 grid
assert run(
"""3 3
***
***
***
"""
) == "36", "dense grid"

# one row only
assert run(
"""1 5
*****
"""
) == "0", "no vertical edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`sao đơn |`0`| Lưới nhỏ nhất có thể | 
| Đầy`2x2`lưới |`4`| Đếm đúng từng góc | 
| Sao chỉ có đường chéo |`0`| Không có hình tam giác dù có nhiều sao | 
| Đầy`3x3`lưới |`36`| Số lượng lớn các hình tam giác chồng lên nhau | 
| Lưới một hàng |`0`| Yêu cầu cả chân ngang và chân dọc | 

## Vỏ cạnh 

Hãy xem xét trường hợp một hàng:```
1 5
*****
```Số hàng cho hàng duy nhất là`5`, nhưng mỗi số cột là`1`. 

Đối với mỗi ngôi sao: 

-`row[i] - 1 = 4`-`col[j] - 1 = 0`Mọi đóng góp đều trở thành:$$4 \times 0 = 0$$Câu trả lời cuối cùng là`0`, điều này đúng vì chân thẳng đứng không thể tồn tại. 

Bây giờ hãy xem xét việc điền đầy đủ`2x2`lưới:```
2 2
**
**
```Mỗi số hàng và số cột bằng nhau`2`. 

Đối với mỗi trong bốn ô: 

-`row[i] - 1 = 1`-`col[j] - 1 = 1`Mỗi người đóng góp chính xác một hình tam giác. 

Tổng cộng:$$4 \times 1 = 4$$Điều này xác nhận tại sao trừ`1`vấn đề. Nếu không có phép trừ, mỗi ô sẽ tự tính chính nó là đối tác một cách không chính xác. 

Cuối cùng, hãy xem xét các ngôi sao bị cô lập trên đường chéo:```
3 3
*..
.*.
..*
```Mỗi số hàng và số cột bằng nhau`1`. 

Đối với mỗi ngôi sao: 

-`row[i] - 1 = 0`-`col[j] - 1 = 0`Tất cả đóng góp đều bằng 0, do đó thuật toán báo cáo chính xác rằng không tồn tại tam giác vuông hợp lệ.
