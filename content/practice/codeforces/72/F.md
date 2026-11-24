---
title: "CF 72F - Dầu"
description: "Chúng ta có một lưới n × m. Một số hàng trống, một số toàn bộ cột trống và mỗi ô trống thuộc về ít nhất một trong các hàng trống hoặc cột trống đó. Tất cả các tế bào còn lại đều chứa dầu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 72
codeforces_index: "F"
codeforces_contest_name: "Unknown Language Round 2"
rating: 1900
weight: 72
solve_time_s: 103
verified: true
draft: false
---

[CF 72F - Dầu](https://codeforces.com/problemset/problem/72/F) 

**Xếp hạng:** 1900 
**Tags:** *đặc biệt, tham lam, toán học 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × m`lưới. Một số hàng trống, một số toàn bộ cột trống và mỗi ô trống thuộc về ít nhất một trong các hàng trống hoặc cột trống đó. 

Tất cả các tế bào còn lại đều chứa dầu. Nếu chúng ta khoan một giếng trong một ô dầu, chúng ta có thể tiếp cận mọi ô dầu được kết nối thông qua các ô dầu liền kề. Nhiệm vụ là tính toán có bao nhiêu vùng dầu được kết nối. 

Một cách khác để nghĩ về lưới là: 

Một ô chứa dầu khi và chỉ khi:```
its row is not empty
and
its column is not empty
```Vì vậy sau khi loại bỏ các hàng cấm và cột cấm, các ô dầu còn lại tạo thành nhiều khối hình chữ nhật. Chúng ta cần đếm xem các khối đó tạo ra bao nhiêu thành phần được kết nối. 

Giới hạn rất nhỏ, chỉ tối đa`100 × 100`. Ngay cả một BFS hoặc DFS đơn giản trên toàn bộ lưới cũng hoàn toàn an toàn. Tối đa một lần chạm truyền tải đầy đủ`10^4`các ô, rất nhỏ trong giới hạn 2 giây. 

Phần khó khăn không phải là hiệu suất, mà là hiểu cấu trúc một cách chính xác. 

Hãy xem xét ví dụ này:```
n = 5, m = 5
empty rows: 3
empty cols: none
```Lưới chia thành:```
rows 1-2
row 3 removed
rows 4-5
```Vậy có đúng 2 vùng dầu thông nhau. 

Bây giờ hãy xem xét:```
n = 5, m = 5
empty rows: 3
empty cols: 3
```Hàng bị loại bỏ và cột bị loại bỏ tạo thành một hình chữ thập. Các ô dầu còn lại chia thành 4 hình chữ nhật rời nhau nên đáp án là 4. 

Việc thực hiện bất cẩn có thể nghĩ sai rằng câu trả lời là:```
(#non-empty row segments) + (#non-empty column segments)
```nhưng cấu trúc đúng là phép nhân chứ không phải phép cộng. 

Một lỗi dễ xảy ra khác là mỗi hàng hoặc mỗi cột đều trống. 

Ví dụ:```
2 3
2 1 2
0
```Tất cả các hàng đều bị loại bỏ nên không còn ô dầu nào cả. Câu trả lời là`0`, không`1`. 

Tương tự:```
3 3
0
0
```Không có hàng hoặc cột nào bị xóa, do đó toàn bộ lưới là một thành phần được kết nối. Câu trả lời là`1`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xây dựng lưới một cách rõ ràng. 

Chúng tôi đánh dấu từng hàng trống và từng cột trống. Sau đó một tế bào`(i, j)`chứa dầu đúng khi:```
row i is not empty
and
column j is not empty
```Khi chúng ta xây dựng lưới boolean này, vấn đề sẽ trở thành vấn đề về các thành phần được kết nối tiêu chuẩn trên lưới 2D. Chúng tôi chạy BFS hoặc DFS từ mọi ô dầu chưa được truy cập và đếm số lần chúng tôi bắt đầu di chuyển. 

Phương pháp bạo lực này hoàn toàn chính xác vì kết nối chính xác là kết nối lưới bốn hướng thông thường. Mỗi DFS phát hiện toàn bộ một vùng dầu. 

Độ phức tạp của nó là:```
O(n * m)
```bởi vì mỗi ô được xử lý một số lần không đổi. 

Với`n, m ≤ 100`, điều này đã trôi qua một cách thoải mái. 

Ngoài ra còn có một quan sát cấu trúc ẩn bên dưới. 

Giả sử chúng ta loại bỏ một số hàng. Các hàng còn lại chia thành các đoạn hàng liền kề. Điều tương tự cũng xảy ra với các cột. 

Ví dụ:```
rows:    valid valid removed valid valid removed valid
segments:   [1]                [2]                [3]
```Mỗi thành phần dầu được kết nối chính xác là:```
(one row segment) × (one column segment)
```bởi vì bên trong một hình chữ nhật như vậy tất cả các ô đều tồn tại và được kết nối với nhau, trong khi các hàng hoặc cột bị loại bỏ sẽ ngăn cản việc giao nhau giữa các hình chữ nhật. 

Vì vậy, câu trả lời bằng:```
(number of non-empty row segments)
*
(number of non-empty column segments)
```Điều này tránh hoàn toàn việc truyền tải đồ thị. 

Giải pháp brute-force hoạt động vì lưới nhỏ, nhưng điểm mấu chốt là việc xóa toàn bộ hàng và cột sẽ phân chia lưới thành các hình chữ nhật độc lập. Một khi chúng ta nhận ra cấu trúc đó, vấn đề sẽ giảm xuống việc đếm các khoảng thời gian sống sót liền kề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS/BFS | O(nm) | O(nm) | Đã chấp nhận | 
| Đếm phân đoạn tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tập hợp hàng trống và cột trống. 

Chúng ta cần kiểm tra liên tục để xem liệu một hàng hoặc cột có bị xóa hay không, vì vậy các tập hợp hoặc mảng boolean là lý tưởng. 
2. Đếm xem có bao nhiêu đoạn liền kề của các hàng không trống. 

Chúng tôi quét các hàng từ`1`ĐẾN`n`. 

Bất cứ khi nào một hàng không trống và:```
it is the first row
or
the previous row is empty
```chúng tôi bắt đầu một phân đoạn hàng mới. 

Các hàng không trống liên tiếp thuộc cùng một khối dọc được kết nối. 
3. Đếm xem tồn tại bao nhiêu đoạn liền kề của các cột không trống. 

Điều này giống hệt với quá trình hàng. 
4. Nhân hai số đếm. 

Mỗi đoạn hàng kết hợp với mỗi đoạn cột để tạo thành một hình chữ nhật được kết nối của các ô dầu. 
5. Tự động xử lý trường hợp thoái hóa. 

Nếu không có hàng hoặc cột còn tồn tại, một trong các số đếm sẽ trở thành`0`, vậy đáp án cuối cùng cũng là`0`. 

### Tại sao nó hoạt động 

Việc loại bỏ các hàng sẽ tạo ra các rào cản ngang không thể vượt qua. Việc loại bỏ các cột sẽ tạo ra các rào cản dọc không thể vượt qua. 

Sau tất cả các lần loại bỏ, mỗi ô dầu còn sót lại thuộc về đúng một cặp:```
(row segment, column segment)
```Bên trong một cặp như vậy, tất cả các ô tạo thành một hình chữ nhật hoàn chỉnh, do đó, hai ô bất kỳ được kết nối thông qua chuyển động lưới thông thường. 

Hai ô thuộc các phân đoạn hàng khác nhau không thể kết nối vì một hàng trống ngăn cách chúng. Hai ô thuộc các phân đoạn cột khác nhau không thể kết nối vì một cột trống ngăn cách chúng. 

Vì vậy, các thành phần được kết nối tương ứng một-một với:```
row segments × column segments
```và nhân các số đếm sẽ cho ra số lượng chính xác các vùng dầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    row_data = list(map(int, input().split()))
    t = row_data[0]
    empty_rows = set(row_data[1:])

    col_data = list(map(int, input().split()))
    s = col_data[0]
    empty_cols = set(col_data[1:])

    row_segments = 0

    for i in range(1, n + 1):
        if i not in empty_rows:
            if i == 1 or (i - 1) in empty_rows:
                row_segments += 1

    col_segments = 0

    for j in range(1, m + 1):
        if j not in empty_cols:
            if j == 1 or (j - 1) in empty_cols:
                col_segments += 1

    print(row_segments * col_segments)

solve()
```Việc thực hiện phản ánh trực tiếp cấu trúc toán học. 

Trước tiên, chúng tôi lưu trữ các hàng và cột trống theo bộ. Kiểm tra tư cách thành viên như:```
i in empty_rows
```chạy trong thời gian dự kiến ​​không đổi. 

Quá trình quét hàng đếm số lần bắt đầu của các khoảng thời gian hợp lệ liền kề. Một phân đoạn mới chỉ bắt đầu khi hàng hiện tại tồn tại nhưng hàng trước đó thì không. 

Ví dụ:```
rows: valid valid empty valid
```Hàng hợp lệ đầu tiên bắt đầu một phân đoạn và hàng sau hàng trống bắt đầu một phân đoạn khác. 

Logic cột giống hệt nhau. 

Bước nhân có hiệu quả vì mỗi khoảng hàng giao nhau với mỗi khoảng cột trong một hình chữ nhật được kết nối. 

Một chi tiết tinh tế là xử lý các lưới hoàn toàn trống. Giả sử tất cả các hàng được loại bỏ. Khi đó không có hàng nào thỏa mãn điều kiện để bắt đầu một đoạn, vì vậy:```
row_segments = 0
```Sản phẩm cuối cùng tự động trở thành số 0. Không có trường hợp đặc biệt là cần thiết. 

Một nguồn lỗi phổ biến khác là việc lập chỉ mục. Đầu vào sử dụng cách đánh số hàng và cột dựa trên 1, do đó các vòng lặp cố ý lặp lại từ`1`bởi vì`n`Và`m`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 2
1 2
```Hàng:```
1 = valid
2 = empty
```Cột:```
1 = valid
2 = empty
3 = valid
```### Quét hàng 

| Hàng | Trống? | Bắt đầu phân khúc mới? | row_segments | 
| --- | --- | --- | --- | 
| 1 | Không | Có | 1 | 
| 2 | Có | Không | 1 | 

### Quét cột 

| Cột | Trống? | Bắt đầu phân khúc mới? | col_segments | 
| --- | --- | --- | --- | 
| 1 | Không | Có | 1 | 
| 2 | Có | Không | 1 | 
| 3 | Không | Có | 2 | 

Câu trả lời cuối cùng:```
1 × 2 = 2
```Các ô dầu còn lại tạo thành hai hình chữ nhật rời rạc:```
(1,1)
(1,3)
```Họ không thể kết nối vì cột 2 hoàn toàn trống. 

### Ví dụ 2 

đầu vào:```
5 5
1 3
1 3
```Hàng:```
1-2 valid
3 empty
4-5 valid
```Cột:```
1-2 valid
3 empty
4-5 valid
```### Quét hàng 

| Hàng | Trống? | Bắt đầu phân khúc mới? | row_segments | 
| --- | --- | --- | --- | 
| 1 | Không | Có | 1 | 
| 2 | Không | Không | 1 | 
| 3 | Có | Không | 1 | 
| 4 | Không | Có | 2 | 
| 5 | Không | Không | 2 | 

### Quét cột 

| Cột | Trống? | Bắt đầu phân khúc mới? | col_segments | 
| --- | --- | --- | --- | 
| 1 | Không | Có | 1 | 
| 2 | Không | Không | 1 | 
| 3 | Có | Không | 1 | 
| 4 | Không | Có | 2 | 
| 5 | Không | Không | 2 | 

Câu trả lời cuối cùng:```
2 × 2 = 4
```Hàng trống và cột trống chia bảng thành bốn góc phần tư bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lần quét tuyến tính trên các hàng và cột | 
| Không gian | O(n + m) | Bộ lưu trữ các hàng và cột đã xóa | 

Các giới hạn là cực kỳ nhỏ, do đó, ngay cả việc truyền tải toàn bộ biểu đồ cũng có thể dễ dàng vượt qua. Giải pháp tối ưu hóa này vẫn hữu ích vì nó nắm bắt được cấu trúc thực tế của vấn đề thay vì coi nó như một nhiệm vụ kết nối chung. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    row_data = list(map(int, input().split()))
    empty_rows = set(row_data[1:])

    col_data = list(map(int, input().split()))
    empty_cols = set(col_data[1:])

    row_segments = 0

    for i in range(1, n + 1):
        if i not in empty_rows:
            if i == 1 or (i - 1) in empty_rows:
                row_segments += 1

    col_segments = 0

    for j in range(1, m + 1):
        if j not in empty_cols:
            if j == 1 or (j - 1) in empty_cols:
                col_segments += 1

    print(row_segments * col_segments)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run("2 3\n1 2\n1 2\n") == "2", "sample 1"

# minimum grid, all oil
assert run("1 1\n0\n0\n") == "1", "single oil cell"

# completely empty grid
assert run("2 2\n2 1 2\n0\n") == "0", "all rows removed"

# cross split into four components
assert run("5 5\n1 3\n1 3\n") == "4", "four quadrants"

# alternating empty columns
assert run("3 5\n0\n2 2 4\n") == "3", "three vertical strips"

# large contiguous surviving block
assert run("100 100\n1 50\n1 50\n") == "4", "large grid split once"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`không cần xóa |`1`| Lưới không trống nhỏ nhất | 
| Đã xóa tất cả các hàng |`0`| Không có tế bào dầu tồn tại | 
| Hàng và cột bị loại bỏ duy nhất |`4`| Cấu trúc thành phần nhân | 
| Các cột trống xen kẽ |`3`| Phân đoạn nhiều cột | 
|`100×100`chia một lần |`4`| Đầu vào có kích thước ranh giới lớn | 

## Vỏ cạnh 

Hãy xem xét trường hợp mỗi hàng trống:```
2 3
2 1 2
0
```Trong quá trình quét hàng:```
row 1 -> empty
row 2 -> empty
```Không có phân đoạn hàng nào được tạo, vì vậy:```
row_segments = 0
```Các cột vẫn tạo thành một đoạn nhưng:```
0 × 1 = 0
```Điều này đúng vì không còn tế bào dầu nào nữa. 

Bây giờ hãy xem xét không có thao tác xóa nào cả:```
3 3
0
0
```Tất cả các hàng thuộc về một phân đoạn hàng liền kề và tất cả các cột thuộc về một phân đoạn cột liền kề. 

Vì thế:```
1 × 1 = 1
```Toàn bộ lưới được kết nối. 

Cuối cùng, hãy xem xét các khoảng thời gian hợp lệ riêng biệt:```
7 1
2 3 6
0
```Các hàng còn sót lại là:```
1-2, 4-5, 7
```Điều đó tạo ra ba phân đoạn hàng. 

Có một đoạn cột vì cột duy nhất còn tồn tại. 

Vì vậy, câu trả lời trở thành:```
3 × 1 = 3
```Thuật toán phát hiện chính xác từng phân đoạn mới khi hàng còn lại theo sau hàng bị xóa, xử lý chính xác các hàng bị cô lập và nhiều khoảng trống.
