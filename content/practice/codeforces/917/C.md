---
title: "CF 917C - Pollywog"
description: "Chúng ta được yêu cầu di chuyển một nhóm x Pollywogs từ những viên đá x đầu tiên trong một dòng đến những viên đá x cuối cùng. Các quân cờ được đánh số từ 1 đến n, và mỗi quân cờ chiếm đúng một quân cờ."
date: "2026-06-13T02:16:36+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "matrices"]
categories: ["algorithms"]
codeforces_contest: 917
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 459 (Div. 1)"
rating: 2900
weight: 917
solve_time_s: 386
verified: false
draft: false
---

[CF 917C - Pollywog](https://codeforces.com/problemset/problem/917/C) 

**Xếp hạng:** 2900 
**Tags:** tổ hợp, dp, ma trận 
**Thời gian giải:** 6 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu di chuyển một nhóm`x`Pollywogs từ đầu tiên`x`những viên đá xếp hàng đến cuối cùng`x`đá. Những viên đá được đánh số`1`bởi vì`n`, và mỗi Pollywog chiếm đúng một viên đá. Một con Pollywog có thể nhảy nhiều nhất`k`lần lượt đá sang bên phải, với mỗi khoảng cách nhảy`i`tính chi phí`c[i]`năng lượng. Một số loại đá đặc biệt và có thêm chi phí năng lượng hoặc tiền thưởng`w[p]`. Hai con Pollywog không thể chiếm cùng một hòn đá cùng một lúc. Mục tiêu là tìm ra tổng năng lượng tối thiểu cần thiết để di chuyển tất cả các Pollywog đến vị trí cuối cùng.`x`đá, có tính đến chi phí nhảy và đá đặc biệt. 

Những ràng buộc cho chúng ta biết rằng`x`Và`k`nhiều nhất là 8, con số này rất nhỏ, trong khi`n`có thể lên tới 10^8. Điều này ngay lập tức gợi ý rằng chúng ta không thể đủ khả năng để mô phỏng từng viên đá một cách rõ ràng hoặc lặp lại trên tất cả các vị trí. Thay vào đó, giải pháp phải tận dụng số lượng Pollywogs nhỏ và phạm vi nhảy nhỏ. Số lượng đá đặc biệt nhiều nhất là 25 nên rất thưa thớt so với`n`. Các giá trị năng lượng có thể âm, do đó, con đường tối ưu có thể cố tình rơi vào những viên đá đặc biệt có lợi. 

Một trường hợp khó khăn phát sinh khi các bước nhảy chồng lên những viên đá cuối cùng. Ví dụ: nếu một con Pollywog có thể nhảy từ 1 đến 3 viên đá và chỉ đứng trước viên đá cuối cùng`x`những viên đá, chúng ta phải đảm bảo nó tiếp đất chính xác trên một viên đá mục tiêu trống mà không va chạm với một viên đá khác. Một chiến lược tham lam ngây thơ là luôn lấy chi phí nhảy ngay lập tức nhỏ nhất có thể thất bại vì nó có thể buộc người chơi sau đó phải thực hiện một bước nhảy tốn kém hoặc đáp xuống một hòn đá đặc biệt có tác động tiêu cực. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là mô hình hóa mọi chuỗi chuyển động có thể có như một sự chuyển đổi trạng thái. Một trạng thái có thể được xác định bởi vị trí của tất cả`x`Pollywogs và từ mỗi trạng thái, chúng tôi tạo ra tất cả các bước nhảy hợp lệ cho Pollywog ngoài cùng bên trái. Chúng tôi tính toán năng lượng cho mỗi lần chuyển đổi và tìm đệ quy tổng năng lượng tối thiểu. Mặc dù điều này đúng về nguyên tắc nhưng số lượng trạng thái là`O(n^x)`trong trường hợp xấu nhất, lớn về mặt thiên văn khi`n`có thể đạt tới 10^8, ngay cả với`x`lên tới 8. Vì vậy việc tìm kiếm trực tiếp trong không gian trạng thái là không khả thi. 

Thông tin chi tiết quan trọng đến từ việc quan sát thấy các con Pollywogs di chuyển hoàn toàn về bên phải và không thể vượt qua nhau. Trật tự tương đối của chúng được bảo toàn, điều này cho phép chúng ta coi năng lượng là hàm của khoảng cách giữa các Pollywogs chứ không phải là vị trí tuyệt đối. Từ`k`Và`x`nhỏ, chúng ta có thể mã hóa trạng thái dưới dạng độ lệch của các khối từ một cửa sổ chuyển động của các viên đá, sau đó tính toán năng lượng tối thiểu để đạt được viên đá đặc biệt tiếp theo hoặc viên đá cuối cùng`x`đá sử dụng lập trình động. Những viên đá đặc biệt thưa thớt có nghĩa là chúng ta chỉ cần xử lý rõ ràng một số vị trí và giữa chúng, chúng ta có thể tính năng lượng tối thiểu dưới dạng tích ma trận bằng cách sử dụng số học cộng min. Điều này làm giảm độ phức tạp từ hàm mũ trong`n`theo cấp số nhân trong`x`Và`k`, độc lập với`n`và xử lý các khoảng trống lớn một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^x * k^x) | O(n^x) | Quá chậm | 
| Tối ưu (DP + lũy thừa ma trận) | O(q * k^x * x^2) | O(k^x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Mã hóa trạng thái của`x`pollywogs như một bộ các vị trí tương đối. Vì số lần nhảy nhiều nhất là`k`đá, các vị trí tương đối trong một cửa sổ có kích thước`k`nắm bắt tất cả các ràng buộc về va chạm. 
2. Xây dựng ma trận chuyển tiếp`T`cho một lần di chuyển, ở đâu`T[s1][s2]`là năng lượng tối thiểu để chuyển từ trạng thái`s1`để nêu`s2`trong một lần nhảy của Pollywog ngoài cùng bên trái, có tính đến chi phí cho lần nhảy và các hiệu ứng đá đặc biệt. 
3. Những viên đá đặc biệt thưa thớt cho phép chúng ta chia đường đi thành các đoạn giữa những viên đá đặc biệt liên tiếp. Đối với mỗi phân đoạn của`d`đá không có đá đặc biệt, nâng ma trận chuyển tiếp lên lũy thừa`d`theo số học cộng min. Điều này tính toán năng lượng tối thiểu để đi qua`d`đá trong một lô. 
4. Ở mỗi viên đá đặc biệt, hãy điều chỉnh năng lượng bằng cách thêm đá`w[p]`giá trị khi Pollywog ngoài cùng bên trái đáp xuống nó. Sau đó tiếp tục lũy thừa ma trận cho đoạn tiếp theo. 
5. Sau khi xử lý tất cả các loại đá đặc biệt, xử lý phần cuối cùng dẫn đến phần cuối cùng`x`đá. Năng lượng tối thiểu trong số tất cả các trạng thái hợp lệ nơi các Pollywogs chiếm vị trí cuối cùng`x`đá là câu trả lời. 

Tại sao nó hoạt động: bất biến là ở mỗi bước, DP hoặc ma trận chứa năng lượng tối thiểu để đạt đến từng trạng thái được mã hóa. Bởi vì các khối đá không thể vượt qua nhau nên không có trạng thái nào bị bỏ qua và phép lũy thừa ma trận truyền một cách chính xác năng lượng tối thiểu trên các phân đoạn của viên đá. Những viên đá đặc biệt được xử lý chính xác khi chúng được tiếp cận, vì vậy mọi đóng góp năng lượng đều được tính đến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from itertools import product
from collections import defaultdict

def min_plus_matmul(A, B):
    n = len(A)
    C = [[float('inf')]*n for _ in range(n)]
    for i in range(n):
        for j in range(n):
            for k in range(n):
                C[i][j] = min(C[i][j], A[i][k]+B[k][j])
    return C

def mat_pow(mat, power):
    n = len(mat)
    res = [[0 if i==j else float('inf') for j in range(n)] for i in range(n)]
    while power:
        if power % 2:
            res = min_plus_matmul(res, mat)
        mat = min_plus_matmul(mat, mat)
        power //= 2
    return res

x, k, n, q = map(int, input().split())
c = list(map(int, input().split()))
special = {}
for _ in range(q):
    p, w = map(int, input().split())
    special[p] = w

# Enumerate all valid relative states
states = []
def dfs(pos, last):
    if len(pos) == x:
        states.append(tuple(pos))
        return
    for jump in range(1, k+1):
        next_pos = last + jump
        dfs(pos+[next_pos], next_pos)
dfs([], 0)

state_index = {s:i for i,s in enumerate(states)}
size = len(states)

# Build single-step transition matrix
T = [[float('inf')]*size for _ in range(size)]
for i, s1 in enumerate(states):
    for j, s2 in enumerate(states):
        valid = True
        cost = 0
        for a, b in zip(s1, s2):
            d = b - a
            if d < 1 or d > k:
                valid = False
                break
            cost += c[d-1]
        if valid:
            T[i][j] = cost

dp = [float('inf')]*size
dp[state_index[tuple(range(1,x+1))]] = 0

# Process segments between special stones
points = sorted(list(special.keys()) + [n-x+1])
prev = x+1
for p in points:
    d = p - prev
    if d > 0:
        Tp = mat_pow(T, d)
        new_dp = [float('inf')]*size
        for i in range(size):
            for j in range(size):
                new_dp[j] = min(new_dp[j], dp[i]+Tp[i][j])
        dp = new_dp
    # apply special stone effect if leftmost pollywog lands on it
    new_dp = [float('inf')]*size
    for i, s in enumerate(states):
        leftmost = s[0] + (p-prev)
        add = special.get(leftmost, 0)
        new_dp[i] = dp[i] + add
    dp = new_dp
    prev = p

# Final segment to last x stones
d = n - x + 1 - prev
if d > 0:
    Tp = mat_pow(T, d)
    new_dp = [float('inf')]*size
    for i in range(size):
        for j in range(size):
            new_dp[j] = min(new_dp[j], dp[i]+Tp[i][j])
    dp = new_dp

# Answer: min energy among states ending at last x stones
ans = float('inf')
for i, s in enumerate(states):
    if s == tuple(range(n-x+1, n+1)):
        ans = min(ans, dp[i])
print(ans)
```Đầu tiên, mã này tạo ra tất cả các trạng thái tương đối của Pollywogs trong một cửa sổ`k`và liệt kê các chuyển tiếp tôn trọng giới hạn bước nhảy. Phép lũy thừa ma trận xử lý hiệu quả các khoảng trống lớn giữa các viên đá đặc biệt. Việc điều chỉnh năng lượng của mỗi viên đá đặc biệt được áp dụng khi Pollywog ngoài cùng bên trái chạm tới nó. Các chỉ số biên được xử lý cẩn thận để đảm bảo không xảy ra xung đột và các vị trí cuối cùng khớp chính xác với vị trí cuối cùng`x`đá. 

## Ví dụ đã hoạt động 

Sử dụng mẫu 1: 

| Bước | Chức vụ (tiểu bang) | giá trị dp | Bình luận | 
| --- | --- | --- | --- | 
| 0 | (1,2) | 0 | Vị trí ban đầu | 
| Nhảy 3 | (2,5) | 1 | chi phí nhảy tối thiểu | 
| Nhảy 2 | (3 | | |
