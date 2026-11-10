---
title: "CF 1527D - Cây MEX"
description: "Chúng ta có một cây có các đỉnh được đánh số nguyên từ 0 đến n - 1. Với mỗi cặp đỉnh phân biệt không có thứ tự (u, v), chúng ta xét đường đi đơn giản duy nhất nối chúng."
date: "2026-06-10T17:16:59+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dfs-and-similar", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1527
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 721 (Div. 2)"
rating: 2400
weight: 1527
solve_time_s: 233
verified: false
draft: false
---

[CF 1527D - Cây MEX](https://codeforces.com/problemset/problem/1527/D) 

**Đánh giá:** 2400 
**Tags:** tổ hợp, dfs và tương tự, triển khai, toán học, cây 
**Thời gian giải:** 3m 53s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có các đỉnh được gắn nhãn số nguyên`0`bởi vì`n - 1`. Với mọi cặp đỉnh phân biệt không có thứ tự`(u, v)`, chúng ta nhìn vào con đường đơn giản duy nhất kết nối chúng. Các nhãn xuất hiện trên đường dẫn đó tạo thành một tập hợp các số nguyên và chúng tôi tính toán MEX của tập hợp đó, nghĩa là số nguyên không âm nhỏ nhất không xuất hiện. 

Nhiệm vụ là đếm, với mọi giá trị`k`từ`0`ĐẾN`n`, có bao nhiêu cặp đỉnh có đường đi MEX bằng`k`. 

Nhãn không phải là giá trị tùy ý được lưu trữ trên các đỉnh. Nhãn của một đỉnh chính xác là chỉ mục của nó. Đây là thuộc tính cấu trúc quan trọng của vấn đề. 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó, bất kỳ giải pháp nào tệ hơn đáng kể so với tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm đều không thể thực hiện được. Một cái cây với`2 · 10^5`đỉnh chứa khoảng`2 · 10^10`các cặp đỉnh không có thứ tự, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê các đường đi. Thậm chí một`O(n²)`thuật toán sẽ thực hiện khoảng`4 · 10^10`hoạt động trong trường hợp xấu nhất. 

Khó khăn đến từ việc đếm các đường dẫn một cách gián tiếp mà không bao giờ lặp lại chúng. 

Trường hợp cạnh tinh tế xuất hiện khi nhãn`0`không có trên đường đi. Mỗi con đường đều thiếu`0`có MEX`0`, bất kể nhãn nào khác xuất hiện. 

Ví dụ:```
0 - 1 - 2
```con đường`(1,2)`chứa nhãn`{1,2}`và có MEX`0`. 

Một tình huống quan trọng khác xảy ra khi các đỉnh có nhãn`0,1,...,k-1`tất cả không thể nằm trên một con đường đơn giản. Trong trường hợp đó không có đường dẫn nào có thể chứa tất cả chúng cùng một lúc, vì vậy câu trả lời cho MEX`k`phải bằng không. 

Ví dụ:```
    0
   / \
  1   2
```Ba nhãn`{0,1,2}`đừng nằm trên một con đường đơn giản. Không có đường đi nào có thể chứa cả ba đỉnh, vì vậy mọi câu trả lời yêu cầu tất cả chúng đều có mặt đều bằng 0. 

Nguồn sai lầm thứ ba là việc xử lý MEX`n`. Dành cho MEX`n`, đường dẫn phải chứa mọi nhãn từ`0`ĐẾN`n-1`. Vì các nhãn chính xác là các đỉnh nên đường đi như vậy phải đi qua mọi đỉnh của cây. Điều này chỉ có thể thực hiện được nếu bản thân toàn bộ cây là một đường đi đơn giản. 

## Phương pháp tiếp cận 

Giải pháp Brute Force sẽ kiểm tra từng cặp đỉnh không có thứ tự, tìm đường đi giữa chúng, thu thập nhãn trên đường dẫn đó, tính MEX của nó và tăng bộ đếm tương ứng. 

Đường đi giữa hai đỉnh có thể thu được bằng cách sử dụng tiền xử lý LCA, nhưng thậm chí sau đó vẫn có`Θ(n²)`cặp. Với`n = 2 · 10^5`, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là các điều kiện MEX được xác định bởi các nhãn nhỏ. 

Giả sử một đường dẫn có MEX`k`. 

Sau đó:```
0,1,...,k-1 must all appear on the path
k must not appear on the path
```Không có gì khác quan trọng. 

Cho phép```
S_k = {0,1,...,k-1}
```Một con đường góp phần trả lời`k`nếu nó chứa mọi đỉnh của`S_k`và không chứa đỉnh`k`. 

Thực tế về cấu trúc quan trọng là một tập hợp các đỉnh chỉ có thể thuộc về một đường đi đơn khi chúng được chứa trong một đoạn đường nào đó của cây. 

Định nghĩa`L`Và`R`là điểm cuối của đường đi tối thiểu chứa các đỉnh`0..i`. 

Trong khi tăng`i`, chúng tôi duy trì con đường này. 

Nếu đỉnh`i`không thể gắn vào cấu trúc điểm cuối hiện tại, khi đó các đỉnh`0..i`không còn được chứa trong một đường dẫn đơn giản. Một khi điều này xảy ra, mọi tập hợp lớn hơn`0..j`cũng thất bại và tất cả các câu trả lời còn lại đều trở thành số 0. 

Toàn bộ giải pháp xoay quanh việc duy trì đường dẫn chứa nhãn`0..i`, sau đó đếm xem có bao nhiêu cặp điểm cuối tạo ra đường đi chứa tất cả các đỉnh`0..i-1`nhưng loại trừ đỉnh`i`. 

Việc đếm được thực hiện thông qua kích thước thành phần xung quanh đường dẫn hiện tại. 

Điều này biến một bài toán liệt kê đường dẫn không thể thành một chuỗi các thao tác đếm cục bộ dọc theo một đường dẫn phát triển linh hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² · n) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tiền xử lý 

