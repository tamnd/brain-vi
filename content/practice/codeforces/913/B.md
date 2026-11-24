---
title: "CF 913B - Vân sam giáng sinh"
description: "Chúng ta được cho một cây có gốc có các đỉnh được đánh số từ 1 đến n. Đỉnh 1 luôn là gốc. Đối với mọi đỉnh khác, đầu vào cho chúng ta biết đỉnh gốc của nó, đỉnh này xác định hoàn toàn cấu trúc cây. Một đỉnh được coi là lá nếu nó không có con và không phải là gốc."
date: "2026-06-12T10:10:03+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 913
codeforces_index: "B"
codeforces_contest_name: "Hello 2018"
rating: 1200
weight: 913
solve_time_s: 178
verified: true
draft: false
---

[CF 913B - Vân sam Giáng sinh](https://codeforces.com/problemset/problem/913/B) 

**Đánh giá:** 1200 
**Tags:** thực hiện, cây cối 
**Thời gian giải:** 2m 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với các đỉnh được đánh số từ 1 đến _n_. Đỉnh 1 luôn là gốc. Đối với mọi đỉnh khác, đầu vào cho chúng ta biết đỉnh gốc của nó, đỉnh này xác định hoàn toàn cấu trúc cây. 

Một đỉnh được coi là lá nếu nó không có con và không phải là gốc. Một cây được gọi là cây vân sam khi mỗi đỉnh không có lá đều có ít nhất ba cây con cũng chính là lá. 

Nhiệm vụ chỉ đơn giản là xác định xem cây đã cho có thỏa mãn điều kiện này hay không. Nếu mỗi đỉnh không có lá đều có ít nhất ba lá con, in`"Yes"`. Nếu không thì in`"No"`. 

Những hạn chế là rất nhỏ. Cây có nhiều nhất là 1000 đỉnh. Ngay cả một thuật toán kiểm tra mọi đỉnh và tất cả các đỉnh con của nó cũng sẽ chạy thoải mái trong giới hạn. Không cần các thuật toán cây nâng cao, thủ thuật đệ quy hoặc tiền xử lý nặng nề. 

Thách thức chính là giải thích điều kiện một cách chính xác. Chúng tôi không tính tất cả con cháu là lá. Chúng tôi chỉ tính những đứa trẻ ngay lập tức là lá. 

Hãy xem xét ví dụ này:```
5
1
1
2
2
```Cây đó là:```
1
├─2
│ ├─4
│ └─5
└─3
```Đỉnh 1 chỉ có một lá con là đỉnh 3. Đỉnh 4 và 5 là lá nhưng là cháu của 1 chứ không phải con. Câu trả lời đúng là:```
No
```Việc thực hiện bất cẩn đếm lá con thay vì lá con sẽ chấp nhận cây này một cách không chính xác. 

Một trường hợp khó phát hiện khác là khi một đỉnh trong có nhiều con nhưng không đủ lá:```
7
1
1
1
2
2
2
```Cây đó là:```
1
├─2
│ ├─5
│ ├─6
│ └─7
├─3
└─4
```Đỉnh 2 hợp lệ vì nó có ba lá con. Đỉnh 1 chỉ có 2 lá con là đỉnh 3 và 4. Đáp án đúng là:```
No
```Một giải pháp bất cẩn có thể chỉ kiểm tra gốc hoặc có thể đếm tổng số con thay vì con lá. 

Trường hợp một cạnh nữa là một đỉnh có đúng ba lá con:```
4
1
1
1
```Gốc có 3 lá con nên đáp án là:```
Yes
```Điều kiện là "ít nhất là 3", vì vậy chính xác là 3 phải được chấp nhận. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là kiểm tra mọi đỉnh không phải là lá và đếm xem có bao nhiêu đỉnh con của nó là lá. 

Một phiên bản brute-force có thể xác định xem một đứa trẻ có phải là một chiếc lá hay không bằng cách quét tất cả các đỉnh mỗi lần. Với mỗi đỉnh, với mỗi đứa trẻ, chúng ta tìm kiếm toàn bộ cây để xem liệu đứa trẻ đó có con riêng của nó hay không. Với _n_ đỉnh, điều này dẫn đến công việc khoảng _O(n2)_. 

Với _n = 1000_, thậm chí _O(n²)_ cũng được chấp nhận. Khoảng một triệu hoạt động là trong giới hạn. 

Có một cách tiếp cận thậm chí còn sạch hơn. Trong khi đọc dữ liệu đầu vào, chúng ta xây dựng một danh sách kề chứa các đỉnh con của mỗi đỉnh. Một khi chúng ta có cấu trúc đó, việc xác định xem một đỉnh có phải là một chiếc lá hay không sẽ trở nên tầm thường: một đỉnh là một chiếc lá chính xác khi danh sách con của nó trống. 

Quan sát quan trọng là tình trạng cây vân sam chỉ phụ thuộc vào con cái ngay lập tức. Chúng ta không bao giờ cần duyệt qua các cây con hoặc tính toán độ sâu. Đối với mỗi đỉnh bên trong, chúng ta chỉ cần đếm xem có bao nhiêu con không có con nào. 

Điều này làm giảm giải pháp thành một lần duyệt qua tất cả các danh sách lân cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh. 
2. Xây dựng danh sách con cho mỗi đỉnh. Đối với mỗi đỉnh`i`từ 2 đến`n`, đọc cha mẹ của nó`p`và nối thêm`i`ĐẾN`children[p]`. 
3. Lặp lại qua mọi đỉnh. 
4. Nếu một đỉnh không có con thì đó là một chiếc lá. Điều kiện vân sam không áp dụng cho lá nên bỏ qua. 
5. Với mỗi đỉnh không phải là lá, hãy đếm xem có bao nhiêu đỉnh con ngay lập tức của nó là lá. Một đứa trẻ chính xác là một chiếc lá khi danh sách con của chính nó trống. 
6. Nếu số này nhỏ hơn 3 đối với bất kỳ đỉnh không có lá nào, hãy in`"No"`và dừng lại ngay lập tức. 
7. Nếu mọi đỉnh không phải lá đều vượt qua kiểm tra, hãy in`"Yes"`. 

### Tại sao nó hoạt động 

Một đỉnh vi phạm định nghĩa về cây vân sam khi và chỉ khi nó là một đỉnh bên trong và có ít hơn ba con trực tiếp của nó là lá. 

Thuật toán kiểm tra chính xác điều kiện này cho mọi đỉnh bên trong. Mọi lá đều bị bỏ qua vì định nghĩa không đặt ra hạn chế nào cho các lá. Mỗi đỉnh bên trong được kiểm tra một lần và số lượng lá con được tính toán chính xác theo định nghĩa. 

Nếu thuật toán in`"No"`, một số đỉnh bên trong không đạt yêu cầu nên cây không thể là cây vân sam. Nếu thuật toán in`"Yes"`, mọi đỉnh trong đều thỏa mãn yêu cầu, phù hợp với định nghĩa của cây vân sam. Như vậy thuật toán là đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

children = [[] for _ in range(n + 1)]

for v in range(2, n + 1):
    p = int(input())
    children[p].append(v)

for v in range(1, n + 1):
    if not children[v]:
        continue

    leaf_children = 0

    for child in children[v]:
        if not children[child]:
            leaf_children += 1

    if leaf_children < 3:
        print("No")
        sys.exit()

print("Yes")
```Phần đầu tiên xây dựng cây gốc dưới dạng danh sách kề. Vì đầu vào trực tiếp cung cấp cho cha mẹ của mỗi đỉnh nên việc xây dựng danh sách con rất đơn giản. 

Phần thứ hai lặp qua tất cả các đỉnh. Đỉnh có danh sách con trống là đỉnh nên bị bỏ qua. 

Đối với mỗi đỉnh bên trong, mã sẽ đếm xem bản thân có bao nhiêu con có danh sách con trống. Đó chính xác là những lá con được yêu cầu theo định nghĩa. 

Thời điểm một đỉnh có ít hơn ba lá con, câu trả lời được biết là`"No"`, nên chương trình kết thúc sớm. Nếu vòng lặp kết thúc thì mọi đỉnh trong đều thỏa mãn điều kiện và câu trả lời là`"Yes"`. 

Một lỗi phổ biến là đếm tất cả các lá con thay vì các lá con ngay lập tức. Việc triển khai sẽ tránh được điều đó bằng cách chỉ kiểm tra các đỉnh trong`children[v]`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
1
1
1
```Cây:```
1
├─2
├─3
└─4
```| Đỉnh | Trẻ em | Lá con đếm | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | {2,3,4} | 3 | Có | 
| 2 | {} | Bỏ qua | - | 
| 3 | {} | Bỏ qua | - | 
| 4 | {} | Bỏ qua | - | 

Gốc có đúng ba lá con, thỏa mãn yêu cầu. Mỗi đỉnh còn lại là một chiếc lá nên đáp án là`"Yes"`. 

### Mẫu 2 

đầu vào:```
5
1
1
2
2
```Cây:```
1
├─2
│ ├─4
│ └─5
└─3
```| Đỉnh | Trẻ em | Lá con đếm | Có hiệu lực? | 
| --- | --- | --- | --- | 
| 1 | {2,3} | 1 | Không | 
| 2 | {4,5} | Chưa đạt | - | 
| 3 | {} | - | - | 
| 4 | {} | - | - | 
| 5 | {} | - | - | 

Đỉnh 1 chỉ có một lá con là đỉnh 3. Đỉnh 4 và 5 là cháu nên không tính. Câu trả lời là`"No"`. 

Ví dụ này cho thấy cạm bẫy phổ biến nhất: chỉ có con cái mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh và mỗi cạnh cha-con được kiểm tra một số lần không đổi | 
| Không gian | O(n) | Danh sách kề lưu trữ tất cả các đỉnh và cạnh | 

Vì cây chứa tối đa 1000 đỉnh nên giải pháp O(n) cực kỳ nhanh. Việc sử dụng bộ nhớ cũng rất nhỏ vì danh sách kề chỉ lưu trữ các cạnh của cây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    children = [[] for _ in range(n + 1)]

    for v in range(2, n + 1):
        p = int(input())
        children[p].append(v)

    for v in range(1, n + 1):
        if not children[v]:
            continue

        cnt = 0
        for child in children[v]:
            if not children[child]:
                cnt += 1

        if cnt < 3:
            print("No")
            return

    print("Yes")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run("4\n1\n1\n1\n") == "Yes", "sample 1"

# minimum valid tree
assert run("4\n1\n1\n1\n") == "Yes", "minimum spruce"

# root has only two leaf children
assert run("3\n1\n1\n") == "No", "root needs at least three leaf children"

# internal node valid, root invalid
assert run("7\n1\n1\n1\n2\n2\n2\n") == "No", "count only immediate leaf children"

# larger valid tree
assert run("8\n1\n1\n1\n2\n2\n2\n1\n") == "Yes", "all internal vertices satisfy condition"

# chain structure
assert run("4\n1\n2\n3\n") == "No", "every internal vertex fails"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 1 1 1`| Có | Đúng ba con lá là đủ | 
|`3 / 1 1`| Không | Gốc chỉ có 2 con lá | 
|`7 / 1 1 1 2 2 2`| Không | Con cháu lá không tính | 
|`8 / 1 1 1 2 2 2 1`| Có | Nhiều đỉnh bên trong đều có thể thỏa mãn quy tắc | 
|`4 / 1 2 3`| Không | Chuỗi sâu nơi mọi đỉnh bên trong đều thất bại | 

## Vỏ cạnh 

Hãy xem xét cây:```
5
1
1
2
2
```Thuật toán xây dựng:```
children[1] = [2, 3]
children[2] = [4, 5]
```Khi xử lý đỉnh 1, con 2 không phải là lá vì nó có con. Con 3 là một chiếc lá. Số đếm là 1, nhỏ hơn 3, do đó thuật toán sẽ in`"No"`. 

Điều này xử lý chính xác trường hợp con cháu lá tồn tại nhưng không phải là con ngay lập tức. 

Coi như:```
7
1
1
1
2
2
2
```Vertex 2 có 3 lá con và các pass. Vertex 1 có con`{2,3,4}`. Chỉ có đỉnh 3 và 4 là lá nên số đếm của nó là 2. Thuật toán in`"No"`. 

Điều này xác minh rằng mọi đỉnh bên trong phải thỏa mãn điều kiện, không chỉ một số đỉnh. 

Coi như:```
4
1
1
1
```Các nút con của gốc đều là lá nên số đếm chính xác là 3. Vì điều kiện là "ít nhất là 3" nên thuật toán chấp nhận cây và in ra`"Yes"`. 

Điều này xác nhận trường hợp ranh giới trong đó số đếm chính xác là mức tối thiểu bắt buộc.
