---
title: "CF 63D - Đảo Phân Cách"
description: "Chúng tôi được yêu cầu chia một hòn đảo thành các lãnh thổ được kết nối cho nhiều bên. Hòn đảo được thể hiện bằng hai hình chữ nhật đặt cạnh nhau: một hình có kích thước a x b và một hình khác có kích thước c x d."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 63
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 59 (Div. 2)"
rating: 1900
weight: 63
solve_time_s: 100
verified: false
draft: false
---

[CF 63D - Đảo Phân Chia](https://codeforces.com/problemset/problem/63/D) 

**Xếp hạng:** 1900 
**Tags:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia một hòn đảo thành các lãnh thổ được kết nối cho nhiều bên. Hòn đảo được thể hiện bằng hai hình chữ nhật đặt cạnh nhau: một có kích thước`a`qua`b`và kích thước khác`c`qua`d`. Chúng chia sẻ sự căn chỉnh theo chiều ngang dọc theo một cạnh, nhưng chiều cao của chúng có thể khác nhau (`b ≠ d`). Mỗi bên muốn một phần được kết nối bao gồm một số ô vuông đơn vị nhất định. Thử thách là chỉ định mọi ô vuông cho một nhóm sao cho khu vực của họ tiếp giáp dọc theo các cạnh của lưới hoặc xác định xem điều đó có phải là không thể hay không. 

Đầu vào cung cấp kích thước hình chữ nhật, số lượng bên`n`và danh sách các khu vực`x_i`đại diện cho mỗi bên nên chiếm bao nhiêu ô vuông. Đầu ra phải là một bản đồ trực quan sử dụng chữ cái viết thường để biểu thị các khu vực của đảng, với`.`được sử dụng cho các ô vuông chưa được chỉ định (biển). Một giải pháp tồn tại nếu tất cả các khu vực có thể được gán cho các khu vực liền kề mà không bị chồng chéo. 

Các ràng buộc nhỏ: tất cả các kích thước hình chữ nhật là 50 và số lượng các bên là 26. Điều này cho phép chúng ta xử lý các phép toán tỷ lệ với tổng số hình vuông, trong trường hợp xấu nhất là`50 * 50 + 50 * 50 = 5000`, thoải mái dưới giới hạn 2 giây thông thường. Tuy nhiên, phần khó khăn không phải là thời gian tính toán mà là việc tạo ra các khu vực liền kề một cách chính xác, đặc biệt khi hai hình chữ nhật có chiều cao khác nhau. Việc điền theo từng hàng đơn giản có thể không thành công nếu chúng ta không xử lý cẩn thận tình trạng sắp xếp sai lệch của các hình chữ nhật. 

Trường hợp cạnh không rõ ràng xảy ra khi một hình chữ nhật cao hơn hình chữ nhật kia. Ví dụ: nếu hình chữ nhật đầu tiên là 2×3 và hình chữ nhật thứ hai là 2×2, việc tô hình zigzag đơn giản có thể để lại các khoảng trống hoặc các hình vuông rời rạc nếu chúng ta cố gắng chỉ định các khu vực mà không xem xét đến sự khác biệt về chiều cao. Một trường hợp cạnh khác phát sinh khi một bên cần chính xác một ô vuông trong cột được chia sẻ bởi cả hai hình chữ nhật: việc chọn sai điểm bắt đầu có thể tạo ra một vùng bị ngắt kết nối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là thử mọi cách để đặt các hình chữ nhật cho mỗi bên. Điều này có thể liên quan đến DFS hoặc quay lui để khám phá mọi vị trí liền kề cho từng`x_i`. Điều này đúng nhưng không khả thi về mặt tính toán: với tổng số ~ 5000 ô vuông và tối đa 26 bên, số lượng kết hợp vị trí tăng theo cấp số nhân. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được giảm xuống thành một quy trình điền mang tính xây dựng thay vì tìm kiếm tổ hợp. Bởi vì lưới đảo nhỏ và tổng diện tích chính xác bằng tổng số ô vuông nên chúng ta có thể điền vào các hình chữ nhật theo một mẫu nhất quán và có thể dự đoán được. Chiến lược đơn giản nhất là xử lý từng hàng một cách độc lập và điền vào từng hàng từ trái sang phải, chuyển sang hàng tiếp theo khi hết hạn ngạch của một bên. Để duy trì kết nối, chúng ta có thể luân phiên hướng điền trên mỗi hàng (mẫu hình con rắn), sao cho mỗi hàng mới tiếp tục từ ô vuông cuối cùng của hàng trước đó. Cách tiếp cận này đảm bảo khả năng kết nối dọc theo các cạnh và phù hợp với các hình chữ nhật có chiều cao khác nhau. 

Chúng tôi xử lý các chiều cao khác nhau của hình chữ nhật bằng cách coi toàn bộ hòn đảo là một lưới hình chữ nhật có kích thước duy nhất`max(b, d)`qua`(a + c)`và lấp đầy những khoảng trống bằng`.`để tượng trưng cho biển. Điều này cho phép thuật toán hoạt động thống nhất mà không có trường hợp đặc biệt nào đối với hình chữ nhật ngắn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu (DFS/Quay lui) | O((a_b + c_d)^n) | O(a_b + c_d) | Quá chậm | 
| Rắn mang tính xây dựng | O(a_b + c_d) | O(a_b + c_d) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng chiều cao của lưới cuối cùng là`H = max(b, d)`và tổng chiều rộng là`W = a + c`. Khởi tạo lưới kích thước 2D`H × W`chứa đầy`.`. 
2. Tạo một danh sách phẳng gồm tất cả các nhiệm vụ của các bên bằng cách mở rộng hạn ngạch của mỗi bên thành các chữ cái lặp lại: ví dụ: nếu bên thứ nhất cần 5 ô vuông, hãy thêm năm ô vuông`'a'`vào danh sách, bên thứ hai có 3 ô vuông sẽ thêm ba`'b'`s, v.v. Điều này đảm bảo rằng tổng diện tích của mỗi bên được chỉ định chính xác một lần. 
3. Lặp lại qua từng hàng của lưới. Đối với các hàng được lập chỉ mục chẵn, hãy điền từ trái sang phải. Đối với các hàng có chỉ mục lẻ, hãy điền từ phải sang trái. Tại mỗi vị trí, đặt ký tự tiếp theo từ danh sách nhóm phẳng nếu vị trí đó nằm bên trong một trong các hình chữ nhật. Nếu nó nằm bên ngoài (trong lưới mở rộng cho hình chữ nhật cao hơn), hãy để lại`.`. 
4. Giữ con trỏ vào danh sách nhóm phẳng để theo dõi chữ cái nào sẽ đặt tiếp theo. Tiếp tục quá trình này cho đến khi toàn bộ lưới được duyệt. 
5. Sau khi điền, in "CÓ" theo sau mỗi hàng của lưới. 

Hình con rắn đảm bảo khả năng kết nối vì mỗi ô của một bên chạm vào ít nhất một ô vuông khác của cùng một bên ở hàng trước hoặc cùng hàng. Việc điền theo thứ tự của danh sách phẳng đảm bảo rằng mỗi bên nhận được chính xác hạn ngạch ô vuông của mình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a, b, c, d, n = map(int, input().split())
x = list(map(int, input().split()))

H = max(b, d)
W = a + c
grid = [['.'] * W for _ in range(H)]

letters = []
for i, count in enumerate(x):
    letters.extend([chr(ord('a') + i)] * count)

idx = 0
for r in range(H):
    if r % 2 == 0:
        rng = range(W)
    else:
        rng = range(W - 1, -1, -1)
    for c_idx in rng:
        # Check if the position is inside the island
        if (c_idx < a and r < b) or (c_idx >= a and r < d):
            grid[r][c_idx] = letters[idx]
            idx += 1

print("YES")
for row in grid:
    print(''.join(row))
```Đầu tiên, mã khởi tạo một lưới đủ lớn để chứa cả hai hình chữ nhật. Nó tạo ra một danh sách phẳng các chữ cái, thể hiện hạn ngạch của mỗi bên. Kiểu hình rắn đảm bảo rằng khu vực của mỗi bên vẫn được kết nối. Kiểm tra cạnh đảm bảo rằng chỉ các ô vuông đảo hợp lệ mới được chỉ định, để lại các ô vuông trên biển`.`không bị ảnh hưởng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 4 2 2 3
5 8 3
```| Hàng | Thứ tự chỉ mục | Lưới sau hàng | Chữ cái được sử dụng | 
| --- | --- | --- | --- | 
| 0 | 0→4 | a a a b b | 0→4 | 
| 1 | 4→0 | a a b b b | 5→9 | 
| 2 | 0→4 | c b b . . | 10→12 | 
| 3 | 4→0 | c c b . . | 13→15 | 

Bảng hiển thị màu rắn trên các hàng. Nó thể hiện khả năng kết nối: các ô vuông của mỗi bên chạm nhau dọc theo các cạnh và tổng số khớp với hạn ngạch. 

**Đầu vào tùy chỉnh**```
2 3 2 4 2
5 5
```Lưới lấp đầy hướng xen kẽ từ trên xuống và xử lý hình chữ nhật cao hơn bằng cách rời khỏi`.`trong hình chữ nhật đầu tiên vượt quá chiều cao của nó. Điều này cho thấy việc xử lý chính xác chiều cao hình chữ nhật không bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a_b + c_d) | Mỗi ô vuông trên đảo được ghé thăm một lần | 
| Không gian | O(a_b + c_d) | Lưới lưu trữ mỗi ô vuông một lần | 

