---
title: "CF 1139C - Cây Sắc Nét"
description: "Chúng ta có một cái cây có các cạnh được tô màu đỏ (0) hoặc đen (1). Chúng ta phải đếm xem có bao nhiêu dãy có độ dài k gồm các đỉnh cây là \"tốt\". Đối với chuỗi [a₁, a₂, ..."
date: "2026-06-12T03:50:37+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1139
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 548 (Div. 2)"
rating: 1500
weight: 1139
solve_time_s: 86
verified: true
draft: false
---

[CF 1139C - Cây sắc nhọn](https://codeforces.com/problemset/problem/1139/C) 

**Đánh giá:** 1500 
**Tags:** dfs và tương tự, dsu, đồ thị, toán học, cây 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cái cây có các cạnh được tô màu đỏ (`0`) hoặc đen (`1`). Chúng ta phải đếm xem có bao nhiêu dãy dài`k`gồm các đỉnh cây là "tốt". 

Đối với một trình tự`[a₁, a₂, ..., aₖ]`, chúng ta liên tục di chuyển dọc theo con đường ngắn nhất duy nhất trên cây từ`a₁`ĐẾN`a₂`, thì từ`a₂`ĐẾN`a₃`, vân vân. Trình tự được coi là tốt nếu có ít nhất một cạnh đen đi qua đâu đó trong toàn bộ hành trình này. 

Cây có tới`10^5`đỉnh, trong khi`k`nhiều nhất là`100`. Tổng số dãy có thể là`n^k`, có giá trị lớn về mặt thiên văn. Ngay cả việc lặp lại tất cả các chuỗi là không thể. Vì bản thân cây có chứa`10^5`các đỉnh, bất kỳ giải pháp nào liên tục tính toán đường đi giữa các đỉnh cũng sẽ thất bại. Chúng ta cần một cái gì đó gần tuyến tính trong`n`. 

Khó khăn chính là mức độ tốt được xác định bằng sự hiện diện của ít nhất một cạnh màu đen trên tập hợp các đường dẫn do chuỗi tạo ra. Kiểm tra trực tiếp từng chuỗi là vô vọng nên chúng ta cần đếm chúng một cách gián tiếp. 

Trường hợp cạnh tinh tế xảy ra khi mọi cạnh đều có màu đen.```
4 2
1 2 1
2 3 1
3 4 1
```Trình tự duy nhất không tốt là`[1,1]`,`[2,2]`,`[3,3]`, Và`[4,4]`. Mọi chuyển động giữa các đỉnh phân biệt đều sử dụng ít nhất một cạnh màu đen. Giải pháp chỉ xem xét các thành phần được kết nối của các cạnh màu đen sẽ hoàn toàn bỏ sót cấu trúc. 

Một trường hợp quan trọng khác là khi mọi cạnh đều có màu đỏ.```
4 3
1 2 0
2 3 0
3 4 0
```Không có đường đi nào chứa cạnh đen nên đáp án là`0`. Việc triển khai bất cẩn khi đếm các chuỗi chạm vào nhiều đỉnh sẽ báo cáo sai câu trả lời tích cực. 

Một ví dụ tinh tế hơn là:```
5 2
1 2 0
2 3 0
3 4 1
4 5 0
```Đỉnh`{1,2,3}`tạo thành một thành phần kết nối chỉ có màu đỏ. Trình tự như`[1,3]`không tốt vì đường đi của chúng nằm hoàn toàn bên trong các cạnh màu đỏ. Điều này cũng đúng đối với`[5,5]`. Câu trả lời phụ thuộc vào kích thước của các thành phần chỉ có màu đỏ chứ không phụ thuộc vào từng đỉnh riêng lẻ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Tạo mọi chuỗi có độ dài`k`, mô phỏng bước đi và xác định xem có ít nhất một cạnh đi qua có màu đen hay không. 

có`n^k`trình tự. Với`n = 10^5`Và`k = 100`, con số này vượt xa mọi tính toán có thể tưởng tượng được. Ngay cả đối với các giá trị nhỏ, việc tính toán lặp đi lặp lại các đường dẫn trong cây sẽ rất tốn kém. Cách tiếp cận bạo lực là đúng vì nó áp dụng trực tiếp định nghĩa, nhưng nó sẽ không thể sử dụng được ngay lập tức. 

Để tìm thấy thứ gì đó nhanh hơn, hãy nghĩ về câu hỏi ngược lại. 

Thay vì đếm những chuỗi tốt, hãy đếm những chuỗi **không** tốt. 

Một chuỗi không chính xác khi mọi đường đi đều bao gồm toàn các cạnh màu đỏ. 

Hãy cân nhắc việc loại bỏ tất cả các cạnh màu đen khỏi cây. Đồ thị còn lại là tập hợp các thành phần liên thông được hình thành chỉ bởi các cạnh màu đỏ. 

Giả sử tất cả các đỉnh trong dãy đều thuộc cùng một thành phần màu đỏ. Khi đó mọi đường đi ngắn nhất giữa các đỉnh liên tiếp đều nằm hoàn toàn bên trong thành phần đó, vì vậy mọi cạnh đi qua đều có màu đỏ. Trình tự không tốt. 

Giả sử dãy chứa các đỉnh thuộc hai thành phần màu đỏ khác nhau. Bất kỳ đường dẫn nào giữa các thành phần đó phải đi qua ít nhất một cạnh bị loại bỏ, nghĩa là ít nhất một cạnh màu đen. Sau đó, trình tự là tốt. 

Điều này hoàn toàn đặc trưng cho các chuỗi xấu: 

Một dãy là xấu khi và chỉ khi tất cả các đỉnh của nó thuộc cùng một thành phần liên thông của đồ thị tạo bởi các cạnh màu đỏ. 

Nếu một thành phần màu đỏ có kích thước`s`, thì nó góp phần`s^k`trình tự xấu, bởi vì mọi vị trí trong trình tự có thể chọn bất kỳ đỉnh nào từ thành phần đó một cách độc lập. 

Tổng số dãy là`n^k`. 

Kể từ đây:`answer = n^k - Σ s^k`trong đó tổng chạy trên tất cả các thành phần được kết nối màu đỏ. 

Việc tìm kiếm các kích thước thành phần này thật dễ dàng với DFS, BFS hoặc DSU sau khi bỏ qua các cạnh màu đen. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k · n) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n + log k · số lượng thành phần) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ chỉ sử dụng các cạnh màu đỏ (`x = 0`). 

