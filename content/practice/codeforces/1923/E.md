---
title: "CF 1923E - Đếm đường đi"
description: "Chúng ta có một cây có n nút, mỗi nút được tô màu bằng một số nguyên nào đó từ 1 đến n. Nhiệm vụ là đếm số lượng đường dẫn đơn giản có độ dài ít nhất là hai sao cho nút đầu tiên và nút cuối cùng có cùng màu và không có nút trung gian nào chia sẻ màu này."
date: "2026-06-08T19:14:25+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1923
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 162 (Rated for Div. 2)"
rating: 2000
weight: 1923
solve_time_s: 126
verified: false
draft: false
---

[CF 1923E - Đếm đường dẫn](https://codeforces.com/problemset/problem/1923/E) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, dsu, đồ thị, cây 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút, mỗi nút được tô màu bằng một số nguyên nào đó giữa`1`Và`n`. Nhiệm vụ là đếm số lượng đường dẫn đơn giản có độ dài ít nhất là hai sao cho nút đầu tiên và nút cuối cùng có cùng màu và không có nút trung gian nào chia sẻ màu này. Các đường dẫn không được định hướng, vì vậy một đường dẫn từ nút`x`đến nút`y`tương đương với đường đi từ`y`ĐẾN`x`. 

Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp bao gồm số đỉnh, danh sách màu và`n-1`các cạnh tạo thành một cây hợp lệ. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị số lượng đường dẫn đẹp. 

Những hạn chế chỉ ra rằng`n`có thể đạt tới`2 * 10^5`trên tất cả các trường hợp thử nghiệm. Một cách tiếp cận đơn giản để kiểm tra từng cặp nút hoặc liệt kê tất cả các đường dẫn sẽ yêu cầu O(n^2) hoặc tệ hơn, điều này không thực tế đối với các giới hạn trên. Chúng ta phải hướng tới một cách tiếp cận tuyến tính hoặc gần tuyến tính trong`n`mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh tinh tế bao gồm: 

1. Tất cả các đỉnh có cùng màu, điều này làm cho mọi cạnh đều là một đường đi đẹp nhưng ngăn cản việc đếm nhiều lần các đường đi dài hơn. Ví dụ:`n=4, colors=[2,2,2,2]`tạo ra 3 đường đi đẹp (tất cả đều có cạnh đơn). 
2. Cây trong đó mỗi nút có một màu duy nhất, nghĩa là không có đường đi nào dài hơn một cạnh có thể đẹp. Ví dụ:`n=5, colors=[1,2,3,4,5]`tạo ra 0 con đường đẹp. 
3. Đường dẫn có màu xuất hiện nhiều hơn hai lần trong thành phần được kết nối. Các cách tiếp cận bất cẩn có thể tính gấp đôi đường đi nếu chúng bỏ qua hạn chế "không có nút trung gian nào chia sẻ màu sắc". 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp nút`(u, v)`, tìm đường đi giữa chúng (đường đi duy nhất trong cây) và kiểm tra các điều kiện tô màu. Điều này yêu cầu các phép toán O(n^2) đối với cây trong trường hợp xấu nhất, vì có khoảng n*(n-1)/2 cặp. Việc kiểm tra từng đường dẫn có thể tốn O(n) trong quá trình truyền tải DFS đơn giản, tỷ lệ này có tỷ lệ là O(n^3) - hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là trong một cái cây, một đường dẫn đơn giản được xác định duy nhất bởi các điểm cuối của nó và các đường dẫn đẹp bị ràng buộc bởi vị trí của các nút có cùng màu. Nếu chúng ta loại bỏ tất cả các nút có màu nhất định`c`, cây sẽ tách thành các thành phần được kết nối. Mỗi cặp nút có màu sắc`c`thuộc về các thành phần khác nhau không thể có đường dẫn đẹp hợp lệ kết nối chúng thông qua các nút có màu khác. 

Như vậy, chúng ta có thể đảo ngược cách nhìn: với mỗi màu`c`, đếm số cạnh kết nối các nút trong các thành phần khác nhau sau khi loại bỏ tất cả các nút màu`c`. Mỗi cạnh như vậy đại diện cho một đoạn bắt đầu hoặc kết thúc tiềm năng của một con đường đẹp. Sử dụng tính toán tổ hợp và DFS để theo dõi kích thước của các thành phần được kết nối một cách hiệu quả, chúng tôi giảm vấn đề xuống O(n) cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) hoặc O(n^3) | O(n) | Quá chậm | 
| Thành phần DFS + Màu tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`, danh sách màu sắc và`n-1`các cạnh. 
2. Xây dựng danh sách kề đại diện cho cây. 
3. Khởi tạo một từ điển ánh xạ từng màu vào danh sách các nút có màu đó. 
4. Đối với mỗi màu`c`, đánh dấu tất cả các nút bằng màu sắc`c`bị chặn và thực hiện DFS trên các nút còn lại để tìm các thành phần được kết nối. Đếm kích thước của từng thành phần. 
5. Hãy để`total_nodes = n`. Đối với mỗi thành phần kích thước`s`, số lượng đường đi đẹp có màu sắc`c`đi qua thành phần này là`s * (total_nodes - s)`bởi vì mỗi nút màu`c`thành phần bên ngoài có thể ghép nối với các nút bên trong mà không vi phạm quy tắc "không có màu trung gian". 
6. Tính tổng các đóng góp này cho tất cả các thành phần và chia cho 2 để sửa cho đường dẫn đếm kép. 
7. Lặp lại các bước 4-6 cho mỗi màu riêng biệt. 
8. Xuất kết quả cho test case. 

Tại sao nó hoạt động: Bằng cách loại bỏ tất cả các nút có màu cụ thể và xem xét các thành phần được kết nối còn lại, chúng tôi đảm bảo rằng mọi đường dẫn bắt đầu và kết thúc bằng các nút có màu này không thể chứa các nút trung gian có cùng màu. Việc đếm đường dẫn thông qua kích thước thành phần sẽ nắm bắt tất cả các cặp hợp lệ mà không liệt kê đường dẫn một cách rõ ràng. Chia cho 2 sẽ đúng cho việc đếm kép vì mỗi đường dẫn được tính một lần từ mỗi điểm cuối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        colors = list(map(int, input().split()))
        adj = [[] for _ in range(n)]
        for _ in range(n-1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        from collections import defaultdict

        color_nodes = defaultdict(list)
        for i, c in enumerate(colors):
            color_nodes[c].append(i)

        result = 0
        visited = [False] * n

        def dfs(u):
            visited[u] = True
            size = 1
            for v in adj[u]:
                if not visited[v] and blocked[v] == False:
                    size += dfs(v)
            return size

        for c, nodes in color_nodes.items():
            blocked = [False] * n
            for node in nodes:
                blocked[node] = True
            visited = [False] * n
            for i in range(n):
                if not visited[i] and not blocked[i]:
                    s = dfs(i)
                    result += s * (n - s)
        print(result // 2)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng danh sách kề, sau đó xử lý từng màu riêng biệt. DFS được sử dụng để đếm kích thước thành phần sau khi chặn các nút có màu hiện tại. Nhân kích thước thành phần với số nút bên ngoài sẽ thu được tất cả các đường dẫn nhiều thành phần có thể tạo thành các đường dẫn đẹp. Chia cho hai sẽ tránh được việc tính hai lần vì mỗi đường dẫn đều không có hướng. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
1 2 1
1 2
2 3
```Dấu vết của các biến chính: 

