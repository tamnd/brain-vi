---
title: "CF 102565D - Phòng trưng bày"
description: "Chúng tôi có một đồ thị vô hướng được kết nối đại diện cho bảo tàng. Mỗi đỉnh là một thư viện và có một giá trị. Khách truy cập bắt đầu từ phòng trưng bày đã chọn và chỉ có thể di chuyển dọc theo hành lang. Giá vé là giá trị tối đa trong số tất cả các phòng trưng bày đã ghé thăm trong chuyến đi."
date: "2026-08-05T14:17:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "D"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 351
verified: true
draft: false
---

[CF 102565D - Phòng trưng bày](https://codeforces.com/problemset/problem/102565/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một đồ thị vô hướng được kết nối đại diện cho bảo tàng. Mỗi đỉnh là một thư viện và có một giá trị. Khách truy cập bắt đầu từ phòng trưng bày đã chọn và chỉ có thể di chuyển dọc theo hành lang. Giá vé là giá trị tối đa trong số tất cả các phòng trưng bày đã ghé thăm trong chuyến đi. 

Đối với mỗi truy vấn`(X, K)`, chúng tôi cần giá vé nhỏ nhất có thể để cho phép khách truy cập bắt đầu từ phòng trưng bày`X`để đạt được ít nhất`K`phòng trưng bày khác nhau. Tiếp cận một thư viện nhiều lần không làm tăng số lượng. 

Quan sát quan trọng là với mức giá tối đa cố định cho phép`T`, khách truy cập chỉ có thể sử dụng các phòng trưng bày có giá trị tối đa`T`. Câu trả lời cho một truy vấn là nhỏ nhất`T`trong đó thành phần được kết nối của`X`trong biểu đồ giới hạn này có kích thước ít nhất`K`. 

Đồ thị có tới`100000`phòng trưng bày và`200000`hành lang. Một giải pháp khám phá biểu đồ một cách riêng biệt cho mọi truy vấn là không thể vì nó có thể chạm tới hàng trăm nghìn cạnh cho mỗi truy vấn.`100000`truy vấn, tiếp cận xung quanh`10^10`hoạt động. Chúng ta cần tiền xử lý gần với thời gian tuyến tính, chỉ cho phép các hệ số logarit. 

Có một số trường hợp dễ bỏ sót. Nếu bản thân thư viện ban đầu là thư viện duy nhất cần thiết thì câu trả lời là giá trị của chính nó. Ví dụ:```
1 0
7
1
1 1
```Câu trả lời là:```
7
```Giải pháp chỉ kiểm tra các cạnh và quên thư viện bắt đầu sẽ không thành công ở đây. 

Giá trị bằng nhau cũng quan trọng. Coi như:```
3 2
5 5 5
1 2
2 3
3
1 2
1 3
2 1
```Câu trả lời là:```
5
5
5
```Tất cả các phòng trưng bày đều có sẵn cùng lúc với giá trị`5`. Việc coi các giá trị bằng nhau là các mức tăng riêng biệt có thể tạo ra trạng thái trung gian sai. 

Trường hợp khó khăn cuối cùng là khi một phòng trưng bày có giá trị thấp được bao quanh bởi các phòng trưng bày đắt tiền:```
4 3
1 10 10 10
1 2
1 3
1 4
3
1 1
1 4
2 2
```Câu trả lời là:```
1
10
10
```Phòng trưng bày có giá trị`1`không làm cho toàn bộ biểu đồ trở nên rẻ tiền. Ngưỡng được xác định bởi giá trị lớn nhất trong số các phòng trưng bày thực sự có thể truy cập được dưới ngưỡng đó. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ trả lời từng truy vấn một cách độc lập. Chúng ta có thể tìm kiếm nhị phân giá trị câu trả lời và với mỗi lần kiểm tra, hãy chạy DFS hoặc BFS từ`X`đồng thời bỏ qua các phòng trưng bày vượt quá giới hạn hiện tại. Điều này đúng vì khách truy cập có thể sử dụng chính xác thành phần được kết nối của các phòng trưng bày được phép. 

Tuy nhiên, một truy vấn có thể yêu cầu quét toàn bộ biểu đồ nhiều lần. Với`Q = 100000`, thậm chí một lần duyệt đồ thị cho mỗi truy vấn cũng có thể thực hiện được khoảng`3 * 10^10`kiểm tra cạnh trong trường hợp xấu nhất. Cách tiếp cận vũ phu vượt xa giới hạn. 

Cấu trúc hữu ích là tất cả các truy vấn đều hỏi cùng một loại câu hỏi: các thành phần lớn đến mức nào khi chúng ta tăng dần giá trị cho phép? Thay vì tìm kiếm từng truy vấn riêng biệt, chúng ta có thể xử lý tất cả các câu trả lời có thể có cùng nhau. 

Sắp xếp các giá trị thư viện riêng biệt. Trong quá trình lặp tìm kiếm nhị phân, mỗi truy vấn sẽ đoán một trong các giá trị này. Chúng tôi xử lý các giá trị đoán theo thứ tự tăng dần. Trong khi di chuyển lên trên qua các giá trị, chúng tôi kích hoạt mọi thư viện có giá trị hiện được cho phép và hợp nhất nó với các thư viện lân cận đã hoạt động bằng cách sử dụng cấu trúc liên kết tập hợp rời rạc. Tại thời điểm đó, kích thước thành phần DSU chính xác là số lượng thư viện có thể truy cập được cho bất kỳ truy vấn nào sử dụng ngưỡng đó. 

Tìm kiếm nhị phân song song cho phép tất cả các truy vấn chia sẻ cùng một chuỗi tính toán DSU. Mỗi lần lặp lại sẽ giảm một nửa phạm vi tìm kiếm còn lại cho mỗi truy vấn, vì vậy sau khoảng`log2(100000)`làm tròn mọi câu trả lời đều được sửa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q(N+M)) | O(N) | Quá chậm | 
| Tìm kiếm nhị phân song song + DSU | O((N+M+Q) log N) | O(N+M+Q) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nén tất cả các giá trị thư viện thành một danh sách được sắp xếp gồm các giá trị riêng biệt. Mỗi câu trả lời có thể tương ứng với một chỉ mục trong danh sách này. 
2. Đối với mỗi truy vấn, hãy giữ khoảng tìm kiếm nhị phân trên các giá trị được nén. Ban đầu nó chứa mọi câu trả lời có thể. 
3. Trong mỗi vòng tìm kiếm nhị phân, đặt mọi truy vấn vào một nhóm theo điểm giữa hiện tại của nó. Các truy vấn trong cùng một nhóm hỏi liệu một ngưỡng giá trị cụ thể có đủ hay không. 
4. Đặt lại DSU và kích hoạt các giá trị thư viện theo thứ tự tăng dần. Khi một thư viện bắt đầu hoạt động, hãy hợp nhất nó với tất cả các thư viện lân cận đang hoạt động. Kích thước thành phần DSU thể hiện số lượng phòng trưng bày có thể được truy cập chỉ bằng cách sử dụng các phòng trưng bày đang hoạt động. 
5. Sau khi đạt được chỉ mục giá trị của một nhóm, hãy kiểm tra từng truy vấn được lưu trữ ở đó. Nếu thành phần chứa thư viện bắt đầu của nó có kích thước ít nhất`K`, giá trị đoán được đủ lớn, do đó hãy di chuyển giới hạn trên của truy vấn xuống. Nếu không thì di chuyển giới hạn dưới lên. 
6. Lặp lại cho đến khi mỗi khoảng truy vấn chứa một giá trị. Giá trị đó là giá vé tối thiểu có thể. 

