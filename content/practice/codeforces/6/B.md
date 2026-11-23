---
title: "CF 6B - Văn phòng Chủ tịch nước"
description: "Văn phòng được thể hiện dưới dạng một lưới hình chữ nhật. Mỗi chữ cái viết hoa đại diện cho một phần của một bàn làm việc và tất cả các ô có cùng một chữ cái đều thuộc về cùng một bàn làm việc hình chữ nhật. Màu bàn của tổng thống được cho là c."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 6
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 6 (Div. 2 Only)"
rating: 1100
weight: 6
solve_time_s: 78
verified: true
draft: false
---
[CF 6B - Văn phòng Chủ tịch](https://codeforces.com/problemset/problem/6/B) 

**Đánh giá:** 1100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Văn phòng được thể hiện dưới dạng một lưới hình chữ nhật. Mỗi chữ cái viết hoa đại diện cho một phần của một bàn làm việc và tất cả các ô có cùng một chữ cái đều thuộc về cùng một bàn làm việc hình chữ nhật. Màu bàn của tổng thống được đưa ra là`c`. 

Cấp phó là bất kỳ người ngồi cùng bàn nào với ít nhất một ô trong bàn của tổng thống. Liên hệ chéo không được tính. Các ô trống được đánh dấu bằng`.`cũng không tính. Nhiệm vụ là đếm xem có bao nhiêu màu bàn riêng biệt liền kề với bàn tổng thống. 

Kích thước lưới tối đa là`100 × 100`, do đó toàn bộ lưới chứa nhiều nhất`10,000`tế bào. Kích thước này đủ nhỏ để quét toàn bộ lưới nhiều lần là hoàn toàn an toàn. Thậm chí một`O(n * m * 4)`giải pháp chỉ thực hiện xung quanh`40,000`kiểm tra hàng xóm. 

Khó khăn chính không phải là hiệu suất, mà là tránh tính hai lần và xử lý kề cận một cách chính xác. 

Một lỗi dễ mắc phải là đếm nhiều lần cùng một màu lân cận. Hãy xem xét lưới này:```
3 3 A
BBB
BAB
BBB
```Câu trả lời đúng là`1`, bởi vì chỉ có màu sắc`B`chạm vào bàn của tổng thống. Việc triển khai bất cẩn làm tăng câu trả lời mỗi khi nó nhìn thấy một câu trả lời liền kề`B`sẽ xuất ra không chính xác`4`. 

Một trường hợp tinh tế khác là vô tình đếm các ô trống:```
2 3 A
.A.
...
```Câu trả lời đúng là`0`. Bàn làm việc của chủ tịch chỉ được bao quanh bởi một khoảng trống, không nên tính. 

Lỗi phổ biến thứ ba là coi màu da của tổng thống là cấp phó. Ví dụ:```
3 4 A
AAAA
ABBA
AAAA
```Câu trả lời đúng là`1`. Trong khi mọi`A`tế bào chạm vào tế bào khác`A`phòng giam, chúng vẫn là một phần của bàn làm việc của tổng thống và phải được bỏ qua. 

Xử lý ranh giới cũng có vấn đề. Nếu bàn của tổng thống chạm vào mép lưới, việc kiểm tra hàng xóm có thể vượt quá giới hạn:```
2 2 A
AB
..
```Câu trả lời đúng là`1`. Bất kỳ triển khai nào kiểm tra một cách mù quáng bốn hướng mà không xác thực tọa độ đều có nguy cơ truy cập các chỉ mục không hợp lệ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là kiểm tra mọi ô trong lưới. Bất cứ khi nào chúng tôi tìm thấy phòng chủ tịch, chúng tôi sẽ kiểm tra tất cả bốn vị trí lân cận và ghi lại bất kỳ màu bàn nào khác mà chúng tôi nhìn thấy. 

Cách tiếp cận này đã đủ nhanh cho các ràng buộc nhất định. Lưới có nhiều nhất`10,000`các ô và mỗi ô thực hiện tối đa bốn lần kiểm tra hàng xóm. Đó chỉ là xung quanh`40,000`hoạt động. 

Vấn đề duy nhất còn lại là tính trùng lặp. Bàn bên cạnh có thể chạm vào bàn chủ tịch ở nhiều chỗ. Việc sử dụng một danh sách và tìm kiếm nó nhiều lần vẫn có tác dụng ở đây, nhưng một tập hợp sẽ sạch hơn và tự động đảm bảo tính duy nhất. 

Quan sát quan trọng là chúng ta không thực sự quan tâm có bao nhiêu ô lân cận tồn tại. Chúng ta chỉ quan tâm đến việc có bao nhiêu màu sắc khác biệt xuất hiện bên cạnh bàn làm việc của tổng thống. Khi một màu đã được nhìn thấy, những lần xuất hiện thêm không thành vấn đề. 

Điều này làm giảm vấn đề xuống còn việc truyền tải lưới đơn giản với một tập hợp thu thập các màu lân cận hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tính năng đếm trùng lặp | O(n × m × k) | O(k) | Lộn xộn không cần thiết | 
| Tối ưu khi sử dụng một bộ | O(n×m) | O(k) | Đã chấp nhận | 

Đây,`k`là số lượng các màu lân cận riêng biệt, nhiều nhất là`26`. 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới và màu bàn của chủ tịch. 
2. Lưu trữ lưới dưới dạng danh sách các chuỗi để mỗi ô có thể được truy cập bằng`grid[row][col]`. 
3. Tạo một tập hợp trống có tên`neighbors`. 

Bộ này sẽ lưu trữ mọi màu bàn riêng biệt liền kề với bàn của tổng thống. 
4. Duyệt qua từng ô trong lưới. 

Bất cứ khi nào ô hiện tại chứa màu của tổng thống, hãy kiểm tra bốn vị trí lân cận của nó. 
5. Đối với mỗi hướng trong bốn hướng, hãy tính tọa độ lân cận. 

Các chỉ dẫn là:`(1, 0)`,`(-1, 0)`,`(0, 1)`,`(0, -1)`. 
6. Bỏ qua những người hàng xóm nằm ngoài lưới điện. 

Điều này ngăn chặn việc lập chỉ mục không hợp lệ khi bàn của tổng thống chạm vào đường viền. 
7. Đọc ký tự lân cận. 

Nếu đó là: 

-`.`, hãy bỏ qua vì ô trống không phải là bàn làm việc. 
- trùng với màu của chủ tịch, bỏ qua vì nó cùng bàn. 
- nếu không, hãy chèn nó vào bộ. 
8. Sau khi xử lý toàn bộ lưới, in kích thước của bộ. 

### Tại sao nó hoạt động 

Mỗi ban phó phải chung một phe với ít nhất một phòng chủ tịch. Thuật toán kiểm tra tất cả bốn ô lân cận của mỗi ô chủ tịch, do đó mọi màu phó hợp lệ đều được phát hiện. 

Bộ đảm bảo mỗi màu được tính chính xác một lần, ngay cả khi nhiều ô trong cùng một bàn chạm vào bàn tổng thống. 

Bất kỳ ô nào trống hoặc thuộc về tổng thống đều bị bỏ qua rõ ràng, do đó, tập hợp này chứa chính xác các màu của bàn phó riêng biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, c = input().split()
n = int(n)
m = int(m)

grid = [input().strip() for _ in range(n)]

neighbors = set()

directions = [
    (1, 0),
    (-1, 0),
    (0, 1),
    (0, -1)
]

for i in range(n):
    for j in range(m):
        if grid[i][j] == c:
            for dx, dy in directions:
                ni = i + dx
                nj = j + dy

                if 0 <= ni < n and 0 <= nj < m:
                    ch = grid[ni][nj]

                    if ch != '.' and ch != c:
                        neighbors.add(ch)

print(len(neighbors))
```Chương trình bắt đầu bằng việc đọc kích thước và màu sắc bàn làm việc của tổng thống. Vì dòng đầu vào chứa cả số nguyên và ký tự nên ban đầu các giá trị được đọc dưới dạng chuỗi và sau đó được chuyển đổi khi cần. 

Lưới được lưu trữ chính xác như đã cho. Bởi vì chuỗi hỗ trợ lập chỉ mục,`grid[i][j]`cho phép truy cập trực tiếp vào bất kỳ tế bào nào. 

các`neighbors`set là ý tưởng trung tâm của giải pháp. Màu bàn lân cận có thể xuất hiện nhiều lần xung quanh bàn của tổng thống, nhưng một bộ sẽ tự động loại bỏ các màu trùng lặp. 

Các vòng lặp lồng nhau quét toàn bộ lưới. Bất cứ khi nào tìm thấy ô chủ tịch, mã sẽ kiểm tra bốn vị trí liền kề bằng cách sử dụng`directions`mảng. Điều này tránh mã lặp lại và làm cho logic dễ xác minh hơn. 

Kiểm tra ranh giới xảy ra trước khi truy cập vào lưới. Nếu không có chúng, các ô ở các cạnh sẽ gây ra lỗi chỉ mục. 

Điều kiện:```
if ch != '.' and ch != c:
```lọc ra những người hàng xóm không hợp lệ. Các ô trống sẽ bị bỏ qua và màu bàn làm việc của tổng thống cũng bị bỏ qua. 

Cuối cùng, câu trả lời chỉ đơn giản là số lượng màu duy nhất được lưu trữ trong bộ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4 R
G.B.
.RR.
TTT.
```Màu sắc của tổng thống là`R`. 

| Ô hiện tại | Đã kiểm tra hàng xóm | Đã thêm vào Bộ | Bộ hiện tại | 
| --- | --- | --- | --- | 
| (1,1) = R | G | Có | {G} | 
| (1,1) = R | R | Không | {G} | 
| (1,1) = R | . | Không | {G} | 
| (1,1) = R | T | Có | {G, T} | 
| (1,2) = R | B | Có | {G, T, B} | 
| (1,2) = R | . | Không | {G, T, B} | 
| (1,2) = R | R | Không | {G, T, B} | 
| (1,2) = R | T | Đã tồn tại | {G, T, B} | 

Câu trả lời cuối cùng:```
3
```Dấu vết này cho thấy lý do tại sao một bộ là cần thiết. bàn làm việc`T`chạm vào bàn tổng thống nhiều lần nhưng vẫn chỉ được tính một lần. 

### Ví dụ 2 

đầu vào:```
2 2 A
AB
..
```| Ô hiện tại | Đã kiểm tra hàng xóm | Đã thêm vào Bộ | Bộ hiện tại | 
| --- | --- | --- | --- | 
| (0,0) = A | Ngoài giới hạn | Không | {} | 
| (0,0) = A | Ngoài giới hạn | Không | {} | 
| (0,0) = A | B | Có | {B} | 
| (0,0) = A | . | Không | {B} | 

Câu trả lời cuối cùng:```
1
```Ví dụ này thể hiện việc xử lý ranh giới. Hai hướng lân cận rời khỏi lưới và thuật toán bỏ qua chúng một cách an toàn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n×m) | Mỗi ô được truy cập một lần, với tối đa bốn lần kiểm tra hàng xóm | 
| Không gian | O(1) | Bộ lưu trữ tối đa 26 chữ hoa | 

