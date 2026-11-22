---
title: "CF 29A - Vấn đề nhổ"
description: "Mỗi con lạc đà đứng ở một tọa độ duy nhất trên trục số. Một con lạc đà ở vị trí x lao thẳng về phía vị trí x + d. Nếu một con lạc đà khác đứng đó, nó sẽ bị trúng đòn. Chúng ta cần xác định xem có tồn tại một cặp lạc đà sao cho mỗi con chạm vào nhau hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 29
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 29 (Div. 2, Codeforces format)"
rating: 1000
weight: 29
solve_time_s: 72
verified: true
draft: false
---
[CF 29A - Vấn đề nhổ](https://codeforces.com/problemset/problem/29/A) 

**Đánh giá:** 1000 
**Tags:** vũ lực 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi con lạc đà đứng ở một tọa độ duy nhất trên trục số. Một con lạc đà ở vị trí`x`nhổ chính xác về phía vị trí`x + d`. Nếu một con lạc đà khác đứng đó, nó sẽ bị trúng đòn. Chúng ta cần xác định xem có tồn tại một cặp lạc đà sao cho mỗi con chạm vào nhau hay không. 

Giả sử lạc đà A đứng ở`x1`và nhổ khoảng cách`d1`. Nó nhắm vào vị trí`x1 + d1`. Lạc đà B đứng ở`x2`và nhổ khoảng cách`d2`. Hai con lạc đà nhổ vào nhau nếu:```
x1 + d1 = x2
x2 + d2 = x1
```Đầu vào cung cấp vị trí và khoảng cách nhổ cho mỗi con lạc đà. Đầu ra là`"YES"`nếu có ít nhất một cặp tương hỗ tồn tại, nếu không thì`"NO"`. 

Những hạn chế là rất nhỏ. Có nhiều nhất là 100 con lạc đà, nên ngay cả một thuật toán kiểm tra trực tiếp từng cặp cũng hoàn toàn ổn. Một vòng lặp kép trên tất cả các cặp hoạt động tối đa`100 * 100 = 10,000`kiểm tra, rất nhỏ trong giới hạn 2 giây. Điều này ngay lập tức cho chúng ta biết rằng chúng ta không cần các cấu trúc dữ liệu phức tạp hoặc các thủ thuật tối ưu hóa. 

Có một số trường hợp khó bỏ sót. 

Lạc đà có thể nhổ sang trái thay vì phải vì`d`có thể tiêu cực. Ví dụ:```
2
5 -2
3 2
```Lạc đà tại`5`mục tiêu`3`, và lạc đà tại`3`mục tiêu`5`, vậy câu trả lời là`"YES"`. Một giải pháp bất cẩn cho rằng tất cả các bước đều đúng sẽ thất bại ở đây. 

Một trường hợp tinh tế khác là khi một con lạc đà va phải một con lạc đà khác, nhưng điều ngược lại không đúng:```
2
0 2
2 5
```Lạc đà tại`0`đánh lạc đà vào`2`, nhưng lạc đà ở`2`mục tiêu`7`. Câu trả lời đúng là`"NO"`. Việc triển khai có lỗi chỉ có thể kiểm tra xem một con lạc đà có thể đâm vào con lạc đà khác hay không, thay vì kiểm tra việc nhắm mục tiêu lẫn nhau. 

Trường hợp thứ ba liên quan đến nhiều con lạc đà trong đó chỉ có một cặp hoạt động:```
4
0 3
3 -3
10 1
20 -5
```Câu trả lời đúng là`"YES"`vì hai con lạc đà đầu tiên nhổ vào nhau. Thuật toán phải dừng ngay khi tìm thấy bất kỳ cặp hợp lệ nào. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là so sánh từng cặp lạc đà. Đối với mỗi cặp`(i, j)`, chúng tôi kiểm tra xem lạc đà`i`nhổ chính xác vào lạc đà`j`, và lạc đà`j`nhổ chính xác vào lạc đà`i`. 
Xưa của lạc đà`i`là`(xi, di)`và lạc đà`j`là`(xj, dj)`, sau đó họ nhổ nước bọt vào nhau khi:```
xi + di = xj
xj + dj = xi
```Cách tiếp cận bạo lực này là đúng vì mọi cặp có thể đều được kiểm tra. Nếu một cặp hợp lệ tồn tại ở bất kỳ đâu trong đầu vào, cuối cùng chúng tôi sẽ kiểm tra nó. 

Độ phức tạp về thời gian là`O(n²)`bởi vì chúng tôi kiểm tra tất cả các cặp đặt hàng. Với`n ≤ 100`, nhiều nhất là 10.000 so sánh, điều này không đáng kể. 

Vì các ràng buộc đã rất nhỏ nên không cần phải tối ưu hóa nâng cao hơn. Chúng tôi có thể lưu trữ các vị trí lạc đà trong bản đồ băm và cố gắng tìm kiếm mục tiêu nhanh hơn, nhưng độ phức tạp tăng thêm không mang lại lợi ích thực tế nào ở đây. Giải pháp theo cặp đã nhanh chóng, đơn giản và dễ xác minh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra cặp Brute Force | O(n²) | O(1) | Đã chấp nhận | 
| Tra cứu bản đồ băm | O(n) trung bình | O(n) | Được chấp nhận nhưng không cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc số lạc đà. 
2. Cất mỗi con lạc đà thành một cặp`(x, d)`Ở đâu`x`là vị trí và`d`là khoảng cách nhổ. 
3. Lặp lại tất cả các cặp lạc đà khác nhau`(i, j)`. 
4. Với mỗi cặp, tính toán các vị trí mục tiêu:```
camel i targets xi + di
camel j targets xj + dj
```5. Kiểm tra xem lạc đà`i`mục tiêu lạc đà`j`và lạc đà`j`mục tiêu lạc đà`i`. 

Điều này có nghĩa là kiểm tra:```
xi + di == xj
xj + dj == xi
```6. Nếu cả hai điều kiện đều đúng thì in ngay`"YES"`và chấm dứt. 

Chúng ta có thể dừng sớm vì bài toán chỉ hỏi liệu có tồn tại ít nhất một cặp như vậy hay không. 
7. Nếu tất cả các cặp được kiểm tra và không có cặp nào thỏa mãn điều kiện, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cặp lạc đà có thể có chính xác một lần. Một cặp chỉ được chấp nhận nếu nhổ của mỗi con lạc đà rơi chính xác vào vị trí của con lạc đà kia. Vì định nghĩa "nhổ vào nhau" chính xác là hai phương trình này nên thuật toán không thể bỏ sót một cặp hợp lệ và không thể chấp nhận sai một cặp không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
camels = []

for _ in range(n):
    x, d = map(int, input().split())
    camels.append((x, d))

for i in range(n):
    xi, di = camels[i]

    for j in range(i + 1, n):
        xj, dj = camels[j]

        if xi + di == xj and xj + dj == xi:
            print("YES")
            sys.exit()

print("NO")
```Chương trình bắt đầu bằng việc đọc tất cả các con lạc đà vào một danh sách`(position, distance)`cặp. 

Các vòng lặp lồng nhau lặp lại trên mỗi cặp duy nhất. sử dụng`j`từ`i + 1`tránh việc kiểm tra cùng một cặp hai lần và tránh so sánh một con lạc đà với chính nó. 

Bên trong vòng lặp, điều kiện khớp trực tiếp với định nghĩa toán học của việc nhổ lẫn nhau. Đẳng thức đầu tiên kiểm tra xem lạc đà`i`đánh lạc đà`j`. Thứ hai kiểm tra hướng ngược lại. 

Ngay khi tìm thấy một cặp hợp lệ, chương trình sẽ in`"YES"`và thoát ra ngay lập tức. Việc chấm dứt sớm giúp việc triển khai đơn giản và tránh những công việc không cần thiết. 

Không có lo ngại về tràn vì số nguyên Python xử lý các phạm vi này một cách dễ dàng. Khoảng cách nhổ âm cũng hoạt động một cách tự nhiên vì phép cộng xử lý cả hai hướng trên trục số. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
0 1
1 -1
```| tôi | j | Lạc đà tôi`(x,d)`| Lạc đà j`(x,d)`|`xi + di`|`xj + dj`| Qua lại? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | (0, 1) | (1, -1) | 1 | 0 | Có | 

Vị trí mục tiêu lạc đà đầu tiên`1`, đó là nơi con lạc đà thứ hai đứng. Vị trí mục tiêu lạc đà thứ hai`0`, nơi con lạc đà đầu tiên đứng. Thuật toán in ngay lập tức`"YES"`. 

### Ví dụ 2 

đầu vào:```
3
0 2
2 5
10 -3
```| tôi | j | Lạc đà tôi`(x,d)`| Lạc đà j`(x,d)`|`xi + di`|`xj + dj`| Qua lại? | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | (0, 2) | (2, 5) | 2 | 7 | Không | 
| 0 | 2 | (0, 2) | (10, -3) | 2 | 7 | Không | 
| 1 | 2 | (2, 5) | (10, -3) | 7 | 7 | Không | 

Con lạc đà đầu tiên đánh con thứ hai nhưng con thứ hai không đánh trả. Không có cặp nào thỏa mãn cả hai phương trình cùng một lúc nên đáp án là`"NO"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Mỗi cặp lạc đà được kiểm tra một lần | 
| Không gian | O(1) thêm | Chỉ có một vài biến ngoài danh sách đầu vào | 

Với tối đa 100 con lạc đà, giải pháp bậc hai chỉ thực hiện khoảng 10.000 lần kiểm tra cặp. Tỷ lệ này thấp hơn nhiều so với giới hạn, vì vậy giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    camels = []

    for _ in range(n):
        x, d = map(int, input().split())
        camels.append((x, d))

    for i in range(n):
        xi, di = camels[i]

        for j in range(i + 1, n):
            xj, dj = camels[j]

            if xi + di == xj and xj + dj == xi:
                return "YES"

    return "NO"

# provided sample
assert run("2\n0 1\n1 -1\n") == "YES", "sample 1"

# minimum size
assert run("1\n0 5\n") == "NO", "single camel"

# one-direction hit only
assert run("2\n0 2\n2 5\n") == "NO", "not mutual"

# negative distances
assert run("2\n5 -2\n3 2\n") == "YES", "left and right spit"

# larger case with one valid pair
assert run(
    "5\n0 3\n3 -3\n10 1\n20 -5\n7 2\n"
) == "YES", "find pair among many"

# no valid pairs
assert run(
    "4\n0 1\n2 1\n4 1\n6 1\n"
) == "NO", "all spit away"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lạc đà đơn | KHÔNG | Không thể tạo thành một cặp | 
| Chỉ đánh một hướng | KHÔNG | Cả hai hướng phải giữ | 
| Khoảng cách âm | CÓ | Xử lý nhổ sang trái đúng cách | 
| Trường hợp hỗn hợp lớn hơn | CÓ | Thuật toán tìm một cặp hợp lệ giữa những con lạc đà không liên quan | 
| Tất cả nhổ đi | KHÔNG | Không có kết quả dương tính giả | 

## Vỏ cạnh 

Hãy xem xét trường hợp nhổ sang trái:```
2
5 -2
3 2
```Thuật toán kiểm tra cặp:```
5 + (-2) = 3
3 + 2 = 5
```Cả hai điều kiện đều đúng, vì vậy nó in`"YES"`. Khoảng cách âm hoạt động một cách tự nhiên vì tính toán chỉ là phép cộng số nguyên. 

Bây giờ hãy xem xét cú đánh một chiều:```
2
0 2
2 5
```Thuật toán tính toán:```
0 + 2 = 2
2 + 5 = 7
```Điều kiện đầu tiên thành công, nhưng điều kiện thứ hai thất bại. Vì cả hai đều được yêu cầu đồng thời nên cặp này bị loại bỏ và câu trả lời cuối cùng trở thành`"NO"`. 

Cuối cùng, hãy xem xét nhiều con lạc đà chỉ có một cặp phù hợp:```
4
0 3
3 -3
10 1
20 -5
```Thuật toán kiểm tra các cặp theo thứ tự. Khi nó đến được hai con lạc đà đầu tiên:```
0 + 3 = 3
3 + (-3) = 0
```Cặp thỏa mãn điều kiện nên thuật toán in ngay`"YES"`mà không mất thời gian kiểm tra các cặp còn lại.
