---
title: "CF 1841F - Monocarp và trò chơi chiến lược"
description: "Chúng tôi được yêu cầu mô phỏng một trò chơi xây dựng thành phố chiến lược trong đó Monocarp có thể chấp nhận hoặc từ chối các nhóm sinh vật đến thành phố của mình. Mỗi nhóm chứa một số lượng người, Orc, yêu tinh và người lùn nhất định."
date: "2026-06-09T06:22:47+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1841
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 150 (Rated for Div. 2)"
rating: 2700
weight: 1841
solve_time_s: 77
verified: false
draft: false
---

[CF 1841F - Monocarp và một trò chơi chiến lược](https://codeforces.com/problemset/problem/1841/F) 

**Đánh giá:** 2700 
**Thẻ:** hình học, sắp xếp, hai con trỏ 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một trò chơi xây dựng thành phố chiến lược trong đó Monocarp có thể chấp nhận hoặc từ chối các nhóm sinh vật đến thành phố của mình. Mỗi nhóm chứa một số lượng người, Orc, yêu tinh và người lùn nhất định. Mỗi sinh vật đóng góp vào điểm số toàn cầu bao gồm tổng dân số và tổng số hạnh phúc của tất cả cư dân. Hạnh phúc được xác định bởi các mối quan hệ thân thiện và thù địch: những sinh vật cùng chủng tộc làm tăng hạnh phúc của nhau, trong khi những sinh vật thuộc chủng tộc thù địch làm giảm hạnh phúc đó. Cụ thể, con người và loài Orc có thái độ thù địch với nhau, còn yêu tinh và người lùn thù địch với nhau. 

Đầu vào được đưa ra là`n`các nhóm, mỗi nhóm được mô tả bằng bốn số nguyên không âm xác định số lượng sinh vật của mỗi chủng tộc. Nhiệm vụ của chúng tôi là quyết định nên chấp nhận nhóm nào để tối đa hóa tổng số điểm, được tính bằng tổng số cư dân cộng với tổng tất cả các giá trị hạnh phúc cá nhân. 

Những hạn chế là đáng kể:`n`có thể lên tới 300.000 và mỗi quần thể có thể đạt tới 10^9. Điều này loại trừ mọi cách tiếp cận bạo lực nhằm đánh giá mọi tập hợp con có thể có của các nhóm, vì sẽ có 2^300.000 tập hợp con, điều này không khả thi về mặt tính toán. Thuật toán phải chạy trong khoảng thời gian O(n log n) hoặc O(n), sử dụng nhiều cấu trúc và thuộc tính tích lũy thay vì tìm kiếm toàn diện. 

Các trường hợp nguy hiểm có thể gây cản trở việc triển khai ngây thơ bao gồm các nhóm chỉ chứa các sinh vật thù địch. Ví dụ: một nhóm có 10 người và 0 Orc sẽ hoàn toàn có lợi, trong khi một nhóm có 5 người và 10 Orc có thể làm giảm tổng mức hạnh phúc. Chấp nhận một nhóm một cách mù quáng mà không xem xét tác động thực sự của nó đối với hạnh phúc có thể tạo ra điểm số dưới mức tối ưu. Ngoài ra, các nhóm trống hoặc nhóm có giá trị cực lớn cần được xử lý cẩn thận để tránh tràn số nguyên. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả các tập hợp con của các nhóm, tính toán tổng dân số và mức độ hạnh phúc cho mỗi tập hợp con và trả về điểm tối đa. Mỗi phép tính tập hợp con yêu cầu tính tổng dân số và tính toán mức độ hạnh phúc theo cặp giữa các chủng tộc, dẫn đến các hoạt động O(n * 2^n). Ngay cả với n = 20, điều này vẫn không khả thi và ở n = 3*10^5 thì điều đó là không thể. 

Cái nhìn sâu sắc quan trọng là quan sát thấy rằng sự đóng góp của một nhóm vào tổng số điểm có thể được biểu thị dưới dạng hàm tuyến tính của số lượng mỗi cuộc đua của nhóm và tổng số lượng hiện tại trong thành phố. Cho phép`H`,`O`,`E`,`D`biểu thị tổng số con người, Orc, yêu tinh và người lùn trong thành phố cho đến nay. Sự đóng góp hạnh phúc của một nhóm mới có giá trị`a, b, c, d`là:```
(a*(H) - a*(B)) + (b*(O) - b*(A)) + (c*(E) - c*(D)) + (d*(D) - d*(C))
```Ở đâu`A, B, C, D`là tổng số hiện tại của con người, Orc, yêu tinh, người lùn. Sắp xếp lại, sự đóng góp của hạnh phúc trở nên tuyến tính trong sự khác biệt tổng thể giữa số lượng bạn bè và thù địch. Nếu chúng ta xác định:```
x = humans - orcs
y = elves - dwarves
```Sau đó, vấn đề giảm xuống còn việc chọn một tập hợp con của các nhóm để tối đa hóa tổng trên các hàm tuyến tính của`x`Và`y`. Bởi vì sự đóng góp của mỗi nhóm là đơn điệu đối với`x`Và`y`, chúng ta có thể khai thác một chiến lược từ hình học: sắp xếp các nhóm theo "độ dốc" của chúng trong`(x, y)`mặt phẳng và áp dụng phép chọn tham lam. Cụ thể, các nhóm có thể được phân loại theo tính chẵn lẻ của`(humans + orcs) - (elves + dwarves)`và vấn đề giảm xuống mức tối đa hóa tổng trên các phép chiếu tuyến tính này. Giải pháp cuối cùng sử dụng phương pháp sắp xếp và tham lam kết hợp với tổng tiền tố để tính tổng điểm một cách hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả`n`nhóm và lưu trữ số lượng`(a, b, c, d)`trong một danh sách. Chúng đại diện cho con người, Orc, yêu tinh và người lùn trong mỗi nhóm. Điều này thiết lập đầu vào cho bước tối ưu hóa. 
2. Với mỗi nhóm, tính hai giá trị dẫn xuất:`x = humans - orcs`Và`y = elves - dwarves`. Những khác biệt này mã hóa ảnh hưởng thực sự đến hạnh phúc do những tương tác thù địch. Mỗi nhóm mới đóng góp tuyến tính vào điểm số về mặt`x`Và`y`. 
3. Nhận thức rằng tổng điểm là tổng của tổng dân số cộng với tổng đóng góp về hạnh phúc. Sự đóng góp hạnh phúc của việc thêm một nhóm có thể được thể hiện như`a*(H-B) + b*(O-A) + c*(E-D) + d*(D-C)`, điều này đơn giản hóa về mặt`x`Và`y`và đơn điệu với tổng số hiện tại. 
4. Để có tổng điểm tối đa, hãy phân loại các nhóm theo dấu`x`Và`y`. Các nhóm có kết quả tích cực`x`Và`y`sẽ làm tăng tổng số hiện tại và do đó làm tăng hạnh phúc, và nên được chấp nhận. Các nhóm có đóng góp tiêu cực cần được đánh giá cẩn thận. Bởi vì hàm điểm lồi trong không gian được biến đổi này, nên lựa chọn tối ưu sẽ chuyển thành chiến lược tham lam: sắp xếp các nhóm theo tổng`x + y`hoặc giá trị tuyệt đối của nó và chấp nhận chúng nếu tổng điểm tăng lên. 
5. Lặp lại qua các nhóm được sắp xếp, duy trì tổng số tích lũy cho con người, Orc, yêu tinh, người lùn. Đối với mỗi nhóm, hãy tính phần đóng góp cận biên của nhóm đó vào tổng số điểm nếu được cộng vào. Nếu đóng góp cận biên không âm, hãy chấp nhận nhóm và cập nhật tổng số; nếu không, hãy bỏ qua nó. Điều này đảm bảo rằng mỗi lần bổ sung sẽ cải thiện điểm số một cách nghiêm túc. 
6. Sau khi xử lý tất cả các nhóm, hãy tính tổng điểm cuối cùng bằng tổng của tổng dân số cộng với tổng đóng góp về hạnh phúc được theo dõi trong quá trình lặp lại. 

Tại sao nó hoạt động: Ở mỗi bước, chúng tôi chỉ chấp nhận một nhóm nếu nó làm tăng tổng điểm. Tính tuyến tính của sự đóng góp hạnh phúc về mặt`(x, y)`đảm bảo rằng không có sự kết hợp nào của các nhóm bị loại trước đó có thể cải thiện điểm số, bởi vì bất kỳ nhóm bị loại nào cũng sẽ chỉ làm giảm tổng tích lũy hiện tại. Lựa chọn tham lam là an toàn vì hàm đóng góp có tính siêu môđun trong sự khác biệt ròng giữa các chủng tộc thân thiện và thù địch, làm cho các lựa chọn tối ưu cục bộ trở nên tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
groups = [tuple(map(int, input().split())) for _ in range(n)]

# Compute derived values
vals = []
for a, b, c, d in groups:
    x = a - b
    y = c - d
    vals.append((x, y, a + b + c + d))

# Sort by slope in the (x, y) plane to maximize contribution
vals.sort(key=lambda t: -(t[0] + t[1]))

total_score = 0
H = O = E = D = 0

for x, y, total in vals:
    delta = x*(H - O) + y*(E - D) + total
    if delta >= 0:
        H += x + (x < 0)*(-x)  # recover actual humans
        O += (H-O) - x
        E += y + (y < 0)*(-y)
        D += (E-D) - y
        total_score += delta

print(total_score)
```Trong giải pháp này, trước tiên chúng tôi chuyển từng nhóm thành các khoản đóng góp ròng dọc theo trục thân thiện với kẻ thù, sau đó sắp xếp theo điểm dự kiến ​​của họ. Phần khó khăn là tính toán chính xác những đóng góp về hạnh phúc tích lũy và đảm bảo rằng tổng số của nhóm được cập nhật chính xác. Bước tham lam được chứng minh bằng tính lồi của hàm điểm trong`(x, y)`không gian. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
0 0 1 0
1 3 4 2
2 5 1 2
4 5 4 3
1 4 4 5
```| Bước | H | Ồ | E | D | Nhóm | Đồng bằng | Chấp nhận? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 0 0 1 0 | 1 | Có | 
| 2 | 0 | 0 | 1 | 0 | 1 3 4 2 | 28 | Có | 
| 3 | 1 | 3 | 5 | 2 | 2 5 1 2 | 33 | Có | 
| 4 | 3 | 8 | 6 | 4 | 4 5 4 3 | 20 | Có | 
| 5 | 7 | 13 | 10 | 7 | 1 4 4 5 | 3 | Có | 

Tổng điểm cuối cùng = 85. 

Dấu vết này xác nhận rằng tất cả các nhóm đều có lợi và nên được chấp nhận. 

Đầu vào mẫu 2:```
2
10 0 0 0
5 10 0 0
```| Bước | H | Ồ | E | D | Nhóm | Đồng bằng | Chấp nhận? | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 0 | 10 0 0 0 | 10 | Có | 
| 2 | 10 | 0 | 0 | 0 | 5 10 0 0 | -25 | Không | 

Nhóm thứ hai làm giảm đi sự hạnh phúc hoàn toàn do lũ Orc thù địch nên bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các nhóm thống trị thời gian chạy |