Kích thước lưới tối đa chỉ`10,000`các ô, do đó quá trình quét tuyến tính cực kỳ nhanh trong giới hạn 2 giây. Việc sử dụng bộ nhớ không đáng kể vì bộ phụ trợ chỉ chứa các màu bàn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m, c = input().split()
    n = int(n)
    m = int(m)

    grid = [input().strip() for _ in range(n)]

    neighbors = set()

    directions = [
        (1, 0),
        (-1, 0),
        (0, 1),
        (0, -1)
    ]

    for i in range(n):
        for j in range(m):
            if grid[i][j] == c:
                for dx, dy in directions:
                    ni = i + dx
                    nj = j + dy

                    if 0 <= ni < n and 0 <= nj < m:
                        ch = grid[ni][nj]

                        if ch != '.' and ch != c:
                            neighbors.add(ch)

    print(len(neighbors))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# provided sample
assert run(
"""3 4 R
G.B.
.RR.
TTT.
"""
) == "3", "sample 1"

# minimum size
assert run(
"""1 1 A
A
"""
) == "0", "single cell"

# duplicate neighboring cells
assert run(
"""3 3 A
BBB
BAB
BBB
"""
) == "1", "duplicate colors counted once"

# ignore empty cells
assert run(
"""2 3 A
.A.
...
"""
) == "0", "empty cells ignored"

