---
title: "CF 1650C - Trọng lượng của hệ thống các phân đoạn lồng nhau"
description: "Chúng ta được cho điểm trên trục số. Mỗi điểm đều có tọa độ và trọng số. Chúng tôi muốn chọn chính xác $2n$ trong số các điểm này và sử dụng chúng làm điểm cuối của phân đoạn $n$. Các phân đoạn phải tạo thành một cấu trúc lồng nhau hoàn hảo."
date: "2026-06-10T03:53:06+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "hashing", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1650
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 776 (Div. 3)"
rating: 1200
weight: 1650
solve_time_s: 155
verified: false
draft: false
---

[CF 1650C - Trọng lượng của hệ thống các phân đoạn lồng nhau](https://codeforces.com/problemset/problem/1650/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, băm, triển khai, sắp xếp 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho điểm trên trục số. Mỗi điểm đều có tọa độ và trọng số. Chúng tôi muốn chọn chính xác$2n$của những điểm này và sử dụng chúng làm điểm cuối của$n$phân đoạn. 

Các phân đoạn phải tạo thành một cấu trúc lồng nhau hoàn hảo. Nếu chúng ta sắp xếp các phân đoạn từ ngoài cùng đến trong cùng thì mọi phân đoạn phải nằm hoàn toàn bên trong phân đoạn trước đó. Về mặt hình học, các đoạn trông giống như một chuỗi các dấu ngoặc đơn phù hợp trên trục số. 

Mục tiêu không phải là giảm thiểu độ dài đoạn. Điều duy nhất quan trọng là tổng trọng số của tất cả các điểm cuối được sử dụng trong quá trình xây dựng. 

Đầu vào cung cấp$m$điểm ứng viên. Mỗi điểm có một tọa độ duy nhất và một chỉ mục gốc. Đầu ra phải chứa tổng trọng lượng tối thiểu có thể và một tập hợp hợp lệ các$n$các phân đoạn lồng nhau đạt được trọng số đó. 

Các ràng buộc ngay lập tức gợi ý rằng chúng ta không thể kiểm tra tất cả các tập hợp con của điểm. Một trường hợp thử nghiệm có thể chứa tới$2 \cdot 10^5$điểm và tổng số điểm trên tất cả các trường hợp thử nghiệm cũng là$2 \cdot 10^5$. Bất kỳ thuật toán nào tệ hơn đại khái$O(m \log m)$mỗi trường hợp thử nghiệm khó có thể phù hợp thoải mái trong thời gian giới hạn. Phương pháp bậc hai hoàn toàn bị loại trừ vì$m^2$có thể đạt được$4 \cdot 10^{10}$. 

Một khía cạnh tinh vi của vấn đề là điều kiện lồng nhau nghe có vẻ hạn chế, nhưng một khi điều kiện đúng$2n$điểm được chọn, việc xây dựng các phân đoạn lồng nhau trở nên đơn giản. 

Hãy xem xét ví dụ này:```
n = 2

coordinate weight
0          1
10         1
20         100
30         100
```Giải pháp tối ưu chỉ sử dụng các điểm ở tọa độ 0 và 10 cùng với hai điểm đắt tiền nếu cần thiết. Vì cần có chính xác bốn điểm cuối nên tất cả các điểm phải được chọn. Một giải pháp bất cẩn cố gắng ghép nối các điểm lân cận một cách tham lam có thể tạo ra các đoạn giao nhau thay vì các đoạn lồng nhau. 

Một sai lầm dễ mắc phải khác là cho rằng độ dài phân đoạn là quan trọng. Ví dụ:```
n = 1

coordinate weight
0          1000
1000000    1000
5          -5
10         -5
```Câu trả lời tối ưu sử dụng tọa độ 5 và 10 mặc dù khoảng cách rất nhỏ của chúng không liên quan. Mục tiêu chỉ phụ thuộc vào trọng lượng. 

Một điều đáng tiếc nữa là quên rằng trọng số âm được cho phép:```
n = 1

coordinate weight
0   -100
1   50
2   -50
```Cặp tốt nhất là hai điểm trọng số âm. Bất kỳ phương pháp phỏng đoán nào dựa trên vị trí tọa độ thay vì trọng lượng đều thất bại ở đây. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con của$2n$điểm, kiểm tra xem các điểm đó có thể tạo thành một hệ thống lồng nhau hay không và tính tổng trọng số. Điều này đúng vì nó tìm kiếm rõ ràng toàn bộ không gian giải pháp. 

Vấn đề là kích thước của không gian đó. Số tập con là$$\binom{m}{2n},$$lớn về mặt thiên văn ngay cả đối với các giá trị khiêm tốn của$m$. Không có mức độ tối ưu hóa nào có thể làm cho điều này trở nên khả thi. 

Quan sát quan trọng là yêu cầu lồng nhau không ảnh hưởng đến điểm nào sẽ được chọn. 

Giả sử chúng ta đã chọn một số tập hợp$2n$điểm. Sắp xếp chúng theo tọa độ:$$p_1, p_2, \dots, p_{2n}.$$Bây giờ ghép điểm ngoài cùng bên trái với điểm ngoài cùng bên phải, điểm ngoài cùng bên trái thứ hai với điểm ngoài cùng bên phải thứ hai, v.v.:$$(p_1,p_{2n}), (p_2,p_{2n-1}), \dots$$Các phân đoạn này được tự động lồng vào nhau. Mỗi bộ được chọn$2n$điểm có thể được chuyển đổi thành một hệ thống lồng nhau hợp lệ. 

Điều đó thay đổi toàn bộ vấn đề. Thay vì lo lắng về cấu trúc phân khúc, chúng ta chỉ cần chọn$2n$điểm có tổng trọng số nhỏ nhất. 

Tập hợp con có trọng số tối thiểu của kích thước$2n$rõ ràng là có được bằng cách sắp xếp các điểm theo trọng lượng và lấy phần nhẹ nhất$2n$. 

Sau khi chọn các điểm đó, chúng tôi sắp xếp chúng theo tọa độ và xây dựng các cặp lồng nhau bằng cách sử dụng cặp đối xứng được mô tả ở trên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(m \log m)$|$O(m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm cùng với chỉ số ban đầu của chúng. 
2. Sắp xếp điểm theo trọng số. 
3. Lấy cái đầu tiên$2n$điểm từ thứ tự này. 

Đây chính xác là những$2n$điểm có tổng trọng lượng tối thiểu. 
4. Tính tổng trọng lượng của chúng. 
5. Sắp xếp đã chọn$2n$điểm theo tọa độ. 
6. Ghép điểm được chọn ngoài cùng bên trái với điểm được chọn ngoài cùng bên phải. 
7. Ghép điểm được chọn ngoài cùng bên trái thứ hai với điểm được chọn ngoài cùng bên phải thứ hai. 
8. Tiếp tục cho đến khi$n$cặp đã được sản xuất. 
9. Xuất ra tổng trọng số và tất cả các cặp được xây dựng. 

### Tại sao nó hoạt động 

Bằng chứng bao gồm hai sự kiện độc lập. 

Đầu tiên, mọi giải pháp đều sử dụng chính xác$2n$điểm cuối. Vì mục tiêu là tổng trọng số điểm cuối nên tổng trọng số tối thiểu có thể đạt được bằng cách chọn$2n$trọng số nhỏ nhất trong số tất cả các điểm có sẵn. Bất kỳ lựa chọn nào khác sẽ thay thế một điểm đã chọn bằng một điểm nặng hơn và không thể cải thiện tổng. 

Thứ hai, sau khi chọn bất kỳ$2n$các điểm, sắp xếp chúng theo tọa độ và ghép các vị trí đối xứng sẽ tạo ra các đoạn lồng nhau. Nếu các điểm được sắp xếp là$$p_1,p_2,\dots,p_{2n},$$sau đó các cặp là$$(p_1,p_{2n}), (p_2,p_{2n-1}), \dots$$Với mỗi cặp phân đoạn liên tiếp,$$p_i < p_{i+1} < p_{2n-i} < p_{2n-i+1},$$đó chính xác là điều kiện lồng nhau. 

Vì chúng tôi đạt được trọng số điểm cuối tối thiểu có thể và luôn xây dựng một hệ thống lồng nhau hợp lệ nên thuật toán là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        input()  # consume blank line

        n, m = map(int, input().split())

        points = []
        for idx in range(1, m + 1):
            x, w = map(int, input().split())
            points.append((w, x, idx))

        points.sort()  # sort by weight

        chosen = points[:2 * n]
        total_weight = sum(w for w, _, _ in chosen)

        chosen.sort(key=lambda p: p[1])  # sort by coordinate

        pairs = []
        l = 0
        r = 2 * n - 1

        while l < r:
            pairs.append((chosen[l][2], chosen[r][2]))
            l += 1
            r -= 1

        out.append(str(total_weight))
        for a, b in pairs:
            out.append(f"{a} {b}")
        out.append("")

    sys.stdout.write("\n".join(out))

solve()
```Loại đầu tiên tìm thấy giá rẻ nhất trên toàn cầu$2n$điểm cuối. Đây là bước tối ưu hóa. 

Loại thứ hai bỏ qua trọng số và sắp xếp các điểm đã chọn theo tọa độ. Khi các điểm đã theo thứ tự tọa độ, việc ghép nối đối xứng sẽ tự động tạo ra sự lồng nhau. 

Các chỉ số ban đầu phải được giữ nguyên vì đầu ra yêu cầu số điểm từ đầu vào chứ không phải tọa độ. 

Dòng trống trước mỗi test case rất dễ bị xử lý sai. Đầu vào chính thức luôn chứa nó, vì vậy việc đọc và loại bỏ một dòng trước khi xử lý từng trường hợp kiểm thử là đủ. 

Số nguyên Python dễ dàng xử lý tổng trọng số vì cường độ tối đa có thể thấp hơn nhiều so với giới hạn số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1, Trường hợp thử nghiệm đầu tiên 

Điểm được chọn sau khi sắp xếp theo trọng số: 

| Tọa độ | Cân nặng | Chỉ mục | 
| --- | --- | --- | 
| 5 | -2 | 8 | 
| 7 | -1 | 5 | 
| -2 | 1 | 2 | 
| 9 | 1 | 6 | 
| 2 | 3 | 7 | 
| 0 | 10 | 1 | 

Tổng trọng lượng:$$-2 + (-1) + 1 + 1 + 3 + 10 = 12$$Sắp xếp các điểm đã chọn này theo tọa độ: 

| Vị trí | Tọa độ | Chỉ mục | 
| --- | --- | --- | 
| 1 | -2 | 2 | 
| 2 | 0 | 1 | 
| 3 | 2 | 7 | 
| 4 | 5 | 8 | 
| 5 | 7 | 5 | 
| 6 | 9 | 6 | 

Xây dựng cặp: 

| Vị Trí Bên Trái | Đúng Vị Trí | Cặp | 
| --- | --- | --- | 
| 1 | 6 | (2, 6) | 
| 2 | 5 | (1, 5) | 
| 3 | 4 | (7, 8) | 

Chúng tương ứng với các phân đoạn lồng nhau:```
[-2, 9]
[0, 7]
[2, 5]
```### Mẫu 1, Trường hợp thử nghiệm thứ ba 

Điểm đầu vào: 

| Tọa độ | Cân nặng | Chỉ mục | 
| --- | --- | --- | 
| 5 | -1 | 1 | 
| 3 | -2 | 2 | 
| 1 | 0 | 3 | 
| -2 | 0 | 4 | 
| -5 | -3 | 5 | 

Chọn 4 trọng lượng nhỏ nhất: 

| Tọa độ | Cân nặng | Chỉ mục | 
| --- | --- | --- | 
| -5 | -3 | 5 | 
| 3 | -2 | 2 | 
| 5 | -1 | 1 | 
| -2 | 0 | 4 | 

Tổng cộng:$$-3-2-1+0=-6$$Sắp xếp theo tọa độ: 

| Vị trí | Tọa độ | Chỉ mục | 
| --- | --- | --- | 
| 1 | -5 | 5 | 
| 2 | -2 | 4 | 
| 3 | 3 | 2 | 
| 4 | 5 | 1 | 

Ghép đôi đối xứng: 

| Cặp | 
| --- | 
| (5,1) | 
| (4,2) | 

Các phân đoạn kết quả được lồng vào nhau và đạt được trọng lượng tối thiểu có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(m \log m)$| Hai thao tác sắp xếp chi phối thời gian chạy | 
| Không gian |$O(m)$| Lưu trữ tất cả các điểm và điểm đã chọn | 

Tổng số điểm trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, Vì thế$O(m \log m)$dễ dàng phù hợp trong giới hạn 2 giây. Việc sử dụng bộ nhớ là tuyến tính ở kích thước đầu vào và vừa vặn trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```
# These tests validate the algorithmic idea.
# Exact pair ordering may vary, so real judging uses
# checker validation rather than string equality.

# minimum size
inp = """1

1 2
0 5
10 1
"""
# answer weight = 6

# negative weights
inp = """1

1 3
0 -10
5 100
10 -5
"""
# answer weight = -15

# exactly 2n points
inp = """1

2 4
0 1
1 2
2 3
3 4
"""
# answer weight = 10

# coordinates unsorted, weights unsorted
inp = """1

2 5
100 7
-5 1
50 2
0 3
10 4
"""
# answer weight = 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Vỏ kích thước tối thiểu | Phân khúc đơn | Đầu vào pháp lý nhỏ nhất | 
| Trọng số âm | Sử dụng điểm cuối tiêu cực | Chỉ giảm thiểu trọng lượng | 
| Chính xác$2n$điểm | Tất cả các điểm được chọn | Không có quyền tự do lựa chọn điểm cuối | 
| Tọa độ chưa sắp xếp | Việc lồng đúng sau khi sắp xếp tọa độ | Logic xây dựng cặp | 

## Vỏ cạnh 

Hãy xem xét:```
n = 1
m = 3

0  -100
1   50
2  -50
```Thuật toán sắp xếp theo trọng số và chọn hai điểm âm. Tổng trọng lượng của chúng là$-150$, rõ ràng là tối ưu. Các vị trí tọa độ không liên quan. 

Coi như:```
n = 2
m = 4

0 1
10 2
20 3
30 4
```Tất cả các điểm phải được chọn. Sau khi sắp xếp theo tọa độ, thuật toán ghép cặp$(0,30)$Và$(10,20)$. Phân đoạn thứ hai nằm hoàn toàn bên trong phân đoạn thứ nhất, do đó điều kiện lồng nhau được giữ tự động. 

Coi như:```
n = 2
m = 5

0 100
1 1
2 2
3 3
4 4
```Thuật toán loại bỏ điểm đắt nhất có trọng số 100 và giữ lại 4 điểm rẻ nhất. Bất kỳ giải pháp nào sử dụng điểm bị loại bỏ sẽ có tổng trọng lượng lớn hơn. Sau khi sắp xếp tọa độ, ghép nối đối xứng lại tạo ra cấu trúc lồng nhau hợp lệ. 

Những ví dụ này nắm bắt được hai ý tưởng quan trọng: việc tối ưu hóa chỉ phụ thuộc vào trọng số và việc lồng ghép chỉ phụ thuộc vào thứ tự tọa độ sau khi các điểm đã được chọn.