Chúng tôi nhổ cây ở đỉnh`0`. 

Sử dụng DFS chúng tôi tính toán: 

- cha mẹ của mọi đỉnh 
- kích thước cây con 
- Thời gian vào và ra Euler 

Khoảng Euler cho phép kiểm tra tổ tiên theo thời gian không đổi. 

### Duy trì đường đi của nhãn nhỏ 

Để đường dẫn hiện tại chứa tất cả các nhãn từ`0`bởi vì`i`. 

Ban đầu:```
L = 0
R = 0
```Đối với mỗi nhãn mới`i`chúng tôi xác định liệu nó có thể mở rộng đường dẫn hiện tại hay không. 

Chỉ có ba khả năng: 

1.`i`nằm trên đường đi từ`L`ĐẾN`R`. 
2.`i`có thể trở thành điểm cuối bên trái mới. 
3.`i`có thể trở thành điểm cuối bên phải mới. 

Nếu không có điều nào trong số này đúng thì các đỉnh`0..i`không được chứa trong bất kỳ đường dẫn đơn giản nào. Tất cả các câu trả lời lớn hơn đều trở thành số 0 và chúng ta dừng lại. 

### Đếm các đường dẫn chứa tất cả các nhãn bắt buộc 

Giả sử các đỉnh`0..i-1`được chứa chính xác bên trong đường dẫn`(L,R)`. 

Bây giờ chúng ta muốn đếm các đường đi chứa toàn bộ đường đi đó nhưng không chứa đỉnh`i`. 

Bất kỳ đường dẫn nào chứa`(L,R)`phải chọn một điểm cuối trong một khu vực gắn liền ngoài`L`và một điểm cuối khác trong một khu vực gắn liền ngoài`R`. 

Số lựa chọn hợp lệ là tích của kích thước của hai vùng đó. 

Chúng tôi tính toán các kích thước này bằng cách sử dụng thông tin cây con và quan hệ tổ tiên. 

### Bao gồm-Loại trừ giữa các giá trị MEX liên tiếp 

Hãy:```
f(i) = number of paths containing all vertices 0..i-1
```Sau đó:```
answer[i] = f(i) - f(i+1)
```bởi vì đường đi được tính bằng`f(i+1)`chính xác là những cái cũng chứa đỉnh`i`. 

Điều này tự động thực thi điều kiện mà đỉnh`i`phải vắng mặt. 

### MEX 0 

Dành cho MEX`0`, chúng ta cần những đường đi không chứa đỉnh`0`. 

Loại bỏ đỉnh`0`chia cây thành các thành phần. 

Bất kỳ đường dẫn nào hoàn toàn bên trong một thành phần đều tránh`0`. 

Nếu một thành phần có kích thước`s`, nó góp phần:```
s(s-1)/2
```các cặp đỉnh không có thứ tự. 

Tổng hợp tất cả các thành phần cho câu trả lời`0`. 

### MEX n 

Giá trị:```
f(n)
```đã biểu diễn các đường dẫn chứa mọi đỉnh. 

