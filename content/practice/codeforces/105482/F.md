---
title: "CF 105482F - \u041f\u043e\u0433\u043e\u043d\u044f \u0432 \u041f\u0443\u0441\u0442\u043e\u0442\u0435"
description: "Ta có đồ thị vô hướng có n vị trí. Các anh hùng bắt đầu ở vị trí 1, nơi trú ẩn ở vị trí n và Alioth đã chiếm k vị trí. Alioth không di chuyển như một nhân vật đơn lẻ. Nó lây lan."
date: "2026-06-25T06:03:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105482
codeforces_index: "F"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u041f\u0435\u0440\u0432\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 105482
solve_time_s: 73
verified: true
draft: false
---

[CF 105482F - \u041f\u043e\u0433\u043e\u043d\u044f \u0432 \u041f\u0443\u0441\u0442\u043e\u0442\u0435](https://codeforces.com/problemset/problem/105482/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có đồ thị vô hướng với`n`địa điểm. Các anh hùng bắt đầu tại vị trí`1`, nơi trú ẩn ở vị trí`n`, và Alioth đã chiếm giữ`k`địa điểm. 

Alioth không di chuyển như một nhân vật đơn lẻ. Nó lây lan. Nếu một địa điểm bị chiếm đóng vào thời điểm đó`t`, nó vẫn bị chiếm giữ mãi mãi và bắt đầu lan rộng hơn nữa. Theo thuật ngữ đồ thị, thời gian đến của Alioth tại một đỉnh đơn giản là khoảng cách ngắn nhất tính từ bất kỳ đỉnh nào bị chiếm giữ ban đầu. 

Chúng tôi được phép xây dựng thêm đường hầm giữa các địa điểm. Mục tiêu là bổ sung càng nhiều đường hầm càng tốt trong khi vẫn cho phép các anh hùng đến nơi trú ẩn sớm hơn Alioth. Nếu tại một địa điểm nào đó cả hai đến cùng lúc, các anh hùng sẽ thua cuộc. 

Đầu vào đưa ra biểu đồ ban đầu và tập hợp các vị trí bị lây nhiễm. Đầu ra là số lượng đường hầm mới tối đa có thể được xây dựng. Nếu không thể sống sót dù chúng ta có làm gì đi nữa, chúng ta phải in`-1`. 

Biểu đồ chứa tới`10^5`đỉnh và`2·10^5`các cạnh. Bất kỳ giải pháp nào cố gắng suy luận về tất cả các cạnh bổ sung có thể có riêng lẻ sẽ quá chậm, bởi vì biểu đồ hoàn chỉnh chứa khoảng`n²/2`các cạnh. Chúng ta cần một đặc tính có thể được kiểm tra theo thời gian tuyến tính hoặc gần tuyến tính. 

Trường hợp nguy hiểm nhất là khi nơi trú ẩn đã liền kề với vị trí bị nhiễm bệnh. 

Ví dụ:```
4 5 1
3
1 4
3 4
4 2
1 3
3 2
```Alioth bắt đầu ở đỉnh`3`. Nơi trú ẩn là đỉnh`4`, và đã có một cạnh`3-4`. Alioth đến nơi trú ẩn chỉ trong một bước. Ngay cả khi các anh hùng sử dụng lợi thế trực tiếp để đến nơi trú ẩn, họ cũng cần một bước để tạo ra một trận hòa. Câu trả lời đúng là:```
-1
```Một giải pháp bất cẩn có thể cho rằng việc thêm nhiều cạnh hơn luôn có thể giúp ích cho các anh hùng, nhưng các cạnh bổ sung cũng giúp ích cho Alioth. 

Một trường hợp quan trọng khác là nơi trú ẩn không liền kề với bất kỳ địa điểm nào bị nhiễm bệnh. 

Ví dụ:```
3 1 1
2
1 3
```Chúng ta có thể thêm cạnh`1-2`, nhưng chúng ta không được thêm cạnh`2-3`. Các anh hùng có thể sử dụng lợi thế trực tiếp`1-3`và đến nơi trong một bước, trong khi Alioth cần hai bước để đến nơi trú ẩn. Câu trả lời là:```
1
```## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng suy luận xem những cạnh nào bị thiếu có thể được thêm vào và những cạnh nào phải không có. Có tới$$\frac{n(n-1)}2$$các cạnh có thể có, đó là về`5·10^9`khi`n = 10^5`. Ngay cả việc lặp lại trên tất cả các cạnh tiềm năng là không thể. 

Điều quan trọng là các anh hùng chỉ cần một con đường an toàn. Vì chúng ta có thể tự do thêm các đường hầm nên con đường tốt nhất rất rõ ràng: kết nối`1`trực tiếp đến`n`nếu cạnh đó chưa tồn tại và di chuyển trong một nước đi. 

Các anh hùng bắt đầu vào thời điểm`0`ở đỉnh`1`và đến nơi trú ẩn đúng lúc`1`. 

Để kế hoạch này thành công, Alioth phải đạt đến đỉnh cao`n`nghiêm ngặt sau thời gian`1`. Vì Alioth lây lan qua những con đường ngắn nhất, điều này có nghĩa là khoảng cách từ mọi đỉnh bị nhiễm đến`n`ít nhất phải có`2`. 

Điều gì có thể khiến Alioth vươn tới`n`trong một bước? Chính xác là một cạnh giữa một đỉnh bị nhiễm và`n`. 

Điều này ngay lập tức đưa ra toàn bộ cấu trúc của đồ thị tối ưu. 

Nếu một đỉnh bị nhiễm đã được kết nối với`n`trong biểu đồ ban đầu, Alioth đến nơi trú ẩn trong một bước bất kể chúng tôi thêm gì sau đó. Sống sót là không thể, vì vậy câu trả lời là`-1`. 

Ngược lại, chúng ta có thể thêm mọi cạnh bị thiếu ngoại trừ những cạnh nối trực tiếp một đỉnh bị nhiễm với`n`. 

Những cạnh bị cấm đó là những thứ duy nhất có thể làm giảm thời gian đến nơi trú ẩn của Alioth ít nhất từ`2`chính xác`1`. 

Cho phép`k`là số đỉnh bị nhiễm. Trong một đồ thị đầy đủ có$$\frac{n(n-1)}2$$các cạnh. Chúng ta phải loại trừ chính xác`k`các cạnh, cụ thể là`(infected, n)`. 

Vì vậy, số cạnh tối đa trong một biểu đồ cuối cùng hợp lệ là$$\frac{n(n-1)}2 - k.$$Vì biểu đồ đã chứa`m`các cạnh, số lượng đường hầm bổ sung tối đa là$$\frac{n(n-1)}2 - k - m.$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(m + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các đỉnh bị nhiễm và đánh dấu chúng trong một mảng boolean. 
2. Đọc tất cả các cạnh hiện có. 
3. Trong khi đọc các cạnh, hãy kiểm tra xem một điểm cuối có phải là nơi trú ẩn không`n`và điểm cuối khác bị nhiễm bệnh. 
4. Nếu có cạnh như vậy, hãy in`-1`. 

Alioth đã có thể tiếp cận nơi trú ẩn chỉ trong một bước. Các anh hùng không thể đến sớm hơn thế. 
5. Ngược lại tính toán$$\text{complete} = \frac{n(n-1)}2.$$6. Đồ thị cuối cùng có thể chứa mọi cạnh ngoại trừ cạnh`k`các cạnh giữa các đỉnh bị nhiễm bệnh và nơi trú ẩn. 
7. Số lượng đường hầm mới tối đa bằng$$\text{complete} - k - m.$$8. In giá trị đó. 

### Tại sao nó hoạt động 

Nếu một đỉnh bị nhiễm trùng liền kề với nơi trú ẩn, Alioth sẽ đến được nơi trú ẩn vào thời điểm đó`1`. Bất kỳ tuyến đường nào từ vị trí xuất phát của anh hùng đều yêu cầu ít nhất một lần di chuyển, vì vậy thời gian đến tốt nhất có thể của anh hùng cũng là`1`. Điều kiện rất khắc nghiệt nên việc sống sót là không thể. 

Giả sử không có đỉnh bị nhiễm nào liền kề với nơi trú ẩn. Xây dựng đồ thị chứa mọi cạnh có thể có ngoại trừ cạnh`k`cạnh cấm`(infected, n)`. 

Các anh hùng du hành trực tiếp từ`1`ĐẾN`n`trong một bước. 

Alioth không thể tiếp cận`n`trong một bước vì mọi cạnh từ đỉnh bị nhiễm đến`n`vắng mặt. Do đó Alioth đạt được`n`không sớm hơn thời gian`2`. 

Các anh hùng đến đúng lúc`1`, tức là sớm hơn. 

Bất kỳ cạnh bổ sung nào không có dạng`(infected, n)`không làm giảm thời gian Alioth đến nơi trú ẩn bên dưới`2`, do đó tất cả các cạnh như vậy có thể được thêm vào một cách an toàn. Đồ thị này là tối đa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, k = map(int, input().split())
infected = list(map(int, input().split()))

bad = [False] * (n + 1)
for v in infected:
    bad[v] = True

impossible = False

for _ in range(m):
    u, v = map(int, input().split())

    if u == n and bad[v]:
        impossible = True
    if v == n and bad[u]:
        impossible = True

if impossible:
    print(-1)
else:
    total_edges = n * (n - 1) // 2
    answer = total_edges - k - m
    print(answer)
```Phần đầu tiên lưu trữ tất cả các đỉnh bị nhiễm trong một mảng boolean để việc kiểm tra tư cách thành viên diễn ra liên tục. 

Trong khi đọc các cạnh, chúng tôi chỉ quan tâm đến một điều kiện: liệu nơi trú ẩn có liền kề với một đỉnh bị nhiễm bệnh hay không. Không cần duyệt đồ thị. 

Nếu có một cạnh như vậy tồn tại thì câu trả lời là ngay lập tức`-1`. 

Mặt khác, chúng tôi tính toán kích thước của biểu đồ hoàn chỉnh và trừ đi`k`các cạnh bị cấm vĩnh viễn nối các đỉnh bị nhiễm bệnh với nơi trú ẩn. Biểu đồ đã chứa`m`các cạnh, do đó số lượng còn lại chính xác là số lượng đường hầm vẫn có thể được xây dựng. 

Tất cả số học sử dụng các giá trị có kích thước 64 bit. Trong Python, số nguyên không bị giới hạn nên không có nguy cơ tràn dữ liệu. Giá trị lớn nhất của`n(n-1)/2`là về`5·10^9`, dù sao cũng vừa vặn một cách thoải mái. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1 1
2
1 3
```Trạng thái trong quá trình xử lý: 

| Biến | Giá trị | 
| --- | --- | 
| n | 3 | 
| m | 1 | 
| k | 1 | 
| bị nhiễm | {2} | 
| bị nhiễm liền kề với 3? | Không | 

Tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| Hoàn thành các cạnh đồ thị | 3 | 
| Các cạnh bị cấm (bị nhiễm, 3) | 1 | 
| Các cạnh hiện có | 1 | 
| Trả lời | 3 - 1 - 1 = 1 | 

Đầu ra:```
1
```Cạnh cấm duy nhất là`2-3`. Mọi cạnh còn thiếu khác có thể được thêm vào. 

### Ví dụ 2 

đầu vào:```
7 6 3
5 6 2
1 7
6 2
1 4
3 6
5 3
2 1
```Trạng thái trong quá trình xử lý: 

| Biến | Giá trị | 
| --- | --- | 
| n | 7 | 
| m | 6 | 
| k | 3 | 
| bị nhiễm | {2,5,6} | 
| bị nhiễm liền kề với 7? | Không | 

Tính toán: 

| Số lượng | Giá trị | 
| --- | --- | 
| Hoàn thành các cạnh đồ thị | 21 | 
| Các cạnh bị cấm | 3 | 
| Các cạnh hiện có | 6 | 
| Trả lời | 21 - 3 - 6 = 12 | 

Đầu ra:```
12
```Ví dụ này cho thấy việc xây dựng tối đa. Mọi cạnh còn thiếu đều có thể được thêm vào ngoại trừ`2-7`,`5-7`, Và`6-7`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m + k) | Đọc các đỉnh bị nhiễm và quét các cạnh một lần | 
| Không gian | O(n) | Mảng Boolean đánh dấu các đỉnh bị nhiễm | 

Các ràng buộc cho phép lên đến`10^5`đỉnh và`2·10^5`các cạnh. Một lần quét tuyến tính đơn giản có thể đủ nhanh và vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    infected = list(map(int, input().split()))

    bad = [False] * (n + 1)
    for v in infected:
        bad[v] = True

    impossible = False

    for _ in range(m):
        u, v = map(int, input().split())
        if u == n and bad[v]:
            impossible = True
        if v == n and bad[u]:
            impossible = True

    if impossible:
        return "-1\n"

    total_edges = n * (n - 1) // 2
    return str(total_edges - k - m) + "\n"

# provided samples
assert run(
"""3 1 1
2
1 3
"""
) == "1\n"

assert run(
"""7 6 3
5 6 2
1 7
6 2
1 4
3 6
5 3
2 1
"""
) == "12\n"

assert run(
"""4 5 1
3
1 4
3 4
4 2
1 3
3 2
"""
) == "-1\n"

# minimum size
assert run(
"""3 1 1
2
1 3
"""
) == "1\n"

# shelter adjacent to infected
assert run(
"""5 1 1
2
2 5
"""
) == "-1\n"

# almost complete graph
assert run(
"""4 5 1
2
1 3
1 4
2 3
3 4
1 2
"""
) == "0\n"

# no forbidden existing edge
assert run(
"""5 0 2
2 3
"""
) == "8\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 1 | Xây dựng hợp lệ cơ bản | 
| Mẫu 2 | 12 | Hoàn thành dày đặc với một số nút bị nhiễm | 
| Mẫu 3 | -1 | Nơi trú ẩn đã liền kề với nhiễm trùng | 
| Biểu đồ kích thước tối thiểu | 1 | Trường hợp hợp lệ nhỏ nhất | 
| Cạnh cấm đơn | -1 | Không thể ngay lập tức | 
| Biểu đồ gần như hoàn chỉnh | 0 | Không có đường hầm bổ sung | 
| Không có cạnh ban đầu | 8 | Công thức trên đồ thị thưa thớt | 

## Vỏ cạnh 

Hãy xem xét lại:```
4 5 1
3
1 4
3 4
4 2
1 3
3 2
```Nơi trú ẩn là đỉnh`4`. Vì cạnh`3-4`đã tồn tại và đỉnh`3`bị nhiễm bệnh, Alioth đến nơi trú ẩn đúng lúc`1`. 

Những anh hùng không thể làm tốt hơn thời gian`1`, bởi vì bất kỳ chuyển động nào cũng cần ít nhất một lần di chuyển cạnh. 

Thuật toán phát hiện cạnh nơi trú ẩn bị nhiễm bệnh trong khi quét đầu vào và trả về ngay lập tức:```
-1
```Bây giờ hãy xem xét:```
3 1 1
2
1 3
```Không có ranh giới nơi trú ẩn bị nhiễm bệnh. Thuật toán tính toán:```
complete = 3
forbidden = 1
existing = 1
answer = 1
```Các anh hùng sử dụng con đường trực tiếp`1 -> 3`trong một bước. Alioth trước tiên phải đi`2 -> 1 -> 3`, thực hiện hai bước. Bất đẳng thức nghiêm ngặt được thỏa mãn, do đó kết quả đầu ra là chính xác.
