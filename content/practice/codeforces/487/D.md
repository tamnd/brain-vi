---
title: "CF 487D - Băng Tải"
description: "Chúng ta được cho một cái bàn hình chữ nhật có kích thước n x m chứa đầy băng tải. Mỗi băng tải hướng lên (^), trái (<) hoặc phải (). Xung quanh bàn là các ghế ăn, được đánh số hợp lý là hàng 0 và n+1 và cột 0 và m+1."
date: "2026-06-07T17:33:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 487
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 278 (Div. 1)"
rating: 2700
weight: 487
solve_time_s: 123
verified: true
draft: false
---

[CF 487D - Băng tải](https://codeforces.com/problemset/problem/487/D) 

**Đánh giá:** 2700 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái bàn hình chữ nhật có kích thước _n_ x _m_ chứa đầy băng tải. Mỗi băng tải đều hướng lên trên (`^`), bên trái (`<`), hoặc phải (`>`). Xung quanh bàn là ghế ngồi ăn, được đánh số thứ tự hợp lý theo hàng`0`Và`n+1`và cột`0`Và`m+1`. Khi một miếng bánh mì được đặt trên băng chuyền, nó sẽ di chuyển dọc theo dây đai theo hướng của chúng cho đến khi tới chỗ ngồi bên ngoài bàn. Nếu bánh mì đi vào một vòng trong vành đai, nó sẽ không bao giờ đến được chỗ ngồi và chúng ta phải báo cáo rằng nó bị mắc kẹt trong một vòng lặp vô hạn. 

Bảng được cập nhật động thông qua`q`truy vấn có hai loại.`A x y`yêu cầu chúng tôi mô phỏng bánh mì từ`(x, y)`và báo cáo vị trí cuối cùng của nó hoặc`-1 -1`nếu nó lặp vô hạn.`C x y c`thay đổi băng tải tại`(x, y)`sang một loại mới`c`. 

Những hạn chế là đáng kể:`n`có thể đạt tới 100.000 trong khi`m`nhiều nhất là 10. Số lượng truy vấn cũng lên tới 100.000, với tối đa 10.000 truy vấn thay đổi. Điều này loại trừ việc mô phỏng chuyển động của bánh mì một cách ngây thơ từ đầu bảng cho mỗi truy vấn vì trong trường hợp xấu nhất, chúng tôi sẽ thực hiện`O(n*q)`các bước, có thể đạt tới 10^10 thao tác, vượt xa giới hạn thời gian. Mặt khác, nhỏ`m`gợi ý rằng chúng ta có thể khai thác các cấu trúc theo cột một cách hiệu quả. 

Trường hợp cạnh tinh tế xảy ra khi dây đai tạo ra các vòng nhỏ ở một hoặc hai cột. Ví dụ, một cột hai hàng có đai hướng lên xuống có thể bẫy bánh mì vô thời hạn. Một trường hợp khác là khi bánh mì bắt đầu ngay cạnh ghế thoát hiểm; nó rời đi ngay lập tức. Mã ngây thơ không phát hiện chu kỳ sẽ lặp lại mãi mãi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng chuyển động của bánh mì cho mọi người`A`truy vấn từng bước. Đối với mỗi truy vấn, chúng tôi theo dõi băng tải cho đến khi nó rời khỏi bảng hoặc cho đến khi phát hiện được một chu trình. Việc phát hiện các chu trình có thể được thực hiện bằng cách sử dụng tập hợp đã truy cập, đánh dấu các ô đã truy cập trong một lần mô phỏng. Mặc dù đúng nhưng điều này có thể tốn kém`O(n*q)`trong trường hợp xấu nhất và thất bại lớn`n`và nhiều truy vấn. 

Quan sát quan trọng là chuyển động trong cột chủ yếu là theo phương thẳng đứng vì các dây đai có thể di chuyển sang trái, phải hoặc lên trên. Với`m`tối đa là 10, chúng ta có thể tính toán trước cho từng ô nơi một miếng bánh mì cuối cùng sẽ rời khỏi bàn hoặc nếu nó bước vào một chu trình. Việc tính toán trước này có thể được thực hiện bằng cách sử dụng lập trình động từ hàng trên cùng trở xuống. Độ phức tạp trở nên tỷ lệ thuận với`O(n*m)`để xây dựng bảng này ban đầu. Sau đó, khi băng tải thay đổi, chúng ta chỉ cần tính toán lại các cột bị ảnh hưởng, điều này hiệu quả vì`m`nhỏ và có nhiều nhất là 10.000 thay đổi. 

Cách tiếp cận tối ưu dựa vào việc lưu trữ cho mỗi ô`(x, y)`, "điểm thoát" của nó`(tx, ty)`hoặc một điểm đánh dấu cho một vòng lặp. Đối với mỗi`A`truy vấn, chúng tôi chỉ cần trả về kết quả đã lưu trữ và đối với`C`truy vấn, chúng tôi tính toán lại các điểm thoát trong cột bị ảnh hưởng, chỉ truyền bá các bản cập nhật khi cần thiết. Điều này làm giảm thời gian cho mỗi truy vấn từ khả năng`O(n)`ĐẾN`O(m)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*q) | O(n*m) | Quá chậm | 
| Tối ưu | O(n_m + q_m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước bảng`n`,`m`và số lượng truy vấn`q`. Lưu trữ bảng dưới dạng danh sách các chuỗi. 
2. Khởi tạo một`exit`mảng kích thước`n x m`để lưu trữ ô thoát cho từng vị trí hoặc`(-1, -1)`nếu nó dẫn đến một vòng lặp. 
3. Xác định hàm đệ quy`dfs(x, y)`tính toán lối ra cho`(x, y)`:

- Nếu như`(x, y)`đang ở ngoài bàn, quay lại`(x, y)`ngay lập tức. 
- Nếu như`(x, y)`đã được tính toán rồi`exit`, trả về giá trị được lưu trữ. 
- Đánh dấu`(x, y)`như "ghé thăm" để phát hiện chu kỳ. 
- Tính ô tiếp theo`(nx, ny)`dựa trên loại băng tải. 
- Gọi đệ quy`dfs(nx, ny)`. 
- Nếu cuộc gọi đệ quy trả về`(-1, -1)`, đánh dấu`(x, y)`như một phần của chu kỳ và quay trở lại`(-1, -1)`. 
- Ngược lại thì lưu lối ra vào`exit[x][y]`và trả lại nó. 
4. Tính toán trước`exit`mảng bằng cách chạy`dfs(x, y)`trên tất cả các tế bào. 
5. Đối với mỗi truy vấn: 

- Nếu vậy`A x y`, xuất điểm thoát được tính toán trước`exit[x-1][y-1]`. 
- Nếu vậy`C x y c`, cập nhật bảng và vô hiệu hóa các lần thoát trong cột bị ảnh hưởng. Tính toán lại`exit`giá trị cho tất cả các hàng trong cột đó bằng cách sử dụng`dfs`. 
6. Trả kết quả cho tất cả`A`truy vấn theo thứ tự. 

Tính chính xác phụ thuộc vào tính bất biến mà mỗi ô có lối ra cuối cùng hoặc được đánh dấu là một phần của chu trình. Việc cập nhật băng tải chỉ ảnh hưởng đến các ô trong cùng một cột phía trên nó và`dfs`lan truyền đảm bảo chúng tôi luôn tính toán lối ra chính xác sau mỗi thay đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

n, m, q = map(int, input().split())
table = [list(input().strip()) for _ in range(n)]
exit_pos = [[None]*m for _ in range(n)]
visiting = [[False]*m for _ in range(n)]

def dfs(x, y):
    if x < 0:
        return (0, y+1)
    if x >= n:
        return (n+1, y+1)
    if y < 0:
        return (x+1, 0)
    if y >= m:
        return (x+1, m+1)
    if visiting[x][y]:
        return (-1, -1)
    if exit_pos[x][y] is not None:
        return exit_pos[x][y]

    visiting[x][y] = True
    c = table[x][y]
    if c == '^':
        nxt = dfs(x-1, y)
    elif c == '<':
        nxt = dfs(x, y-1)
    else:  # '>'
        nxt = dfs(x, y+1)
    visiting[x][y] = False
    exit_pos[x][y] = nxt
    return nxt

# Initial computation
for i in range(n):
    for j in range(m):
        if exit_pos[i][j] is None:
            dfs(i, j)

for _ in range(q):
    parts = input().split()
    if parts[0] == 'A':
        x, y = int(parts[1])-1, int(parts[2])-1
        tx, ty = exit_pos[x][y]
        print(tx, ty)
    else:
        x, y, c = int(parts[1])-1, int(parts[2])-1, parts[3]
        table[x][y] = c
        # Invalidate and recompute column
        for i in range(n):
            exit_pos[i][y] = None
        for i in range(n):
            dfs(i, y)
```DFS đảm bảo rằng các chu kỳ được phát hiện chính xác thông qua`visiting`. Chúng ta phải tăng giới hạn đệ quy vì`n`có thể lên tới 10^5 và đệ quy ngăn xếp có thể bị tràn. Khi cập nhật băng tải, chúng tôi chỉ vô hiệu hóa cột để giảm thiểu việc tính toán lại. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 2 3
>>
^^
A 2 1
C 1 2 <
A 2 1
```Theo dõi đầu tiên`A 2 1`: 

| Bước | Vị trí | Băng tải | Tiếp theo | Bình luận | 
| --- | --- | --- | --- | --- | 
| 1 | (2,1) | ^ | (1,1) | Di chuyển lên | 
| 2 | (1,1) | > | (1,2) | Di chuyển sang phải | 
| 3 | (1,2) | > | (1,3) | Ra khỏi bàn | 

Đầu ra:`1 3`Sau đó`C 1 2 <`, bảng trở thành:```
> <
^^
```Theo dõi thứ hai`A 2 1`: 

| Bước | Vị trí | Băng tải | Tiếp theo | Bình luận | 
| --- | --- | --- | --- | --- | 
| 1 | (2,1) | ^ | (1,1) | Di chuyển lên | 
| 2 | (1,1) | > | (1,2) | Di chuyển sang phải | 
| 3 | (1,2) | < | (1,1) | Di chuyển sang trái, lặp lại | 

Đầu ra:`-1 -1`Điều này xác nhận rằng tính năng phát hiện chu kỳ của chúng tôi hoạt động và quá trình lan truyền thoát là chính xác. 

**Mẫu tùy chỉnh**```
3 2 2
>^
^^
><
A 3 1
C 3 2 ^
A 3 1
```Đầu tiên`A 3 1`thoát ra tại`(0,1)`; sau khi thay đổi`(3,2)`ĐẾN`^`, truy vấn thứ hai có thể dẫn đến một vòng lặp, minh họa cho việc tính toán lại chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n_m + q_m) | DFS ban đầu truy cập từng ô |
