---
title: "CF 106452L - MST"
description: "Chúng ta có một đồ thị vô hướng có trọng số được kết nối. MST là cây bao trùm có tổng trọng lượng cạnh nhỏ nhất có thể, nhưng một số cây khác nhau có thể có cùng chi phí tối thiểu."
date: "2026-06-25T09:18:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106452
codeforces_index: "L"
codeforces_contest_name: "UTPC April Fools Contest 2026"
rating: 0
weight: 106452
solve_time_s: 32
verified: true
draft: false
---

[CF 106452L - MST](https://codeforces.com/problemset/problem/106452/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 32s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có trọng số được kết nối. MST là cây bao trùm có tổng trọng lượng cạnh nhỏ nhất có thể, nhưng một số cây khác nhau có thể có cùng chi phí tối thiểu. 

Hoạt động mà bài toán cho phép là không bình thường: chúng ta chỉ có thể tăng trọng số cạnh và mỗi lần tăng sẽ tốn một thao tác. Mục đích không phải là tìm MST mà là loại bỏ mọi sự mơ hồ khỏi cấu trúc MST hiện có. Chúng ta cần tăng trọng số của số cạnh tối thiểu để chi phí cây bao trùm tối thiểu vẫn giữ nguyên và chỉ có một MST khả thi. 

Đầu vào mô tả các đỉnh và cạnh. Mỗi cạnh nối hai đỉnh và có trọng số dương. Đầu ra là số lượng cá thể tối thiểu`+1`cần tăng cân. 

Các ràng buộc lớn: cả số đỉnh và số cạnh đều có thể đạt tới`2 * 10^5`. Một thuật toán kiểm tra mọi cây khung có thể là không thể vì số lượng cây khung có thể là số mũ. Ngay cả các phương pháp tìm kiếm đồ thị lặp đi lặp lại trên tất cả các cạnh cũng quá chậm. Chúng ta cần một đường gần tuyến tính hoặc`O(m log m)`giải pháp hướng tới việc sử dụng cấu trúc của thuật toán Kruskal. 

Phần khó khăn là xử lý các nhóm cạnh có trọng số bằng nhau. Trọng số bằng nhau là lý do duy nhất khiến nhiều MST tồn tại. Ví dụ, hãy xem xét:```
3 3
1 2 1
2 3 1
1 3 1
```Câu trả lời là`1`. Một giải pháp bất cẩn có thể cho rằng cả ba cạnh đều cần thiết vì chúng đều có trọng số nhỏ nhất, nhưng chỉ cần hai cạnh để nối ba đỉnh. Nâng bất kỳ cạnh nào lên trọng lượng`2`để lại chính xác một MST. 

Một trường hợp khác là khi các trọng số bằng nhau không tạo ra bất kỳ sự lựa chọn nào:```
3 3
1 2 1
2 3 2
1 3 3
```Câu trả lời là`0`. MST vốn đã là duy nhất vì các lựa chọn cạnh nhỏ nhất sẽ buộc cây. Một giải pháp đếm mọi cạnh không phải MST sẽ trả về không chính xác`1`hoặc hơn thế nữa. 

Trường hợp biên cuối cùng là đồ thị có một đỉnh:```
1 0
```Câu trả lời là`0`. Không có cạnh nào để sửa đổi và đỉnh đơn đã tạo thành cây bao trùm. Mã giả định có ít nhất một cạnh hoặc cố gắng xây dựng MST bằng`n-1`các cạnh có thể thất bại ở đây. 

# Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng MST và sau đó kiểm tra mọi cây thay thế. Nếu tồn tại một số MST, chúng ta có thể thử tăng từng cạnh một và kiểm tra xem MST có trở thành duy nhất hay không. Ý tưởng này đúng vì tính duy nhất có thể được kiểm tra bằng các thuộc tính MST, nhưng số lượng sửa đổi có thể có là quá lớn. Trong trường hợp xấu nhất, một biểu đồ dày đặc với nhiều cạnh có trọng số bằng nhau sẽ yêu cầu kiểm tra liên tục nhiều ứng cử viên MST, vượt xa giới hạn. 

Quan sát quan trọng đến từ thuật toán Kruskal. Kruskal xử lý các cạnh theo thứ tự trọng lượng tăng dần. Khi tất cả các cạnh có trọng số nhỏ hơn`w`đã được xử lý, các thành phần DSU hiện tại đại diện cho mọi thứ đã bị ép buộc trước khi xem xét các cạnh trọng lượng`w`. 

Bây giờ chỉ nhìn vào các cạnh có cùng trọng lượng`w`. Bên trong mỗi thành phần DSU có trọng lượng nhỏ hơn, các cạnh đó không thể ảnh hưởng đến MST. Các cạnh có trọng lượng bằng nhau còn lại kết nối các thành phần hiện tại với nhau. Nếu các cạnh này chứa một chu trình thì có nhiều cách để chọn các cạnh có trọng số MST`w`. 

Đối với mọi thành phần liên thông được hình thành bởi các cạnh có trọng số bằng nhau này, chúng ta chỉ cần một cây bao trùm trong số chúng. Bất kỳ cạnh bổ sung nào ngoài cây đó sẽ tạo ra một MST thay thế và phải được tăng lên. Nếu một thành phần tạm thời chứa`E`các cạnh và`V`Chính xác là các thành phần DSU`E - (V - 1)`các cạnh cần được nâng lên. 

Điều này có thể được tính toán trong khi xử lý từng nhóm trọng lượng. Chúng tôi đếm các cạnh có trọng số bằng nhau kết nối các thành phần DSU hiện tại khác nhau, sau đó đếm xem có bao nhiêu trong số chúng thực sự hợp nhất các thành phần. Sự khác biệt là số cạnh không cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong`n`| O(n + m) | Quá chậm | 
| Tối ưu | O(m log m) | O(n + m) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Sắp xếp tất cả các cạnh theo trọng số của chúng. Thuật toán Kruskal phụ thuộc vào việc xử lý các trọng số nhỏ hơn trước tiên, vì các cạnh có trọng số thấp hơn đã được cố định trước khi xem xét nhóm nặng hơn. 
2. Duy trì một DSU chứa tất cả các cạnh có trọng số nhỏ hơn nhóm hiện tại. Đối với một nhóm cạnh có cùng trọng số, tạm thời bỏ qua các cạnh đã kết nối hai đỉnh bên trong cùng thành phần DSU. Những cạnh đó không bao giờ có thể thay đổi MST vì một đường dẫn rẻ hơn đã kết nối các điểm cuối của chúng. 
3. Đối với các cạnh còn lại của nhóm trọng lượng này, hãy đếm xem có bao nhiêu cạnh có thể kết nối các thành phần DSU khác nhau. Đây là những cạnh có thể tham gia vào một số MST. 
4. Hợp nhất các cạnh còn lại trong DSU. Mỗi lần hợp nhất thành công sẽ giảm số lượng kết nối độc lập cần thiết. Nếu việc hợp nhất không thành công, cạnh sẽ tạo ra một chu trình giữa các lựa chọn có trọng số bằng nhau, nghĩa là cạnh đó gây ra tính không duy nhất. 
5. Thêm số lần hợp nhất không thành công từ nhóm trọng số này vào câu trả lời. Đây chính xác là các cạnh có trọng số cần được tăng lên. 
6. Tiếp tục với nhóm trọng lượng tiếp theo cho đến khi mọi cạnh đều được xử lý. 

Tại sao nó hoạt động: 

Thuật toán của Kruskal cho chúng ta biết rằng mọi MST đều có thể được xây dựng bằng cách chọn các cạnh bên trong mỗi nhóm trọng số sao cho chúng kết nối càng nhiều thành phần hiện tại riêng biệt càng tốt. Sau khi tất cả các trọng số nhỏ hơn được cố định, một nhóm các cạnh có trọng số bằng nhau được kết nối với một chu trình sẽ đưa ra nhiều lựa chọn hợp lệ. Việc loại bỏ đủ các cạnh để tạo thành một cây sẽ loại bỏ tất cả các lựa chọn trong khi vẫn duy trì khả năng xây dựng MST với tổng chi phí như nhau. Việc hợp nhất DSU không thành công chính xác là các cạnh chu kỳ dư thừa, vì vậy việc đếm chúng sẽ cho ra số lượng tăng yêu cầu tối thiểu. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def unite(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return False
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

def solve():
    n, m = map(int, input().split())
    edges = []

    for _ in range(m):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1))

    edges.sort()

    dsu = DSU(n)
    ans = 0
    i = 0

    while i < m:
        j = i
        while j < m and edges[j][0] == edges[i][0]:
            j += 1

        before = 0
        for k in range(i, j):
            _, u, v = edges[k]
            if dsu.find(u) != dsu.find(v):
                before += 1

        merged = 0
        for k in range(i, j):
            _, u, v = edges[k]
            if dsu.unite(u, v):
                merged += 1

        ans += before - merged
        i = j

    print(ans)

