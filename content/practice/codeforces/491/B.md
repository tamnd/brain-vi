---
title: "CF 491B - Khách sạn New York"
description: "Thành phố là một mạng lưới hình chữ nhật. Mọi khách sạn, nhà hàng đều nằm ở giao lộ có tọa độ $(x,y)$."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 491
codeforces_index: "B"
codeforces_contest_name: "Testing Round 11"
rating: 2100
weight: 491
solve_time_s: 700
verified: false
draft: false
---

[CF 491B - Khách sạn New York](https://codeforces.com/problemset/problem/491/B) 

**Xếp hạng:** 2100 
**Tags:** tham lam, toán học 
**Thời gian giải:** 11 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Thành phố là một mạng lưới hình chữ nhật. Mọi khách sạn, nhà hàng đều nằm ở một ngã tư có tọa độ$(x,y)$. Di chuyển giữa các nút giao thông lân cận tốn một km, do đó khoảng cách di chuyển giữa hai điểm là khoảng cách Manhattan$$|x_1-x_2| + |y_1-y_2|.$$Tất cả bạn bè đều xuất phát từ khách sạn của họ và đi đến cùng một nhà hàng. Đối với một nhà hàng đã chọn, chi phí mà chúng ta quan tâm không phải là tổng quãng đường mà tất cả bạn bè đã đi mà là quãng đường lớn nhất mà bất kỳ người bạn nào đã đi. 

Đối với mỗi nhà hàng chúng ta phải tính toán$$\max_{\text{hotel } h} \left( |x_h-x_r|+|y_h-y_r| \right)$$và chọn một nhà hàng giảm thiểu giá trị này. Chúng ta phải xuất ra cả khoảng cách tối đa tối ưu và chỉ số của bất kỳ nhà hàng nào đạt được nó. 

Kích thước của thành phố có thể lớn bằng$10^9$, nhưng điều này hóa ra không liên quan vì chúng ta không bao giờ cần lặp lại các ô lưới. Những hạn chế quan trọng là số lượng khách sạn và nhà hàng, cả hai đều lên tới$10^5$. 

Việc so sánh trực tiếp từng nhà hàng với từng khách sạn sẽ đòi hỏi$$10^5 \times 10^5 = 10^{10}$$tính toán khoảng cách trong trường hợp xấu nhất, điều này hoàn toàn không thể thực hiện được trong giới hạn 2 giây. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log n)$. 

Một số tình huống tế nhị có thể gây ra suy luận sai. 

Hãy xem xét hai khách sạn:```
(1,100)
(100,1)
```và một nhà hàng:```
(50,50)
```Một giải pháp bất cẩn có thể cố gắng sử dụng khách sạn lớn nhất$x$hoặc lớn nhất$y$một cách độc lập. Khách sạn xa nhất được xác định theo số liệu Manhattan, kết hợp cả hai tọa độ. Chỉ nhìn vào một tọa độ sẽ mất thông tin. 

Một sai lầm dễ mắc phải khác là cho rằng giới hạn thành phố là quan trọng. Ví dụ:```
N = M = 10^9
hotel = (1,1)
restaurant = (10^9,10^9)
```Khoảng cách chỉ đơn giản là$$(10^9-1)+(10^9-1),$$và không cần phải truyền qua lưới. Bất kỳ giải pháp nào tùy thuộc vào khu vực của thành phố sẽ thất bại ngay lập tức. 

Cái bẫy thứ ba là xử lý các vị trí trùng lặp. Ví dụ:```
Hotels:
(5,5)
(5,5)

Restaurants:
(5,5)
```Câu trả lời là khoảng cách$0$. Thuật toán phải xử lý các điểm trùng lặp một cách bình thường và không cho rằng tất cả các tọa độ đều khác nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi nhà hàng, hãy tính khoảng cách từ Manhattan đến mọi khách sạn và giữ mức tối đa. Sau khi đánh giá tất cả các khách sạn, chúng tôi biết khoảng cách tồi tệ nhất đối với nhà hàng đó. Lặp lại điều này cho tất cả các nhà hàng sẽ cho câu trả lời tối ưu. 