Các cạnh màu đen bị bỏ qua vì chúng phân tách các vùng chỉ có màu đỏ xác định trình tự xấu. 
2. Tìm tất cả các thành phần liên thông của biểu đồ chỉ màu đỏ này bằng DFS hoặc BFS. 

Mỗi thành phần đại diện cho một tập hợp các đỉnh tối đa được kết nối mà không sử dụng bất kỳ cạnh đen nào. 
3. Tính toán`total = n^k mod M`, Ở đâu`M = 10^9 + 7`. 

Đây là số lượng của tất cả các chuỗi có thể. 
4. Đối với mỗi thành phần màu đỏ có kích thước`s`, tính toán`s^k mod M`và trừ nó khỏi`total`. 

Mọi dãy có các đỉnh đều nằm bên trong thành phần này đều xấu. 
5. Thực hiện tất cả các modulo số học`10^9 + 7`. 
6. Xuất kết quả cuối cùng. 

### Tại sao nó hoạt động 

Sau khi loại bỏ các cạnh màu đen, các đỉnh được chia thành các thành phần liên kết màu đỏ. 

Nếu mọi đỉnh của dãy đều thuộc về cùng một thành phần thì mọi đường đi giữa các đỉnh liên tiếp vẫn hoàn toàn nằm trong thành phần đó và chỉ sử dụng các cạnh màu đỏ. Trình tự như vậy là xấu. 