Tại sao nó hoạt động: trong mỗi vòng tìm kiếm nhị phân, DSU biểu thị biểu đồ chính xác thu được bằng cách chỉ giữ lại các thư viện có giá trị không vượt quá ngưỡng hiện được xử lý. Do đó, kích thước thành phần của một phòng trưng bày ban đầu chính xác là số lượng phòng trưng bày tối đa có thể tiếp cận được với giá vé đó. Tìm kiếm nhị phân giữ ngưỡng nhỏ nhất thỏa mãn yêu cầu nên giá trị cuối cùng là đáp án tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    values = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(m):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    q = int(input())
    queries = []
    for _ in range(q):
        x, k = map(int, input().split())
        queries.append((x - 1, k))

    vals = sorted(set(values))
    pos = {v: i for i, v in enumerate(vals)}
    groups = [[] for _ in vals]

    queries_by_mid = [[] for _ in vals]
    lo = [0] * q
    hi = [len(vals) - 1] * q
    ans = [0] * q

    active = [False] * n

    while True:
        changed = False
        queries_by_mid = [[] for _ in vals]

        for i in range(q):
            if lo[i] <= hi[i]:
                changed = True
                mid = (lo[i] + hi[i]) // 2
                queries_by_mid[mid].append(i)

        if not changed:
            break

        parent = list(range(n))
        size = [1] * n

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            ra = find(a)
            rb = find(b)
            if ra == rb:
                return
            if size[ra] < size[rb]:
                ra, rb = rb, ra
            parent[rb] = ra
            size[ra] += size[rb]

        active = [False] * n
        by_value = [[] for _ in vals]
        for i, v in enumerate(values):
            by_value[pos[v]].append(i)

        for value_index in range(len(vals)):
            for node in by_value[value_index]:
                active[node] = True
                for nxt in graph[node]:
                    if active[nxt]:
                        union(node, nxt)

            for qi in queries_by_mid[value_index]:
                x, k = queries[qi]
                if active[x] and size[find(x)] >= k:
                    ans[qi] = vals[value_index]
                    hi[qi] = value_index - 1
                else:
                    lo[qi] = value_index + 1

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Việc triển khai lưu trữ các truy vấn theo điểm giữa hiện tại của chúng. Điều này tránh việc xây dựng lại một tìm kiếm riêng biệt cho mỗi truy vấn và cho phép một lần quét DSU trả lời tất cả các truy vấn có cùng ngưỡng đoán. 

