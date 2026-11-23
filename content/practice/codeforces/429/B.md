---
title: "CF 429B - Tập luyện"
description: "Chúng ta có một phòng tập thể dục hai chiều được biểu diễn dưới dạng lưới có kích thước n × m. Mỗi ô chứa một số nguyên dương biểu thị lượng calo được đốt cháy khi thực hiện bài tập tại vị trí đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 429
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 245 (Div. 1)"
rating: 1600
weight: 429
solve_time_s: 57
verified: true
draft: false
---

[CF 429B - Đang tập luyện](https://codeforces.com/problemset/problem/429/B) 

**Đánh giá:** 1600 
**Thẻ:** dp 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một phòng tập thể dục hai chiều được biểu diễn dưới dạng lưới có kích thước _n_ × _m_. Mỗi ô chứa một số nguyên dương biểu thị lượng calo được đốt cháy khi thực hiện bài tập tại vị trí đó. Hai người, Iahub và Iahubina, mỗi người xuất phát từ các góc đối diện và cần đến phía bên kia: Iahub bắt đầu ở phía trên bên trái và đi về phía dưới bên phải, chỉ di chuyển xuống dưới hoặc sang phải, trong khi Iahubina bắt đầu ở phía dưới bên trái và đi lên phía trên bên phải, chỉ di chuyển lên hoặc sang phải. Tại đúng một ô, chúng gặp nhau và tạm dừng mà không kết thúc. Mục tiêu là tối đa hóa tổng lượng calo được đốt cháy trên cả hai con đường. 

Độ phức tạp chính phát sinh từ kích thước của lưới: _n_ và _m_ mỗi cái có thể lên tới 1000. Một cách tiếp cận đơn giản là thử tất cả các điểm gặp nhau có thể và tất cả các đường dẫn có thể sẽ chậm theo cấp số nhân. Vì lưới có tới 1.000.000 ô nên mọi giải pháp thực hiện các thao tác O(n²·m²) sẽ vượt quá giới hạn thời gian. Chúng ta cần thứ gì đó xung quanh O(n·m) để được an toàn. 

Các trường hợp ranh giới không rõ ràng bao gồm các tình huống trong đó lợi ích tối đa đến từ việc gặp nhau gần biên giới, buộc một con đường phải uốn cong theo những cách không trực quan. Ví dụ: hãy xem xét lưới 3 × 3 trong đó ô ở giữa có giá trị nhỏ nhất. Nếu ô họp là trung tâm, mỗi người phải tránh lãng phí các ô có giá trị cao một cách không cần thiết và cách tiếp cận tham lam bất cẩn có thể chọn mức tối đa cục bộ nhưng lại bỏ lỡ tổng kết hợp tối ưu. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực xem xét mọi tế bào họp có thể. Đối với mỗi ô, chúng tôi sẽ tính toán lượng calo tối đa được thu thập dọc theo tất cả các đường dẫn có thể từ đầu đến ô đó đối với Iahub và từ đầu đến ô đó đối với Iahubina. Sau đó, chúng tôi cũng sẽ tính toán đường đi từ ô họp đến cuối cho mỗi người. Về nguyên tắc, điều này đúng, nhưng đối với mỗi ô họp trong số n·m, việc tính toán một đường dẫn có thể mất O(n·m), dẫn đến tổng số hoạt động là O(n·m²). Với n, m ≤ 1000, đây là khoảng 10¹² hoạt động, quá chậm. 

Thông tin chi tiết quan trọng là mỗi người chỉ có thể di chuyển theo hai hướng: Iahub xuống hoặc sang phải và Iahubina lên hoặc sang phải. Điều này có nghĩa là lượng calo tối đa được thu thập vào bất kỳ ô nào có thể được tính toán trước bằng cách sử dụng lập trình động. Cụ thể, chúng ta có thể tính toán bốn mảng DP: 

1.`dp1[i][j]`- lượng calo tối đa từ trên cùng bên trái đến`(i, j)`(Iahub bắt đầu) 
2.`dp2[i][j]`- lượng calo tối đa từ dưới cùng bên phải đến`(i, j)`(cuối Iahub) 
3.`dp3[i][j]`- lượng calo tối đa từ dưới cùng bên trái đến`(i, j)`(Iahubina bắt đầu) 
4.`dp4[i][j]`- lượng calo tối đa từ trên cùng bên phải đến`(i, j)`(cuối Iahubina) 

Khi những thứ này đã được lấp đầy, đối với mỗi ô cuộc họp tiềm năng`(i, j)`, chúng ta có thể tính tổng mức tăng là tổng của hai đường dẫn riêng biệt để tránh tính hai lần ô họp. Có hai cách mà chúng có thể gặp nhau trong khi tối đa hóa tổng lợi ích, tương ứng với hai kiểu "giao nhau": một cách là chúng giao nhau mà không chia sẻ các cạnh theo chiều ngang và cách khác là theo chiều dọc. Lặp lại trên tất cả các ô bên trong (không bao gồm đường viền) và tính tổng từ các mảng DP này là O(n·m), rất hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²·m²) | O(n·m) | Quá chậm | 
| Tính toán trước DP | O(n·m) | O(n·m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước`n`,`m`và lưới điện`a`. Chúng ta cần tính toán mức tăng tối đa theo bốn hướng. 
2. Khởi tạo bốn mảng DP:`dp1`,`dp2`,`dp3`,`dp4`. Mỗi mảng lưu trữ lượng calo tối đa từ một điểm khởi đầu cụ thể đến từng tế bào. Khởi tạo ô bắt đầu của mỗi mảng DP với lượng calo của ô tương ứng. 
3. Điền vào`dp1`(Iahub trên cùng bên trái đến dưới cùng bên phải). Đối với mỗi ô`(i, j)`, lượng calo tối đa để đạt được là`a[i][j] + max(dp1[i-1][j], dp1[i][j-1])`. Hàng và cột đầu tiên xử lý các trường hợp cạnh chỉ tồn tại một ô trước đó. 
4. Điền vào`dp2`(Iahub từ dưới cùng bên phải đến trên cùng bên trái). Đối với mỗi ô`(i, j)`,`dp2[i][j] = a[i][j] + max(dp2[i+1][j], dp2[i][j+1])`. Bắt đầu từ phía dưới bên phải và di chuyển về phía sau. 
5. Điền vào`dp3`(Iahubina từ dưới bên trái sang trên cùng bên phải). Đối với mỗi ô`(i, j)`,`dp3[i][j] = a[i][j] + max(dp3[i+1][j], dp3[i][j-1])`. Bắt đầu từ phía dưới bên trái. 
6. Điền vào`dp4`(Iahubina trên cùng bên phải đến dưới cùng bên trái). Đối với mỗi ô`(i, j)`,`dp4[i][j] = a[i][j] + max(dp4[i-1][j], dp4[i][j+1])`. Bắt đầu từ trên cùng bên phải. 
7. Lặp lại tất cả các ô bên trong`(i, j)`Ở đâu`1 ≤ i ≤ n-2`Và`1 ≤ j ≤ m-2`. Đối với mỗi ô, hãy tính hai tổng số có thể có: 

- Mẫu 1: Iahub đi xuống rồi sang phải, Iahubina đi thẳng rồi lên.`total1 = dp1[i-1][j] + dp2[i+1][j] + dp3[i][j-1] + dp4[i][j+1]`- Mẫu 2: Iahub đi sang phải rồi đi xuống, Iahubina đi lên rồi sang phải.`total2 = dp1[i][j-1] + dp2[i][j+1] + dp3[i+1][j] + dp4[i-1][j]`Cập nhật câu trả lời với mức tối đa`total1`Và`total2`. 
8. Xuất ra tổng mức tăng tối đa. 

**Tại sao nó hoạt động:** Mảng DP đảm bảo rằng đối với bất kỳ ô nào, chúng ta có mức tăng tối đa dọc theo các hướng cho phép. Bằng cách tính toán trước cả bốn hướng, các mẫu giao nhau có thể được kết hợp theo O(1) cho mỗi ô bên trong mà không cần tính hai lần điểm gặp nhau. Vì chúng tôi xem xét tất cả các ô bên trong nên chúng tôi đảm bảo chọn được điểm gặp mặt tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
a = [list(map(int, input().split())) for _ in range(n)]

dp1 = [[0]*m for _ in range(n)]
dp2 = [[0]*m for _ in range(n)]
dp3 = [[0]*m for _ in range(n)]
dp4 = [[0]*m for _ in range(n)]

# dp1: top-left to bottom-right
for i in range(n):
    for j in range(m):
        best = 0
        if i > 0:
            best = max(best, dp1[i-1][j])
        if j > 0:
            best = max(best, dp1[i][j-1])
        dp1[i][j] = a[i][j] + best

# dp2: bottom-right to top-left
for i in range(n-1, -1, -1):
    for j in range(m-1, -1, -1):
        best = 0
        if i < n-1:
            best = max(best, dp2[i+1][j])
        if j < m-1:
            best = max(best, dp2[i][j+1])
        dp2[i][j] = a[i][j] + best

# dp3: bottom-left to top-right
for i in range(n-1, -1, -1):
    for j in range(m):
        best = 0
        if i < n-1:
            best = max(best, dp3[i+1][j])
        if j > 0:
            best = max(best, dp3[i][j-1])
        dp3[i][j] = a[i][j] + best

# dp4: top-right to bottom-left
for i in range(n):
    for j in range(m-1, -1, -1):
        best = 0
        if i > 0:
            best = max(best, dp4[i-1][j])
        if j < m-1:
            best = max(best, dp4[i][j+1])
        dp4[i][j] = a[i][j] + best

ans = 0
for i in range(1, n-1):
    for j in range(1, m-1):
        total1 = dp1[i-1][j] + dp2[i+1][j] + dp3[i][j-1] + dp4[i][j+1]
        total2 = dp1[i][j-1] + dp2[i][j+1] + dp3[i+1][j] + dp4[i-1][j]
        ans = max(ans, total1, total2)

print(ans)
```Bốn khối đầu tiên tính toán các bảng DP cho cả bốn hướng. Vòng lặp trên các ô bên trong đảm bảo chúng tôi tránh được các đường viền vì điểm gặp nhau phải cho phép tất cả các đường dẫn tiếp tục. Hai mô hình giao nhau xử lý cả hai cách có thể kết hợp các đường đi mà không có lượng calo chồng chéo. Tắt