Nếu một dãy chứa các đỉnh từ các thành phần khác nhau, thì một đường đi nào đó giữa các đỉnh được chọn liên tiếp phải đi qua cạnh đen, vì các thành phần màu đỏ riêng biệt bị ngắt kết nối mà không có cạnh đen. Trình tự như vậy là tốt. 

Các chuỗi xấu được phân chia theo thành phần chứa tất cả các đỉnh của chúng. Thành phần kích thước`s`đóng góp chính xác`s^k`trình tự xấu. Tính tổng tất cả các thành phần sẽ tính mỗi chuỗi xấu chính xác một lần. 

Trừ đi số lượng trình tự xấu khỏi tổng số trình tự sẽ để lại chính xác số lượng trình tự tốt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def main():
    n, k = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, x = map(int, input().split())
        u -= 1
        v -= 1

        if x == 0:
            g[u].append(v)
            g[v].append(u)

    visited = [False] * n

    ans = pow(n, k, MOD)

    for start in range(n):
        if visited[start]:
            continue

        stack = [start]
        visited[start] = True
        size = 0

        while stack:
            v = stack.pop()
            size += 1

            for to in g[v]:
                if not visited[to]:
                    visited[to] = True
                    stack.append(to)

        ans = (ans - pow(size, k, MOD)) % MOD

    print(ans)

if __name__ == "__main__":
    main()
```Biểu đồ chỉ lưu trữ các cạnh màu đỏ. Đây là quan sát trung tâm của giải pháp. Chúng ta không quan tâm đến việc đi qua các cạnh đen vì chúng chỉ tách các vùng tạo ra chuỗi xấu. 

DFS tính toán kích thước của mọi thành phần được kết nối màu đỏ. Vì đồ thị ban đầu là một cây nên đồ thị chỉ có màu đỏ chứa nhiều nhất`n - 1`các cạnh, do đó việc đi qua nó vẫn tuyến tính. 

Biến`ans`bắt đầu như`n^k`, số lượng của tất cả các chuỗi. Mỗi thành phần góp phần`size^k`chuỗi xấu sẽ bị trừ ngay lập tức. 

Sử dụng tính năng tích hợp sẵn của Python`pow(base, exp, mod)`là quan trọng. Các giá trị`n^k`Và`size^k`là rất lớn nếu không có lũy thừa mô đun. 

Phép trừ được thực hiện modulo`MOD`. Viết```
(ans - value) % MOD
```tránh các giá trị trung gian âm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 4
1 2 1
2 3 1
3 4 1
```Tất cả các cạnh đều có màu đen. 

| Thành phần | Kích thước | 
| --- | --- | 
| {1} | 1 | 
| {2} | 1 | 
| {3} | 1 | 
| {4} | 1 | 

| Bước | Giá trị | 
| --- | --- | 
| Tổng số trình tự | 4⁴ = 256 | 
| Trừ 1⁴ | 255 | 
| Trừ 1⁴ | 254 | 
| Trừ 1⁴ | 253 | 
| Trừ 1⁴ | 252 | 

Trả lời =`252`. 

Điều này chứng tỏ rằng chỉ những chuỗi không đổi vẫn còn xấu. Mọi chuyển động giữa các đỉnh phân biệt đều đi qua một cạnh màu đen. 

### Mẫu 2```
4 3
1 2 0
2 3 0
3 4 0
```Tất cả các cạnh đều có màu đỏ. 

| Thành phần | Kích thước | 
| --- | --- | 
| {1,2,3,4} | 4 | 

| Bước | Giá trị | 
| --- | --- | 
| Tổng số trình tự | 4³ = 64 | 
| Trừ 4³ | 0 | 

Trả lời =`0`. 

Điều này xác nhận rằng khi mọi đường dẫn chỉ có màu đỏ thì không có chuỗi nào có thể tốt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + c log k) | DFS truy cập mỗi đỉnh một lần, mỗi thành phần đóng góp một lũy thừa mô-đun | 
| Không gian | O(n) | Danh sách kề, mảng đã truy cập, ngăn xếp DFS | 

Đây`c`là số thành phần được kết nối màu đỏ, nhiều nhất là`n`. Từ`k ≤ 100`, lũy thừa mô đun thực tế là thời gian không đổi. Thuật toán tuyến tính theo kích thước của cây và dễ dàng phù hợp với giới hạn của`n = 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 7