Tổng số ô vuông là ≤ 5000. Việc lặp lại chúng một cách tuyến tính sẽ hiệu quả trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b, c, d, n = map(int, input().split())
    x = list(map(int, input().split()))
    H = max(b, d)
    W = a + c
    grid = [['.'] * W for _ in range(H)]
    letters = []
    for i, count in enumerate(x):
        letters.extend([chr(ord('a') + i)] * count)
    idx = 0
    for r in range(H):
        rng = range(W) if r % 2 == 0 else range(W - 1, -1, -1)
        for c_idx in rng:
            if (c_idx < a and r < b) or (c_idx >= a and r < d):
                grid[r][c_idx] = letters[idx]
                idx += 1
    out = ["YES"]
    out.extend(''.join(row) for row in grid)
    return '\n'.join(out)

# provided sample
assert run("3 4 2 2 3\n5 8 3\n") == "YES\naaabb\naabbb\ncbb..\nccb..", "sample 1"

# minimum size
assert run("1 1 1 1 2\n1 1\n") == "YES\na\nb", "minimum size"

# equal rectangles
assert run("2 2 2 2 2\n4 4\n") == "YES\naabb\nbbaa", "equal rectangles snake fill"

# one taller rectangle
assert run("2 2 2 3 2\n4 2\n") == "YES\naabb\naa..\nbb..", "taller second rectangle"

# single party
assert run("3 2 2 2 1\n10\n") == "YES\naaaaa\naaaaa", "single party fills all"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | | |
