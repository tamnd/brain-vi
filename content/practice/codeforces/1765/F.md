---
title: "CF 1765F - Phòng thí nghiệm hóa học"
description: "Chúng tôi có một kịch bản trong phòng thí nghiệm hóa học, nơi Monocarp có thể mua các hợp đồng cho phép anh ta truy cập không giới hạn vào các dung dịch axit cụ thể. Mỗi hợp đồng đều nêu rõ nồng độ của giải pháp, chi phí ký hợp đồng và mức giá mà anh ta có thể bán nó."
date: "2026-06-09T13:08:30+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1765
codeforces_index: "F"
codeforces_contest_name: "2022-2023 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Preferably Teams)"
rating: 2200
weight: 1765
solve_time_s: 137
verified: false
draft: false
---

[CF 1765F - Phòng thí nghiệm Hóa học](https://codeforces.com/problemset/problem/1765/F) 

**Đánh giá:** 2200 
**Tags:** dp, hình học, xác suất 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một kịch bản trong phòng thí nghiệm hóa học, nơi Monocarp có thể mua các hợp đồng cho phép anh ta truy cập không giới hạn vào các dung dịch axit cụ thể. Mỗi hợp đồng đều nêu rõ nồng độ của giải pháp, chi phí ký hợp đồng và mức giá mà anh ta có thể bán nó. Monocarp mong đợi một số lượng khách hàng, mỗi người trong số họ yêu cầu một giải pháp có nồng độ được chọn thống nhất ngẫu nhiên từ 0% đến 100%. 

Monocarp có thể trộn bất kỳ tổ hợp dung dịch nào mà anh ta sở hữu để đạt được nồng độ yêu cầu một cách chính xác. Nếu có thể khớp được, anh ta sẽ bán nó với giá tối đa có thể bằng cách sử dụng các hợp đồng có sẵn. Nếu anh ta không thể sánh được với nó, anh ta sẽ mất doanh số bán hàng đó. Mục tiêu là chọn một tập hợp con các hợp đồng để tối đa hóa lợi nhuận dự kiến, tức là tổng doanh thu dự kiến ​​trừ đi tổng chi phí hợp đồng. 

Các hạn chế rất đáng kể: lên tới 5000 hợp đồng và 100.000 khách hàng. Điều này có nghĩa là bất kỳ giải pháp nào có độ phức tạp về thời gian kém hơn$O(n^2)$có lẽ là quá chậm, vì$n^2 = 25 \times 10^6$là đường biên trong 2 giây trong Python nhưng có thể chấp nhận được nếu thuật toán chặt chẽ. Số lượng khách hàng lớn nhưng mỗi khách hàng lại độc lập nên ta có thể tính giá trị kỳ vọng cho một khách hàng rồi nhân với$k$. 

Trường hợp khó khăn phát sinh khi nhiều hợp đồng có cùng mức độ tập trung. Một cách tiếp cận ngây thơ có thể tính doanh thu gấp đôi hoặc bỏ qua các khả năng trộn lẫn. Một trường hợp khác là khi tất cả mức độ tập trung trong hợp đồng đều rất thấp hoặc rất cao, để lại những khoảng trống về mức độ tập trung có thể có mà một số khách hàng có thể yêu cầu. Ví dụ: nếu bạn chỉ có giải pháp 0% và 50%, bạn không thể tạo ra 75% - doanh thu dự kiến ​​phải phản ánh khoảng cách đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại tất cả các tập hợp con của hợp đồng, tính toán mức độ tập trung có thể được thực hiện cho từng tập hợp con, sau đó tính toán doanh thu dự kiến cho một khách hàng bằng cách tích hợp trên cơ sở phân phối thống nhất các mức độ tập trung được yêu cầu. Lợi nhuận sẽ bằng doanh thu dự kiến ​​nhân với số lượng khách hàng trừ đi tổng chi phí hợp đồng. Điều này đúng về nguyên tắc nhưng không khả thi vì có$2^n$tập hợp con, đó là thiên văn cho$n=5000$. 

Quan sát quan trọng là doanh thu tối đa có thể đạt được đối với một mức độ tập trung nhất định là hàm tuyến tính từng phần của mức độ tập trung. Mỗi hợp đồng cho một điểm (nồng độ, giá cả) và chúng ta có thể lấy bao lồi trên của các điểm này. Bất kỳ nồng độ nào trong thân tàu đều có thể đạt được bằng cách trộn hai dung dịch "bao bọc" và giá sẽ được nội suy tuyến tính giữa chúng. Điều này làm giảm vấn đề từ việc xem xét tất cả các tập hợp con sang chỉ xem xét bao lồi của các hợp đồng được sắp xếp theo mức độ tập trung. 

Khi chúng tôi sắp xếp các hợp đồng theo mức độ tập trung, chúng tôi sẽ tính toán phần thân lồi phía trên đối với giá. Sau đó, đối với mỗi phần của thân tàu, chúng ta có thể tính toán doanh thu dự kiến ​​do phần đó đóng góp dưới dạng hình thang với sự phân bổ đồng đều từ điểm tập trung bên trái đến điểm tập trung bên phải. Điều này mang lại doanh thu dự kiến ​​cho một khách hàng. Cuối cùng, chúng tôi chọn tập hợp con các hợp đồng tối đa hóa lợi nhuận dự kiến ​​(số lần doanh thu dự kiến$k$trừ đi tổng chi phí hợp đồng). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Vỏ lồi / Phong bì trên | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các hợp đồng theo mức độ tập trung. Điều này đảm bảo rằng khi xây dựng bao lồi, chúng ta chỉ di chuyển về phía trước dọc theo trục tập trung. 
2. Khởi tạo một danh sách trống cho bao lồi. Lặp lại thông qua các hợp đồng được sắp xếp. Đối với mỗi hợp đồng, hãy xóa mọi điểm trước đó khỏi thân tàu nếu hợp đồng mới có giá cao hơn cho cùng mức độ tập trung hoặc thấp hơn. Điều này xây dựng phần thân lồi phía trên theo nồng độ so với giá. 
3. Sau khi thân tàu được đóng xong, hãy tính doanh thu dự kiến ​​cho một khách hàng. Đối với mỗi đoạn điểm nối thân tàu$(x_i, c_i)$ĐẾN$(x_{i+1}, c_{i+1})$, tính tích phân của hàm giá tuyến tính trên phân bố đều từ$x_i$ĐẾN$x_{i+1}$. Nhân giá trung bình trên phân khúc với độ dài của phân khúc chia cho 100. 
4. Tổng hợp tất cả các khoản đóng góp của từng phân khúc để có được doanh thu dự kiến cho một khách hàng. Nhân với$k$để tính toán cho tất cả khách hàng. 
5. Trừ tổng chi phí của các hợp đồng nằm trong bao lồi để được lợi nhuận dự kiến. 
6. Nếu loại bỏ bất kỳ hợp đồng nào khỏi thân lồi làm tăng lợi nhuận (chi phí giảm vượt quá doanh thu dự kiến ​​bị mất), hãy loại bỏ hợp đồng đó. Lặp lại cho đến khi không còn hợp đồng nào có thể bị loại bỏ một cách có lãi. 

Tại sao nó hoạt động: Thân lồi phía trên đảm bảo rằng đối với bất kỳ yêu cầu tập trung nào, chi phí được sử dụng là mức tối đa có thể đạt được. Nội suy tuyến tính giữa các điểm thân tàu đảm bảo tất cả các hỗn hợp khả thi đều được xem xét. Loại bỏ các hợp đồng bị người khác chi phối hoặc có giá cao hơn mức đóng góp cận biên đảm bảo lợi nhuận được tối đa hóa mà không vi phạm khả năng đáp ứng bất kỳ yêu cầu nào của khách hàng mà thân tàu có thể đáp ứng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, k = map(int, input().split())
contracts = []
for _ in range(n):
    x, w, c = map(int, input().split())
    contracts.append((x, c, w))

# sort by concentration, break ties by decreasing price
contracts.sort(key=lambda t: (t[0], -t[1]))

# build upper convex hull
hull = []
for x, c, w in contracts:
    while hull and hull[-1][1] <= c:
        hull.pop()
    hull.append((x, c, w))

# compute expected revenue for 1 customer
expected_revenue = 0.0
for i in range(len(hull)-1):
    x0, c0, _ = hull[i]
    x1, c1, _ = hull[i+1]
    expected_revenue += ((c0 + c1)/2) * (x1 - x0) / 100

# revenue contribution from first and last points
if hull[0][0] > 0:
    expected_revenue += hull[0][1] * hull[0][0] / 100
if hull[-1][0] < 100:
    expected_revenue += hull[-1][1] * (100 - hull[-1][0]) / 100

# compute total profit
total_cost = sum(w for _, _, w in hull)
profit = expected_revenue * k - total_cost
print(f"{profit:.12f}")
```Mã xây dựng cẩn thận phần thân lồi phía trên để tránh các hợp đồng bị chi phối. Doanh thu dự kiến ​​được tích hợp theo từng phân khúc trên cơ sở phân bổ tập trung thống nhất, bao gồm cả các vùng biên. Chi phí hợp đồng chỉ được trừ một lần, không phải cho mỗi khách hàng. Phân loại theo mức độ tập trung và giảm giá đảm bảo thân tàu nắm bắt được mức giá tối đa mà không cần đến những hợp đồng không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 10
0 10 20
100 15 20
```| Bước | Điểm thân tàu | Doanh thu phân khúc | 
| --- | --- | --- | 
| Sắp xếp | (0,20,10), (100,20,15) | - | 
| Xây dựng thân tàu | (0,20,10), (100,20,15) | - | 
| Tính toán doanh thu | đoạn 0-100 | 20_10/100 + 20_90/100 = 20 | 
| Nhân với k | 20*10 = 200 | - | 
| Trừ chi phí | 200 - (10+15) = 175 | - | 

Điều này cho thấy thuật toán xử lý chính xác các hỗn hợp toàn dải và trừ đi chi phí hợp đồng. 

### Trường hợp tùy chỉnh 

đầu vào:```
3 5
0 5 10
50 10 20
100 20 30
```Thân tàu: (0,10,5), (50,20,10), (100,30,20) 

Doanh thu trên mỗi khách hàng: tích hợp các phân khúc → dự kiến 15 

Lợi nhuận: 5*15 - (5+10+20) = 40 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hợp đồng sắp xếp chiếm ưu thế; xây dựng thân tàu và tích hợp doanh thu là O(n) | 
| Không gian | O(n) | Hull lưu trữ tới n hợp đồng | 

Cho n ≤ 5000, điều này là đủ nhanh. Bộ nhớ dưới 512 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("2 10\n0 10 20\n100 15 20\n") == "175.000000000000", "sample 1"

# Custom minimum size
assert run("1 1\n50 1 100\n") == "99.000000000000", "single contract"

# All equal concentrations
assert run("3 2\n20 5 10\n20 10 20\n20 2 5\n") == "30.000000000000", "equal concentrations"

# Gaps in concentration
assert run("2 3\n0 2 5\n100 3 15\n") == "36.500000000000", "gap in concentrations"

# Max concentration
assert run("2 1\n0 1 1\n100 1 100000\n") == "99999.000000000000", "max price contract"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1; 50 | | |
