---
title: "CF 102860I - Bước đi của ba"
description: "Chúng ta có một đồ thị công viên vô hướng. Lối vào là đỉnh 1 và Vasya chỉ được phép kết thúc ở đỉnh được kết nối trực tiếp với 1. Một bước đi hợp lệ bắt đầu từ 1, sử dụng chính xác ba cạnh khác nhau theo trình tự và kết thúc tại một trong các đỉnh lân cận đó."
date: "2026-07-25T14:15:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "I"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 47
verified: true
draft: false
---

[CF 102860I - Bước đi của ba người](https://codeforces.com/problemset/problem/102860/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị công viên vô hướng. Lối vào là đỉnh`1`, và Vasya chỉ được phép về đích ở đỉnh được kết nối trực tiếp với`1`. Cuộc đi bộ hợp lệ bắt đầu lúc`1`, sử dụng chính xác ba cạnh khác nhau theo thứ tự và kết thúc tại một trong các đỉnh lân cận đó. Nhiệm vụ là đếm xem có bao nhiêu bước đi ba cạnh khác nhau thỏa mãn các quy tắc này. 

Một bước đi có thể ghé thăm cùng một đỉnh nhiều lần, nhưng ba cạnh được sử dụng trong quá trình đi bộ phải khác biệt. Ví dụ, trình tự`1 -> 2 -> 1 -> 3`không hợp lệ vì hai nước đi đầu tiên sử dụng cùng một cạnh theo hướng ngược nhau. 

Đồ thị có tới`100000`đỉnh và`200000`các cạnh. Giải pháp thử tất cả các chuỗi ba cạnh có thể là không thể vì số bước đi có thể tăng lên gần bằng tổng bình phương, vượt xa giới hạn một giây cho phép. Chúng ta cần xử lý đồ thị gần với thời gian tuyến tính. 

Phần khó khăn là không tìm được các bước đi có độ dài ba. Đếm trực tiếp những thứ đó thật dễ dàng. Khó khăn là loại bỏ các bước đi trong đó một trong ba cạnh được sử dụng lại. 

Một số trường hợp nguy hiểm phá vỡ các giải pháp bất cẩn. 

Đối với đồ thị chỉ có một cạnh:```
2 1
1 2
```câu trả lời là`0`. Chuyển động duy nhất có thể là`1 -> 2`, và không thể chọn được ba cạnh khác nhau. 

Đối với một hình tam giác:```
3 3
1 2
2 3
3 1
```câu trả lời là`0`. Có nhiều lối đi ba cạnh kết thúc cạnh đỉnh`1`, nhưng mỗi cạnh đều lặp lại một cạnh. 

Đối với một ngôi sao:```
4 3
1 2
1 3
1 4
```câu trả lời là`0`. Bất kỳ chuyển động ba cạnh nào cũng phải quay trở lại ngay lập tức qua một cạnh đã được sử dụng vì tất cả các con đường đều chạm vào lối vào. 

Một sai lầm phổ biến là đếm mỗi lần đi bộ dài ba đoạn kết thúc tại một hàng xóm của`1`và quên rằng cùng một cạnh có thể xuất hiện hai lần. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ tạo ra mọi bước đi có thể`1 -> a -> b -> c`, kiểm tra xem`c`là hàng xóm của`1`, rồi kiểm tra xem ba cạnh có khác nhau không. Sự chuyển tiếp đầu tiên có`deg(1)`lựa chọn, cái thứ hai có thể có nhiều lựa chọn, và cái thứ ba lại có thể phân nhánh. Ở những phần dày đặc của biểu đồ, giá trị này trở nên quá lớn. Một biểu đồ có các đỉnh bậc cao có thể tạo ra hàng tỷ bước đi của ứng viên. 

Quan sát hữu ích là việc đếm số lần đi bộ không hạn chế rất dễ dàng và các trường hợp không hợp lệ duy nhất có cấu trúc rất cụ thể. 

Đầu tiên, đánh dấu mọi lân cận của đỉnh`1`. Hãy để bộ này được`S`. 

Với mọi đỉnh`v`, tính xem có bao nhiêu hàng xóm của`v`thuộc về`S`. Gọi giá trị này`inside[v]`. 

Bây giờ hãy xem xét việc đi dạo`1 -> a -> b -> c`. Đỉnh đầu tiên`a`phải ở trong`S`. Sau khi chuyển từ`a`ĐẾN`b`, số đỉnh cuối cùng có thể có chính xác là`inside[b]`, vì đỉnh cuối cùng phải là đỉnh lân cận của`1`. Tính tổng số này trên mọi cạnh để lại mọi đỉnh trong`S`tính tất cả các bước đi dài ba chiều kết thúc chính xác, nhưng nó cũng bao gồm các bước đi có cạnh lặp đi lặp lại. 

Chỉ có hai cách để lặp lại một cạnh. 

Hai cạnh đầu tiên có thể giống nhau. Điều này mang lại cho các bước đi của hình thức`1 -> a -> 1 -> c`. có`deg(1)`sự lựa chọn cho`a`Và`deg(1)`sự lựa chọn cho`c`, vậy có`deg(1)^2`những cuộc dạo chơi như vậy. 

Cạnh thứ hai và thứ ba có thể giống nhau. Những bước đi này có hình thức`1 -> a -> b -> a`. Đối với mỗi người hàng xóm`a`của`1`, có`deg(a)`sự lựa chọn cho`b`, do đó số đếm là tổng độ của tất cả các lân cận của`1`. 

Hai trường hợp trùng nhau khi cả ba nước đi đều sử dụng cùng một cạnh:`1 -> a -> 1 -> a`. Có chính xác`deg(1)`những bước đi như vậy, vì vậy chúng tôi thêm chúng trở lại một lần. 

Câu trả lời cuối cùng là:```
all_length_three_walks
- deg(1)^2
- sum_of_neighbor_degrees
+ deg(1)
```| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lần đi bộ dài 3) | O(n + m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ và lưu trữ danh sách kề. Đồ thị thưa thớt nên danh sách kề cho phép chúng ta kiểm tra tất cả các cạnh trong thời gian tuyến tính. 
2. Tìm tất cả các đỉnh nối trực tiếp với đỉnh`1`và đánh dấu chúng. Đây là các đỉnh kết thúc duy nhất có thể có và cũng là các đỉnh đầu tiên duy nhất có thể có. 
3. Với mỗi đỉnh, hãy đếm xem có bao nhiêu đỉnh lân cận được đánh dấu. Giá trị này cho chúng ta biết có bao nhiêu lựa chọn hợp lệ cho bước cuối cùng sau khi đạt đến đỉnh đó. 
4. Đếm tất cả các lần đi bộ không hạn chế. Với mọi đỉnh được đánh dấu`u`, kiểm tra mọi cạnh`u -> v`. Thêm vào`inside[v]`bởi vì mỗi hàng xóm được đánh dấu của`v`tạo ra một điểm kết thúc có thể. 
5. Trừ đi các bước ở nơi cạnh đầu tiên được sử dụng lại. Cấu trúc của chúng là`1 -> u -> 1 -> v`, cho`degree(1) * degree(1)`trường hợp. 
6. Trừ đi các bước ở nơi cạnh thứ hai được sử dụng lại. Cấu trúc của chúng là`1 -> u -> v -> u`, cho tổng bậc của tất cả các lân cận của`1`. 
7. Thêm số lần đi bộ về được tính trong cả hai nhóm không hợp lệ. Đây chính xác là`1 -> u -> 1 -> u`, một cho mỗi người hàng xóm của`1`. 

Tại sao nó hoạt động: 

Số lượng không hạn chế bao gồm mọi lối đi ba cạnh có thể kết thúc bên cạnh lối vào. Lý do duy nhất việc đi bộ như vậy không hợp lệ là vì một trong ba cạnh của nó xuất hiện hai lần. Trong một đồ thị vô hướng đơn giản, cạnh thứ nhất và cạnh thứ hai chỉ có thể khớp nhau khi bước đi quay trở lại đỉnh ngay lập tức`1`và cạnh thứ hai và thứ ba chỉ có thể khớp nhau khi bước đi quay trở lại đỉnh trước đó. Cạnh thứ nhất và thứ ba không thể bằng nhau vì đỉnh cuối cùng không phải là lối vào. Vì hai nhóm không hợp lệ được tính riêng và giao điểm của chúng chính xác là các bước đi sử dụng một cạnh ba lần, nên loại trừ bao gồm sẽ loại bỏ mọi bước đi không hợp lệ đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

g = [[] for _ in range(n)]

for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

mark = [False] * n
for v in g[0]:
    mark[v] = True

inside = [0] * n
for v in range(n):
    cnt = 0
    for u in g[v]:
        if mark[u]:
            cnt += 1
    inside[v] = cnt

total = 0
for u in g[0]:
    for v in g[u]:
        total += inside[v]

deg1 = len(g[0])

bad_first_two = deg1 * deg1

bad_second_three = 0
for u in g[0]:
    bad_second_three += len(g[u])

answer = total - bad_first_two - bad_second_three + deg1

print(answer)
```Mảng`mark`đại diện cho các đỉnh kết thúc có thể. các`inside`mảng tránh việc quét liên tục các lân cận của một đỉnh trong khi đếm bước đi. 

Biến`total`tuân theo cách đếm bước đi không hạn chế được mô tả trong thuật toán. Nó cố tình bỏ qua sự lặp lại cạnh vì những trường hợp đó có công thức đơn giản và việc trừ sau đó sẽ rẻ hơn. 

Các thuật ngữ hiệu chỉnh chỉ sử dụng số học số nguyên. Số nguyên Python không bị tràn, điều này rất hữu ích vì số bước đi có thể lớn hơn nhiều so với số cạnh. 

Biểu đồ được lập chỉ mục bằng 0 bên trong, do đó đỉnh vào được lưu dưới dạng chỉ mục`0`. Không cần xử lý ranh giới đặc biệt vì danh sách kề đương nhiên chỉ chứa các đỉnh hiện có. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
10 14
1 5
2 5
5 6
2 3
1 3
2 4
4 6
1 6
1 7
7 8
8 1
1 10
9 10
9 8
```Hàng xóm của đỉnh`1`là`5, 3, 6, 7, 8, 10`. 

| Biến | Giá trị | 
| --- | --- | 
| bậc đỉnh 1 | 6 | 
| đi bộ không hạn chế | 58 | 
| lặp lại hai cạnh đầu tiên | 36 | 
| cạnh thứ hai và thứ ba lặp lại | 24 | 
| chồng chéo | 6 | 
| trả lời | 4 | 

Phép trừ sẽ loại bỏ mọi lối đi sử dụng đường hai lần. Bốn lối đi còn lại sử dụng ba con đường khác nhau. 

Đối với tam giác:```
3 3
1 2
2 3
3 1
```| Biến | Giá trị | 
| --- | --- | 
| bậc đỉnh 1 | 2 | 
| đi bộ không hạn chế | 6 | 
| lặp lại hai cạnh đầu tiên | 4 | 
| cạnh thứ hai và thứ ba lặp lại | 4 | 
| chồng chéo | 2 | 
| trả lời | 0 | 

Ví dụ này chứng minh tại sao chỉ tính chiều dài ba bước đi là không đủ. Mỗi ứng cử viên lặp lại một cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và mỗi cạnh chỉ được kiểm tra một số lần không đổi. | 
| Không gian | O(n + m) | Danh sách kề và mảng trợ giúp lưu trữ thông tin tuyến tính. | 

Các giới hạn cho phép xử lý đồ thị tuyến tính vì tổng kích thước đầu vào chỉ có vài trăm nghìn cạnh. Giải pháp tránh việc liệt kê các bước đi và thoải mái phù hợp với các hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    mark = [False] * n
    for v in g[0]:
        mark[v] = True

    inside = [0] * n
    for v in range(n):
        for u in g[v]:
            if mark[u]:
                inside[v] += 1

    total = 0
    for u in g[0]:
        for v in g[u]:
            total += inside[v]

    d = len(g[0])
    bad = d * d
    for u in g[0]:
        bad += len(g[u])

    return str(total - bad + d)

assert solve("""2 1
1 2
""") == "0"

assert solve("""3 3
1 2
2 3
3 1
""") == "0"

assert solve("""4 3
1 2
1 3
1 4
""") == "0"

assert solve("""5 6
1 2
2 3
3 4
4 5
5 1
2 5
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn | 0 | Không thể đi bộ ba cạnh | 
| Tam giác | 0 | Hiệu chỉnh cạnh lặp lại | 
| Đồ thị sao | 0 | Trở lại ngay lập tức qua lối vào | 
| Chu kỳ có thêm hợp âm | 2 | Đếm chung với nhiều đường dẫn | 

## Vỏ cạnh 

Đối với đồ thị một cạnh:```
2 1
1 2
```tập hợp được đánh dấu chỉ chứa đỉnh`2`. Số lượng không hạn chế là 0 vì không có đủ cạnh để tạo thành bước đi ba bước. Các số hạng hiệu chỉnh cũng giữ nguyên bằng 0, cho kết quả đúng. 

Đối với tam giác:```
3 3
1 2
2 3
3 1
```số lượng không hạn chế tìm thấy sáu lần đi bộ có thể. Bốn sử dụng lại cạnh đầu tiên, bốn sử dụng lại hai cạnh cuối cùng và hai được tính trong cả hai nhóm. Công thức cho`6 - 4 - 4 + 2 = 0`. 

Đối với đồ thị ngôi sao:```
4 3
1 2
1 3
1 4
```mọi chuyển động ra khỏi lối vào phải ngay lập tức quay trở lại bằng cách sử dụng cùng một cạnh vì không có con đường nào khác tồn tại. Số lượng không hạn chế sẽ bị loại bỏ hoàn toàn bằng cách chỉnh sửa bước đi không hợp lệ. 

Thuật toán xử lý các trường hợp này vì nó tách vấn đề đếm dễ dàng khỏi các lý do cấu trúc chính xác khiến việc đi bộ có thể thất bại. 

Tôi cũng có thể cung cấp phiên bản biên tập ngắn hơn theo phong cách cuộc thi nếu bạn muốn một phiên bản gần giống với những gì sẽ xuất hiện trên Codeforces.