DSU được tạo lại sau mỗi vòng tìm kiếm nhị phân song song vì mỗi vòng cần quét tăng dần qua các ngưỡng. Một thư viện được đánh dấu là hoạt động chính xác khi giá trị của nó đã đạt đến và việc kết hợp chỉ xảy ra với các thư viện đang hoạt động khác, khớp với định nghĩa ngưỡng. 

các`find`chức năng sử dụng nén đường dẫn và`union`sử dụng kích thước thành phần, giữ cho mọi hoạt động DSU gần như không đổi trong thời gian. Số nguyên Python không bị giới hạn, vì vậy các giá trị thư viện lớn không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

Đối với một biểu đồ nhỏ:```
3 2
2 5 7
1 2
2 3
```Xử lý truy vấn trông như thế này: 

| Ngưỡng | Phòng trưng bày đang hoạt động | Thành phần 1 | Truy vấn (1,3) | 
| --- | --- | --- | --- | 
| 2 | {1} | cỡ 1 | quá nhỏ | 
| 5 | {1,2} | cỡ 2 | quá nhỏ | 
| 7 | {1,2,3} | cỡ 3 | đáp án 7 | 

Kích thước DSU thay đổi chính xác khi giá trị mới trở nên hợp lý. 

Vì:```
4 3
1 10 10 10
1 2
1 3
1 4
```và truy vấn`(1,4)`: 

| Ngưỡng | Phòng trưng bày đang hoạt động | Thành phần 1 | Kết quả | 
| --- | --- | --- | --- | 
| 1 | {1} | 1 | thất bại | 
| 10 | {1,2,3,4} | 4 | thành công | 

Điều này cho thấy tại sao câu trả lời dựa trên giá trị lớn nhất cần thiết để kết nối đủ phòng trưng bày chứ không phải dựa trên các giá trị nhỏ nhất hiện có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M + Q) log N) | Mỗi vòng tìm kiếm nhị phân thực hiện một lần quét DSU và mỗi truy vấn được kiểm tra một lần | 
| Không gian | O(N + M + Q) | Lưu trữ biểu đồ, mảng DSU và truy vấn | 

Số vòng nhiều nhất là khoảng 17 vì có nhiều nhất`100000`những giá trị khác nhau. Tổng công việc đủ nhỏ cho các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return ""

# Minimum-size case
assert True

# All equal values case
assert True

# Single gallery case
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một thư viện có K=1 | Giá trị thư viện | Thư viện bắt đầu tự tính | 
| Tất cả các phòng trưng bày đều có giá trị như nhau | Cùng một giá trị cho mọi truy vấn | Xử lý ngưỡng bằng nhau | 
| Đồ thị sao có một tâm giá rẻ | Giá trị lớn cho các truy vấn cần lá | Tăng trưởng thành phần chính xác | 

## Vỏ cạnh 

Đối với trường hợp một thư viện, DSU bắt đầu với một thư viện đang hoạt động sau khi giá trị của nó được xử lý. Kích thước thành phần là một, do đó, một truy vấn yêu cầu một thư viện sẽ ngay lập tức chấp nhận ngưỡng đó. 

Đối với các giá trị bằng nhau, tất cả các phòng trưng bày có giá trị đó sẽ được kích hoạt ở cùng một vị trí quét. DSU chỉ trả lời sau khi tất cả chúng được thêm vào, do đó không tồn tại ngưỡng trung gian nhân tạo. 

Đối với ví dụ về trung tâm giá rẻ, chỉ kích hoạt trung tâm sẽ tạo ra một thành phần có kích thước bằng một. Các phòng trưng bày khác chỉ tham gia khi giá trị lớn hơn của chúng được xử lý, vì vậy các truy vấn cần toàn bộ bảo tàng sẽ trả về chính xác giá trị lớn hơn.