Lý do điều này hoạt động rất đơn giản. Định nghĩa của mục tiêu chính xác là khoảng cách tối đa từ khách sạn đến nhà hàng, do đó, việc kiểm tra rõ ràng từng cặp sẽ mang lại kết quả chính xác. 

Vấn đề là quy mô. Với$10^5$khách sạn và$10^5$nhà hàng, chúng tôi sẽ đánh giá$10^{10}$khoảng cách. Ngay cả khi việc tính toán khoảng cách chỉ mất một vài lệnh máy thì điều này vẫn vượt xa giới hạn. 

Để cải thiện điều này, chúng ta cần hiểu cấu trúc của khoảng cách Manhattan. 

Đối với bất kỳ khách sạn nào$(a,b)$và nhà hàng$(x,y)$,$$|a-x|+|b-y|.$$Một nhận dạng tiêu chuẩn cho hình học Manhattan là$$|u|+|v| = \max \{ (u+v), (u-v), (-u+v), (-u-v) \}.$$Thay thế$u=a-x$Và$v=b-y$,$$|a-x|+|b-y| = \max \{ (a+b)-(x+y), (a-b)-(x-y), (-a+b)-(-x+y), (-a-b)-(-x-y) \}.$$Đối với một nhà hàng cố định, mức tối đa trên tất cả các khách sạn sẽ trở thành$$\max \left( \max(a+b)-(x+y), \max(a-b)-(x-y), \max(-a+b)-(-x+y), \max(-a-b)-(-x-y) \right).$$Quan sát quan trọng là tất cả thông tin về khách sạn được nén thành bốn cực đại toàn cầu. 

Cho phép$$M_1=\max(a+b),$$

$$M_2=\max(a-b),$$

$$M_3=\max(-a+b),$$

$$M_4=\max(-a-b).$$Khi đã biết bốn giá trị này, khoảng cách khách sạn xa nhất đối với bất kỳ nhà hàng nào có thể được tính theo thời gian không đổi. 

Lực lượng vũ phu hoạt động vì nó tìm kiếm khách sạn xa nhất một cách rõ ràng. Quan sát ở trên thay thế tìm kiếm đó bằng bốn giá trị cực trị được tính toán trước. Chúng tôi dành$O(C)$thời gian xử lý trước khách sạn và$O(H)$thời gian đánh giá nhà hàng 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(CH)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(C+H)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả tọa độ khách sạn. 
2. Tính bốn giá trị trên tất cả các khách sạn:$$M_1=\max(x+y)$$

$$M_2=\max(x-y)$$

$$M_3=\max(-x+y)$$

$$M_4=\max(-x-y)$$Đây là những số liệu thống kê duy nhất của khách sạn cần thiết sau này. 
3. Xử lý từng nhà hàng một. 
4. Đối với nhà hàng$(x,y)$, tính toán$$d= \max( M_1-(x+y), M_2-(x-y), M_3-(-x+y), M_4-(-x-y) ).$$Giá trị này bằng khoảng cách tối đa của Manhattan từ nhà hàng đến bất kỳ khách sạn nào. 
5. Giữ nhà hàng có giá trị nhỏ nhất$d$. 
6. Xuất ra khoảng cách tốt nhất và chỉ số nhà hàng dựa trên 1 tương ứng. 

### Tại sao nó hoạt động 

Đối với mỗi khách sạn, khoảng cách Manhattan có thể được viết lại thành tối đa bốn biểu thức tuyến tính. Khi chúng tôi lấy mức tối đa trên tất cả các khách sạn, mỗi biểu thức tuyến tính phụ thuộc vào giá trị lớn nhất mà khách sạn đạt được theo hướng đó. Những giá trị lớn nhất đó chính xác là$M_1,M_2,M_3,M_4$. 

