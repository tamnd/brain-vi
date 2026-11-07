---
title: "CF 132B - Piet"
description: "Chúng tôi được yêu cầu mô phỏng trình thông dịch Piet đơn giản hóa trên một lưới hình chữ nhật nhỏ. Mỗi ô là một pixel có màu từ 0 đến 9, trong đó 0 là màu đen và các chữ số khác là các khối màu."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 132
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 96 (Div. 1)"
rating: 2100
weight: 132
solve_time_s: 116
verified: true
draft: false
---

[CF 132B - Piet](https://codeforces.com/problemset/problem/132/B) 

**Xếp hạng:** 2100 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng trình thông dịch Piet đơn giản hóa trên một lưới hình chữ nhật nhỏ. Mỗi ô là một pixel có màu từ 0 đến 9, trong đó 0 là màu đen và các chữ số khác là các khối màu. Các khối màu luôn có hình chữ nhật và được kết nối với nhau nên mỗi cụm màu tạo thành một hình chữ nhật hoàn hảo. Pixel đen có thể có hình dạng tùy ý. Chương trình bắt đầu bằng con trỏ lệnh (IP) ở khối màu trên cùng bên trái. IP được xác định bởi ba thành phần: khối hiện tại (BP), con trỏ hướng (DP) có thể trỏ lên, xuống, trái hoặc phải và bộ chọn khối (CP) điều chỉnh chuyển động khi IP gặp các cạnh hoặc pixel đen. 

Ở mỗi bước, trình thông dịch sẽ di chuyển BP theo hướng DP sang khối tiếp theo nếu có thể, nếu không, nó sẽ cập nhật CP và có thể xoay DP. Nhiệm vụ là xác định màu của khối nơi BP hạ cánh sau`n`các bước. Lưới có kích thước tối đa là 50×50 pixel, nhưng`n`có thể lên tới 50 triệu, con số này quá lớn để mô phỏng từng bước một cách đơn giản. 

Các trường hợp cạnh không rõ ràng bao gồm các khối một hàng hoặc một cột, chuỗi các pixel màu đen buộc DP và CP phải xoay liên tục và các vòng lặp trong đó BP truy cập lại cùng một khối nhiều lần. Ví dụ: một lưới:```
12
34
```với`n = 10`quay vòng qua các khối theo một mô hình có thể dự đoán được. Một mô phỏng đơn giản sẽ thử tất cả 10 bước, nhưng ở quy mô lớn hơn`n`, điều này là không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: bắt đầu từ khối ban đầu, lặp lại từng bước, áp dụng các quy tắc cập nhật DP và CP, đồng thời theo dõi khối hiện tại sau mỗi lần di chuyển. Điều này hiệu quả vì mỗi bước đều mang tính quyết định và các quy tắc chỉ định đầy đủ trạng thái mới. Tuy nhiên, với`n`lên tới 50 triệu, cách làm này thực hiện quá nhiều thao tác; mỗi bước yêu cầu công việc O(1), do đó tổng độ phức tạp là O(n), quá chậm với giới hạn thời gian là 2 giây. 

Cái nhìn sâu sắc quan trọng cho một giải pháp nhanh hơn là coi việc chuyển đổi IP như một máy trạng thái xác định hữu hạn. Mỗi trạng thái được xác định bởi khối hiện tại, DP và CP. Bởi vì số lượng khối bị giới hạn (nhiều nhất là 2500 cho lưới 50 × 50) và có 8 cấu hình DP/CP có thể có (4 hướng DP × 2 hướng CP), nên tổng số trạng thái nhiều nhất là 20.000. Bằng cách mô phỏng các bước cho đến khi trạng thái lặp lại, chúng tôi phát hiện được chu kỳ. Khi một chu trình được tìm thấy, chúng ta có thể tính toán khối cuối cùng sau`n`các bước sử dụng số học mô-đun thay vì lặp lại từng bước. 

Bạo lực tàn bạo chỉ có tác dụng với nhỏ`n`hoặc lưới nhỏ nhưng không thành công khi`n`là lớn. Quan sát rằng có nhiều trạng thái hữu hạn cho phép chúng ta giảm mô phỏng xuống nhiều nhất là một phát hiện chu kỳ, sau đó là tính toán modulo. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm đối với n lớn | 
| Phát hiện chu kỳ | O(m × w × 8) | O(m × w × 8) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước lưới để xác định tất cả các khối màu. Đối với mỗi khối, lưu trữ tọa độ trên cùng bên trái và dưới cùng bên phải của nó. Điều này cho phép kiểm tra liên tục các cạnh của khối khi di chuyển theo bất kỳ hướng nào. 
2. Xác định hướng DP và CP dưới dạng vectơ: DP có thể lên`(−1,0)`, xuống`(1,0)`, bên trái`(0,−1)`, Phải`(0,1)`và CP có thể ở bên trái hoặc bên phải so với DP. Điều này làm cho việc tính toán pixel tiếp theo dọc theo cạnh trở nên đơn giản. 
3. Khởi tạo IP ở khối trên cùng bên trái với DP chỉ sang phải và CP chỉ sang trái. Mã hóa mỗi trạng thái thành một bộ dữ liệu`(block_id, dp, cp)`. 
4. Mô phỏng từng bước chương trình trong khi ghi lại các trạng thái đã truy cập trong ánh xạ từ điển`(block_id, dp, cp)`đến số bước. Đối với mỗi bước, xác định khối tiếp theo theo quy tắc DP và CP. 
5. Nếu trạng thái lặp lại, hãy phát hiện độ dài chu kỳ. Cho phép`start`là bước bắt đầu chu kỳ và`length`số bước trong chu kỳ. Tính bước cuối cùng modulo độ dài chu kỳ và trả về khối ở bước đó. 
6. Nếu không phát hiện thấy chu kỳ nào trong tổng số bước (không thể do trạng thái hữu hạn), hãy tiếp tục mô phỏng bình thường cho đến bước`n`. 

Tại sao nó hoạt động: Số lượng khối và hướng hữu hạn đảm bảo rằng mô phỏng cuối cùng phải lặp lại một trạng thái. Bởi vì trình thông dịch Piet có tính xác định nên việc nhập lại cùng một trạng thái luôn tạo ra cùng một chuỗi các khối. Bằng cách giảm lớn`n`đến một bước trong chu trình được phát hiện, chúng tôi đạt được độ chính xác mà không cần mô phỏng từng bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# direction vectors: 0=right, 1=down, 2=left, 3=up
dp_moves = [(0,1), (1,0), (0,-1), (-1,0)]

# CP left/right: -1 for left, +1 for right relative to DP
cp_changes = {'L': -1, 'R': 1}

def rotate_dp(dp):
    return (dp + 1) % 4

def parse_grid(m):
    grid = []
    for _ in range(m):
        row = list(map(int, list(input().strip())))
        grid.append(row)
    return grid

def find_blocks(grid):
    m, w = len(grid), len(grid[0])
    block_id = 0
    block_map = [[-1]*w for _ in range(m)]
    blocks = []
    for i in range(m):
        for j in range(w):
            color = grid[i][j]
            if color == 0 or block_map[i][j] != -1:
                continue
            # expand rectangle
            x0, y0 = i, j
            x1, y1 = i, j
            while x1+1 < m and all(grid[x1+1][y] == color for y in range(y0, y1+1)):
                x1 += 1
            while y1+1 < w and all(grid[x][y1+1] == color for x in range(x0, x1+1)):
                y1 += 1
            # mark cells
            for x in range(x0, x1+1):
                for y in range(y0, y1+1):
                    block_map[x][y] = block_id
            blocks.append((x0, y0, x1, y1, color))
            block_id += 1
    return block_map, blocks

def next_block(block_map, blocks, block_idx, dp, cp):
    x0, y0, x1, y1, color = blocks[block_idx]
    dx, dy = dp_moves[dp]
    # find furthest edge in DP
    if dp == 0:  # right
        edge_cells = [(i, y1) for i in range(x0, x1+1)]
    elif dp == 1:  # down
        edge_cells = [(x1, j) for j in range(y0, y1+1)]
    elif dp == 2:  # left
        edge_cells = [(i, y0) for i in range(x0, x1+1)]
    else:  # up
        edge_cells = [(x0, j) for j in range(y0, y1+1)]
    # select cell in CP direction
    if cp == 'L':
        if dp % 2 == 0:
            next_cell = min(edge_cells, key=lambda x: x[0] if dp==0 else x[1])
        else:
            next_cell = min(edge_cells, key=lambda x: x[1] if dp==1 else x[0])
    else:
        if dp % 2 == 0:
            next_cell = max(edge_cells, key=lambda x: x[0] if dp==0 else x[1])
        else:
            next_cell = max(edge_cells, key=lambda x: x[1] if dp==1 else x[0])
    nx, ny = next_cell[0]+dx, next_cell[1]+dy
    m, w = len(block_map), len(block_map[0])
    if 0 <= nx < m and 0 <= ny < w and block_map[nx][ny] != -1:
        return block_map[nx][ny], dp, cp
    else:
        if cp == 'L':
            return block_idx, dp, 'R'
        else:
            return block_idx, rotate_dp(dp), 'L'

def main():
    m, n = map(int, input().split())
    grid = parse_grid(m)
    block_map, blocks = find_blocks(grid)
    dp, cp = 0, 'L'
    block_idx = block_map[0][0]

    visited = {}
    states = []
    step = 0
    while step < n:
        key = (block_idx, dp, cp)
        if key in visited:
            cycle_start = visited[key]
            cycle_len = step - cycle_start
            remaining = (n - step) % cycle_len
            block_idx, dp, cp = states[cycle_start + remaining]
            break
        visited[key] = step
        states.append((block_idx, dp, cp))
        block_idx, dp, cp = next_block(block_map, blocks, block_idx, dp, cp)
        step += 1
    print(blocks[block_idx][4])

if __name__ == "__main__":
    main()
``
```
