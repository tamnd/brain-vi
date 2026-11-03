---
title: "CF 1095F - Kết nối mọi thứ"
description: "Chúng ta có n đỉnh, mỗi đỉnh được viết trên đó một số ai và ban đầu không có cạnh nào. Chúng ta có thể nối hai đỉnh bất kỳ bằng cách tính tổng các số ax + ay của chúng. Ngoài ra, có m ưu đãi đặc biệt, mỗi ưu đãi cho phép thêm một lợi thế cụ thể với chi phí chiết khấu w."
date: "2026-06-12T05:52:30+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1095
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 529 (Div. 3)"
rating: 1900
weight: 1095
solve_time_s: 68
verified: true
draft: false
---

[CF 1095F - Kết nối mọi thứ](https://codeforces.com/problemset/problem/1095/F) 

**Xếp hạng:** 1900 
**Tags:** dsu, đồ thị, tham lam 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`n`đỉnh, mỗi đỉnh có một số`a_i`được viết trên đó và ban đầu không có cạnh. Chúng ta có thể nối hai đỉnh bất kỳ bằng cách tính tổng các số của chúng`a_x + a_y`. Ngoài ra, có`m`ưu đãi đặc biệt, mỗi ưu đãi cho phép thêm một lợi thế cụ thể với chi phí chiết khấu`w`. Mục tiêu là làm cho đồ thị được kết nối với tổng chi phí tối thiểu. 

Từ`n`có thể lớn tới 200.000, bất kỳ giải pháp nào kiểm tra trực tiếp tất cả các cặp đỉnh có thể đều quá chậm vì điều đó sẽ yêu cầu các phép toán O(n^2), tức là khoảng 4 * 10^10 và không khả thi trong 2 giây. Vì vậy, chúng ta cần một cách tiếp cận tận dụng cấu trúc bài toán để giảm số cạnh mà chúng ta xem xét. Các số trên các đỉnh có thể lớn tới 10^12, do đó, tràn số nguyên là mối lo ngại nếu tính tổng chi phí một cách bất cẩn, nhưng Python xử lý các số nguyên lớn một cách tự nhiên. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó ưu đãi đặc biệt rẻ hơn rất nhiều so với kết nối thông qua tổng số đỉnh hoặc khi phải sử dụng đỉnh rẻ nhất để kết nối nhiều thành phần. Ví dụ, với đỉnh`[1, 3, 3]`và ưu đãi đặc biệt`(2,3,5)`Và`(1,2,1)`, cách tiếp cận ngây thơ là luôn sử dụng các ưu đãi đặc biệt một cách tham lam có thể tạo ra tổng số không chính xác nếu nó bỏ qua việc kết hợp đỉnh thấp nhất với các đỉnh khác. 

## Phương pháp tiếp cận 

Phương pháp vũ phu sẽ xem xét tất cả`n*(n-1)/2`các cạnh có thể có giữa các đỉnh, gán cho chúng một giá`a_x + a_y`hoặc một ưu đãi đặc biệt rẻ hơn nếu có, sau đó chạy thuật toán Kruskal để tìm cây bao trùm tối thiểu. Về mặt lý thuyết, điều này đúng, nhưng việc tạo và sắp xếp các cạnh O(n^2) quá chậm với n=2*10^5, vì nó sẽ yêu cầu hàng chục tỷ thao tác. 

Quan sát quan trọng để làm cho điều này dễ thực hiện là trong một biểu đồ nối hai đỉnh`x`Và`y`chi phí`a_x + a_y`, một trong những cách rẻ nhất để kết nối các thành phần là luôn liên quan đến đỉnh có kích thước nhỏ nhất`a_i`. Xét đỉnh có giá trị nhỏ nhất`a_i`; bất kỳ cạnh nào sử dụng nó đều có khả năng rẻ hơn các cạnh giữa các đỉnh lớn hơn. Do đó, thay vì thêm tất cả n^2 cạnh có thể, chúng ta có thể mô phỏng việc kết nối tất cả các đỉnh với đỉnh tối thiểu này, vốn chỉ yêu cầu O(n) cạnh. Bên cạnh các ưu đãi đặc biệt, giờ đây chúng tôi còn có các cạnh O(n + m) để xem xét, điều này khả thi. 

Bằng cách coi đây là bài toán cây bao trùm tối thiểu và sử dụng cấu trúc Disjoint Set Union (DSU) để triển khai thuật toán Kruskal một cách hiệu quả, chúng ta có thể tính chi phí tối thiểu để kết nối tất cả các đỉnh. Việc sắp xếp các cạnh O(n + m) và các phép toán hợp mất O((n + m) log(n + m)) thời gian, có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 log n) | O(n^2) | Quá chậm | 
| Tối ưu | O((n + m) log(n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định đỉnh có giá trị nhỏ nhất`a_i`. Gọi đỉnh này`min_vertex`có giá trị`min_value`. Lý do của bước này là để nối bất kỳ đỉnh nào với`min_vertex`được đảm bảo rẻ hơn so với việc kết nối hai đỉnh không tối thiểu, trừ khi có ưu đãi đặc biệt mang lại chi phí thấp hơn. 
2. Khởi tạo danh sách trống`edges`. Với mọi đỉnh`v`không bằng`min_vertex`, thêm một cạnh`(min_vertex, v, min_value + a_v)`. Điều này đảm bảo tất cả các đỉnh đều có kết nối tiềm năng với chi phí thấp tới đỉnh có chi phí tối thiểu. 
3. Đọc các ưu đãi đặc biệt. Đối với mỗi ưu đãi`(x, y, w)`, thêm nó vào`edges`. Mỗi ưu đãi đặc biệt có thể có khả năng rẻ hơn tổng các con số và phải được xem xét trong MST. 
4. Sắp xếp tất cả các cạnh theo chi phí. Việc sắp xếp là cần thiết đối với thuật toán Kruskal để đảm bảo chúng ta luôn chọn cạnh có sẵn rẻ nhất mà không tạo thành chu trình. 
5. Khởi tạo DSU cho`n`đỉnh. Xử lý các cạnh theo thứ tự chi phí tăng dần. Đối với mỗi cạnh`(u, v, cost)`, nếu như`u`Và`v`thuộc về các thành phần khác nhau trong DSU, hợp nhất chúng và thêm`cost`vào tổng số đang chạy. Bỏ qua các cạnh kết nối các thành phần đã được kết nối. 
6. Sau khi xử lý tất cả các cạnh, tổng chạy biểu thị chi phí tối thiểu để làm cho biểu đồ được kết nối. In nó. 

Tại sao nó hoạt động: Mỗi MST phải bao gồm các cạnh kết nối tất cả các thành phần với chi phí tối thiểu. Bằng cách bao gồm các cạnh từ đỉnh tối thiểu đến tất cả các cạnh khác, chúng tôi đảm bảo rằng mọi kết nối cần thiết tiềm năng giữa các thành phần đều có sẵn với chi phí tối thiểu. Các ưu đãi đặc biệt được bổ sung và xem xét một cách rõ ràng, do đó không bỏ lỡ cơ hội kết nối rẻ hơn. DSU đảm bảo rằng chúng tôi không bao giờ hình thành chu trình, duy trì tính bất biến của MST. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0]*n

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, x, y):
        xroot = self.find(x)
        yroot = self.find(y)
        if xroot == yroot:
            return False
        if self.rank[xroot] < self.rank[yroot]:
            self.parent[xroot] = yroot
        else:
            self.parent[yroot] = xroot
            if self.rank[xroot] == self.rank[yroot]:
                self.rank[xroot] += 1
        return True