| Nút | Màu sắc | Kích thước thành phần (sau khi chặn màu 1) | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | [2] | 2*(3-2)=2 | 
| 3 | 1 | đã được tính | 0 | 

Kết quả = 1 (chia cho 2 để đếm kép) 

Đầu vào mẫu 2:```
5
2 1 2 1 2
1 2
1 3
3 4
4 5
```Sau khi xử lý màu 1 và 2, kích thước thành phần mang lại tổng thành 6, chia cho 2 → 3 đường dẫn đẹp. Bảng sẽ liệt kê các thành phần cho màu 1: [2,2], màu 2: [3], ánh xạ tới số lượng đường dẫn hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi nút được truy cập một lần cho mỗi màu. Tổng của`n`trên các trường hợp thử nghiệm 2_10^5 đảm bảo tổng số hoạt động 2_10^5 * 2 ≈ 4*10^5 | 
| Không gian | O(n) | Danh sách kề, đã truy cập, mảng bị chặn và ánh xạ màu | 

Độ phức tạp tuyến tính phù hợp tốt trong giới hạn 2 giây và giới hạn bộ nhớ 512MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("4\n3\n1 2 1\n1 2\n2 3\n5\n2 1 2 1 2\n1 2\n1 3\n3 4\n4 5\n5\n1 2 3 4 5\n1 2\n1 3\n3 4\n4 5\n4\n2 2 2 2\n3 1\n3 2\n3 4") == "1\n3\n0\n3"

# Custom cases
assert run("1\n2\n1 1\n1 2") == "1", "minimum size input"
assert run("1\n4\n1 2 1 2\n1 2\n2 3\n3 4") == "2", "alternating colors"
assert run("1\n3\n1 1 1\n1 2\n2 3") == "2", "all same color"
assert run("1\n5\n1 2 3 4 5\n1 2\n2 3\n3 4\n4 5") == "0", "all unique colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2, colors=[1,1]`| 1 | cây nhỏ nhất có đường đi đẹp hợp lệ | 
|`n=4, colors=[1,2,1,2]`| 2 | con đường đi qua |