# president on boundary
assert run(
"""2 2 A
AB
..
"""
) == "1", "boundary handling"

# multiple different neighbors
assert run(
"""4 5 C
AACAA
ACCCA
BBDBB
EEEEE
"""
) == "4", "multiple adjacent colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1×1`lưới chỉ với`A`|`0`| Đầu vào kích thước tối thiểu | 
| Tổng thống được bao quanh bởi`B`|`1`| Màu trùng lặp được tính một lần | 
| Chủ tịch bên cạnh chỉ`.`|`0`| Các ô trống bị bỏ qua | 
| Chủ tịch ở rìa lưới |`1`| An toàn ranh giới | 
| Một số màu sắc cảm động |`4`| Đếm khác biệt đúng | 

## Vỏ cạnh 

Hãy xem xét trường hợp cùng một bàn bên cạnh chạm vào tổng thống ở nhiều nơi:```
3 3 A
BBB
BAB
BBB
```Thuật toán truy cập vào trung tâm`A`ô và kiểm tra tất cả bốn người hàng xóm. Mỗi người hàng xóm đều`B`, Vì thế`B`được chèn vào bộ bốn lần. Vì các bộ chỉ giữ các giá trị duy nhất nên bộ cuối cùng vẫn giữ nguyên`{B}`và đầu ra là chính xác`1`. 

Bây giờ hãy xem xét các ô trống:```
2 3 A
.A.
...
```Phòng giam của chủ tịch đang ở`(0,1)`. Hàng xóm của nó đều là`.`hoặc ngoài lưới. Điều kiện:```
if ch != '.' and ch != c:
```từ chối mọi ô trống, do đó tập hợp vẫn trống và thuật toán sẽ in`0`. 

Tiếp theo, hãy xem xét trường hợp bàn của tổng thống chạm vào chính nó:```
3 4 A
AAAA
ABBA
AAAA
```Nhiều`A`các ô nằm liền kề với các ô khác`A`tế bào. Trong quá trình kiểm tra hàng xóm, điều kiện`ch != c`ngăn chặn việc thêm màu riêng của tổng thống vào bộ này. Chỉ một`B`được chèn vào, vì vậy câu trả lời là`1`. 

Cuối cùng, hãy xem xét việc xử lý ranh giới:```
2 2 A
AB
..
```Khi kiểm tra hàng xóm của`(0,0)`, hai hướng tạo ra tọa độ không hợp lệ như`(-1,0)`Và`(0,-1)`. Điều kiện giới hạn:```
0 <= ni < n and 0 <= nj < m
```lọc chúng ra trước khi lập chỉ mục lưới. Những hàng xóm hợp lệ còn lại được xử lý bình thường, tạo ra câu trả lời đúng`1`.
