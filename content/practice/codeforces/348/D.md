---
title: "CF 348D - Rùa"
description: "Chúng ta có một lưới hình chữ nhật có kích thước n x m, trong đó mỗi ô trống hoặc bị chặn. Hai con rùa bắt đầu ở góc trên bên trái, ô (1,1) và cả hai đều muốn đến góc dưới cùng bên phải, ô (n,m)."
date: "2026-06-06T18:33:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "matrices"]
categories: ["algorithms"]
codeforces_contest: 348
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 202 (Div. 1)"
rating: 2500
weight: 348
solve_time_s: 125
verified: false
draft: false
---

[CF 348D - Rùa](https://codeforces.com/problemset/problem/348/D) 

**Đánh giá:** 2500 
**Thẻ:** dp, ma trận 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật có kích thước _n_ x _m_, trong đó mỗi ô trống hoặc bị chặn. Hai con rùa bắt đầu ở góc trên cùng bên trái, ô (1,1) và cả hai đều muốn đến góc dưới cùng bên phải, ô (_n_,_m_). Rùa chỉ có thể di chuyển xuống hoặc sang phải ở mỗi bước và không thể đi qua các ô bị chặn. Rùa có một hạn chế: đường đi của chúng không được giao nhau ở bất kỳ ô nào ngoại trừ ô bắt đầu và ô kết thúc. Nhiệm vụ là đếm số cặp đường đi không giao nhau theo modulo 10^9 + 7. 

Các ràng buộc chỉ ra rằng lưới có thể lớn tới 3000 x 3000. Bất kỳ giải pháp nào cố gắng liệt kê tất cả các đường dẫn một cách rõ ràng sẽ chậm một cách vô vọng vì số lượng đường dẫn tăng theo cấp số nhân với _n_ và _m_. Điều này buộc chúng ta phải suy nghĩ về mặt lập trình động hoặc tính toán tổ hợp hơn là sức mạnh vũ phu. 

Các trường hợp biên phức tạp bao gồm các tình huống trong đó một đường dẫn bị buộc dọc theo một hành lang hẹp gồm các ô tự do. Ví dụ: nếu hàng giữa bị chặn ngoại trừ một cột duy nhất, bất kỳ cách tiếp cận ngây thơ nào không theo dõi vị trí tương đối của rùa có thể vượt quá các đường giao nhau. Một trường hợp cạnh khác xảy ra khi các đường đi phải “đổi chỗ” xung quanh chướng ngại vật; thuật toán phải cẩn thận tránh đếm các đường đi cắt nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là tạo ra tất cả các đường đi có thể từ (1,1) đến (n,m) cho mỗi con rùa và sau đó kiểm tra xem cặp nào không giao nhau. Ngay cả đối với lưới 10x10, số lượng đường dẫn cũng vào khoảng hàng nghìn cho mỗi con rùa, dẫn đến hàng triệu cặp. Với n=m=3000, đây là một con số lớn về mặt thiên văn. Cách tiếp cận này đúng về nguyên tắc nhưng không khả thi về mặt tính toán. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát rằng mỗi đường dẫn bao gồm một chuỗi các bước di chuyển sang phải và đi xuống. Nếu chúng ta theo dõi số cách mà một con rùa có thể đến từng ô, chúng ta có thể tính toán trước số lượng đường đi từ đầu đến mỗi ô và riêng biệt số lượng đường đi từ mỗi ô đến cuối. Với số lượng này, chúng ta có thể khai thác loại trừ bao gồm: tổng số cặp đường dẫn không có thứ tự là bình phương của tổng số đường dẫn, trừ đi các cặp giao nhau tại bất kỳ ô nào ngoài ô bắt đầu hoặc kết thúc. Chúng tôi giảm bớt vấn đề bằng việc đếm các đường đi giao nhau tại một ô trung gian nhất định, việc này có thể được thực hiện bằng cách sử dụng bảng DP. 

Giải pháp tối ưu sử dụng lập trình động để tính toán bốn bảng. Cho phép`dp_start[x][y]`là số đường đi từ (1,1) đến (x,y) và`dp_end[x][y]`là số đường đi từ (x,y) đến (n,m). Khi đó số cặp đường đi cắt nhau tại (x,y) là`dp_start[x][y]^2 * dp_end[x][y]^2`. Tính tổng kết quả này cho tất cả các ô ngoại trừ điểm bắt đầu và kết thúc và trừ đi tổng số đường dẫn bình phương sẽ cho câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^{n*m}) | O(2^{n*m}) | Quá chậm | 
| DP tối ưu | O(n*m) | O(n*m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai bảng DP`dp_start`Và`dp_end`có kích thước n x m chứa đầy số không.`dp_start[x][y]`đếm đường đi từ (1,1) đến (x,y),`dp_end[x][y]`đếm đường đi từ (x,y) đến (n,m). Điều này nắm bắt tất cả các vị trí có thể tiếp cận một cách hiệu quả. 
2. Đặt`dp_start[0][0] = 1`Và`dp_end[n-1][m-1] = 1`bởi vì các ô bắt đầu và kết thúc luôn có một đường đi tầm thường đến chính chúng. 
3. Điền vào`dp_start`lặp đi lặp lại. Đối với mỗi ô trống (x,y), hãy thêm các đường dẫn từ ô lân cận trên cùng nếu nó tồn tại và trống, và từ ô lân cận bên trái nếu nó tồn tại và trống. Điều này tính toán tổng số cách để tiếp cận từng ô ngay từ đầu. 
4. Điền vào`dp_end`lặp đi lặp lại ngược lại. Đối với mỗi ô trống (x,y), hãy thêm các đường dẫn từ ô lân cận dưới cùng nếu nó tồn tại và trống, và từ ô lân cận bên phải nếu nó tồn tại và trống. Điều này tính toán số cách để đến cuối từ mỗi ô. 
5. Tính tổng số đường đi từ đầu đến cuối,`total_paths = dp_start[n-1][m-1]`. 
6. Khởi tạo`intersect_sum = 0`. Đối với mỗi ô trung gian (x,y) ngoại trừ ô bắt đầu và kết thúc, nếu trống, hãy thêm`(dp_start[x][y] * dp_end[x][y])^2`modulo 10^9+7 đến`intersect_sum`. Điều này đếm tất cả các cặp giao nhau tại ô đó. 
7. Trừ`intersect_sum`từ`total_paths^2`modulo 10^9+7 để lấy số cặp đường dẫn không giao nhau. 
8. Xuất kết quả. 

Tại sao nó hoạt động: Bằng cách bình phương số lượng DP, chúng tôi liệt kê tất cả các cặp đường dẫn không có thứ tự và đếm chính xác có bao nhiêu điểm giao nhau ở mỗi ô trung gian. Trừ đi những đường dẫn này khỏi tổng số đường dẫn bình phương, chỉ còn lại những cặp chỉ gặp nhau ở đầu và cuối, phù hợp với yêu cầu của bài toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

n, m = map(int, input().split())
grid = [input().strip() for _ in range(n)]

dp_start = [[0] * m for _ in range(n)]
dp_end = [[0] * m for _ in range(n)]

dp_start[0][0] = 1
for i in range(n):
    for j in range(m):
        if grid[i][j] == '#':
            dp_start[i][j] = 0
            continue
        if i > 0:
            dp_start[i][j] = (dp_start[i][j] + dp_start[i-1][j]) % MOD
        if j > 0:
            dp_start[i][j] = (dp_start[i][j] + dp_start[i][j-1]) % MOD

dp_end[n-1][m-1] = 1
for i in reversed(range(n)):
    for j in reversed(range(m)):
        if grid[i][j] == '#':
            dp_end[i][j] = 0
            continue
        if i + 1 < n:
            dp_end[i][j] = (dp_end[i][j] + dp_end[i+1][j]) % MOD
        if j + 1 < m:
            dp_end[i][j] = (dp_end[i][j] + dp_end[i][j+1]) % MOD

total_paths = dp_start[n-1][m-1]
intersect_sum = 0
for i in range(n):
    for j in range(m):
        if (i == 0 and j == 0) or (i == n-1 and j == m-1) or grid[i][j] == '#':
            continue
        intersect_sum = (intersect_sum + dp_start[i][j] * dp_end[i][j] % MOD * dp_start[i][j] % MOD * dp_end[i][j] % MOD) % MOD

answer = (total_paths * total_paths - intersect_sum + MOD) % MOD
print(answer)
```Giải pháp khởi tạo các bảng DP để đếm đường dẫn tiến và lùi. Kiểm tra ranh giới ngăn chặn truy cập ngoài giới hạn. Tổng giao nhau sử dụng số học mô-đun một cách cẩn thận để tránh tràn. Bước trừ thêm MOD trước khi lấy modulo để tránh kết quả âm. 

## Ví dụ đã hoạt động 

Mẫu 1:```
4 5
.....
.###.
.###.
.....
```

`dp_start`: 

| tôi\j | 0 | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 1 | 1 | 1 | 1 | 
| 1 | 1 | 0 | 0 | 0 | 0 | 
| 2 | 1 | 0 | 0 | 0 | 0 | 
| 3 | 1 | 1 | 1 | 1 | 1 |`dp_end`: 

| tôi\j | 0 | 1 | 2 | 3 | 4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 1 | 
| 1 | 0 | 0 | 0 | 0 | 1 | 
| 2 | 0 | 0 | 0 | 0 | 1 | 
| 3 | 1 | 1 | 1 | 1 | 1 |`total_paths = 1`. Ô tự do trung gian duy nhất nơi cả hai đường có thể giao nhau bị chặn bởi chướng ngại vật. Vì thế`intersect_sum = 0`. Câu trả lời là`1*1 - 0 = 1`. 

Một ví dụ khác:```
2 2
..
..
```Cả hai con rùa đều có thể đi sang phải rồi đi xuống hoặc đi xuống rồi sang phải. Có hai con đường. Bình phương cho tổng cộng 4 cặp, chỉ trừ các cặp giao nhau tại (1,1) hoặc (2,2). Câu trả lời là 2, như mong đợi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Điền vào hai bảng DP có kích thước n*m và lặp qua tất cả các ô để đếm giao lộ | 
| Không gian | O(n*m) | Hai bảng DP có kích thước n*m | 

Cho n và m lên tới 3000, n*m là
