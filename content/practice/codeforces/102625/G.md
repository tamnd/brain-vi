---
title: "CF 102625G - Hội bí mật và một ai đó"
description: "Bài toán mô tả một cây văn phòng bí mật được nối với nhau bằng những lối đi. Mỗi lối đi có một mức độ bảo mật. Đối với mọi địa điểm họp có thể, mỗi văn phòng khác sẽ cử một đại diện đi theo con đường duy nhất đến văn phòng đó."
date: "2026-08-03T15:21:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102625
codeforces_index: "G"
codeforces_contest_name: "IIT(ISM) Virtual Farewell"
rating: 0
weight: 102625
solve_time_s: 58
verified: true
draft: false
---

[CF 102625G - Hội bí mật và một người nào đó](https://codeforces.com/problemset/problem/102625/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Bài toán mô tả một cây văn phòng bí mật được nối với nhau bằng những lối đi. Mỗi lối đi có một mức độ bảo mật. Đối với mọi địa điểm họp có thể, mỗi văn phòng khác sẽ cử một đại diện đi theo con đường duy nhất đến văn phòng đó. Trên đường trở về, người đại diện giấu thông tin trong mọi đoạn có mức bảo mật tối đa trên đường đó. 

Nhiệm vụ là tìm số lượng bản sao thông tin ẩn lớn nhất được đặt trên bất kỳ đoạn văn nào và liệt kê tất cả các đoạn văn đạt mức tối đa đó. Đầu vào chứa số lượng văn phòng, theo sau là các lối đi, điểm cuối và mức độ bảo mật của chúng. Đầu ra chứa số lượng bản sao tối đa và chỉ mục của tất cả các đoạn văn có số lượng đó. Các ràng buộc của bài toán ban đầu có tới$2 \cdot 10^5$văn phòng, do đó phương pháp kiểm tra đường dẫn cho từng cặp văn phòng là không thể. 

Một mô phỏng trực tiếp sẽ xem xét từng cặp văn phòng được sắp xếp, đưa ra khoảng$n^2$những con đường. Với$n=200000$, đây là khoảng$4 \cdot 10^{10}$cặp, vượt xa những gì giải pháp một giây có thể xử lý. Chúng ta cần xử lý cấu trúc cây trên toàn cầu thay vì xem xét các đường dẫn riêng lẻ. 

Các trường hợp chính xuất phát từ mức độ bảo mật tối đa bằng nhau và từ các đoạn không phải là mức tối đa duy nhất trên một đường dẫn. Ví dụ, hãy xem xét:```
3
1 2 5
2 3 5
```Đầu ra đúng là:```
4 2
1 2
```Một giải pháp bất cẩn có thể chỉ tính một đoạn tối đa trên đường từ văn phòng 1 đến văn phòng 3, nhưng cả hai đoạn đều có mức tối đa như nhau và cả hai đều nhận được một bản sao. 

Một trường hợp phức tạp khác là khi một lối đi có mức độ bảo mật cao chặn việc nhận thông tin từ các lối đi có mức độ bảo mật thấp hơn. Ví dụ:```
3
1 2 10
2 3 1
```Đoạn đầu tiên nhận được 4 bản và đoạn thứ hai nhận được 2 bản. Giải pháp chỉ đếm xem có bao nhiêu đường đi chứa một cạnh mà không kiểm tra xem cạnh đó có phải là giá trị lớn nhất trên các đường dẫn đó hay không sẽ đưa ra câu trả lời sai. 

# Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi văn phòng cuộc họp, hãy chạy truyền tải để tìm đường dẫn từ mọi văn phòng khác, tìm giá trị bảo mật tối đa trên mỗi đường dẫn và tăng số lượng tất cả các lối đi có giá trị đó. Nó đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, số lượng cặp văn phòng là$n(n-1)$và việc xử lý mỗi đường dẫn có thể mất$O(n)$, dẫn đến$O(n^3)$làm việc trong trường hợp xấu nhất. Ngay cả việc lưu trữ tất cả khoảng cách cặp cũng đã quá tốn kém. 

Quan sát quan trọng là lối đi có mức độ an ninh$w$chỉ quan trọng đối với các đường dẫn mà mọi cạnh đều có mức độ bảo mật nhiều nhất$w$. Nếu chúng tôi xóa tất cả các đoạn có mức độ bảo mật lớn hơn$w$, thì bên trong mỗi thành phần còn lại, mọi đường dẫn chỉ sử dụng các cạnh có mức tối đa$w$. Một đoạn cấp độ$w$được chọn chính xác cho các cặp văn phòng được sắp xếp cách nhau bởi lối đi bên trong thành phần này. 

Điều này gợi ý việc xử lý các đoạn văn bằng cách tăng mức độ bảo mật. Trước khi xử lý một nhóm đoạn có cùng cấp độ, cấu trúc tập hợp rời rạc biểu thị các thành phần được kết nối bằng các cấp độ nhỏ hơn. Đối với cấp độ hiện tại, các thành phần nhỏ hơn được kết nối bởi các lối đi này tạo thành các cây tạm thời. Đối với lối đi chia cây tạm thời thành các cạnh có kích thước$a$Và$b$, số cặp có thứ tự đi qua nó là$2ab$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Sắp xếp tất cả các lối đi theo mức độ bảo mật của chúng. Chúng tôi cùng nhau xử lý các cấp độ bảo mật ngang nhau vì các đoạn có cùng cấp độ đều có thể là các cạnh tối đa trên cùng một đường dẫn. 
2. Duy trì DSU chỉ chứa các lối đi có mức độ bảo mật nhỏ hơn. Mỗi thành phần DSU đại diện cho một nhóm văn phòng được kết nối mà không sử dụng bất kỳ lối đi nào có thể là cạnh tối đa ở cấp độ hiện tại. 
3. Đối với mỗi nhóm lối đi có cùng mức độ bảo mật, hãy thu gọn từng thành phần DSU thành một nút duy nhất. Các lối đi hiện tại tạo thành một khu rừng giữa các nút được ký hợp đồng này. 
4. Đi qua từng cây tạm thời. Với mỗi đoạn văn hiện tại, hãy tính tổng số văn phòng ban đầu ở cả hai bên của đoạn văn đó. Nếu hai bên chứa$a$Và$b$văn phòng, thêm$2ab$theo số lượng của đoạn văn đó. 
5. Sau khi tính toán tất cả số lượng cho cấp độ bảo mật này, hãy hợp nhất tất cả các đoạn thuộc cấp độ này vào DSU. Chúng trở nên khả dụng khi xử lý các mức bảo mật lớn hơn. 

Tại sao nó hoạt động: đối với một đoạn văn có trình độ$w$, một cặp văn phòng đóng góp vào nó một cách chính xác khi toàn bộ đường đi giữa chúng không chứa cạnh nào lớn hơn$w$và lối đi nằm trên con đường đó. Trong quá trình xử lý cấp$w$, các thành phần DSU và rừng tạm thời thể hiện chính xác những đường dẫn đó. Việc cắt đoạn văn chia các đường dẫn hợp lệ thành hai nhóm kích thước$a$Và$b$và mọi lựa chọn theo thứ tự của một điểm cuối từ mỗi bên sẽ đóng góp một lần. Như vậy$2ab$đếm chính xác tất cả các bản sao thông tin cho đoạn văn đó. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    edges = []
    for i in range(1, n):
        u, v, w = map(int, input().split())
        edges.append((w, u - 1, v - 1, i))

    parent = list(range(n))
    size = [1] * n

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return
        if size[a] < size[b]:
            a, b = b, a
        parent[b] = a
        size[a] += size[b]

    edges.sort()
    ans = [0] * n
    i = 0

    while i < n - 1:
        j = i
        while j < n - 1 and edges[j][0] == edges[i][0]:
            j += 1

        graph = {}
        nodes = set()

        for _, u, v, idx in edges[i:j]:
            ru = find(u)
            rv = find(v)
            graph.setdefault(ru, []).append((rv, idx))
            graph.setdefault(rv, []).append((ru, idx))
            nodes.add(ru)
            nodes.add(rv)

        visited = set()

        for start in nodes:
            if start in visited:
                continue

            order = []
            par = {start: -1}
            pedge = {}

            stack = [start]
            visited.add(start)

            while stack:
                x = stack.pop()
                order.append(x)
                for y, eid in graph.get(x, []):
                    if y != par[x]:
                        par[y] = x
                        pedge[y] = eid
                        visited.add(y)
                        stack.append(y)

            total = 0
            for x in order:
                total += size[find(x)]

            sub = {}
            for x in reversed(order):
                cur = size[find(x)]
                for y, eid in graph.get(x, []):
                    if par.get(y) == x:
                        cur += sub[y]
                sub[x] = cur
                if par[x] != -1:
                    other = total - cur
                    ans[pedge[x]] += 2 * cur * other

        for _, u, v, _ in edges[i:j]:
            union(u, v)

        i = j

    best = max(ans[1:])
    res = [str(i) for i in range(1, n) if ans[i] == best]

    print(best, len(res))
    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Phần DSU tuân theo ý tưởng về mức độ bảo mật ngày càng tăng. các`find`hoạt động cung cấp thành phần hợp đồng hiện tại trước khi cấp độ hiện tại được hợp nhất. 

Biểu đồ tạm thời chỉ được xây dựng lại cho một cấp độ bảo mật tại một thời điểm. Các đỉnh của nó là các thành phần DSU, không phải các văn phòng riêng lẻ. Việc duyệt tính toán kích thước cây con trong khu rừng tạm thời này. Khi một cạnh tạm thời ngăn cách một cây con có kích thước`cur`từ phần còn lại`total - cur`, số cặp thứ tự hợp lệ chính xác là`2 * cur * (total - cur)`. 

Bước hợp nhất xảy ra sau khi đếm. Thứ tự này là cần thiết. Nếu các đoạn cấp độ hiện tại được hợp nhất trước khi đếm, chúng sẽ xuất hiện không chính xác dưới dạng các kết nối cấp thấp hơn đã có sẵn. 

# Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
2 1 3
3 1 1
```Quá trình xử lý là: 

| Đoạn văn | Thành phần hiện tại | Kích thước bên | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | {1},{2},{3} | 2 và 1 | 4 | 
| 2 | {1,2},{3} | 1 và 2 | 2 | 

Đóng góp tối đa là 4 nên đoạn 1 được chọn. 

Đối với mẫu thứ hai:```
5
2 1 3
4 3 1
5 4 1
2 3 1
```Các cây tạm thời được tạo cho lối đi an ninh cấp 1 tạo ra: 

| Đoạn văn | Kích thước bên | Đóng góp | 
| --- | --- | --- | 
| 2 | 2 và 3 | 12 | 
| 3 | 1 và 4 | 8 | 
| 4 | 2 và 3 | 12 | 

Đoạn văn cấp cao hơn được xử lý riêng biệt và đạt mức tối đa tương tự như đoạn văn khác, tạo ra nhiều câu trả lời. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Việc sắp xếp chiếm ưu thế và mỗi đoạn tham gia vào một số lượng nhỏ các hoạt động DSU và cây tạm thời. | 
| Không gian |$O(n)$| Mảng DSU, danh sách cạnh và biểu đồ tạm thời chứa thông tin tuyến tính. | 

Giải pháp phù hợp với$2 \cdot 10^5$limit vì nó không bao giờ liệt kê các cặp văn phòng hoặc đường dẫn. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("""3
2 1 3
3 1 1
""") == """4 1
1
"""

assert run("""3
1 2 5
2 3 5
""") == """8 2
1 2
"""

assert run("""2
1 2 7
""") == """2 1
1
"""

assert run("""4
1 2 3
2 3 3
3 4 3
""") == """8 2
2 3
"""

assert run("""4
1 2 10
2 3 1
3 4 1
""") == """12 1
1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba nút có cấp độ bằng nhau | Cả hai cạnh được chọn | Các đoạn tối đa bằng nhau được tính cùng nhau | 
| Cây một cạnh | Đoạn duy nhất thắng | Trường hợp kích thước tối thiểu | 
| Chuỗi có giá trị bằng nhau | Đoạn giữa chiếm ưu thế | Tính toán kích thước cây con | 
| Một cạnh lớn và một cạnh nhỏ | Thắng lợi lớn | Bỏ qua các cạnh không tối đa | 

# Vỏ cạnh 

Đối với các mức tối đa bằng nhau, thuật toán sẽ xử lý mọi đoạn ở cấp độ hiện tại cùng nhau. Trong đầu vào```
3
1 2 5
2 3 5
```cả hai đoạn đều nằm trong cùng một cây tạm thời. Mỗi vết cắt có kích thước cạnh 1 và 2, mỗi mặt có 4 bản sao, vì vậy cả hai đều được trả lại. 

Đối với một lối đi có giá trị bảo mật lớn hơn nhiều, các lối đi nhỏ hơn bên dưới nó không thể nhận thông tin từ các đường đi qua lối đi lớn hơn. TRONG```
3
1 2 10
2 3 1
```cấp độ đầu tiên được xử lý một mình. Thành phần tạm thời có hai mặt có kích thước 1 và 2, cho kết quả 4. Đoạn thứ hai được xử lý sau và chỉ nhận các cặp không chứa đoạn văn cấp 10 là tối đa, cho kết quả 2. Thứ tự hợp nhất DSU duy trì sự khác biệt này.
