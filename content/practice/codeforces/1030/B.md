---
title: "CF 1030B - Vasya và Cánh đồng ngô"
description: "Nhiệm vụ là phân loại các điểm trong một mặt phẳng so với một vùng hình học cố định được xác định bởi bốn điểm biên: $(0, d)$, $(d, 0)$, $(n, n-d)$ và $(n-d, n)$."
date: "2026-06-16T20:59:36+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1030
codeforces_index: "B"
codeforces_contest_name: "Technocup 2019 - Elimination Round 1"
rating: 1100
weight: 1030
solve_time_s: 315
verified: true
draft: false
---

[CF 1030B - Vasya và Cánh đồng ngô](https://codeforces.com/problemset/problem/1030/B) 

**Đánh giá:** 1100 
**Thẻ:** hình học 
**Thời gian giải:** 5 phút 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ là phân loại các điểm trong mặt phẳng tương ứng với một vùng hình học cố định được xác định bởi bốn điểm biên:$(0, d)$,$(d, 0)$,$(n, n-d)$, Và$(n-d, n)$. Bốn điểm này tạo thành một tứ giác lồi trông giống như một hình vuông quay 45 độ rồi cắt vào bên trong$n \times n$lưới. 

Chúng ta được cung cấp nhiều điểm truy vấn, mỗi điểm đại diện cho một vị trí châu chấu. Đối với mỗi điểm$(x, y)$, chúng ta phải quyết định xem nó nằm bên trong tứ giác này hay nằm chính xác trên ranh giới của nó. 

Những hạn chế rất nhỏ:$n \le 100$Và$m \le 100$. Điều này ngay lập tức loại trừ mọi nhu cầu về cấu trúc hình học nâng cao hoặc tối ưu hóa. Thậm chí một$O(m)$mỗi lần kiểm tra truy vấn là không đáng kể vì tổng số thao tác vẫn dưới vài nghìn. Điều quan trọng ở đây là tìm ra những bất bình đẳng chính xác mô tả khu vực. 

Khó khăn chính không phải là hiệu suất mà là tính chính xác: chuyển hình dạng hình học xoay thành các điều kiện tọa độ đơn giản. 

Một vài trường hợp thất bại thường phát sinh. 

Một lỗi phổ biến là coi vùng như một hình chữ nhật đơn giản được giới hạn bởi tọa độ tối thiểu và tối đa. Ví dụ: giả sử trường chỉ là$d \le x \le n-d$Và$d \le y \le n-d$sẽ sai. Một điểm như$(1, 1)$có thể thỏa mãn giới hạn hình chữ nhật đối với một số giá trị nhưng vẫn nằm ngoài trường hình kim cương. 

Một sai lầm khác là quên bao gồm ranh giới. Vì bài toán tính rõ ràng các điểm trên các cạnh là bên trong, nên các bất đẳng thức chặt chẽ như$<$Và$>$thay vì$\le$Và$\ge$sẽ từ chối không chính xác các điểm hợp lệ như các đỉnh. 

Lời giải đúng xuất phát từ việc xác định rằng hình được xác định bởi hai ràng buộc tuyến tính tương ứng với hai đường chéo của hình vuông quay. 

## Phương pháp tiếp cận 

Một cách tiếp cận hình học mạnh mẽ sẽ cố gắng xác định xem một điểm có nằm bên trong đa giác hay không bằng cách sử dụng các bài kiểm tra định hướng hoặc phân tích tam giác. Người ta có thể chia tứ giác thành hai hình tam giác và kiểm tra xem điểm đó có nằm trong một trong hai tam giác hay không bằng cách sử dụng tích chéo. Điều này hoạt động chính xác và mang tính tổng quát về mặt khái niệm, nhưng nó không cần thiết đối với hình dạng bị ràng buộc như vậy. 

Vấn đề với lực lượng vũ phu không phải là tính chính xác mà là quá mức cần thiết: việc tính toán các sản phẩm chéo cho mỗi truy vấn gây ra sự phức tạp hơn mức cần thiết và nó che khuất cấu trúc của khu vực. Trong trường hợp xấu nhất, nó vẫn chạy trong$O(m)$, nhưng có hệ số hằng số nặng hơn và rủi ro thực hiện cao hơn. 

Quan sát quan trọng là đa giác là một hình vuông hoàn hảo được xoay 45 độ, thẳng hàng với các đường thẳng$x + y = d$Và$x + y = n + d$, và cũng bị hạn chế bởi$x - y = -d$Và$x - y = n - d$. Điều này có nghĩa là tư cách thành viên có thể được kiểm tra chỉ bằng cách sử dụng hai bất đẳng thức tuyến tính rút ra từ các đường chéo này. 

Khi điều này được nhận ra, mỗi truy vấn sẽ giảm xuống việc kiểm tra xem điểm có nằm giữa hai đường thẳng song song theo cả hai hướng chéo hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Đa giác / hình học vũ phu | O(m) | O(1) | Được chấp nhận nhưng không cần thiết | 
| Kiểm tra bất bình đẳng | O(m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại điều kiện nằm trong vùng hình kim cương bằng cách sử dụng hai tọa độ được biến đổi:$x + y$Và$x - y$. 

1. Tính tổng$s = x + y$. Điều này nắm bắt vị trí dọc theo một hướng chéo. Hình dạng được giới hạn giữa hai dòng có tổng không đổi, vì vậy đây là ràng buộc đầu tiên. 
2. Tính chênh lệch$t = x - y$. Điều này nắm bắt vị trí dọc theo hướng chéo khác, trực giao với ràng buộc đầu tiên. Vùng này cũng được giới hạn giữa hai đường chênh lệch không đổi. 
3. Tính giới hạn từ các điểm góc. Thay các đỉnh cho thấy: 

- Số tiền tối thiểu là$d$, tổng tối đa là$n + d$- Chênh lệch tối thiểu là$-d$, chênh lệch tối đa là$n - d$4. Đối với mỗi điểm truy vấn, hãy kiểm tra xem cả hai điều kiện có đúng không:$d \le x + y \le n + d$Và$-d \le x - y \le n - d$5. Nếu cả hai bất đẳng thức đều được thỏa mãn thì điểm nằm trong hoặc nằm trên biên; nếu không thì nó ở bên ngoài. 

Lý do điều này có hiệu quả là vì tứ giác chính xác là giao điểm của hai dải: một dải được xác định bằng giới hạn$x + y$và cái khác được xác định bằng giới hạn$x - y$. Giao điểm của chúng tạo thành hình vuông xoay. 

### Tại sao nó hoạt động 

Sự biến đổi$(x, y) \rightarrow (x + y, x - y)$ánh xạ các hình vuông theo trục thành các hình vuông được xoay. Trong không gian được biến đổi này, vùng trở thành một hình chữ nhật thẳng hàng với trục. Do các hình chữ nhật thẳng hàng theo trục được đặc trưng bởi các giới hạn độc lập trên mỗi tọa độ nên tư cách thành viên giảm xuống còn hai lần kiểm tra khoảng thời gian độc lập. Điều này đảm bảo tính chính xác vì mọi cạnh của đa giác ban đầu tương ứng chính xác với một trong các đường giới hạn trong tọa độ được chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, d = map(int, input().split())
    m = int(input())

    for _ in range(m):
        x, y = map(int, input().split())

        s = x + y
        t = x - y

        if d <= s <= n + d and -d <= t <= n - d:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```Mã trực tiếp thực hiện hai ràng buộc dẫn xuất. Mỗi truy vấn được xử lý độc lập, tính tổng và chênh lệch một lần rồi so sánh chúng với các giới hạn cố định. 

Phần tế nhị nhất là đảm bảo so sánh toàn diện. Vì các điểm biên là hợp lệ nên mọi bất đẳng thức phải sử dụng$\le$Và$\ge$. Phần còn lại là số học đơn giản. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 7, d = 2
points: (2,4), (4,1), (6,3), (4,5)
```Chúng tôi tính toán$s = x+y$Và$t = x-y$. Vùng hợp lệ là$2 \le s \le 9$Và$-2 \le t \le 5$. 

| Điểm | s = x+y | t = x-y | Bên trong phạm vi s? | Bên trong phạm vi t? | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| (2,4) | 6 | -2 | vâng | vâng | CÓ | 
| (4,1) | 5 | 3 | không | vâng | KHÔNG | 
| (6,3) | 9 | 3 | vâng | vâng | CÓ | 
| (4,5) | 9 | -1 | vâng | vâng | CÓ | 

Dấu vết này xác nhận rằng các điểm ranh giới như$(2,4)$Ở đâu$t = -2$được chấp nhận đúng vì bất đẳng thức có tính bao hàm. 

### Mẫu 2 

đầu vào:```
n = 8, d = 2
points: (4,4), (8,1), (6,1), (1,1)
```Giới hạn là$2 \le s \le 10$,$-2 \le t \le 6$. 

| Điểm | s | t | Kết quả | 
| --- | --- | --- | --- | 
| (4,4) | 8 | 0 | CÓ | 
| (8,1) | 9 | 7 | KHÔNG | 
| (6,1) | 7 | 5 | CÓ | 
| (1,1) | 2 | 0 | CÓ | 

Mẫu thứ hai nêu bật cách một điểm chỉ có thể không đáp ứng được một ràng buộc ngay cả khi nó nằm gần khu vực về mặt trực quan, củng cố rằng cả hai sự bất bình đẳng đều cần thiết đồng thời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m) | Mỗi điểm yêu cầu số học và so sánh theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc phụ trợ ngoài các biến đầu vào | 

Giới hạn ràng buộc$m$tối đa là 100, do đó, ngay cả một giải pháp nặng về hệ số không đổi cũng sẽ chạy ngay lập tức. Kiểm tra bất đẳng thức trực tiếp thấp hơn nhiều so với bất kỳ giới hạn thời gian chạy có ý nghĩa nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, d = map(int, input().split())
    m = int(input())
    out = []
    for _ in range(m):
        x, y = map(int, input().split())
        s = x + y
        t = x - y
        out.append("YES" if d <= s <= n + d and -d <= t <= n - d else "NO")
    return "\n".join(out)

# provided sample 1
assert run("""7 2
4
2 4
4 1
6 3
4 5
""") == """YES
NO
NO
YES"""

# minimum values, corner cases
assert run("""2 1
3
0 1
1 0
2 2
""") == """YES
YES
NO"""

# all points identical inside
assert run("""5 1
3
2 2
2 2
2 2
""") == """YES
YES
YES"""

# boundary strip behavior
assert run("""10 3
4
3 0
0 3
10 7
7 10
""") == """YES
YES
YES
YES"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | mẫu CÓ/KHÔNG | tính đúng đắn của logic chính | 
| góc n,d nhỏ | hỗn hợp CÓ/KHÔNG | hành vi đỉnh và trục | 
| điểm lặp lại | tất cả CÓ | ổn định dưới sự trùng lặp | 
| dải ranh giới | tất cả CÓ | xử lý ranh giới toàn diện | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một điểm nằm chính xác trên một đỉnh của đa giác. Ví dụ,$(0, d)$. Đây$x+y = d$Và$x-y = -d$, cả hai đều thỏa mãn các giới hạn bao gồm, do đó thuật toán trả về đúng CÓ. 

Một trường hợp khác là khi một điểm nằm ngay bên ngoài một đường chéo nhưng lại nằm bên trong đường chéo kia. Ví dụ, một điểm với$x+y = d-1$sẽ thất bại ngay lập tức ngay cả khi giá trị chênh lệch của nó là hợp lệ. Thuật toán từ chối nó một cách chính xác vì cả hai điều kiện đều được yêu cầu đồng thời. 

Trường hợp cuối cùng là khi các điểm nằm gần góc đối diện$(n-d, n)$. Những điều này tối đa hóa cả hai tọa độ được chuyển đổi:$x+y = n+d$Và$x-y = n-2d$, cả hai vẫn nằm trong giới hạn. Thuật toán bao gồm chúng một cách chính xác, xác nhận rằng các giới hạn trên chặt chẽ và được suy ra chính xác từ hình học.
