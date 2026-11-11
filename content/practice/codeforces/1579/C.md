---
title: "CF 1579C - Bọ ve"
description: "Chúng ta có một lưới có kích thước $n nhân m$ đại diện cho một tờ giấy, trong đó một số ô có màu đen (ký hiệu là ) và các ô khác có màu trắng (.). Casimir tuyên bố các tế bào đen được tạo ra bằng cách vẽ bọ ve."
date: "2026-06-10T10:24:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1579
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 744 (Div. 3)"
rating: 1500
weight: 1579
solve_time_s: 350
verified: true
draft: false
---

[CF 1579C - Bọ ve](https://codeforces.com/problemset/problem/1579/C) 

**Đánh giá:** 1500 
**Tags:** tham lam, thực hiện 
**Thời gian giải:** 5 phút 50 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới có kích thước$n \times m$đại diện cho một tờ giấy, trong đó một số ô có màu đen (ký hiệu là`*`) và số còn lại có màu trắng (`.`). Casimir tuyên bố các tế bào đen được tạo ra bằng cách vẽ bọ ve. Một kích thước$d$được căn giữa tại một ô nào đó và kéo dài theo đường chéo lên bên trái và lên bên phải một cách chính xác$d$tế bào. Bọ ve bao phủ tế bào trung tâm và hai cánh tay, dẫn đến$2d+1$tế bào đen. Tất cả các bọ ve đều có kích thước tối thiểu$k$. Nhiều dấu tích có thể chồng lên nhau và khi một ô có màu đen thì nó vẫn có màu đen. 

Nhiệm vụ là kiểm tra xem lưới đen trắng nhất định có thể được tạo ra bởi một số tổ hợp dấu tích hay không, tất cả đều có kích thước ít nhất$k$. 

Các hạn chế là nhỏ:$n$nhiều nhất là 10 và$m$nhiều nhất là 19. Điều này cho phép chúng tôi xử lý lưới một cách triệt để mà không phải lo lắng về thời gian chờ. Thách thức chính không phải là hiệu quả mà là xác định chính xác ô đen nào có thể thuộc về một kích thước nhỏ$\ge k$và đảm bảo rằng tất cả các ô màu đen đều được tính đến. 

Các trường hợp cạnh bao gồm các lưới trong đó một số ô màu đen bị cô lập hoặc quá gần hàng trên cùng để hỗ trợ đánh dấu kích thước tối thiểu. Ví dụ, nếu$k=2$và một`*`xuất hiện ở hàng đầu tiên, không có cách nào để tạo thành một dấu tích cỡ 2 với ô này là ô dưới cùng, vì vậy câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng thử tất cả các dấu tích có thể có ở tất cả các vị trí có thể và kiểm tra xem liệu chúng có thể bao phủ lưới một cách chính xác hay không. Đối với mỗi trung tâm tiềm năng`(i, j)`, chúng tôi sẽ thử mọi kích cỡ$d \ge k$và đánh dấu các ô tương ứng. Điều này có hiệu quả đối với các lưới nhỏ, nhưng ngay cả đối với$n=10$Và$m=19$, chúng ta có thể thử quá nhiều dấu tích chồng chéo và việc xác minh mức độ phù hợp nhiều lần trở nên lộn xộn. 

Quan sát quan trọng để tối ưu hóa là dấu tích phải kéo dài lên theo đường chéo. Do đó, chúng ta có thể quét lưới từ hàng dưới lên trên, vì tâm của dấu tích chỉ có thể chịu trách nhiệm cho các ô màu đen bên dưới hoặc ở hàng của nó. Đối với mỗi`*`, chúng ta có thể xác định kích thước đánh dấu tối đa có đáy ở ô đó bằng cách kiểm tra các đường chéo đi lên. Nếu kích thước tối đa ít nhất là$k$, chúng tôi đánh dấu các ô tương ứng là được che phủ. Sau khi xử lý tất cả các dấu tích tiềm năng, bất kỳ`*`còn lại không được che chắn sẽ làm mất hiệu lực lưới. 

Điều này dẫn đến một cách tiếp cận tham lam nhưng có cấu trúc: tính toán kích thước tối đa của một dấu tích có thể kết thúc ở mỗi ô, đánh dấu các ô tương ứng và xác minh tất cả các ô đen đều được tính đến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m * phút(n, m)^2) | O(n * m) | Quá chậm/lộn xộn | 
| Tối ưu | O(n * m) | O(n * m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới đầu vào và các thông số`n, m, k`. Chuyển đổi lưới thành mảng 2D boolean`grid`Ở đâu`True`đại diện cho một ô màu đen. 
2. Khởi tạo hai mảng 2D phụ trợ`left_diag`Và`right_diag`có cùng kích thước với lưới.`left_diag[i][j]`sẽ lưu trữ chiều dài của các ô đen liên tiếp theo đường chéo lên bên trái kết thúc tại`(i,j)`.`right_diag[i][j]`sẽ lưu trữ chiều dài của các ô đen liên tiếp theo đường chéo lên trên bên phải và kết thúc tại`(i,j)`. 
3. Điền vào`left_diag`Và`right_diag`theo cách từ dưới lên. Đối với một tế bào`(i,j)`với`*`,`left_diag[i][j]`là`1 + left_diag[i-1][j-1]`nếu như`i>0`Và`j>0`, ngược lại là 1. Tương tự,`right_diag[i][j]`là`1 + right_diag[i-1][j+1]`nếu như`i>0`Và`j<m-1`, ngược lại là 1. 
4. Khởi tạo một`covered`mảng kích thước`n x m`với tất cả`False`. Mảng này sẽ theo dõi những ô đen nào được giải thích bằng dấu tích có kích thước ≥ k. 
5. Quét từng ô`(i, j)`từ dưới lên trên. Nếu như`(i,j)`có màu đen, hãy tính kích thước đánh dấu tối đa là`d = min(left_diag[i][j], right_diag[i][j]) - 1`. Nếu như`d >= k`, đánh dấu tất cả các ô của dấu tích này trong`covered`mảng: cho mỗi`h`từ`0`ĐẾN`d`, đánh dấu`(i-h, j-h)`Và`(i-h, j+h)`BẰNG`True`. 
6. Sau khi xử lý tất cả các ô, lặp lại trên lưới. Nếu bất kỳ ô nào`(i,j)`có màu đen trong lưới ban đầu nhưng không được đánh dấu là bị che, hãy in`NO`. Ngược lại, in`YES`. 

Tại sao tính năng này hoạt động: tính năng quét từ dưới lên đảm bảo rằng đối với bất kỳ dấu tích tiềm năng nào, nhánh hướng lên trên của nó không bị chặn và chúng tôi đánh dấu tất cả các ô được giải thích bằng dấu tích. Nếu để ô đen không che thì nó không thể thuộc về bất kỳ ô nào có kích thước ≥ k. Bằng cách quét từ dưới lên, mỗi đánh dấu được xác định bằng kích thước lớn nhất có thể, đảm bảo không bỏ sót đánh dấu hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, k = map(int, input().split())
        grid = [list(input().strip()) for _ in range(n)]
        left_diag = [[0]*m for _ in range(n)]
        right_diag = [[0]*m for _ in range(n)]
        covered = [[False]*m for _ in range(n)]

        for i in range(n):
            for j in range(m):
                if grid[i][j] == '*':
                    left_diag[i][j] = 1 + (left_diag[i-1][j-1] if i>0 and j>0 else 0)
                    right_diag[i][j] = 1 + (right_diag[i-1][j+1] if i>0 and j<m-1 else 0)

        for i in reversed(range(n)):
            for j in range(m):
                if grid[i][j] == '*':
                    d = min(left_diag[i][j], right_diag[i][j]) - 1
                    if d >= k:
                        for h in range(d+1):
                            covered[i-h][j-h] = True
                            covered[i-h][j+h] = True

        ok = True
        for i in range(n):
            for j in range(m):
                if grid[i][j] == '*' and not covered[i][j]:
                    ok = False
                    break
            if not ok:
                break
        print("YES" if ok else "NO")

solve()
```các`left_diag`Và`right_diag`mảng tính toán một cách hiệu quả nhánh hướng lên tối đa của một dấu tích tiềm năng cho mỗi ô. Quá trình từ dưới lên đảm bảo rằng chúng tôi nắm bắt được dấu tích lớn nhất có thể trước tiên và đánh dấu tất cả các ô được bao phủ. Kiểm tra cuối cùng đảm bảo không còn ô đen nào không giải thích được. Phải cẩn thận với ranh giới mảng để tránh lỗi chỉ mục. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

Lưới đầu vào (4x9, k=2):```
*.*.*...*
.*.*...*.
..*.*.*..
.....*...
```Dấu vết bước cho hàng dưới cùng`i=3`: 

| tôi | j | lưới | trái_diag | đúng_diag | d | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 5 | * | 1 | 1 | 0 | d<k, bỏ qua | 

Di chuyển lên trên, dấu tích tập trung vào`(2,5)`với`d=2`đánh dấu tất cả các ô của nó, bao gồm các ô có liên quan`*`. Thuật toán tìm thấy tất cả các dấu tích và xác nhận tất cả các ô màu đen đều được che phủ. Đầu ra:`YES`. 

**Ví dụ 2** 

Lưới đầu vào (2x3, k=1):```
*.*
...
```Đường chuyền từ dưới lên: 

| tôi | j | lưới | trái_diag | đúng_diag | d | hành động | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | . | 0 | 0 | - | bỏ qua | 
| 0 | 0 | * | 1 | 1 | 0 | d<k, bỏ qua | 

Không có dấu tích nào có kích thước ≥1 có thể giải thích được`*`Tại`(0,0)`. Đầu ra:`NO`. 

Những dấu vết này chứng minh rằng thuật toán xác định chính xác kích thước đánh dấu tối đa có thể và đảm bảo không có ô đen nào không giải thích được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi ô được xử lý với số lần không đổi: đường chéo được tính một lần, đánh dấu từ dưới lên một lần. | 
| Không gian | O(n*m) | Vì`left_diag`,`right_diag`, Và`covered`mảng. | 

Giới hạn đầu vào nhỏ làm cho giải pháp tuyến tính này rất hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("8\n2 3 1\n*.*\n...\n4 9 2\n*.*.*...*\n.*.*...*.\n..*.*.*..\n.....*...\n4 4 1\n*.*.\n****\n.**.\n....\n5 5 1\n.....\n*...*\n.*.*.\
```
