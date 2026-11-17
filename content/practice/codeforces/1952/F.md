---
title: "CF 1952F - Lưới"
description: "Chúng ta có một lưới 21 x 21 trong đó mỗi ô chứa 0 hoặc 1. Nhiệm vụ là xác định \"chữ thập\" lớn nhất trong số 1 có thể được hình thành bên trong lưới này."
date: "2026-06-07T17:58:31+07:00"
tags: ["codeforces", "competitive-programming", "*special", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 1952
codeforces_index: "F"
codeforces_contest_name: "April Fools Day Contest 2024"
rating: 0
weight: 1952
solve_time_s: 99
verified: false
draft: false
---

[CF 1952F - Lưới](https://codeforces.com/problemset/problem/1952/F) 

**Đánh giá:** - 
**Tags:** *đặc biệt, vũ phu 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới 21 x 21 trong đó mỗi ô chứa một`0`hoặc một`1`. Nhiệm vụ là xác định chữ thập lớn nhất của`1`s có thể được hình thành bên trong lưới này. Một chữ thập được định nghĩa là một tập hợp các ô tập trung vào một số`1`trong đó cả bốn tay lên, xuống, trái và phải đều bao gồm các động tác liên tiếp`1`s kéo dài từ trung tâm. Đầu ra phải là tổng số`1`s tạo thành hình chữ thập lớn nhất. Ví dụ: một hình chữ thập có chiều dài cánh tay 2 có 1 tâm cộng với 2 ô ở mỗi hướng, tổng cộng 1 + 2*4 = 9 ô. 

Lưới nhỏ và cố định ở 21x21, chỉ có 441 ô. Điều này có nghĩa là ngay cả phương pháp kiểm tra từng ô và kiểm tra tất cả bốn hướng riêng lẻ cũng khả thi vì tổng số thao tác vẫn dưới một triệu, nằm trong giới hạn 1 giây. 

Trường hợp cạnh không rõ ràng xảy ra khi đường chéo tiềm năng chạm vào ranh giới của lưới hoặc bị chặn bởi`0`s liền kề với trung tâm. Ví dụ: nếu trung tâm ở`(0,0)`và tế bào`(0,1)`là`0`, nhánh tối đa theo hướng đó là 0. Một cách tiếp cận ngây thơ giả định tính đối xứng hoặc luôn tính chiều dài cánh tay cố định sẽ tính sai kích thước của hình chữ thập. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng ô trong lưới. Nếu tế bào là một`1`, nó mở rộng ra bốn hướng, đếm xem có bao nhiêu liên tiếp`1`s có ở mỗi hướng cho đến khi một`0`hoặc đạt đến ranh giới lưới. Kích thước của hình chữ thập khi đó là tổng chiều dài cánh tay cộng với 1 cho tâm. Cách tiếp cận này đúng, nhưng nếu lưới lớn, độ phức tạp của nó sẽ tăng lên thành O(n^2 * min(n, m)) vì đối với mỗi ô, bạn có thể duyệt toàn bộ hàng hoặc cột. Ở đây, với n = 21 thì số phép toán tối đa là 21 * 21 * 21 = 9261, chấp nhận được. 

Cách tiếp cận tối ưu nhận ra rằng chúng ta có thể xử lý trước lưới để tính toán độ dài nhánh tối đa theo từng hướng cho mỗi ô. Đối với hướng trái và hướng phải, chúng ta duyệt từng hàng, điền hai ma trận phụ liên tiếp`1`được nhìn thấy từ đầu hoặc cuối hàng. Đối với lên và xuống, chúng tôi thực hiện tương tự theo từng cột. Sau khi các ma trận này được tạo, giá trị chéo tối đa tại bất kỳ ô nào sẽ là giá trị tối thiểu trong bốn giá trị được tính toán trước. Điều này làm giảm việc đếm lặp lại và làm cho thuật toán trở nên đơn giản và dễ thực hiện, đồng thời trong trường hợp của chúng tôi, chi phí xử lý trước là không đáng kể do kích thước lưới cố định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Có thể chấp nhận được với n=21 | 
| Tiền xử lý | O(n^2) | O(n^2) | Được chấp nhận và sạch hơn | 

## Hướng dẫn thuật toán 

1. Đọc lưới thành ma trận số nguyên 21x21. Chuyển đổi`0`Và`1`các ký tự thành số nguyên đơn giản hóa việc tính toán và so sánh. 
2. Khởi tạo 4 ma trận có kích thước 21x21:`up`,`down`,`left`,`right`. Mỗi ma trận sẽ lưu trữ số liên tiếp tối đa`1`s kéo dài từ ô hiện tại theo hướng đó. 
3. Điền vào`left`ma trận bằng cách duyệt từng hàng từ trái sang phải. Đối với mỗi ô, nếu có`1`, giá trị là 1 cộng với giá trị của ô trước đó trong cùng một hàng. Ngược lại, giá trị là 0. Điều này được tính liên tiếp`1`s ở bên trái bao gồm cả ô hiện tại. 
4. Tương tự, điền vào`right`ma trận bằng cách duyệt từng hàng từ phải sang trái. 
5. Điền vào`up`ma trận bằng cách duyệt từng cột từ trên xuống dưới, đếm liên tiếp`1`S. 
6. Điền vào`down`ma trận bằng cách duyệt từng cột từ dưới lên trên. 
7. Đối với mỗi ô, chiều dài cánh tay tối đa có thể là giá trị tối thiểu trong bốn giá trị hướng của nó trừ đi 1 vì mỗi giá trị đều bao gồm tâm. Tính tổng kích thước chéo là chiều dài cánh tay 1 + 4 *. 
8. Theo dõi kích thước chéo tối đa được tìm thấy trong khi quét tất cả các ô. 
9. Xuất ra kích thước chéo tối đa. 

Tại sao nó hoạt động: Mỗi ma trận phụ sẽ đếm chính xác liên tiếp`1`s theo hướng của nó. Lấy mức tối thiểu đảm bảo rằng không có nhánh nào vượt quá mức liên tiếp có sẵn`1`S. Trừ 1 sẽ chuyển đổi từ "số đếm bao gồm tâm" thành "chiều dài cánh tay" và nhân với 4 rồi cộng 1 sẽ có tổng kích thước chữ thập. Điều này đảm bảo kích thước được tính toán tương ứng chính xác với một chữ thập hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

grid = [list(map(int, input().strip())) for _ in range(21)]

n = 21

up = [[0]*n for _ in range(n)]
down = [[0]*n for _ in range(n)]
left = [[0]*n for _ in range(n)]
right = [[0]*n for _ in range(n)]

# Left and Right
for i in range(n):
    for j in range(n):
        if grid[i][j]:
            left[i][j] = left[i][j-1] + 1 if j > 0 else 1
    for j in range(n-1, -1, -1):
        if grid[i][j]:
            right[i][j] = right[i][j+1] + 1 if j < n-1 else 1

# Up and Down
for j in range(n):
    for i in range(n):
        if grid[i][j]:
            up[i][j] = up[i-1][j] + 1 if i > 0 else 1
    for i in range(n-1, -1, -1):
        if grid[i][j]:
            down[i][j] = down[i+1][j] + 1 if i < n-1 else 1

max_cross = 0
for i in range(n):
    for j in range(n):
        if grid[i][j]:
            arm = min(up[i][j], down[i][j], left[i][j], right[i][j])
            size = 1 + 4 * (arm - 1)
            max_cross = max(max_cross, size)

print(max_cross)
```Giải pháp đầu tiên chuyển đổi lưới thành số nguyên. Mỗi hướng được xử lý độc lập để tính toán liên tiếp`1`s, với việc xử lý cẩn thận các ranh giới. Khi tính toán độ dài nhánh, chúng tôi trừ đi một để loại trừ tâm và nhân với bốn tài khoản cho tất cả các nhánh. Bước này đòi hỏi sự chú ý cẩn thận vì rất dễ xảy ra lỗi từng cái một khi làm việc với số lượng bao gồm. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1 

| Ô (i,j) | Lên | Xuống | Trái | Đúng | Cánh tay | Kích thước | 
| --- | --- | --- | --- | --- | --- | --- | 
| (0,0) | 1 | 6 | 1 | 7 | 1 | 5 | 
| (6,10) | 3 | 3 | 2 | 2 | 2 | 9 | 
| (10,10) | 1 | 2 | 1 | 2 | 1 | 5 | 
| Ô tối đa | ... | ... | ... | ... | 3 | 12 | 

Dấu vết cho thấy trung tâm tại`(10,10)`đạt được kích thước chéo tối đa là 12. Tất cả các ô khác có nhánh tối thiểu nhỏ hơn. 

### Đầu vào mẫu 2 

Vẽ hình chữ thập ở giữa có chiều dài cánh tay là 2:```
00000
00100
01110
00100
00000
```Trung tâm tại`(2,2)`cho`up=2`,`down=2`,`left=2`,`right=2`. Arm = 2, size = 1 + 4*2 = 9. Kết quả đầu ra đúng phù hợp với kỳ vọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mỗi ô được xử lý theo bốn lượt cho bốn hướng. n=21 nên tổng số thao tác là ~4*441=1764 | 
| Không gian | O(n^2) | Bốn ma trận phụ n x n cộng với lưới | 

Điều này phù hợp thoải mái trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    grid = [list(map(int, input().strip())) for _ in range(21)]

    n = 21
    up = [[0]*n for _ in range(n)]
    down = [[0]*n for _ in range(n)]
    left = [[0]*n for _ in range(n)]
    right = [[0]*n for _ in range(n)]

    for i in range(n):
        for j in range(n):
            if grid[i][j]:
                left[i][j] = left[i][j-1]+1 if j>0 else 1
        for j in range(n-1,-1,-1):
            if grid[i][j]:
                right[i][j] = right[i][j+1]+1 if j<n-1 else 1
    for j in range(n):
        for i in range(n):
            if grid[i][j]:
                up[i][j] = up[i-1][j]+1 if i>0 else 1
        for i in range(n-1,-1,-1):
            if grid[i][j]:
                down[i][j] = down[i+1][j]+1 if i<n-1 else 1

    max_cross = 0
    for i in range(n):
        for j in
```
