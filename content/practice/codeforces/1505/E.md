---
title: "CF 1505E - Cakewalk"
description: "Chúng ta được tặng một chiếc bánh hình chữ nhật nhỏ được chia thành một lưới các hình vuông. Mỗi ô vuông có một quả mọng hoặc trống. Chuột bắt đầu ở góc trên bên trái của chiếc bánh và chỉ có thể di chuyển sang phải hoặc xuống cho đến khi chạm đến góc dưới cùng bên phải."
date: "2026-06-10T20:30:42+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "implementation", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1505
codeforces_index: "E"
codeforces_contest_name: "April Fools Day Contest 2021"
rating: 1800
weight: 1505
solve_time_s: 123
verified: true
draft: false
---

[CF 1505E - Cakewalk](https://codeforces.com/problemset/problem/1505/E) 

**Đánh giá:** 1800 
**Tags:** *đặc biệt, tham lam, thực hiện, con đường ngắn nhất 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một chiếc bánh hình chữ nhật nhỏ được chia thành một lưới các hình vuông. Mỗi ô vuông có một quả mọng hoặc trống. Chuột bắt đầu ở góc trên bên trái của chiếc bánh và chỉ có thể di chuyển sang phải hoặc xuống cho đến khi chạm đến góc dưới cùng bên phải. Mục tiêu là tính toán xem con chuột sẽ ăn bao nhiêu quả mọng dọc theo con đường mà nó chọn nếu nó đang cố gắng tối đa hóa lượng quả mọng ăn vào. 

Đầu vào đại diện cho chiếc bánh như một`H × W`lưới các ký tự, ở đâu`H`là số hàng và`W`là số cột. Mỗi ô chứa một trong hai`'*'`cho một quả mọng hoặc`'.'`để trống. Đầu ra là một số nguyên duy nhất, số lượng quả mọng tối đa có thể được thu thập dọc theo một đường dẫn hợp lệ từ góc trên bên trái đến góc dưới cùng bên phải. 

Các ràng buộc rất chặt chẽ:`H`Và`W`tối đa là 5. Kích thước nhỏ này cho phép chúng tôi xem xét các phương pháp tiếp cận quá chậm đối với các lưới lớn. Đặc biệt, việc liệt kê tất cả các đường dẫn từ trên cùng bên trái đến dưới cùng bên phải là khả thi vì tổng số đường dẫn nhiều nhất là`C(H+W-2, H-1)`, nhiều nhất là`C(8,4) = 70`. 

Các trường hợp cạnh không rõ ràng phát sinh khi đường dẫn bị hạn chế bởi các cạnh của lưới hoặc khi các quả được nhóm dọc theo các hàng hoặc cột nhất định. Ví dụ: nếu hàng trên cùng trống và tất cả các quả mọng đều ở hàng dưới cùng, thì cách tiếp cận tham lam bất cẩn luôn thích các quả mọng có thể nhìn thấy ngay lập tức có thể chọn sai đường dẫn. Một trường hợp cạnh khác xảy ra khi lưới`1×N`hoặc`N×1`, trong đó chuột chỉ có một đường đi duy nhất. Một giải pháp đúng phải xử lý được những kích thước nhỏ này mà không mắc lỗi. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực ngây thơ sẽ liệt kê tất cả các đường dẫn hợp lệ từ góc trên bên trái đến góc dưới bên phải. Đối với mỗi đường dẫn, chúng tôi sẽ đếm số lượng quả thu được và trả về số lượng tối đa. Cách tiếp cận này hoạt động vì số lượng đường dẫn được giới hạn một cách tổ hợp bởi`(H+W-2 choose H-1)`. Trong trường hợp xấu nhất của lưới 5×5, đây là`C(8,4) = 70`đường dẫn, và mỗi đường dẫn mất`O(H+W)`các bước để đánh giá, đưa ra tổng số tối đa 560 thao tác. Mặc dù khả thi ở đây nhưng cách tiếp cận này không khái quát hóa cho các lưới lớn hơn và khó thực hiện chính xác. 

Một cách tiếp cận tổng quát và tao nhã hơn là sử dụng lập trình động. Chúng tôi xác định`dp[i][j]`là số lượng quả mọng tối đa có thể được thu thập từ góc trên bên trái`(0,0)`đến tế bào`(i,j)`. Cái nhìn sâu sắc quan trọng là để tiếp cận tế bào`(i,j)`, chuột phải đến từ ô phía trên`(i-1,j)`hoặc từ ô bên trái`(i,j-1)`. Do đó, mối quan hệ truy hồi là`dp[i][j] = max(dp[i-1][j], dp[i][j-1]) + (1 if cell (i,j) has a berry else 0)`. Điều này làm giảm vấn đề để điền một cách đơn giản`H×W`bảng, đơn giản và đảm bảo tính chính xác. Ý tưởng brute-force hoạt động vì nó kiểm tra tất cả các đường dẫn, nhưng lập trình động tận dụng các bài toán con chồng chéo để tính toán cùng một kết quả hiệu quả và rõ ràng hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C(H+W-2, H-1)·(H+W)) | O(H+W) | Khả thi ở đây nhưng dài dòng | 
| Lập trình động | O(H·W) | O(H·W) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chiều cao`H`và chiều rộng`W`và lưu trữ lưới bánh dưới dạng danh sách các chuỗi. 
2. Khởi tạo một`dp`bảng kích thước`H×W`, Ở đâu`dp[i][j]`sẽ lưu trữ số lượng quả mọng tối đa được thu thập để tiếp cận tế bào`(i,j)`. 
3. Lặp lại tất cả các ô`(i,j)`theo thứ tự hàng lớn. Đối với mỗi ô, hãy xác định cách tốt nhất để tiếp cận ô đó: 