if __name__ == "__main__":
    solve()
```DSU lưu trữ các thành phần được hình thành bởi tất cả các cạnh nhẹ hơn đã được xử lý. Bước sắp xếp đưa ra thứ tự tương tự được Kruskal sử dụng. 

Đối với mỗi nhóm cân nặng,`before`đếm số cạnh có trọng số bằng nhau vẫn có cơ hội xuất hiện trong MST. Các cạnh đã nằm trong một thành phần sẽ bị bỏ qua vì chúng đã có thể được thay thế bằng các đường dẫn nhẹ hơn. 

các`merged`count ghi lại số cạnh thực sự cần thiết để kết nối các thành phần. Mỗi sự hợp nhất thành công đều đóng góp một cạnh vào khu rừng bao trùm có thể. Sự khác biệt`before - merged`là số cạnh chu trình trong nhóm trọng số đó. 

Mã sử ​​dụng tính năng nén đường dẫn lặp trong`find`và liên kết theo kích thước trong`unite`, giữ cho thời gian hoạt động của DSU gần như không đổi. Tất cả các trọng số được xử lý dưới dạng số nguyên và câu trả lời phù hợp với số nguyên Python bình thường mà không cần lo lắng về lỗi tràn. 

# Ví dụ đã hoạt động 

Hãy xem xét:```
4 3
1 2 3
2 4 1
4 3 4
```Các cạnh được sắp xếp được xử lý theo thứ tự tăng dần. 

| Cân nặng | Xử lý cạnh | Thành phần trước nhóm |`before`|`merged`| Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Thêm 2-4 | {1},{2},{3},{4} | 1 | 1 | 0 | 
| 3 | Thêm 1-2 | {1},{2,4},{3} | 1 | 1 | 0 | 
| 4 | Thêm 4-3 | {1,2,4},{3} | 1 | 1 | 0 | 

Biểu đồ đã có MST duy nhất nên không cần tăng cạnh nào. 

Bây giờ hãy xem xét:```
3 3
1 2 1
2 3 1
1 3 1
```| Cân nặng | Xử lý cạnh | Thành phần trước nhóm |`before`|`merged`| Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | Xử lý cả ba cạnh | {1},{2},{3} | 3 | 2 | 1 | 

Ba cạnh bằng nhau nối ba đỉnh riêng biệt. Một cây chỉ cần hai cạnh nên một cạnh phải được nâng lên. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Việc sắp xếp chiếm ưu thế trong quá trình xử lý DSU | 
| Không gian | O(n + m) | Các cạnh đồ thị và mảng DSU được lưu trữ | 

Những ràng buộc cho phép`2 * 10^5`các cạnh, vì vậy một`O(m log m)`giải pháp thoải mái phù hợp. Các hoạt động DSU tuyến tính một cách hiệu quả do nén đường dẫn và kết hợp theo kích thước. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert run("""3 3
1 2 1
2 3 1
1 3 1
""") == "1\n", "all equal triangle"

assert run("""3 3
1 2 1
2 3 2
1 3 3
""") == "0\n", "already unique"

assert run("""1 0
""") == "0\n", "single vertex"

assert run("""5 6
1 2 2
2 3 1
4 5 3
2 4 2
1 4 2
1 5 3
""") == "2\n", "multiple equal weight cycles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác có ba cạnh bằng nhau |`1`| Phát hiện một chu trình trọng lượng bằng nhau đơn giản | 
| Tăng cường lựa chọn MST |`0`| Xác nhận không có sửa đổi không cần thiết | 
| Một đỉnh |`0`| Xử lý biểu đồ nhỏ nhất | 
| Một số nhóm có trọng lượng bằng nhau |`2`| Kiểm tra việc xử lý nhiều lớp trọng lượng | 

# Vỏ cạnh 

Đối với tam giác cân bằng:```
3 3
1 2 1
2 3 1
1 3 1
```Ban đầu mọi cạnh có thể xuất hiện trong MST. DSU bắt đầu với ba thành phần riêng biệt. Hai sự hợp nhất đầu tiên tạo ra một cây bao trùm. Cạnh thứ ba nối các đỉnh đã được nối bằng các cạnh có trọng số một, do đó nó tạo thành một chu trình. Thuật toán đếm một lần hợp nhất thất bại và trả về`1`. 

Đối với MST đã là duy nhất:```
3 3
1 2 1
2 3 2
1 3 3
```Cạnh đầu tiên kết nối hai thành phần. Cạnh thứ hai nối đỉnh còn lại. Cạnh cuối cùng nặng hơn và không bao giờ tạo ra sự lựa chọn giữa các cạnh tối thiểu. Không có sự hợp nhất thất bại nào xảy ra, vì vậy câu trả lời vẫn còn`0`. 

Đối với đồ thị một đỉnh:```
1 0
```Vòng lặp cạnh không bao giờ được nhập. DSU đã biểu diễn cây khung hoàn chỉnh của một đỉnh nên câu trả lời vẫn là`0`. 

Đối với nhiều nhóm có trọng số bằng nhau:```
5 6
1 2 2
2 3 1
4 5 3
2 4 2
1 4 2
1 5 3
```Trọng lượng một cạnh bị ép buộc. Trong số các cạnh có trọng số hai, một cạnh là dư thừa sau khi xem xét thành phần rẻ hơn, và trong số các cạnh có trọng số ba, một cạnh dư thừa khác xuất hiện. Thuật toán tính cả lựa chọn dư thừa và kết quả trả về`2`.
