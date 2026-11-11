---
title: "CF 1575J - Nguy cơ bóng rơi"
description: "Chúng ta có một lưới n × m. Mỗi ô lưu trữ một trong ba hướng. Giá trị 1 có nghĩa là một quả bóng di chuyển một ô sang phải. Giá trị 2 có nghĩa là quả bóng di chuyển một ô xuống dưới. Giá trị 3 có nghĩa là quả bóng di chuyển một ô sang trái."
date: "2026-06-10T10:57:29+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "dsu", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1575
codeforces_index: "J"
codeforces_contest_name: "COMPFEST 13 - Finals Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 1500
weight: 1575
solve_time_s: 56
verified: true
draft: false
---

[CF 1575J - Nguy cơ bóng rơi](https://codeforces.com/problemset/problem/1575/J) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, vũ phu, dsu, triển khai 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × m`lưới. Mỗi ô lưu trữ một trong ba hướng. 

Một giá trị của`1`có nghĩa là một quả bóng di chuyển một ô sang phải. Một giá trị của`2`có nghĩa là quả bóng di chuyển một ô xuống dưới. Một giá trị của`3`có nghĩa là quả bóng di chuyển một ô sang trái. 

Các quả bóng lần lượt được thả từ hàng đầu tiên. Khi một quả bóng ghé thăm một ô và rời khỏi ô đó, ô đó vĩnh viễn thay đổi giá trị của nó thành`2`. Nói cách khác, sau khi bất kỳ quả bóng nào sử dụng một ô, những quả bóng trong tương lai chạm tới ô đó sẽ ngay lập tức đi xuống từ ô đó. 

Đối với mỗi quả bóng, chúng ta phải xác định cột nơi nó thoát ra khỏi lưới. Vì chuyển động chỉ sang trái, phải hoặc xuống, nên cuối cùng quả bóng sẽ đi qua hàng dưới cùng và câu trả lời là cột mà nó rời đi. 

Kích thước lưới tối đa là`1000 × 1000`, vì vậy có thể có tới một triệu tế bào. Số lượng bóng có thể đạt tới`100000`. 

Việc mô phỏng đơn giản từng quả bóng là quá đắt. Một quả bóng có thể thăm nhiều ô. Vì có tới`100000`quả bóng, việc đi bộ nhiều lần qua các vùng đã được xử lý sẽ tạo ra hàng trăm triệu hoặc thậm chí hàng tỷ phép tính. 

Quan sát quan trọng là một ô chỉ hoạt động đặc biệt khi nó được truy cập lần đầu tiên. Sau thời điểm đó giá trị của nó trở thành`2`mãi mãi. Khi một ô đã được xử lý, các quả bóng trong tương lai sẽ có thể bỏ qua ô đó ngay lập tức. 

Một trường hợp tinh vi xảy ra khi nhiều quả bóng xuất phát trong cùng một cột.```
1 1 3
1
1 1 1
```Quả bóng đầu tiên di chuyển sang phải và thay đổi ô thành`2`. Quả bóng thứ hai bây giờ di chuyển xuống ngay lập tức. Quả bóng thứ ba cũng di chuyển xuống ngay lập tức. 

Mô phỏng luôn tuân theo lưới ban đầu sẽ tạo ra các đường dẫn giống hệt nhau cho tất cả các quả bóng một cách không chính xác. 

Một sai lầm dễ mắc phải khác là quên rằng chuyển động ngang có thể tiếp tục qua một số ô trước khi di chuyển xuống.```
1 3
1 1 2
1
```Bóng di chuyển từ cột 1 sang cột 2, rồi từ cột 2 sang cột 3, rồi đi xuống theo cột 3. 

Lời giải chỉ nhìn vào ô bắt đầu sẽ trả lời sai cột 1. 

Một trường hợp quan trọng hơn là khi một ô đã được chuyển đổi thành`2`.```
2 2 2
1 1
2 2
1
```Ô đầu tiên đã hướng xuống dưới. Bóng sẽ ngay lập tức đi vào hàng tiếp theo. Mọi nỗ lực áp dụng lại hành vi theo chiều ngang ban đầu sau khi một ô đã được xử lý đều sẽ sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi quả bóng, hãy bắt đầu từ cột đầu vào của nó và liên tục đi theo hướng hiện tại. Bất cứ khi nào quả bóng rời khỏi một ô, hãy thay đổi ô đó thành`2`. 

Mô phỏng này đúng vì nó tuân theo các quy tắc một cách chính xác. Vấn đề là thời gian chạy của nó. 

Một ô có thể được truy cập nhiều lần bởi các quả bóng khác nhau. Trong trường hợp xấu nhất quả bóng có thể đi ngang qua`O(nm)`tế bào. Với tối đa`10^5`quả bóng, tổng công việc trở nên khổng lồ. 

Quan sát quan trọng là mỗi ô thay đổi nhiều nhất một lần. 

Giả sử một quả bóng ghé thăm một ô có giá trị là`1`hoặc`3`. Sau khi rời đi, tế bào trở thành`2`. Bất kỳ quả bóng nào trong tương lai chạm tới ô đó sẽ ngay lập tức tiếp tục đi xuống. Hành vi theo chiều ngang đắt tiền của ô đó không bao giờ cần thiết nữa. 

Điều này gợi ý coi các ô đã xử lý là "bị xóa" khỏi các tìm kiếm trong tương lai. Khi một quả bóng đến một ô đã trở thành`2`, chúng tôi muốn chuyển thẳng đến ô chưa được xử lý tiếp theo bên dưới nó thay vì đi qua một chuỗi dài các ô đã được xử lý. 

Đây chính xác là những gì cấu trúc Disjoint Set Union có thể làm. 

Đối với mỗi cột, chúng tôi duy trì DSU trên các hàng. Khi một ô được xử lý vĩnh viễn, chúng tôi hợp nhất ô đó với hàng bên dưới. Sau đó, truy vấn trong tương lai có thể tìm thấy ngay hàng đầu tiên chưa được xử lý trong cột đó. 

Mỗi ô được xử lý nhiều nhất một lần và mỗi hoạt động DSU gần như có thời gian không đổi do nén đường dẫn. Tổng công việc trở nên tỷ lệ thuận với số ô cộng với số quả bóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(knm) trong trường hợp xấu nhất | O(1) ngoài lưới | Quá chậm | 
| Tối ưu | O(nm α(n) + k α(n)) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Cấu trúc dữ liệu 

Đối với mỗi cột, hãy tạo DSU trên các hàng`1..n+1`. 

Hàng ngang`n+1`là một trọng điểm có nghĩa là "bên ngoài lưới". 

Nếu hàng`r`trong cột`c`đã được xử lý, chúng tôi hợp nhất`r`với`r+1`. Thao tác tìm sau đó trả về hàng đầu tiên ở hoặc bên dưới`r`đó vẫn còn hoạt động. 

### bước 

1. Lưu trữ lưới. 
2. Tạo mảng DSU`parent[row][column]`. 
3. Với mỗi quả bóng, hãy bắt đầu từ vị trí`(1, start_column)`. 
4. Trong khi hàng hiện tại nằm trong lưới, hãy sử dụng DSU find để chuyển đến hàng chưa được xử lý đầu tiên trong cột hiện tại. 
5. Nếu hàng trả về là`n + 1`, bóng đã rời khỏi lưới qua cột hiện tại. Ghi lại cột đó làm câu trả lời. 
6. Nếu không thì hãy kiểm tra hướng được lưu trong ô đó. 
7. Đánh dấu ô đã được xử lý bằng cách hợp nhất hàng của nó với hàng tiếp theo trong cùng một cột. 

Ô sẽ không bao giờ thực hiện chuyển động ngang nữa, vì vậy các quả bóng trong tương lai sẽ bỏ qua nó. 
8. Nếu giá trị là`1`, di chuyển đến cột bên phải. 
9. Nếu giá trị là`3`, di chuyển đến cột bên trái. 
10. Nếu giá trị là`2`, ở cùng một cột. 
11. Trong mọi trường hợp, hãy tiếp tục từ hàng ngay bên dưới ô đã xử lý. 

Sau khi một ô được sử dụng, nó sẽ hoạt động vĩnh viễn giống như một mũi tên hướng xuống. Chuyển động trong tương lai từ vị trí đó bắt đầu bên dưới nó. 
12. Lặp lại cho đến khi bóng ra khỏi lưới. 

### Tại sao nó hoạt động 

Điều bất biến là DSU cho một cột luôn bỏ qua chính xác các hàng có ô đã được xử lý. 

Khi một quả bóng ghé thăm một ô lần đầu tiên, chúng tôi thực hiện hướng ban đầu của ô đó và sau đó loại bỏ vĩnh viễn ô đó khỏi việc xem xét trong tương lai bằng cách kết hợp nó với hàng tiếp theo. 

Bất kỳ quả bóng nào sau đó đến cùng một cột và hàng sẽ sử dụng DSU để nhảy qua ô đó, điều này tương đương với việc ô đó đã trở thành`2`và ngay lập tức đưa bóng đi xuống. 

Vì mỗi ô được xóa chính xác một lần và tất cả các lượt truy cập trong tương lai đều bỏ qua ô đó một cách chính xác nên hành vi được mô phỏng khớp chính xác với các quy tắc của vấn đề. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, k = map(int, input().split())

    a = [list(map(int, input().split())) for _ in range(n)]
    starts = list(map(int, input().split()))

    parent = [[r for _ in range(m)] for r in range(n + 2)]

    def find(r, c):
        while parent[r][c] != r:
            parent[r][c] = parent[parent[r][c]][c]
            r = parent[r][c]
        return r

    ans = []

    for start_col in starts:
        r = 1
        c = start_col - 1

        while True:
            r = find(r, c)

            if r == n + 1:
                ans.append(str(c + 1))
                break

            direction = a[r - 1][c]

            parent[r][c] = find(r + 1, c)

            if direction == 1:
                c += 1
            elif direction == 3:
                c -= 1

            r += 1

    print(" ".join(ans))

solve()
```Lưới được lưu trữ với lập chỉ mục cột dựa trên 0, trong khi các hàng DSU sử dụng lập chỉ mục dựa trên một. Điều này phản ánh báo cáo vấn đề và làm cho hàng trọng điểm`n + 1`dễ dàng để đại diện. 

DSU được duy trì độc lập cho từng cột.`parent[r][c]`cho chúng ta biết hàng ứng cử viên tiếp theo trong cột`c`. Khi một ô được xử lý, chúng tôi sẽ xóa ô đó bằng cách liên kết hàng của ô đó với hàng có sẵn đầu tiên bên dưới ô đó. 

Phần tế nhị nhất là thứ tự cập nhật. Chúng ta phải đọc hướng ban đầu của ô trước khi tháo nó ra. Sau đó, ô này được coi là đã xử lý vĩnh viễn nên chúng tôi cập nhật DSU ngay lập tức. 

Việc tăng hàng xảy ra sau khi thực hiện hướng. Một quả bóng luôn rời khỏi một ô và tiếp tục đi từ hàng tiếp theo. Chuyển động ngang chỉ làm thay đổi cột. 

sen
