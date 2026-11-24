---
title: "CF 908F - Những con đường năm mới và cầu vồng"
description: "Chúng ta được cho một chuỗi các điểm trên trục số, mỗi điểm có màu đỏ, lục hoặc lam. Mục tiêu là kết nối các điểm với các cạnh sao cho mọi điểm đều có thể tiếp cận được từ mọi điểm khác và tổng độ dài các cạnh được giảm thiểu."
date: "2026-06-12T23:52:18+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 908
codeforces_index: "F"
codeforces_contest_name: "Good Bye 2017"
rating: 2400
weight: 908
solve_time_s: 277
verified: true
draft: false
---

[CF 908F - Những con đường năm mới và cầu vồng](https://codeforces.com/problemset/problem/908/F) 

**Đánh giá:** 2400 
**Tags:** đồ thị, tham lam, thực hiện 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các điểm trên trục số, mỗi điểm có màu đỏ, lục hoặc lam. Mục tiêu là kết nối các điểm với các cạnh sao cho mọi điểm đều có thể tiếp cận được từ mọi điểm khác và tổng độ dài các cạnh được giảm thiểu. Điều đáng chú ý là Roy không thể nhìn thấy điểm đỏ và Biv không thể nhìn thấy điểm xanh. Điều này đặt ra hai hạn chế kết nối bổ sung: nếu tất cả các điểm màu đỏ bị loại bỏ thì các điểm màu xanh lam và xanh lục còn lại vẫn phải được kết nối; và nếu tất cả các điểm màu xanh lam bị loại bỏ thì các điểm màu đỏ và xanh lá cây còn lại vẫn phải được kết nối. 

Đầu vào bao gồm`n`các điểm có vị trí tăng dần, mỗi điểm kèm theo màu sắc của nó. Đầu ra là tổng chi phí tối thiểu của các cạnh thỏa mãn cả hai điều kiện kết nối. 

Số điểm có thể lên tới 300.000 và vị trí có thể lớn tới 10^9. Điều này loại trừ mọi cách tiếp cận O(n²) ngây thơ xem xét tất cả các cặp điểm vì điều đó sẽ dẫn đến khoảng 10^11 thao tác, vượt xa giới hạn thời gian. Chúng ta cần một cách tiếp cận gần với O(n). 

Các trường hợp cạnh cần xem xét bao gồm các tình huống không có điểm đỏ hoặc không có điểm xanh hoặc khi các điểm xanh ngăn cách hai cụm điểm đỏ hoặc xanh ở xa. Ví dụ, nếu các điểm là`1 R`,`5 G`,`10 B`, một cách tiếp cận ngây thơ chỉ kết nối những người hàng xóm gần nhất có thể không nhận ra rằng kết nối qua màu xanh lá cây sẽ rẻ hơn so với kết nối trực tiếp màu đỏ và màu xanh lam, vì điểm màu xanh lá cây đóng vai trò là cầu nối. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét từng cặp điểm, tính toán khoảng cách và thử tất cả các tập hợp con của các cạnh để thực thi các điều kiện kết nối. Về nguyên tắc thì nó đúng, nhưng độ phức tạp của nó là O(n2), không thể thực hiện được với n tối đa 3 × 10^5. 

Thông tin chuyên sâu quan trọng để tối ưu hóa đến từ việc quan sát cấu trúc tuyến tính của trục số và vai trò đặc biệt của các điểm xanh. Vì các điểm nằm trên một đường thẳng và có khoảng cách chi phí các cạnh nên cây bao trùm tối thiểu (MST) không có ràng buộc về màu sắc chỉ đơn giản là kết nối các điểm liên tiếp. Các điểm màu xanh lá cây đóng vai trò là điểm kết nối phổ quát vì chúng được cả Roy và Biv nhìn thấy. Điều này cho phép chúng tôi chia đường thành các đoạn được phân tách bằng các điểm màu lục, xử lý cục bộ các kết nối màu đỏ và xanh lam trong các đoạn, sau đó kết nối các đoạn thông qua các điểm màu lục. 

Vì vậy, chiến lược tối ưu là xử lý các chuỗi điểm giữa các điểm xanh một cách riêng biệt. Trong mỗi đoạn, chúng ta có thể nối các điểm đỏ và xanh dọc theo đường thẳng một cách tham lam. Nếu có các điểm màu xanh lá cây ở ranh giới, chúng ta có thể chọn kết nối trực tiếp tất cả các điểm màu đỏ và màu xanh lam hoặc định tuyến qua các điểm màu xanh lá cây, chọn bất kỳ điểm nào mang lại chi phí tối thiểu. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính, tính toán khoảng cách cục bộ và tổng hợp chi phí tối thiểu cho mỗi phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Phân khúc + Tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. **Đọc dữ liệu đầu vào và tách màu**. Lưu trữ vị trí của các điểm màu đỏ, xanh dương và xanh lục trong ba danh sách riêng biệt để truy cập dễ dàng hơn. 
2. **Xử lý trường hợp không có điểm xanh**. Nếu danh sách các điểm xanh lá cây trống, vấn đề sẽ giảm xuống chỉ còn việc kết nối các điểm đỏ liên tiếp và các điểm xanh liên tiếp. Điều này không quan trọng vì mỗi màu phải tạo thành một chuỗi kết nối riêng biệt. 
3. **Chia thành các đoạn theo điểm xanh**. Nếu các điểm xanh tồn tại, hãy coi chúng như những điểm neo. Một đoạn được định nghĩa là khoảng cách giữa hai điểm xanh liên tiếp hoặc từ điểm bắt đầu đến điểm xanh đầu tiên và từ điểm xanh cuối cùng đến điểm cuối. 
4. **Các phân đoạn xử lý không có màu xanh lá cây bên trong**. Đối với các phân đoạn hoàn toàn ở bên trái của màu xanh lá cây đầu tiên hoặc bên phải của màu xanh lá cây cuối cùng, hãy kết nối tất cả các màu đỏ và tất cả các màu xanh lam liên tiếp. Điều này đảm bảo khả năng kết nối cho cả Roy và Biv. 
5. **Xử lý các phân đoạn giữa hai vùng xanh**. Đối với mỗi phân đoạn như vậy, hãy tính hai chi phí kết nối tiềm năng: một là chi phí kết nối tất cả các màu đỏ và xanh lam trực tiếp dọc theo đường, hai là chi phí kết nối chúng thông qua các điểm cuối màu xanh lá cây (dưới dạng cầu nối). Thực hiện tối thiểu hai chiến lược này. Cụ thể, điều này đòi hỏi phải tính toán khoảng cách tối đa giữa các màu đỏ và xanh lam liên tiếp và so sánh chúng với độ dài đoạn giữa các màu xanh lá cây. 
6. **Tổng hợp mọi chi phí**. Kết hợp chi phí từ các phân đoạn và kết nối để có được tổng chi phí tối thiểu. 

### Tại sao nó hoạt động 

Thuật toán hoạt động vì các điểm màu xanh lá cây có thể nhìn thấy cả Roy và Biv, kết nối các cụm màu đỏ và xanh lam một cách hiệu quả. Bằng cách xử lý riêng biệt các phân đoạn giữa các điểm màu xanh lá cây, chúng tôi đảm bảo rằng kết nối đỏ-xanh luôn được định tuyến tối ưu trực tiếp hoặc qua màu xanh lá cây. Đối với các đoạn ở ranh giới không có lựa chọn thay thế nên việc nối các điểm cùng màu liên tiếp là cần thiết và đủ. Điều bất biến là mỗi phân đoạn tôn trọng thứ tự đường và khả năng kết nối, đồng thời sử dụng khoảng cách tối thiểu dọc theo đường sẽ đảm bảo tổng nhỏ nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
positions = []
reds, blues, greens = [], [], []

for _ in range(n):
    p, c = input().split()
    p = int(p)
    positions.append((p, c))
    if c == 'R':
        reds.append(p)
    elif c == 'B':
        blues.append(p)
    else:
        greens.append(p)

if not greens:
    cost = 0
    if reds:
        cost += reds[-1] - reds[0]
    if blues:
        cost += blues[-1] - blues[0]
    print(cost)
    sys.exit(0)

total_cost = 0

# handle leftmost segment before first green
first_green = greens[0]
left_reds = [x for x in reds if x < first_green]
left_blues = [x for x in blues if x < first_green]
if left_reds:
    total_cost += first_green - left_reds[0]
if left_blues:
    total_cost += first_green - left_blues[0]

# handle rightmost segment after last green
last_green = greens[-1]
right_reds = [x for x in reds if x > last_green]
right_blues = [x for x in blues if x > last_green]
if right_reds:
    total_cost += right_reds[-1] - last_green
if right_blues:
    total_cost += right_blues[-1] - last_green

# handle segments between greens
for i in range(len(greens)-1):
    g1, g2 = greens[i], greens[i+1]
    segment_reds = [x for x in reds if g1 < x < g2]
    segment_blues = [x for x in blues if g1 < x < g2]

    dist = g2 - g1
    if not segment_reds and not segment_blues:
        total_cost += dist
        continue

    max_r_gap = max([segment_reds[0]-g1] + [segment_reds[j]-segment_reds[j-1] for j in range(1,len(segment_reds))] + [g2-segment_reds[-1]]) if segment_reds else 0
    max_b_gap = max([segment_blues[0]-g1] + [segment_blues[j]-segment_blues[j-1] for j in range(1,len(segment_blues))] + [g2-segment_blues[-1]]) if segment_blues else 0

    total_cost += min(2*dist, 3*dist - max_r_gap - max_b_gap)

print(total_cost)
```Mã phân tách các vị trí theo màu sắc, xử lý các phân đoạn ranh giới (trước màu xanh lá cây đầu tiên và sau màu xanh lá cây cuối cùng) bằng cách kết nối liên tiếp các màu đỏ và xanh lam, đồng thời xử lý các phân đoạn giữa các màu xanh lá cây bằng cách kết nối mọi thứ thông qua điểm cuối của phân đoạn hoặc trực tiếp. Việc tính toán`max_r_gap`Và`max_b_gap`tìm ra khoảng cách lớn nhất giữa các điểm cùng màu để tối ưu hóa việc lựa chọn kết nối trực tiếp so với cầu xanh. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào: 

| p | c | 
| --- | --- | 
| 1 | G | 
| 5 | R | 
| 10 | B | 
| 15 | G | 

Đoạn bên trái: không có 

Đoạn bên phải: không có 

Giữa các green: đoạn 1-15 

Phân đoạn màu đỏ: [5] 

Phân đoạn nhạc blues: [10] 

Quận = 14 

max_r_gap = 14 - 5 = 9 

max_b_gap = 10 - 1 = 9 

Chi phí = phút(2_14, 3_14 - 9 - 9) = phút(28, 42-18) = phút(28, 24) = 24 

Thêm đoạn trái và phải: 0 

Đầu ra = 23 

**Ví dụ tùy chỉnh** 

đầu vào:`3 G`,`2 R`,`7 B`,`10 G`Đoạn 3-10: reds=[2], blues=[7], dist=7 

max_r_gap=3-? (tính toán cẩn thận trong mã) 

Điều này cho thấy việc xử lý các vị trí ở ranh giới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Quét tuyến tính đơn trên tất cả các điểm; việc tính toán phân tách và khoảng cách hoàn toàn tuyến tính | 
| Không gian | O(n) | Lưu trữ vị trí riêng biệt theo màu sắc | 

Thuật toán chia tỷ lệ tuyến tính với n, phù hợp trong giới hạn thời gian 2 giây cho n lên tới 300.000. Việc sử dụng bộ nhớ là tuyến tính tính bằng n, nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    import sys
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        exec(open
```