def solve():
    input = sys.stdin.readline

    n, k = map(int, input().split())

    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, x = map(int, input().split())
        u -= 1
        v -= 1

        if x == 0:
            g[u].append(v)
            g[v].append(u)

    vis = [False] * n
    ans = pow(n, k, MOD)

    for i in range(n):
        if vis[i]:
            continue

        stack = [i]
        vis[i] = True
        sz = 0

        while stack:
            v = stack.pop()
            sz += 1

            for to in g[v]:
                if not vis[to]:
                    vis[to] = True
                    stack.append(to)

        ans = (ans - pow(sz, k, MOD)) % MOD

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old
    return out.getvalue()

# sample 1
assert run(
"""4 4
1 2 1
2 3 1
3 4 1
"""
) == "252\n"

# all red
assert run(
"""4 3
1 2 0
2 3 0
3 4 0
"""
) == "0\n"

# minimum size, black edge
assert run(
"""2 2
1 2 1
"""
) == "2\n"

# minimum size, red edge
assert run(
"""2 2
1 2 0
"""
) == "0\n"

# two red components of size 2
assert run(
"""4 2
1 2 0
2 3 1
3 4 0
"""
) == "8\n"

# single vertex components
assert run(
"""3 2
1 2 1
2 3 1
"""
) == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 đỉnh, cạnh đen | 2 | Trường hợp tốt nhỏ nhất không tầm thường | 
| 2 đỉnh, cạnh đỏ | 0 | Toàn bộ cây là một thành phần màu đỏ | 
| Hai thành phần màu đỏ cỡ 2 | 8 | Đếm đúng nhiều thành phần | 
| Tất cả các cạnh màu đen trên 3 đỉnh | 6 | Mỗi thành phần đơn lẻ đóng góp chính xác một chuỗi xấu | 

## Vỏ cạnh 

Hãy xem xét một cây bao gồm toàn bộ các cạnh màu đen.```
4 2
1 2 1
2 3 1
3 4 1
```Loại bỏ các cạnh màu đen để lại bốn đỉnh bị cô lập. Kích thước thành phần là`1,1,1,1`. 

Thuật toán tính toán:```
4² - 1² - 1² - 1² - 1²
= 16 - 4
= 12
```Trình tự xấu duy nhất là`[1,1]`,`[2,2]`,`[3,3]`, Và`[4,4]`. Mọi dãy khác đều di chuyển qua ít nhất một cạnh màu đen. 

Bây giờ hãy xem xét một cây bao gồm toàn bộ các cạnh màu đỏ.```
4 2
1 2 0
2 3 0
3 4 0
```Việc loại bỏ các cạnh màu đen không thay đổi gì cả. Có một thành phần duy nhất có kích thước`4`. 

Thuật toán tính toán:```
4² - 4² = 0
```Mọi đường dẫn đều chỉ có màu đỏ nên không có chuỗi nào là tốt. 

Cuối cùng, hãy xem xét màu sắc hỗn hợp.```
5 2
1 2 0
2 3 0
3 4 1
4 5 0
```Các thành phần màu đỏ có kích thước`3`Và`2`. 

Thuật toán tính toán:```
5² - 3² - 2²
= 25 - 9 - 4
= 12
```Các dãy xấu chính xác là các dãy có hai đỉnh đều thuộc về`{1,2,3}`hoặc cả hai đều thuộc về`{4,5}`. Bất kỳ chuỗi nào nối hai thành phần đều phải vượt qua cạnh đen`(3,4)`, làm cho nó tốt. Đây chính xác là thuộc tính mà giải pháp có giá trị.
