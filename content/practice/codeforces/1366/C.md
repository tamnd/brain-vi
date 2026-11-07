---
title: "CF 1366C - Đường dẫn Palindromic"
description: "Chúng ta có một ma trận nhị phân. Đường dẫn bắt đầu ở ô trên cùng bên trái và kết thúc ở ô dưới cùng bên phải, chỉ di chuyển sang phải hoặc xuống dưới. Mỗi đường dẫn truy cập chính xác một ô từ mỗi \"lớp khoảng cách\" được đo từ đầu."
date: "2026-06-11T12:03:03+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1366
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 89 (Rated for Div. 2)"
rating: 1500
weight: 1366
solve_time_s: 130
verified: true
draft: false
---

[CF 1366C - Đường dẫn Palindromic](https://codeforces.com/problemset/problem/1366/C) 

**Đánh giá:** 1500 
**Tags:** tham lam, toán học 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ma trận nhị phân. Đường dẫn bắt đầu ở ô trên cùng bên trái và kết thúc ở ô dưới cùng bên phải, chỉ di chuyển sang phải hoặc xuống dưới. 

Mỗi đường dẫn truy cập chính xác một ô từ mỗi "lớp khoảng cách" được đo từ đầu. Nếu một ô ở vị trí`(i, j)`, thì lớp của nó là`i + j`nếu chúng ta sử dụng lập chỉ mục dựa trên số không. Dọc theo bất kỳ đường dẫn nào, các lớp được truy cập theo thứ tự tăng dần từ`0`ĐẾN`n + m - 2`. 

Chúng tôi muốn mọi đường dẫn có thể tạo ra một chuỗi giá trị palindromic. Chúng tôi có thể lật bất kỳ ô nào từ`0`ĐẾN`1`hoặc từ`1`ĐẾN`0`và mục tiêu là giảm thiểu số lượng ô bị thay đổi. 

Kích thước ma trận nhiều nhất là`30 × 30`. Mặc dù có thể có tới`200`trường hợp kiểm thử, mỗi ma trận chứa tối đa`900`tế bào. Điều này ngay lập tức gợi ý rằng bất kỳ giải pháp nào xung quanh`O(nm)`hoặc`O(nm log nm)`mỗi trường hợp thử nghiệm có thể đủ nhanh một cách dễ dàng, trong khi mọi thứ liên quan đến việc liệt kê các đường dẫn đều không thể thực hiện được. 

Số đường đi từ`(1,1)`ĐẾN`(n,m)`là$$\binom{n+m-2}{n-1},$$trở nên khổng lồ ngay cả đối với kích thước vừa phải. Cách tiếp cận trực tiếp mà lý giải về các đường dẫn riêng lẻ không thể hiệu quả. 

Trường hợp cạnh tinh tế xuất hiện khi một lớp được ghép nối với chính nó. 

Coi như:```
2 3
0 1 0
1 0 1
```Các lớp là:```
layer 0: (0,0)
layer 1: (0,1) (1,0)
layer 2: (0,2) (1,1)
layer 3: (1,2)
```Lớp`1`Và`2`được ghép đôi vì khoảng cách của chúng đến hai đầu bằng nhau. Chúng tôi chỉ xử lý mỗi cặp một lần. Việc thực hiện bất cẩn sẽ xử lý cả hai`(1,2)`Và`(2,1)`sẽ tăng gấp đôi số lượng thay đổi. 

Một trường hợp quan trọng khác là lớp bắt đầu và lớp kết thúc. 

Ví dụ:```
2 2
0 1
1 1
```Các ô đầu tiên và cuối cùng không bao giờ được đưa vào nhóm tối ưu hóa. Mọi con đường luôn bắt đầu và kết thúc ở đó. Cặp lớp`(0,2)`không được xử lý vì những lớp đó nằm ở phần cuối của bảng màu. Giải pháp tiêu chuẩn đương nhiên bỏ qua chúng. 

Bẫy thứ ba là lớp giữa khi tổng chiều dài đường đi là số lẻ. 

Ví dụ:```
3 3
0 1 0
1 1 1
0 1 0
```Lớp`2`là trung tâm chính xác của mọi con đường. Một palindrome không đặt hạn chế ở vị trí trung tâm, do đó lớp này đóng góp chi phí bằng không. Việc xử lý nó sẽ thêm các lần lật không chính xác. 

## Phương pháp tiếp cận 

Một cách mạnh mẽ để suy nghĩ về vấn đề này là kiểm tra mọi đường đi từ góc trên bên trái đến góc dưới bên phải và đảm bảo rằng chuỗi dọc theo đường đi đó là một bảng màu. 

Lý do này là đúng, nhưng số lượng đường dẫn tăng theo cấp số nhân. Đối với một`30 × 30`lưới, số lượng là$$\binom{58}{29},$$lớn về mặt thiên văn. Ngay cả việc tạo ra tất cả các đường dẫn là không thể. 

Quan sát quan trọng là tính tương xứng chỉ phụ thuộc vào các vị trí trong đường dẫn chứ không phụ thuộc vào tuyến đường cụ thể được thực hiện. 

Mỗi ô thuộc về một lớp được xác định bởi$$d = i + j.$$Tất cả các ô trong cùng một lớp xuất hiện ở cùng một vị trí trong mọi đường dẫn đến thăm chúng. 

Giả sử vị trí đường dẫn ở lớp`d`. Vị trí được nhân đôi của nó trong bảng màu là ở lớp$$(n+m-2)-d.$$Để mọi đường dẫn đều là palindromic, mọi ô trong lớp`d`và mọi ô trong lớp$$(n+m-2)-d$$phải chứa cùng một giá trị. 

Tại sao điều đó lại cần thiết? 

Chọn bất kỳ ô nào từ lớp`d`và bất kỳ ô nào từ lớp được nhân đôi của nó. Tồn tại một con đường đi qua cả hai. Vị trí của chúng trở nên đối xứng trên đường đi đó nên giá trị của chúng phải khớp nhau. 

Điều này có nghĩa là mỗi cặp lớp được nhân đôi tạo thành một nhóm độc lập. Mọi ô bên trong hai lớp đó cuối cùng phải trở thành tất cả`0`hoặc tất cả`1`. 

Đối với một nhóm có chứa`z`số không và`o`chi phí tối thiểu chỉ đơn giản là$$\min(z, o).$$Chúng ta hoặc lật tất cả số 0 thành số 1 hoặc tất cả số 1 thành số 0. 

Vì các cặp lớp khác nhau không bao giờ tương tác nên chúng tôi tính tổng các chi phí này một cách độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ của số đường dẫn | Hàm mũ | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`L = n + m - 2`, chỉ số lớp tối đa. 
2. Nhóm tất cả các ô theo lớp của chúng`d = i + j`. 
3. Đối với mỗi chỉ mục lớp`d`, lớp được nhân đôi của nó là`L - d`. 
4. Chỉ xử lý các cặp có`d < L - d`. Điều này tránh việc đếm hai lần và tự động bỏ qua lớp giữa khi nó tự phản chiếu. 
5. Thu thập tất cả các ô thuộc lớp`d`và lớp`L - d`. 
6. Đếm xem có bao nhiêu giá trị`0`và có bao nhiêu`1`bên trong nhóm kết hợp này. 
7. Cách rẻ nhất để khiến cả nhóm bình đẳng là lật đổ giá trị thiểu số. Thêm vào`min(zeros, ones)`để trả lời. 
8. Lặp lại cho mỗi cặp lớp được nhân đôi. 
9. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Đối với bất kỳ lớp nào`d`, mọi vị trí đối xứng trong một đường dẫn đều tương ứng với lớp`L-d`. Xem xét bất kỳ ô nào từ lớp đầu tiên và bất kỳ ô nào từ lớp thứ hai. Một đường dẫn có thể được xây dựng để truy cập cả hai ô, đặt chúng vào các vị trí phản chiếu của đường dẫn đó. 

Nếu hai ô như vậy có giá trị khác nhau thì đường đi đó sẽ không có tính chất palindromic. Do đó, mọi ô trong liên kết của hai lớp phải chia sẻ một giá trị chung. 

Ràng buộc đối với một cặp lớp được nhân đôi không liên quan đến các ô từ bất kỳ cặp nào khác. Mỗi cặp có thể được tối ưu hóa độc lập. Việc chọn giá trị đa số sẽ giảm thiểu các lần lật trong nhóm đó, mang lại`min(zeros, ones)`những thay đổi. Tổng hợp các chi phí tối ưu độc lập này sẽ tạo ra mức tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        layers = [[] for _ in range(n + m - 1)]

        for i in range(n):
            for j in range(m):
                layers[i + j].append(a[i][j])

        L = n + m - 2
        ans = 0

        for d in range((L + 1) // 2):
            mirror = L - d

            zeros = 0
            ones = 0

            for x in layers[d]:
                if x == 0:
                    zeros += 1
                else:
                    ones += 1

            for x in layers[mirror]:
                if x == 0:
                    zeros += 1
                else:
                    ones += 1

            ans += min(zeros, ones)

        print(ans)

solve()
```Phần đầu tiên xây dựng sự phân hủy lớp. Mỗi ô có cùng giá trị`i + j`thuộc cùng một lớp. 

Biến`L`là chỉ số lớp cuối cùng. Lớp`d`được ghép nối với lớp`L-d`. 

Vòng lặp chỉ chạy đến điểm giữa. Đây là chi tiết dễ gây ra sai sót nhất. Khi`d`đạt đến một lớp phản chiếu chính nó, không tồn tại ràng buộc nào và lớp đó phải bị bỏ qua. sử dụng```
range((L + 1) // 2)
```xử lý chính xác các cặp được nhân đôi hợp lệ. 

Đối với mỗi cặp, mã sẽ đếm số 0 và số 1 trên cả hai lớp cùng nhau. Vì nhóm cuối cùng phải trở nên đồng nhất nên lựa chọn tối ưu là giữ lại giá trị nào đã xuất hiện thường xuyên hơn. 

Không có vấn đề tràn tồn tại vì lưới chứa nhiều nhất`900`tế bào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 1 0
1 0 0
```Lớp: 

| Lớp | Tế bào | Giá trị | 
| --- | --- | --- | 
| 0 | (0,0) | 1 | 
| 1 | (0,1),(1,0) | 1,1 | 
| 2 | (0,2),(1,1) | 0,0 | 
| 3 | (1,2) | 0 | 

Đây`L = 3`. 

| d | Gương | Giá trị kết hợp | Số không | Những cái | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | 1,0 | 1 | 1 | 1 | 
| 1 | 2 | 1,1,0,0 | 2 | 2 | 2 | 

Tổng số câu trả lời =`1 + 2 = 3`. 

Điều này chứng tỏ rằng giải pháp hoạt động hoàn toàn ở cấp độ lớp. Chúng tôi không bao giờ cần phải kiểm tra các đường dẫn riêng lẻ. 

### Ví dụ 2 

đầu vào:```
3 5
1 0 1 0 0
1 1 1 1 0
0 0 1 0 0
```Lớp: 

| Lớp | Giá trị | 
| --- | --- | 
| 0 | 1 | 
| 1 | 0,1 | 
| 2 | 1,1,0 | 
| 3 | 0,1,0 | 
| 4 | 0,1,1 | 
| 5 | 0,0 | 
| 6 | 0 |`L = 6`. 

| d | Gương | Giá trị kết hợp | Số không | Những cái | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 6 | 1,0 | 1 | 1 | 1 | 
| 1 | 5 | 0,1,0,0 | 3 | 1 | 1 | 
| 2 | 4 | 1,1,0,0,1,1 | 2 | 4 | 2 | 

Tổng số câu trả lời =`1 + 1 + 2 = 4`. 

Lớp giữa`3`bị bỏ qua vì nó phản chiếu chính nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được truy cập với số lần không đổi | 
| Không gian | O(nm) | Lưu trữ lớp chứa tất cả các ô một lần | 
| | | | 

Lưới lớn nhất chỉ chứa`900`tế bào. Ngay cả với`200`trường hợp thử nghiệm, tổng công việc vẫn còn nhỏ. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        layers = [[] for _ in range(n + m - 1)]

        for i in range(n):
            for j in range(m):
                layers[i + j].append(a[i][j])

        L = n + m - 2
        ans = 0

        for d in range((L + 1) // 2):
            mirror = L - d

            zeros = 0
            ones = 0

            for x in layers[d]:
                if x == 0:
                    zeros += 1
                else:
                    ones += 1

            for x in layers[mirror]:
                if x == 0:
                    zeros += 1
                else:
                    ones += 1

            ans += min(zeros, ones)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""4
2 2
1 1
0 1
2 3
1 1 0
1 0 0
3 7
1 0 1 1 1 1 1
0 0 0 0 0 0 0
1 1 1 1 1 0 1
3 5
1 0 1 0 0
1 1 1 1 0
0 0 1 0 0
"""
) == "0\n3\n4\n4"

# minimum size
assert run(
"""1
2 2
0 0
0 0
"""
) == "0"

# all ones
assert run(
"""1
3 3
1 1 1
1 1 1
1 1 1
"""
) == "0"

# middle layer should be ignored
assert run(
"""1
3 3
0 0 0
0 1 0
0 0 0
"""
) == "0"

# catches double counting of mirrored pairs
assert run(
"""1
2 3
1 1 0
1 0 0
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2×2 tất cả số không | 0 | Kích thước tối thiểu | 
| 3×3 tất cả những cái | 0 | Ma trận đã hợp lệ | 
| 3×3 chỉ khác tâm | 0 | Lớp giữa phải được bỏ qua | 
| mẫu 2×3 | 3 | Logic ghép nối lớp được nhân đôi | 

## Vỏ cạnh 

### Lớp giữa phản chiếu chính nó 

đầu vào:```
1
3 3
0 0 0
0 1 0
0 0 0
```Các lớp là:```
0, 1, 2, 3, 4
```Lớp`2`là trung tâm. Thuật toán chỉ xử lý các cặp`(0,4)`Và`(1,3)`. Lớp trung tâm bị bỏ qua hoàn toàn, tạo ra câu trả lời`0`. 

Giải pháp bao gồm lớp trung tâm sẽ đếm sai một lần lật. 

### Đếm đôi các cặp được nhân đôi 

đầu vào:```
1
2 3
1 1 0
1 0 0
```Các cặp hợp lệ chỉ là:```
(0,3)
(1,2)
```Thuật toán dừng lại trước khi đến nửa sau của lớp. Mỗi nhóm ràng buộc đóng góp chính xác một lần. 

Một vòng lặp trên tất cả các lớp sẽ xử lý`(1,2)`và sau này`(2,1)`, nhân đôi câu trả lời. 

### Ô bắt đầu và ô kết thúc khác nhau 

đầu vào:```
1
2 2
0 1
1 1
```Lớp:```
0 : [0]
1 : [1,1]
2 : [1]
```Cặp lớp duy nhất`(0,2)`vấn đề. Các giá trị kết hợp là`[0,1]`, vì vậy cần phải có một sự thay đổi. 

Thuật toán tính toán:```
zeros = 1
ones = 1
cost = 1
```và đầu ra`1`, đó là số lần lật tối thiểu cần thiết để làm cho mọi đường đi trở thành đối xứng.
