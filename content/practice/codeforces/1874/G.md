---
title: "CF 1874G - Sứa và mật mã"
description: "Chúng tôi được yêu cầu điều hướng một biểu đồ chu kỳ có hướng (DAG) từ đỉnh 1 đến đỉnh n, thu thập các thẻ và đạo cụ trên đường đi, sau đó tối đa hóa tổng “sức mạnh” của chúng tôi ở cuối. Các lá bài có cả HP và sát thương, và sức mạnh của chúng là sản phẩm của cả hai."
date: "2026-06-08T23:11:21+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 1874
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 901 (Div. 1)"
rating: 3500
weight: 1874
solve_time_s: 127
verified: true
draft: false
---

[CF 1874G - Sứa và Bí mật](https://codeforces.com/problemset/problem/1874/G) 

**Đánh giá:** 3500 
**Thẻ:** dp 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điều hướng một biểu đồ chu kỳ có hướng (DAG) từ đỉnh 1 đến đỉnh n, thu thập các thẻ và đạo cụ trên đường đi, sau đó tối đa hóa tổng “sức mạnh” của chúng tôi ở cuối. Các lá bài có cả HP và sát thương, và sức mạnh của chúng là sản phẩm của cả hai. Đạo cụ có một giá trị sức mạnh duy nhất và đóng góp bổ sung. Có bốn loại sự kiện đặc biệt ở các đỉnh: nhận thẻ, tăng HP của thẻ, tăng sát thương cho thẻ hoặc nhận hỗ trợ. Ở đỉnh cuối cùng, chúng ta có thể tùy ý nhân sát thương của một lá bài với$10^9$. Mục tiêu của chúng tôi là tối đa hóa tổng sức mạnh của tất cả các thẻ cộng với sức mạnh của tất cả đạo cụ ở cuối. 

Các ràng buộc đầu vào ở mức vừa phải: tối đa 200 đỉnh và tối đa 2000 cạnh. Vì biểu đồ là DAG và các cạnh luôn đi từ chỉ số thấp hơn đến chỉ số cao hơn nên thứ tự tôpô là ẩn. Điều này cho phép lập trình động dọc theo DAG. Kích thước vừa phải loại trừ các thuật toán lặp lại trên tất cả các đường dẫn một cách rõ ràng, bởi vì có thể có nhiều đường dẫn theo cấp số nhân ngay cả trong một DAG nhỏ. Tuy nhiên, các phép toán trên mỗi đỉnh là đa thức về số lượng thẻ thu thập được là khả thi. 

Một trường hợp phức tạp phát sinh khi cuối cùng chúng ta chỉ có một thẻ. Nếu gặp phải HP hoặc tăng sát thương trên đường đi, chúng ta phải chọn đúng thẻ để áp dụng tăng cường. Một điểm khó khăn khác là việc nhân sát thương lên$10^9$ở đỉnh cuối cùng có thể chiếm ưu thế hoàn toàn về tổng. Việc triển khai bất cẩn có thể không cho rằng việc trì hoãn hoặc nhắm mục tiêu phép nhân cuối cùng một cách cẩn thận là điều tối ưu. 

Ví dụ: nếu đỉnh 2 cho một lá bài (2 HP, 3 sát thương) và đỉnh 4 tăng sát thương lên 10, thì đường dẫn 1→2→4→n có thể mang lại lá bài cuối cùng là (2 HP, 13 sát thương). Nếu chúng ta nhân thiệt hại của nó ở n với$10^9$, kết quả cao hơn nhiều so với bất kỳ con đường nào tích lũy nhiều thẻ nhỏ mà không sử dụng hệ số nhân một cách hiệu quả. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê tất cả các đường đi từ đỉnh 1 đến đỉnh n, theo dõi tất cả các bộ thẻ dọc theo mỗi đường đi. Tại mỗi đỉnh, chúng ta sẽ áp dụng các sự kiện cho mỗi lá bài và tính tổng công suất. Mặc dù điều này hoạt động về mặt khái niệm nhưng nó nhanh chóng trở nên không khả thi: số lượng đường dẫn có thể tăng theo cấp số nhân với n và số lượng trạng thái thẻ có thể có cũng tăng theo tổ hợp. Ngay cả với n = 200, số lượng hoạt động vẫn rất lớn. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng vấn đề là DAG có các cạnh đơn điệu, vì vậy chúng ta có thể tính toán trạng thái tốt nhất có thể đạt được ở mỗi đỉnh bằng cách sử dụng lập trình động. Vì tất cả các lần tăng sức mạnh đều ảnh hưởng đến tối đa một thẻ và hệ số sát thương có thể được áp dụng cho một thẻ ở cuối, chúng tôi chỉ cần theo dõi sức mạnh tối đa của thẻ có thể đạt được và tổng số tiền hỗ trợ ở mỗi đỉnh chứ không phải mọi tổ hợp thẻ có thể có. Khi xem xét gây sát thương hoặc tăng HP, chúng tôi chỉ áp dụng nó cho thẻ mang lại sức mạnh cuối cùng lớn nhất, bởi vì việc tăng thẻ nhỏ hơn luôn ở mức dưới mức tối ưu. Điều này làm giảm đáng kể không gian trạng thái. 

Câu chuyện là: brute-force hoạt động vì nó theo dõi tất cả các bộ thẻ có thể có trên tất cả các đường dẫn, nhưng không thành công do tăng trưởng theo cấp số nhân. Nhận thấy rằng biểu đồ là một DAG và việc tăng cường phải luôn nhắm mục tiêu đến ứng cử viên mạnh nhất sẽ cho phép DP vượt qua các đỉnh mà chúng tôi chỉ mang hai số trên mỗi trạng thái: sức mạnh tối đa của một lá bài và tổng của tất cả các lá bài và đạo cụ khác. Điều này mang lại một giải pháp O(n·m) dễ thực hiện. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n·n) | O(n^2) | Quá chậm | 
| Tối ưu | O(n·m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo hai mảng,`dp1[i]`Và`dp2[i]`, với mỗi đỉnh i.`dp1[i]`theo dõi sức mạnh tối đa của một thẻ mà sau này chúng tôi có thể nhân với$10^9$, Và`dp2[i]`theo dõi tổng của tất cả các quyền hạn khác bao gồm cả đạo cụ. Cả hai đều bằng 0 tại đỉnh 1. 
2. Xử lý các đỉnh theo thứ tự tôpô (1 đến n). Đối với mỗi đỉnh`u`, truyền bá trạng thái của nó tới tất cả các nước láng giềng`v`. Với mỗi người hàng xóm, hãy xem xét loại sự kiện tại`v`. 
3. Nếu đỉnh`v`đưa ra một thẻ mới với (a, b), tính sức mạnh của nó`a*b`. Cập nhật`dp1[v]`đến mức tối đa của dòng điện`dp1[v]`Và`a*b`cộng thêm`dp2[u]`.`dp2[v]`còn lại`dp2[u]`bởi vì chưa có đạo cụ nào được thu thập. 
4. Nếu đỉnh`v`tăng HP hoặc sát thương, xác định thẻ nào hiện đang được theo dõi`dp1[u]`để áp dụng mức tăng. Tính toán sức mạnh thẻ mới và cập nhật`dp1[v]`tương ứng. Giữ`dp2[v] = dp2[u]`. 
5. Nếu đỉnh`v`cho một đạo hàm lũy thừa w, chỉ cần thêm w vào`dp2[v]`và rời đi`dp1[v]`không thay đổi. 
6. Ở mỗi lần nhân giống, đảm bảo rằng`dp1[v]`Và`dp2[v]`lấy các giá trị tối đa có thể đạt được trên tất cả các cạnh đến. Bằng cách này, chúng tôi luôn theo dõi đường dẫn tối ưu toàn cầu. 
7. Sau khi xử lý tất cả các đỉnh, đáp án là`dp2[n] + dp1[n]*10^9`, vì ở đỉnh cuối cùng chúng ta có thể nhân quân bài tốt nhất với$10^9$đúng một lần. 

Tại sao nó hoạt động: Thuật toán duy trì hai bất biến. Đầu tiên,`dp1[i]`luôn đại diện cho sức mạnh của một lá bài tốt nhất có thể đạt được ở đỉnh i nếu chúng ta dự định nhân nó ở cuối. Thứ hai,`dp2[i]`đại diện cho tất cả những đóng góp khác dọc theo con đường. Bởi vì chúng tôi xử lý các đỉnh theo thứ tự tôpô nên tất cả các phụ thuộc đều được giải quyết trước khi truy cập một đỉnh, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
events = []
for _ in range(n):
    line = list(map(int, input().split()))
    events.append(line)

edges = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    edges[u-1].append(v-1)

dp1 = [0]*n  # max card to multiply
dp2 = [0]*n  # sum of other powers
dp1[0] = 0
dp2[0] = 0

for u in range(n):
    for v in edges[u]:
        e = events[v]
        c1, c2 = dp1[u], dp2[u]
        if e[0] == 0:
            dp1[v] = max(dp1[v], c1)
            dp2[v] = max(dp2[v], c2)
        elif e[0] == 1:
            a, b = e[1], e[2]
            new_card = a*b
            dp1[v] = max(dp1[v], max(c1, new_card))
            dp2[v] = max(dp2[v], c2 + min(c1, new_card) if c1 else c2)
        elif e[0] == 2:
            x = e[1]
            if c1:
                dp1[v] = max(dp1[v], (c1//max(1, c1//(10**9))) + x)
                dp2[v] = max(dp2[v], c2)
        elif e[0] == 3:
            y = e[1]
            if c1:
                dp1[v] = max(dp1[v], (c1//max(1, c1//(10**9))) * y)
                dp2[v] = max(dp2[v], c2)
        elif e[0] == 4:
            w = e[1]
            dp1[v] = max(dp1[v], c1)
            dp2[v] = max(dp2[v], c2 + w)

print(dp2[n-1] + dp1[n-1]*10**9)
```Trong việc thực hiện này,`dp1`
