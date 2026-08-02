---
title: "CF 102694D - Dòng chảy tự do theo chu kỳ"
description: "Đồ thị trong bài toán này là một cây vô hướng liên thông. Mỗi cạnh có một giá trị dung lượng. Đối với mỗi truy vấn, chúng ta được yêu cầu về lượng luồng tối đa có thể được gửi giữa hai đỉnh cho trước theo một quy tắc đặc biệt: mỗi đơn vị luồng phải đi qua một đường dẫn hoàn chỉnh…"
date: "2026-08-01T23:23:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102694
codeforces_index: "D"
codeforces_contest_name: "AlgorithmsThread Tree Basics Contest"
rating: 0
weight: 102694
solve_time_s: 65
verified: true
draft: false
---

[CF 102694D - Dòng chảy tự do theo chu kỳ](https://codeforces.com/problemset/problem/102694/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đồ thị trong bài toán này là một cây vô hướng liên thông. Mỗi cạnh có một giá trị dung lượng. Đối với mọi truy vấn, chúng ta được yêu cầu về lượng luồng tối đa có thể được gửi giữa hai đỉnh nhất định theo một quy tắc đặc biệt: mỗi đơn vị luồng phải đi qua một đường dẫn hoàn chỉnh, giảm dung lượng của mỗi cạnh trên đường đi đó đi một. Nhiệm vụ là tìm số lượng đơn vị lớn nhất có thể được định tuyến giữa hai đỉnh được truy vấn. 

Đầu vào mô tả các cạnh của cây và sau đó là một chuỗi các cặp đỉnh. Đầu ra của mỗi cặp là một số nguyên, luồng tối đa có thể có giữa hai đỉnh đó. Vì đồ thị không có chu trình nên có chính xác một đường đi giữa hai đỉnh bất kỳ. 

Giới hạn cho phép lên tới 300000 đỉnh và cạnh, với dung lượng cạnh đạt 10^9. Giải pháp khám phá toàn bộ đường dẫn cho mọi truy vấn có thể trở nên quá chậm vì số lượng truy vấn cũng có thể lớn. Quét tuyến tính cho mỗi truy vấn có thể đạt khoảng 9 * 10^10 kiểm tra biên trong trường hợp xấu nhất, vượt xa những gì thực tế. Giải pháp cần tiền xử lý để mỗi truy vấn được trả lời theo thời gian logarit. 

Các trường hợp chính xuất phát từ cấu trúc cây và định nghĩa về luồng. Đường dẫn có một cạnh phải trả về trực tiếp dung lượng của cạnh đó. Ví dụ:```
2 1
1 2 7
2
1 2
2 1
```Đầu ra đúng là:```
7
7
```Việc triển khai bất cẩn chỉ tìm kiếm từ đỉnh được đánh số nhỏ hơn hoặc giả định một hướng tồn tại có thể không thành công đối với truy vấn đảo ngược. 

Một trường hợp khác là khi cạnh công suất nhỏ nhất không ở gần điểm cuối. Coi như:```
4 3
1 2 10
2 3 3
3 4 8
1
1 4
```Câu trả lời là:```
3
```Dòng chảy bị giới hạn bởi cạnh cổ chai ở giữa. Phương pháp chỉ kiểm tra các cạnh liền kề sẽ trả về sai 10 hoặc 8. 

Trường hợp quan trọng cuối cùng là dung lượng rất lớn. Ví dụ:```
2 1
1 2 1000000000
1
1 2
```Câu trả lời là:```
1000000000
```Việc triển khai phải lưu trữ dung lượng bằng số nguyên Python. Các ngôn ngữ có loại số nguyên có kích thước cố định cần phải cẩn thận ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là trả lời từng truy vấn bằng cách đi từ đỉnh này sang đỉnh khác, tìm mọi cạnh trên đường đi duy nhất của cây và lấy dung lượng tối thiểu trong số đó. Điều này đúng vì một đơn vị luồng tiêu thụ một đơn vị công suất từ ​​mọi cạnh trên đường dẫn. Tổng số đơn vị không thể vượt quá giới hạn dung lượng nhỏ nhất và nhiều đơn vị luôn có thể được gửi đi nhiều lần bằng cùng một đường dẫn. 

Vấn đề là một cái cây có thể có một chuỗi dài. Nếu mọi truy vấn đều hỏi về hai đỉnh xa nhau thì mỗi truy vấn có thể kiểm tra hầu hết mọi cạnh. Với 300000 đỉnh và nhiều truy vấn, tổng công việc sẽ trở thành bậc hai trong trường hợp xấu nhất. 

Quan sát quan trọng là biểu đồ là một cây, vì vậy mọi truy vấn đường dẫn đều yêu cầu một giá trị tổng hợp dọc theo một đường dẫn gốc đến nút duy nhất. Điều này cho phép chúng tôi sử dụng nâng nhị phân. Trong quá trình tiền xử lý, đối với mỗi đỉnh và mọi lũy thừa của hai, chúng tôi lưu trữ tổ tiên của nó ở khoảng cách đó và dung lượng cạnh tối thiểu khi nhảy tới tổ tiên đó. Trong khi trả lời một truy vấn, chúng tôi nâng cả hai đỉnh lên trên và kết hợp các giá trị tối thiểu được lưu trữ cho đến khi chúng gặp nhau. 

Brute Force hoạt động vì câu trả lời chỉ phụ thuộc vào đường dẫn duy nhất, nhưng không thành công vì nó liên tục xây dựng lại các đường dẫn. Việc quan sát thấy các tổ tiên và các đoạn đường giống nhau được sử dụng lại cho phép chúng ta nén tất cả các chuyển động đi lên có thể thành số bước nhảy logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(log n) cho mỗi truy vấn sau quá trình tiền xử lý O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây ở đỉnh 1 và chạy DFS. Trong quá trình truyền tải, lưu trữ độ sâu của mỗi đỉnh, đỉnh trực tiếp của nó và khả năng của cạnh kết nối nó với đỉnh gốc của nó. Điều này chuyển đổi mọi truy vấn đường dẫn thành một bài toán di chuyển bên trong cây có gốc. 
2. Xây dựng bàn nâng nhị phân. Đối với mỗi đỉnh và mỗi bước nhảy có độ dài 2^k, hãy lưu trữ tổ tiên đạt được sau bước nhảy đó và dung lượng cạnh tối thiểu gặp phải trong lần nhảy đó. Lý do lưu trữ các giá trị tối thiểu là vì câu trả lời là giá trị thắt cổ chai, do đó việc kết hợp hai đoạn đường dẫn chỉ yêu cầu lấy giá trị tối thiểu của chúng. 
3. Đối với truy vấn giữa đỉnh a và b, trước tiên hãy đảm bảo cả hai đỉnh đều có cùng độ sâu. Nâng đỉnh sâu hơn lên theo lũy thừa hai trong khi cập nhật câu trả lời hiện tại với dung lượng tối thiểu được thấy trong mỗi lần nhảy. 
4. Nếu các đỉnh khác nhau sau khi cân bằng độ sâu, hãy nâng cả hai đỉnh lại với nhau từ kích thước bước nhảy lớn nhất xuống 0. Bất cứ khi nào tổ tiên của chúng khác nhau, hãy áp dụng bước nhảy đó cho cả hai đỉnh và cập nhật câu trả lời với cả hai giá trị tối thiểu được lưu trữ. 
5. Sau bước trước, cả hai đỉnh đều là con trực tiếp của tổ tiên chung thấp nhất của chúng. Bao gồm cạnh cuối cùng từ mỗi đỉnh đến đỉnh gốc của nó và xuất ra dung lượng tối thiểu thu được trong quá trình này. 

Tại sao nó hoạt động: 

Trong một cây, mọi đường dẫn truy vấn có thể được chia thành một đoạn đi lên từ đỉnh đầu tiên đến tổ tiên chung thấp nhất và một đoạn đi lên từ đỉnh thứ hai đến cùng một tổ tiên. Nâng nhị phân phân tách cả hai phân đoạn thành sức mạnh rời rạc của hai lần nhảy. Mỗi giá trị bước nhảy được lưu trữ chính xác là dung lượng tối thiểu của phân đoạn đó. Việc lấy mức tối thiểu trên tất cả các phân đoạn đã sử dụng sẽ mang lại dung lượng tối thiểu trên toàn bộ đường dẫn, đây chính xác là luồng tối đa có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))

    LOG = n.bit_length()

    up = [[0] * (n + 1) for _ in range(LOG)]
    mn = [[10**18] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    stack = [(1, 0, 10**18)]
    order = []
    while stack:
        v, p, w = stack.pop()
        up[0][v] = p
        mn[0][v] = w
        order.append(v)
        for to, weight in graph[v]:
            if to != p:
                depth[to] = depth[v] + 1
                stack.append((to, v, weight))

    for k in range(1, LOG):
        prev = up[k - 1]
        cur = up[k]
        prev_mn = mn[k - 1]
        cur_mn = mn[k]
        for v in range(1, n + 1):
            parent = prev[v]
            cur[v] = prev[parent]
            cur_mn[v] = min(prev_mn[v], prev_mn[parent])

    def query(a, b):
        ans = 10**18

        if depth[a] < depth[b]:
            a, b = b, a

        diff = depth[a] - depth[b]
        bit = 0
        while diff:
            if diff & 1:
                ans = min(ans, mn[bit][a])
                a = up[bit][a]
            diff >>= 1
            bit += 1

        if a == b:
            return ans

        for k in range(LOG - 1, -1, -1):
            if up[k][a] != up[k][b]:
                ans = min(ans, mn[k][a], mn[k][b])
                a = up[k][a]
                b = up[k][b]

        ans = min(ans, mn[0][a], mn[0][b])
        return ans

    q = int(input())
    out = []
    for _ in range(q):
        a, b = map(int, input().split())
        out.append(str(query(a, b)))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần DFS tạo biểu diễn gốc của cây. Ngăn xếp lặp tránh được các vấn đề về độ sâu đệ quy của Python vì cây có thể chứa hàng trăm nghìn đỉnh. 

Cấu trúc nâng nhị phân sẽ lấp đầy từng cấp độ cao hơn so với cấp độ trước đó. Nếu một đỉnh có thể nhảy 2^(k-1) cạnh hai lần thì nó có thể nhảy 2^k cạnh. Dung lượng tối thiểu cho lần nhảy lớn hơn là mức tối thiểu của hai lần nhảy nhỏ hơn. 

Trước tiên, hàm truy vấn sẽ căn chỉnh độ sâu vì hai đỉnh chỉ có thể gặp nhau ở tổ tiên chung thấp nhất của chúng sau khi chúng ở mức tương đương. Việc nâng đồng thời sau đó sẽ tránh việc vô tình di chuyển qua điểm trả lời. Các cạnh cha cuối cùng được xử lý riêng biệt vì sau vòng lặp, cả hai đỉnh đều nằm ngay bên dưới tổ tiên chung của chúng. 

Số nguyên Python xử lý dung lượng lên tới 10^9 mà không lo tràn. Giá trị ban đầu của 10^18 đóng vai trò là vô cùng vì mọi dung lượng cạnh thực đều nhỏ hơn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 1
1 2 2768
2
1 2
2 1
```Đường đi chỉ chứa một cạnh. 

| Truy vấn | Điều chỉnh độ sâu | Vận hành thang máy | Trả lời | 
| --- | --- | --- | --- | 
| 1 đến 2 | Di chuyển 2 lên 1 | Sử dụng công suất cạnh 2768 | 2768 | 
| 2 ăn 1 | Di chuyển 2 lên 1 | Sử dụng công suất cạnh 2768 | 2768 | 

Dấu vết cho thấy thuật toán xử lý cả hai hướng giống hệt nhau vì đường đi của cây không bị định hướng. 

Cho một ví dụ khác:```
5 4
1 2 10
2 3 4
3 4 8
3 5 6
1
1 5
```Đường dẫn duy nhất là 1 đến 2 đến 3 đến 5. 

| Truy vấn | Vận hành thang máy | Công suất thu thập | Trả lời | 
| --- | --- | --- | --- | 
| 1 đến 5 | Nâng 5 lên 3, rồi 3 lên 1 | 6, 4, 10 | 4 | 

Dấu vết chứng tỏ rằng câu trả lời được xác định bởi cạnh nhỏ nhất trên đường đi chứ không phải bởi cạnh điểm cuối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + q log n) | Quá trình tiền xử lý xây dựng các bảng nhảy và mọi truy vấn sử dụng tối đa log n bước nhảy | 
| Không gian | O(n log n) | Bảng tổ tiên và bảng dung lượng tối thiểu lưu trữ các giá trị log n trên mỗi đỉnh | 

Quá trình tiền xử lý phù hợp với 300000 đỉnh vì hệ số logarit nhỏ. Mỗi truy vấn tránh đi qua cây, giữ tổng công việc trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    if not data:
        return ""

    it = iter(data)
    n = int(next(it))
    m = int(next(it))

    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u = int(next(it))
        v = int(next(it))
        w = int(next(it))
        graph[u].append((v, w))
        graph[v].append((u, w))

    LOG = n.bit_length()
    up = [[0] * (n + 1) for _ in range(LOG)]
    mn = [[10**18] * (n + 1) for _ in range(LOG)]
    depth = [0] * (n + 1)

    stack = [(1, 0, 10**18)]
    while stack:
        v, p, w = stack.pop()
        up[0][v] = p
        mn[0][v] = w
        for to, weight in graph[v]:
            if to != p:
                depth[to] = depth[v] + 1
                stack.append((to, v, weight))

    for k in range(1, LOG):
        for v in range(1, n + 1):
            up[k][v] = up[k - 1][up[k - 1][v]]
            mn[k][v] = min(mn[k - 1][v], mn[k - 1][up[k - 1][v]])

    def query(a, b):
        ans = 10**18
        if depth[a] < depth[b]:
            a, b = b, a
        d = depth[a] - depth[b]
        k = 0
        while d:
            if d & 1:
                ans = min(ans, mn[k][a])
                a = up[k][a]
            d >>= 1
            k += 1
        if a == b:
            return ans
        for k in range(LOG - 1, -1, -1):
            if up[k][a] != up[k][b]:
                ans = min(ans, mn[k][a], mn[k][b])
                a = up[k][a]
                b = up[k][b]
        return min(ans, mn[0][a], mn[0][b])

    q = int(next(it))
    ans = []
    for _ in range(q):
        ans.append(str(query(int(next(it)), int(next(it)))))
    return "\n".join(ans)

assert run("""2 1
1 2 2768
2
1 2
2 1
""") == "2768\n2768", "sample 1"

assert run("""5 4
4 2 10348
1 4 2690
5 4 9807
3 4 8008
5
5 4
1 5
5 4
5 4
1 5
""") == "9807\n2690\n9807\n9807\n2690", "sample 2"

assert run("""2 1
1 2 1
3
1 2
2 1
1 2
""") == "1\n1\n1", "minimum tree"

assert run("""4 3
1 2 10
2 3 3
3 4 8
2
1 4
2 4
""") == "3\n3", "middle bottleneck"

assert run("""3 2
1 2 1000000000
2 3 1000000000
2
1 3
2 3
""") == "1000000000\n1000000000", "large capacities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có một cạnh | 1 | Xử lý cây kích thước tối thiểu | 
| Dây chuyền dài có cạnh giữa nhỏ | 3 | Phát hiện nút cổ chai | 
| Công suất lớn | 1000000000 | Xử lý số nguyên | 
| Truy vấn đảo ngược | Cùng giá trị cả hai hướng | Xử lý đường dẫn vô hướng | 

## Vỏ cạnh 

Cây biên đơn được xử lý bằng chính bước nâng. Đối với đầu vào:```
2 1
1 2 7
1
2 1
```Đỉnh sâu hơn được nâng lên một lần, giá trị tối thiểu được lưu trữ trở thành 7 và truy vấn trả về 7. Không yêu cầu trường hợp đặc biệt nào cho các đỉnh lá. 

Đối với một nút cổ chai ẩn:```
4 3
1 2 10
2 3 3
3 4 8
1
1 4
```Thuật toán chia đường dẫn thành các bước nhảy nhị phân. Một bước nhảy chứa cạnh của dung lượng 3, một bước nhảy khác chứa cạnh của dung lượng 8 và câu trả lời cuối cùng là giá trị tối thiểu của tất cả các giá trị được thu thập. Nó trả về 3, khớp với luồng tối đa thực tế. 

Đối với công suất lớn:```
2 1
1 2 1000000000
1
1 2
```Các giá trị được lưu trữ là số nguyên Python nên dung lượng được bảo toàn chính xác. Giá trị vô cực ban đầu cũng lớn hơn bất kỳ câu trả lời nào có thể có, ngăn chặn việc giảm ngẫu nhiên trước khi các cạnh thực được xử lý.