- Nếu`i > 0`, coi như`dp[i-1][j]`bằng số lượng quả thu được từ ô trên. 
- Nếu như`j > 0`, coi như`dp[i][j-1]`bằng số quả thu được từ ô bên trái. 
- Lấy giá trị lớn nhất của hai giá trị đó. Nếu như`(i,j)`có một quả mọng, thêm 1. 
- Trường hợp đặc biệt: ô trên cùng bên trái`(0,0)`không có hàng xóm ở trên hoặc bên trái, vì vậy`dp[0][0]`là 1 nếu nó có quả mọng, 0 nếu không. 
4. Sau khi điền vào bảng, giá trị tại`dp[H-1][W-1]`là số quả mọng tối đa mà chuột có thể thu thập dọc theo đường đi hợp lệ. 

Lý do điều này có tác dụng là`dp[i][j]`luôn thể hiện chính xác giải pháp tối ưu cho đến từng ô`(i,j)`. Vì chuột chỉ có thể di chuyển sang phải hoặc xuống nên bất kỳ đường dẫn nào tới`(i,j)`phải đến từ bên trái hoặc phía trên. Bằng cách tận dụng tối đa hai khả năng ở mỗi bước và thêm quả mọng của ô hiện tại nếu có, chúng tôi đảm bảo rằng chúng tôi không bao giờ đánh giá thấp đường dẫn tối ưu. Bất biến này đúng cho toàn bộ lưới. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

H, W = map(int, input().split())
cake = [input().strip() for _ in range(H)]

dp = [[0] * W for _ in range(H)]

for i in range(H):
    for j in range(W):
        best_from_above = dp[i-1][j] if i > 0 else 0
        best_from_left = dp[i][j-1] if j > 0 else 0
        dp[i][j] = max(best_from_above, best_from_left) + (1 if cake[i][j] == '*' else 0)

print(dp[H-1][W-1])
```Mã khởi tạo bảng DP và lặp lại theo thứ tự hàng lớn để lấp đầy mỗi ô với số quả mọng có thể thu thập được tối đa. Chúng tôi xử lý các ranh giới bằng cách kiểm tra xem`i > 0`Và`j > 0`. Ô cuối cùng chứa câu trả lời. sử dụng`strip()`đảm bảo rằng các ký tự dòng mới ở cuối từ đầu vào không ảnh hưởng đến việc so sánh nội dung ô. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
4 3
*..
.*.
..*
...
```Trạng thái bảng DP trong quá trình lặp: 

| tôi | j | bánh[i][j] | dp[i][j] | Giải thích | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | * | 1 | Ô khởi đầu, có quả mọng | 
| 0 | 1 | . | 1 | Từ trái 1, không có quả mọng | 
| 0 | 2 | . | 1 | Từ trái 1, không có quả mọng | 
| 1 | 0 | . | 1 | Từ trên 1, không có quả mọng | 
| 1 | 1 | * | 2 | max(1 từ trên, 1 từ trái) +1 berry | 
| 1 | 2 | . | 2 | max(1 từ trên, 2 từ trái), không có quả mọng | 
| 2 | 0 | . | 1 | từ trên 1, không có quả mọng | 
| 2 | 1 | . | 2 | max(2 từ trên, 1 từ trái), không có quả mọng | 
| 2 | 2 | * | 3 | tối đa(2,2)+1 quả | 
| 3 | 0 | . | 1 | từ trên 1, không có quả mọng | 
| 3 | 1 | . | 2 | max(2 từ trên, 1 từ trái) | 
| 3 | 2 | . | 3 | max(3 từ trên, 2 từ trái) | 

Câu trả lời cuối cùng: 3 

Mẫu 2: Lưới 1×1 không có quả mọng: 

đầu vào:```
1 1
.
```bảng DP: 

| 0 | 0 | . | 0 | 

Trả lời: 0 

Điều này cho thấy thuật toán xử lý các lưới ô đơn và khởi tạo chính xác ô trên cùng bên trái. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(H·W) | Mỗi ô được truy cập một lần và mỗi phép tính là O(1) | 
| Không gian | O(H·W) | Chúng tôi lưu trữ một bảng DP có kích thước H×W | 

Với H,W ≤ 5 thì điều này không đáng kể và hoàn thành tốt trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    H, W = map(int, input().split())
    cake = [input().strip() for _ in range(H)]
    dp = [[0]*W for _ in range(H)]
    for i in range(H):
        for j in range(W):
            best_from_above = dp[i-1][j] if i > 0 else 0
            best_from_left = dp[i][j-1] if j > 0 else 0
            dp[i][j] = max(best_from_above, best_from_left) + (1 if cake[i][j] == '*' else 0)
    return str(dp[H-1][W-1])

# provided sample
assert run("4 3\n*..\n.*.\n..*\n...\n") == "3", "sample 1"

# custom cases
assert run("1 1\n.\n") == "0", "single cell empty"
assert run("1 1\n*\n") == "1", "single cell with berry"
assert run("5 5\n
```