Đối với một nhà hàng cố định, việc đánh giá bốn biểu thức bằng cách sử dụng các giá trị cực đại được tính toán trước này sẽ cho kết quả tương tự như việc kiểm tra từng khách sạn riêng lẻ. Vì giá trị được tính toán chính xác là khoảng cách xa nhất đến khách sạn nên việc so sánh các giá trị này giữa các nhà hàng sẽ chọn ra nhà hàng giảm thiểu khoảng cách di chuyển tồi tệ nhất. Không có sự gần đúng nào được đưa ra, vì vậy thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    c = int(input())

    INF = 10**30

    m1 = -INF
    m2 = -INF
    m3 = -INF
    m4 = -INF

    for _ in range(c):
        x, y = map(int, input().split())

        m1 = max(m1, x + y)
        m2 = max(m2, x - y)
        m3 = max(m3, -x + y)
        m4 = max(m4, -x - y)

    h = int(input())

    best_dist = INF
    best_idx = 1

    for idx in range(1, h + 1):
        x, y = map(int, input().split())

        dist = max(
            m1 - (x + y),
            m2 - (x - y),
            m3 - (-x + y),
            m4 - (-x - y)
        )

        if dist < best_dist:
            best_dist = dist
            best_idx = idx

    print(best_dist)
    print(best_idx)

solve()
```Phần đầu tiên của mã tính toán bốn tọa độ biến đổi cực trị từ tất cả các khách sạn. Điều này tương ứng với bước tiền xử lý nén tất cả thông tin khách sạn thành bốn số. 

Vòng lặp nhà hàng đánh giá khoảng cách xa nhất đến khách sạn trong thời gian không đổi bằng cách sử dụng bốn giá trị cực đại đó. Vì đầu ra bắt buộc phải có chỉ mục nhà hàng nên vòng lặp được chạy với chỉ mục dựa trên 1. 

Tất cả số học dễ dàng phù hợp với số nguyên 64 bit. Tọa độ nhiều nhất là$10^9$, do đó các giá trị được chuyển đổi được giới hạn bởi khoảng$2 \cdot 10^9$. Số nguyên Python tự động xử lý việc này. 

Một lỗi triển khai phổ biến là quên rằng câu trả lời của nhà hàng là giá trị tối đa trong bốn biểu thức được chuyển đổi chứ không phải giá trị tối thiểu. Mỗi biểu thức đại diện cho một cấu hình dấu hiệu có thể có trong nhận dạng khoảng cách Manhattan. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 10
2
1 1
3 3
2
1 10
4 4
```Tiền xử lý khách sạn: 

| Khách sạn | x+y | x-y | -x+y | -x-y | 
| --- | --- | --- | --- | --- | 
| (1,1) | 2 | 0 | 0 | -2 | 
| (3,3) | 6 | 0 | 0 | -6 | 

Cực đại cuối cùng: 

| M1 | M2 | M3 | M4 | 
| --- | --- | --- | --- | 
| 6 | 0 | 0 | -2 | 

Đánh giá nhà hàng: 

| Mục lục nhà hàng | Tọa độ | Khoảng cách | 
| --- | --- | --- | 
| 1 | (1,10) | 9 | 
| 2 | (4,4) | 6 | 

Nhà hàng thứ hai có khoảng cách trong trường hợp xấu nhất nhỏ hơn, vì vậy câu trả lời là:```
6
2
```Dấu vết này cho thấy cách mọi nhà hàng có thể được đánh giá mà không cần xem lại khách sạn. 

### Ví dụ 2 

đầu vào:```
100 100
2
1 100
100 1
2
50 50
1 1
```Tiền xử lý khách sạn: 

| Khách sạn | x+y | x-y | -x+y | -x-y | 
| --- | --- | --- | --- | --- | 
| (1.100) | 101 | -99 | 99 | -101 | 
| (100,1) | 101 | 99 | -99 | -101 | 

Tối đa: 

| M1 | M2 | M3 | M4 | 
| --- | --- | --- | --- | 
| 101 | 99 | 99 | -101 | 

Đánh giá nhà hàng: 

