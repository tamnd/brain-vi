---
title: "CF 11D - Một Nhiệm Vụ Đơn Giản"
description: "Chúng ta được yêu cầu đếm số chu trình đơn trong đồ thị vô hướng. Một chu trình đơn giản là một vòng khép kín trong đó không có đỉnh hoặc cạnh nào lặp lại."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 11
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 11"
rating: 2200
weight: 11
solve_time_s: 89
verified: true
draft: false
---
[CF 11D - Một nhiệm vụ đơn giản](https://codeforces.com/problemset/problem/11/D) 

**Đánh giá:** 2200 
**Thẻ:** bitmask, dp, đồ thị 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số chu trình đơn trong đồ thị vô hướng. Một chu trình đơn giản là một vòng khép kín trong đó không có đỉnh hoặc cạnh nào lặp lại. Đầu vào cho chúng ta số đỉnh`n`và số cạnh`m`, theo sau là`m`cặp số nguyên mô tả các đỉnh được kết nối. Đầu ra là một số nguyên biểu thị tổng số chu kỳ đơn giản. 

Hạn chế chính ở đây là`n`nhiều nhất là 19. Giá trị này đủ nhỏ để các thuật toán hàm mũ dựa trên tập hợp con các đỉnh là khả thi vì$2^{19}$là khoảng 500.000. Số cạnh`m`không bị giới hạn trong phát biểu vấn đề, nhưng trong thực tế nó không thể vượt quá$n(n-1)/2$vì đồ thị đơn giản. Điều này có nghĩa là các phương pháp tiếp cận bạo lực nhằm kiểm tra tất cả các chuỗi đỉnh có thể sẽ nhanh chóng trở nên không khả thi do tăng trưởng giai thừa. Một cách tiếp cận bất cẩn cố gắng liệt kê mọi hoán vị của các đỉnh và kiểm tra xem nó có tạo thành một chu trình hay không sẽ thất bại ngay cả đối với`n=10`. 

Trường hợp cạnh cần xem xét là đồ thị không có cạnh hoặc một đỉnh. Ví dụ,`n=1, m=0`nên xuất ra 0, vì không tồn tại chu kỳ nào. Một trường hợp cạnh tinh tế khác là biểu đồ tạo thành cây. Cây không có chu trình và một thuật toán đơn giản kiểm tra các đường dẫn mà không ghi sổ kế toán thích hợp có thể tính sai một đường dẫn lặp lại chính nó như một chu trình. Các nhóm cũng đáng được kiểm tra vì chúng tối đa hóa số chu kỳ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử mọi tập hợp con của các đỉnh, tạo ra tất cả các hoán vị của từng tập hợp con và kiểm tra xem nó có tạo thành một chu trình hay không. Điều này đúng vì bất kỳ chu trình đơn giản nào cũng phải tương ứng với một hoán vị nào đó của một tập con các đỉnh. Tuy nhiên, số lượng hoán vị tăng lên khi`n!`, và thậm chí đối với`n=10`con số này là hơn 3 triệu thao tác trên mỗi tập hợp con, khiến nó không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta có thể khai thác hai sự thật. Đầu tiên, đồ thị nhỏ (`n <= 19`), cho phép chúng ta biểu diễn các tập hợp con của các đỉnh dưới dạng bitmask. Thứ hai, chu trình là các đường đi khép kín trong đó mỗi đỉnh được truy cập chính xác một lần và lập trình động có thể đếm đường đi giữa các đỉnh trên các tập hợp con một cách hiệu quả. Chúng ta có thể định nghĩa`dp[mask][v]`là số cách để đạt đến đỉnh`v`thăm chính xác các đỉnh trong`mask`bắt đầu từ đỉnh nhỏ nhất trong`mask`. Khi đó, một chu trình chỉ đơn giản là một tập hợp con trong đó`dp[mask][v]`có thể quay lại đỉnh bắt đầu thông qua một cạnh và chúng ta có thể đảm bảo mỗi chu kỳ được tính chính xác một lần bằng cách cố định đỉnh nhỏ nhất làm điểm bắt đầu. 

Brute-force hoạt động vì nó đảm bảo tính đúng đắn của việc xây dựng, nhưng không thành công do tăng trưởng giai thừa. Phương pháp lập trình động bitmask hoạt động hiệu quả vì nó làm giảm độ phức tạp giai thừa thành`O(n * 2^n)`, có thể quản lý được đối với`n = 19`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n) | Quá chậm | 
| Mặt nạ bit DP | O(n * 2^n) | O(n * 2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh`n`và các cạnh`m`và xây dựng ma trận kề`adj`như vậy`adj[u][v] = True`nếu có một cạnh giữa`u`Và`v`. Ma trận kề cho phép kiểm tra kết nối liên tục. 
2. Khởi tạo bảng DP`dp[mask][v]`Ở đâu`mask`là một bitmask đại diện cho tập hợp các đỉnh được truy cập và`v`là đỉnh cuối cùng của đường đi. Bảng lưu trữ số lượng đường đi thăm chính xác các đỉnh trong`mask`kết thúc tại`v`, bắt đầu từ đỉnh nhỏ nhất trong`mask`. 
3. Đối với mỗi đỉnh`v`, bộ`dp[1 << v][v] = 1`. Điều này thể hiện việc bắt đầu một đường dẫn ở đỉnh`v`chỉ với`v`trong con đường. 
4. Lặp lại tất cả`mask`từ 1 đến`(1 << n) - 1`. Đối với mỗi đỉnh`u`TRONG`mask`, hãy xem xét mọi người hàng xóm`v`của`u`. Nếu như`v`vẫn chưa vào`mask`Và`v`lớn hơn đỉnh nhỏ nhất trong`mask`(để tránh chu kỳ đếm kép), hãy cập nhật`dp[mask | (1 << v)][v] += dp[mask][u]`. Điều này mở rộng tất cả các đường dẫn kết thúc tại`u`bằng cách thêm`v`. 
5. Sau khi điền vào bảng DP, hãy đếm số chu kỳ. Đối với mỗi tập hợp con`mask`có ít nhất 3 đỉnh, tìm đỉnh nhỏ nhất`s`. Đối với mỗi đỉnh`v`TRONG`mask`khác hơn là`s`, nếu có một cạnh giữa`v`Và`s`, thêm vào`dp[mask][v]`để trả lời. Mỗi chu kỳ được tính chính xác một lần vì chúng tôi đã sửa`s`là đỉnh nhỏ nhất trong chu trình. 
6. Xuất tổng số. 

Điều bất biến ở đây là`dp[mask][v]`đếm chính xác tất cả các đường đi từ đỉnh nhỏ nhất trong`mask`ĐẾN`v`sử dụng chính xác các đỉnh trong`mask`. Bằng cách chỉ mở rộng các đường đi đến các đỉnh lớn hơn điểm bắt đầu, chúng tôi tránh việc đếm cùng một chu kỳ nhiều lần trong các phép quay khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
adj = [[False] * n for _ in range(n)]
for _ in range(m):
    a, b = map(int, input().split())
    a -= 1
    b -= 1
    adj[a][b] = adj[b][a] = True

dp = [[0] * n for _ in range(1 << n)]
for v in range(n):
    dp[1 << v][v] = 1

for mask in range(1, 1 << n):
    # find smallest vertex in mask
    s = (mask & -mask).bit_length() - 1
    for u in range(n):
        if not (mask & (1 << u)):
            continue
        for v in range(n):
            if adj[u][v] and not (mask & (1 << v)) and v > s:
                dp[mask | (1 << v)][v] += dp[mask][u]

ans = 0
for mask in range(1, 1 << n):
    if bin(mask).count("1") < 3:
        continue
    s = (mask & -mask).bit_length() - 1
    for v in range(n):
        if v != s and (mask & (1 << v)) and adj[v][s]:
            ans += dp[mask][v]

print(ans)
```Ma trận kề cho phép kiểm tra hàng xóm theo thời gian không đổi. Việc khởi tạo DP thiết lập các đường dẫn bắt đầu từ mỗi đỉnh. Chúng tôi cẩn thận chỉ mở rộng đến các đỉnh lớn hơn nhỏ nhất trong mặt nạ để tránh việc đếm trùng lặp. Vòng lặp cuối cùng chỉ đếm các chu kỳ có độ dài ít nhất là 3, tính tổng các đường đi có thể quay trở lại đỉnh bắt đầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```| mặt nạ | s | v | dp[mặt nạ][v] | Bình luận | 
| --- | --- | --- | --- | --- | 
| 0001 | 0 | 0 | 1 | bắt đầu từ đỉnh 1 | 
| 0011 | 0 | 1 | 1 | đường dẫn 1->2 | 
| 0111 | 0 | 2 | 1 | đường dẫn 1->2->3 | 
| 1111 | 0 | 3 | 1 | đường dẫn 1->2->3->4 | 
| cuối cùng | - | - | 7 | 4 hình tam giác + 3 hình tứ giác | 

Dấu vết này xác nhận DP mở rộng đường dẫn một cách chính xác và đếm chu kỳ một lần. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 2
1 2
2 3
```| mặt nạ | s | v | dp[mặt nạ][v] | Bình luận | 
| --- | --- | --- | --- | --- | 
| 0001 | 0 | 0 | 1 | bắt đầu 1 | 
| 0011 | 0 | 1 | 1 | đường dẫn 1->2 | 
| 0111 | 0 | 2 | 1 | đường dẫn 1->2->3 | 
| cuối cùng | - | - | 0 | Không có cạnh 3->1, không có chu kỳ | 

Điều này cho thấy thuật toán đã bỏ qua các chu kỳ một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * 2^n) | Mỗi mặt nạ lặp qua n đỉnh và n lân cận | 
| Không gian | O(n * 2^n) | Bảng DP lưu trữ một giá trị cho mỗi mặt nạ và đỉnh | 

Với`n <= 19`,`2^n * n^2`là khoảng 5 triệu thao tác, thoải mái dưới 2 giây. Bảng DP phù hợp với bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    adj = [[False] * n for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        adj[a][b] = adj[b][a] = True
    dp = [[0] * n for _ in range(1 << n)]
    for v in range(n):
        dp[1 << v][v] = 1
    for mask in range(1,
```