def main():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    min_value = min(a)
    min_index = a.index(min_value)

    edges = []
    for i in range(n):
        if i != min_index:
            edges.append((min_value + a[i], min_index, i))

    for _ in range(m):
        x, y, w = map(int, input().split())
        edges.append((w, x-1, y-1))

    edges.sort()
    dsu = DSU(n)
    total_cost = 0
    for cost, u, v in edges:
        if dsu.union(u, v):
            total_cost += cost

    print(total_cost)

if __name__ == "__main__":
    main()
```Việc triển khai DSU đảm bảo rằng các hoạt động kết hợp và tìm kiếm duy trì thời gian gần như không đổi, điều này rất quan trọng đối với hiệu suất trên các đỉnh 2*10^5. Việc sắp xếp các cạnh đảm bảo rằng chúng ta luôn xem xét cạnh rẻ nhất tiếp theo. sử dụng`min_index`đảm bảo rằng chúng tôi chỉ tạo ra`n-1`các cạnh từ đỉnh tối thiểu, tránh thế hệ O(n^2). 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
3 2
1 3 3
2 3 5
2 1 1
```| Cạnh được xem xét | Chi phí | Hoạt động DSU | Tổng chi phí | 
| --- | --- | --- | --- | 
| 1-2 (đặc biệt) | 1 | công đoàn | 1 | 
| 1-3 (cạnh tối thiểu) | 4 | công đoàn | 5 | 
| 2-3 (đặc biệt) | 5 | bỏ qua | 5 | 

MST bao gồm ưu đãi đặc biệt từ 1 đến 2 và lợi thế tối thiểu từ 1 đến 3. Tổng chi phí là 5. 

**Ví dụ tùy chỉnh** 

đầu vào:```
4 1
2 2 3 4
3 4 1
```| Cạnh được xem xét | Chi phí | Hoạt động DSU | Tổng chi phí | 
| --- | --- | --- | --- | 
| 3-4 (đặc biệt) | 1 | công đoàn | 1 | 
| 1-2 (cạnh tối thiểu) | 4 | công đoàn | 5 | 
| 1-3 (cạnh tối thiểu) | 5 | công đoàn | 10 | 
| 1-4 (cạnh tối thiểu) | 6 | bỏ qua | 10 | 

Tổng chi phí là 10. 

Điều này cho thấy cách thuật toán cân bằng các kết nối đỉnh tối thiểu và các ưu đãi đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log(n + m)) | Việc sắp xếp các cạnh và thực hiện các phép toán hợp chiếm ưu thế. Mỗi liên kết/tìm gần như O (1). | 
| Không gian | O(n + m) | Lưu trữ các cạnh và mảng cha/xếp hạng DSU. | 

Điều này phù hợp thoải mái trong giới hạn thời gian 2 giây đối với n, m 2*10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        main()
    return f.getvalue().strip()

# provided sample
assert run("3 2\n1 3 3\n2 3 5\n2 1 1\n") == "5", "sample 1"

# minimum-size input
assert run("1 0\n10\n") == "0", "single vertex"

# all-equal values
assert run("3 0\n2 2 2\n") == "6", "equal vertex costs"

# special offer cheaper than min edges
assert run("3 1\n5 7 9\n1 3 4\n") == "9", "special cheaper than sum edges"

# maximum-size edge cost
assert run("2 1\n1000000000000 100
```