| Mục lục nhà hàng | Tọa độ | Khoảng cách | 
| --- | --- | --- | 
| 1 | (50,50) | 99 | 
| 2 | (1,1) | 99 | 

Cả hai nhà hàng đều đạt được giá trị như nhau. Chỉ số nào cũng hợp lệ. 

Ví dụ này chứng minh rằng khách sạn xa nhất được xác định bằng tọa độ được chuyển đổi, không phải bằng cách chỉ kiểm tra khách sạn lớn nhất$x$hoặc lớn nhất$y$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(C+H)$| Một lượt qua khách sạn và một lượt qua nhà hàng | 
| Không gian |$O(1)$| Chỉ có một vài biến được lưu trữ | 

Kích thước đầu vào có thể đạt tới$2 \cdot 10^5$điểm. Quét tuyến tính trên tất cả các điểm có thể dễ dàng đủ nhanh trong thời gian giới hạn. Việc sử dụng bộ nhớ không đổi và thấp hơn nhiều so với mức 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    c = int(input())

    INF = 10**30
    m1 = m2 = m3 = m4 = -INF

    for _ in range(c):
        x, y = map(int, input().split())
        m1 = max(m1, x + y)
        m2 = max(m2, x - y)
        m3 = max(m3, -x + y)
        m4 = max(m4, -x - y)

    h = int(input())

    best_dist = INF
    best_idx = 1

    for idx in range(1, h + 1):
        x, y = map(int, input().split())

        dist = max(
            m1 - (x + y),
            m2 - (x - y),
            m3 - (-x + y),
            m4 - (-x - y)
        )

        if dist < best_dist:
            best_dist = dist
            best_idx = idx

    return f"{best_dist}\n{best_idx}\n"

# provided sample
assert run(
"""10 10
2
1 1
3 3
2
1 10
4 4
"""
) == "6\n2\n"

# minimum size
assert run(
"""1 1
1
1 1
1
1 1
"""
) == "0\n1\n"

# duplicate hotels
assert run(
"""10 10
2
5 5
5 5
1
5 5
"""
) == "0\n1\n"

# tie between restaurants
assert run(
"""100 100
2
1 100
100 1
2
50 50
1 1
"""
) in ("99\n1\n", "99\n2\n")

# boundary coordinates
assert run(
"""1000000000 1000000000
1
1 1
1
1000000000 1000000000
"""
) == "1999999998\n1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Khách sạn và nhà hàng đơn lẻ ở cùng một điểm | 0 | Vỏ kích thước tối thiểu | 
| Tọa độ khách sạn trùng lặp | 0 | Điểm trùng lặp được xử lý chính xác | 
| Vị trí khách sạn đối xứng | Chỉ số nhà hàng | Xử lý cà vạt đúng cách | 
| Tọa độ gần$10^9$| 1999999998 | Giá trị lớn và an toàn tràn | 

## Vỏ cạnh 

Xem xét các vị trí trùng lặp:```
10 10
2
5 5
5 5
1
5 5
```cực đại trở thành:$$M_1=10,\quad M_2=0,\quad M_3=0,\quad M_4=-10.$$Đối với nhà hàng$(5,5)$,$$\max(0,0,0,0)=0.$$Khoảng cách đầu ra của thuật toán$0$, khớp chính xác với câu trả lời đúng. 

Xét tọa độ cực trị:```
1000000000 1000000000
1
1 1
1
1000000000 1000000000
```Cực đại chuyển đổi được tính toán từ một khách sạn. Đánh giá sản lượng nhà hàng$$(10^9-1)+(10^9-1)=1999999998.$$Không xảy ra sự lặp lại trên các ô thành phố nên kích thước lưới khổng lồ không gây khó khăn gì. 

Hãy xem xét một trận hòa:```
100 100
2
1 100
100 1
2
50 50
1 1
```Cả hai nhà hàng đều tạo ra khoảng cách tối đa$99$. Thuật toán giữ giá trị đầu tiên nó gặp với giá trị tối ưu. Vì tuyên bố cho phép bất kỳ nhà hàng tối ưu nào nên hành vi này là chính xác.