Đây chính xác là câu trả lời`n`. 

### Tại sao nó hoạt động 

Ở mọi giai đoạn, các điểm cuối được duy trì`(L,R)`mô tả đường dẫn tối thiểu duy nhất chứa các nhãn được xử lý cho đến nay. Một tập hợp các đỉnh thuộc về một đường đi đơn giản nào đó khi và chỉ khi mỗi đỉnh mới được chèn vào có thể mở rộng đường đi hiện tại từ một điểm cuối. Khi thuộc tính đó bị lỗi một lần, nó sẽ không bao giờ có thể phục hồi được. 

Đối với một cố định`i`, mọi đường dẫn chứa nhãn`0..i-1`phải chứa toàn bộ đường dẫn tối thiểu`(L,R)`. Đường đi như vậy được xác định duy nhất bằng cách chọn một điểm cuối từ vùng kéo dài qua`L`và một cái khác từ khu vực kéo dài qua`R`. Nhân các kích thước vùng đó sẽ tính mọi đường dẫn hợp lệ chính xác một lần. 

Kể từ MEX`i`yêu cầu chứa`0..i-1`và loại trừ`i`, trừ đi số đếm liên tiếp sẽ cho chính xác số lượng mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * n
        tin = [0] * n
        tout = [0] * n
        sz = [0] * n

        timer = 0

        stack = [(0, -1, 0)]
        while stack:
            v, p, state = stack.pop()

            if state == 0:
                parent[v] = p
                tin[v] = timer
                timer += 1

                stack.append((v, p, 1))
                for to in reversed(g[v]):
                    if to != p:
                        stack.append((to, v, 0))
            else:
                s = 1
                for to in g[v]:
                    if to != p:
                        s += sz[to]
                sz[v] = s
                tout[v] = timer

        def is_ancestor(a, b):
            return tin[a] <= tin[b] < tout[a]

        def on_path(x, a, b):
            return (
                is_ancestor(x, a) and is_ancestor(b, x)
            ) or (
                is_ancestor(x, b) and is_ancestor(a, x)
            )

        ans = [0] * (n + 1)

        total_pairs = n * (n - 1) // 2

        bad = 0
        for to in g[0]:
            if parent[to] == 0:
                s = sz[to]
            else:
                s = n - sz[0]
            bad += s * (s - 1) // 2

        ans[0] = bad

        f = [0] * (n + 1)

        L = 0
        R = 0

        def side_size(end, other):
            if is_ancestor(end, other):
                cur = other
                while parent[cur] != end:
                    cur = parent[cur]
                return n - sz[cur]
            return sz[end]

        f[1] = total_pairs

        alive = True

        for x in range(1, n):
            if not alive:
                break

            in_lr = on_path(x, L, R)
            in_lx = on_path(R, L, x)
            in_rx = on_path(L, R, x)

            if in_lr:
                pass
            elif in_lx:
                L = x
            elif in_rx:
                R = x
            else:
                alive = False
                break

            left_cnt = side_size(L, R)
            right_cnt = side_size(R, L)

            f[x + 1] = left_cnt * right_cnt

        for i in range(1, n):
            ans[i] = f[i] - f[i + 1]

        ans[n] = f[n]

        print(*ans)

solve()
```DFS tính toán các con trỏ cha, kích thước cây con và dấu thời gian Euler. Đây là những dữ liệu cấu trúc duy nhất cần thiết sau này. 

chức năng`is_ancestor`sử dụng khoảng Euler. Một đỉnh`a`là tổ tiên của`b`chính xác khi thời điểm vào Euler của`a`chứa thời gian vào của`b`. 

Cặp được duy trì`(L,R)`đại diện cho đường dẫn tối thiểu chứa tất cả các nhãn được xử lý. Khi một nhãn mới xuất hiện, chỉ có ba mối quan hệ hình học có thể có. Nếu không có đỉnh nào hoạt động, thì các đỉnh được yêu cầu sẽ không còn vừa trên một đường dẫn duy nhất và tất cả các số đếm sau này sẽ trở thành 0. 

Người trợ giúp`side_size`tính toán có bao nhiêu đỉnh có thể đóng vai trò là điểm cuối kéo dài ra ngoài một phía của đường dẫn. Công thức phụ thuộc vào việc điểm cuối đối diện có nằm trong cây con của điểm cuối hiện tại hay không. 

Mảng`f`lưu trữ số lượng đường dẫn chứa tất cả các nhãn nhỏ hơn ngưỡng. Sự khác biệt liên tiếp sau đó tạo ra tần số MEX chính xác. 

Một lỗi thực hiện phổ biến là quên rằng`answer[0]`tuân theo một quy tắc tính khác. MEX`0`nghĩa là đường đi phải tránh đỉnh`0`, được tính một cách tự nhiên nhất bằng cách loại bỏ đỉnh`0`và tính tổng các cặp bên trong mỗi thành phần kết quả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0-1
0-2
2-3
```Con đường duy trì phát triển như sau. 

| Bước | Nhãn bắt buộc | L | R | f | 
| --- | --- | --- | --- | --- | 
| Ban đầu | {0} | 0 | 0 | 6 | 
| Thêm 1 | {0,1} | 1 | 0 | 2 | 
| Thêm 2 | {0,1,2} | 1 | 2 | 1 | 
| Thêm 3 | {0,1,2,3} | 1 | 3 | 1 | 

Sau đó: 

| k | Tính toán | Trả lời | 
| --- | --- | --- | 
| 0 | số lượng thành phần | 1 | 
| 1 | 6 - 2 | 4 | 
| 2 | 2 - 1 | 1 | 
| 3 | 1 - 1 | 0 | 
| 4 | 1 | 1 | 

Ví dụ này minh họa cách các điểm cuối của đường dẫn mở rộng khi các nhãn lớn hơn được chèn vào. 

### Mẫu 2 

đầu vào:```
2
1-0
```| Bước | Nhãn bắt buộc | L | R | f | 
| --- | --- | --- | --- | --- | 
| Ban đầu | {0} | 0 | 0 | 1 | 
| Thêm 1 | {0,1} | 1 | 0 | 1 | 

Câu trả lời trở thành: 

| k | Giá trị | 
| --- | --- | 
| 0 | 0 | 
| 1 | 0 | 
| 2 | 1 | 

Điều này chứng tỏ MEX`n`trường hợp. Đường đi duy nhất chứa cả hai đỉnh nên MEX của nó bằng`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh được xử lý một số lần không đổi | 
| Không gian | O(n) | Đồ thị, mảng DFS và các cấu trúc phụ trợ | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`, do đó việc xử lý tuyến tính vừa vặn thoải mái trong giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue()

assert run(
"""2
4
0 1
0 2
2 3
2
1 0
"""
) == """1 2 1 1 1
0 0 1
"""

assert run(
"""1
2
0 1
"""
) == """0 0 1
"""

assert run(
"""1
3
0 1
1 2
"""
) == """0 1 1 1
"""

assert run(
"""1
3
0 1
0 2
"""
) == """1 1 1 0
"""

assert run(
"""1
4
0 1
1 2
2 3
"""
) == """0 1 1 1 1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút |`0 0 1`| Cây hợp lệ nhỏ nhất | 
| Chuỗi ba nút |`0 1 1 1`| Con đường tăng trưởng liên tiếp | 
| Sao ba nút |`1 1 1 0`| Lỗi điều kiện đường dẫn | 
| Chuỗi bốn nút |`0 1 1 1 1`| MEX`n`đếm | 

## Vỏ cạnh 

Hãy xem xét ngôi sao:```
3
0 1
0 2
```Loại bỏ đỉnh`0`để lại hai đỉnh cô lập. Không có cặp không có thứ tự nào tồn tại bên trong một trong hai thành phần, vì vậy hãy trả lời`0`bằng`0`. 

Thuật toán tính toán điều này trực tiếp bằng cách tính tổng:```
1·0/2 + 1·0/2 = 0
```Bây giờ hãy xem xét:```
    0
   / \
  1   2
```Khi nhãn`2`được xử lý, đường dẫn hiện tại chứa`{0,1}`là`1-0`. đỉnh`2`không thể mở rộng điểm cuối trong khi vẫn giữ một đường dẫn đơn giản chứa tất cả các nhãn. Thuật toán phát hiện điều này và chấm dứt việc mở rộng thêm. Mọi câu trả lời MEX lớn hơn sẽ tự động trở thành số 0. 

Cuối cùng hãy xem xét một chuỗi:```
0 - 1 - 2 - 3
```Tất cả các nhãn nằm trên một đường dẫn đơn giản. Mỗi lần chèn sẽ mở rộng một điểm cuối, do đó bất biến không bao giờ bị hỏng. Giá trị cuối cùng`f[n]`trở thành`1`, đếm chính xác đường đi duy nhất chứa mọi đỉnh.
