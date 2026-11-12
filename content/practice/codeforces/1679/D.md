---
title: "CF 1679D - Tung đồng xu vào biểu đồ của bạn..."
description: "Bài toán đưa ra một đồ thị có hướng trong đó mỗi đỉnh có một giá trị nguyên dương. Masha có thể đặt một đồng xu lên bất kỳ đỉnh nào và sau đó di chuyển nó dọc theo các cạnh của biểu đồ chính xác $k-1$ lần. Mỗi khi đồng xu ghé thăm một đỉnh, số đỉnh đó sẽ được ghi vào sổ."
date: "2026-06-10T00:41:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1679
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 791 (Div. 2)"
rating: 1900
weight: 1679
solve_time_s: 123
verified: true
draft: false
---

[CF 1679D - Tung đồng xu vào biểu đồ của bạn...](https://codeforces.com/problemset/problem/1679/D) 

**Xếp hạng:** 1900 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, dp, đồ thị 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một đồ thị có hướng trong đó mỗi đỉnh có một giá trị nguyên dương. Masha có thể đặt một đồng xu lên bất kỳ đỉnh nào và sau đó di chuyển nó dọc theo các cạnh của biểu đồ một cách chính xác$k-1$lần. Mỗi khi đồng xu ghé thăm một đỉnh, số đỉnh đó sẽ được ghi vào sổ. Nhiệm vụ là giảm thiểu số lượng lớn nhất xuất hiện trong sổ ghi chép sau những$k$vị trí đặt tiền xu, bao gồm cả vị trí ban đầu. Nếu không thể thực hiện được$k-1$di chuyển vì biểu đồ quá nhỏ hoặc bị ngắt kết nối, đầu ra sẽ là$-1$. 

Các ràng buộc đầu vào lớn: lên tới$2 \cdot 10^5$các đỉnh và các cạnh và$k$có thể cao như$10^{18}$. Điều này loại trừ bất kỳ thuật toán nào mô phỏng rõ ràng tất cả các chuỗi di chuyển hoặc lưu trữ có thể có$k$các phần tử. Chúng ta cũng cần xử lý các số lượng lớn trong các giá trị đỉnh và các biểu đồ có khả năng rất thưa thớt hoặc mất kết nối. 

Các trường hợp cạnh rất tinh tế. Nếu đồ thị chứa các chu trình, Masha có thể lặp lại các đỉnh vô thời hạn, điều này làm cho$k$không liên quan miễn là chu kỳ có thể đạt được ngay từ đầu. Nếu đồ thị không có chu kỳ và đường đi dài nhất có ít hơn$k$đỉnh, đầu ra là$-1$. Các đồ thị nhỏ hoặc đồ thị trong đó các đỉnh nhỏ nhất bị cô lập cũng đưa ra bẫy cho các thuật toán ngây thơ chỉ nhìn cục bộ hoặc cho rằng tất cả các đỉnh đều có thể truy cập được. 

Ví dụ: Xét đồ thị có ba đỉnh, các cạnh$1 \to 2$Và$2 \to 3$, Và$k = 5$. Con đường dài nhất chỉ có ba đỉnh nên dù Masha có di chuyển thế nào cô cũng không thể ghi được năm con số. Đầu ra đúng là$-1$. Một cách tiếp cận bất cẩn giả định các chu kỳ hoặc chuyển động không giới hạn sẽ cố gắng sử dụng các đỉnh hiện có nhiều lần một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử mọi đỉnh làm điểm bắt đầu và khám phá tất cả các đường đi có độ dài$k$. Chúng ta có thể theo dõi giá trị tối đa dọc theo mỗi đường dẫn và chọn giá trị tối thiểu trong số đó. Về nguyên tắc, điều này đúng nhưng khó thực hiện: số lượng đường đi tăng theo cấp số nhân với kích thước biểu đồ và$k$tùy thuộc vào$10^{18}$. Ngay cả DFS bị giới hạn độ sâu cho mức độ vừa phải$k$là không thể. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng số lớn nhất được ghi dọc theo một chuỗi là số lớn nhất trong số các đỉnh được truy cập. Nếu chúng ta đoán một ngưỡng$x$, chúng ta có thể kiểm tra xem có tồn tại một chuỗi có độ dài không$k$chỉ truy cập các đỉnh có giá trị nhiều nhất$x$. Điều này làm giảm vấn đề thành câu hỏi về khả năng tiếp cận trong đồ thị con được hình thành bởi các đỉnh có giá trị$\le x$. Nếu một thành phần liên thông mạnh của đồ thị con này có một chu trình, chúng ta có thể lặp lại các bước di chuyển vô thời hạn để đạt được bất kỳ$k$. Nếu đồ thị không có chu trình thì đường đi dài nhất trong đồ thị con ít nhất phải bằng$k$. Bằng cách sử dụng thuộc tính này, chúng ta có thể tìm kiếm nhị phân trên các giá trị tối đa ứng viên để tìm ra giá trị khả thi tối thiểu một cách hiệu quả.$x$. 

Tìm kiếm brute-force thử tất cả các đường dẫn một cách rõ ràng và thất bại tại$n = 2 \cdot 10^5$. Giải pháp tối ưu giúp giảm vấn đề tìm kiếm nhị phân trên các giá trị được kết hợp với DFS để phát hiện các chu kỳ hoặc tính toán đường đi dài nhất trong đồ thị con có ngưỡng. Điều này làm giảm độ phức tạp xuống còn khoảng$O((n+m) \log n)$, điều đó là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^k) | O(k) | Quá chậm | 
| Tìm kiếm nhị phân + DFS | O((n+m) log(max a_i)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các giá trị đỉnh duy nhất và sắp xếp chúng. Đây là những ứng cử viên duy nhất cho giá trị tối đa tối thiểu dọc theo một chuỗi. 
2. Thực hiện chức năng kiểm tra`can(x)`trả về True nếu tồn tại một chuỗi có độ dài$k$chỉ truy cập các đỉnh có giá trị$\le x$. Xây dựng đồ thị con chỉ chứa các đỉnh có`a[i] <= x`và các cạnh giữa chúng. 
3. Trong`can(x)`, phát hiện xem có chu trình nào trong sơ đồ con sử dụng DFS với ngăn xếp đệ quy hay không. Nếu có một chu kỳ, chuỗi có thể được kéo dài vô tận. Trả về True trong trường hợp này. 
4. Nếu không tồn tại chu trình, hãy tính độ dài của đường đi dài nhất trong sơ đồ con bằng cách sử dụng DFS có tính năng ghi nhớ. Nếu con đường dài nhất có ít nhất$k$đỉnh, trả về True; nếu không thì trả về Sai. 
5. Tìm kiếm nhị phân trên các giá trị đỉnh duy nhất được sắp xếp. Đối với mỗi ứng viên`x`, sử dụng`can(x)`để xác định xem nó có thể hay không. Tiếp tục thu hẹp tìm kiếm để tìm giá trị tối đa khả thi tối thiểu. 
6. Nếu không có ứng viên nào vượt qua bài kiểm tra (tất cả các biểu đồ con không hỗ trợ$k$di chuyển), quay trở lại$-1$. 

Điều bất biến là chúng tôi luôn tìm kiếm trong số các giá trị đỉnh hợp lệ và chỉ xem xét các đồ thị con không có giá trị nào vượt quá ngưỡng ứng cử viên. DFS xác định chính xác các chu kỳ, đảm bảo rằng chúng tôi không đánh giá thấp khả năng tiếp cận. Tính toán đường dẫn dài nhất được ghi nhớ đảm bảo đánh giá chính xác trong các sơ đồ con không theo chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)

def solve():
    n, m, k = map(int, input().split())
    a = list(map(int, input().split()))
    edges = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        edges[u-1].append(v-1)

    unique_vals = sorted(set(a))

    def can(x):
        subgraph = [[] for _ in range(n)]
        for u in range(n):
            if a[u] <= x:
                for v in edges[u]:
                    if a[v] <= x:
                        subgraph[u].append(v)

        visited = [0]*n
        dp = [0]*n

        def dfs(u):
            visited[u] = 1
            max_len = 1
            for v in subgraph[u]:
                if visited[v] == 0:
                    ret = dfs(v)
                    max_len = max(max_len, 1 + ret)
                elif visited[v] == 1:
                    return k  # cycle detected
                else:
                    max_len = max(max_len, 1 + dp[v])
            visited[u] = 2
            dp[u] = max_len
            return dp[u]

        max_path = 0
        for i in range(n):
            if a[i] <= x and visited[i] == 0:
                length = dfs(i)
                if length >= k:
                    return True
                max_path = max(max_path, length)
        return max_path >= k

    left, right = 0, len(unique_vals)-1
    ans = -1
    while left <= right:
        mid = (left + right)//2
        if can(unique_vals[mid]):
            ans = unique_vals[mid]
            right = mid - 1
        else:
            left = mid + 1
    print(ans)

solve()
```Giải pháp trước tiên thiết lập sơ đồ con cho mỗi giá trị tối đa ứng viên. DFS được sử dụng cho cả việc phát hiện chu kỳ và tính toán đường đi dài nhất. các`visited`mảng phân biệt các nút không được truy cập, đang truy cập và đã xử lý để phát hiện các chu kỳ. Tìm kiếm nhị phân chỉ lặp lại các giá trị duy nhất, tránh việc kiểm tra lặp lại không cần thiết. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Bước | Ứng viên x | Đồ thị con đỉnh | Xe đạp? | Con đường dài nhất | có thể (x)? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | [1] | Không | 1 | Sai | 
| 2 | 2 | [1,3] | Không | 2 | Sai | 
| 3 | 3 | [1,3,4] | Không | 3 | Sai | 
| 4 | 4 | [1,3,4,5] | Không | 4 | Đúng | 

Mức tối đa khả thi tối thiểu là 4. Điều này phù hợp với đầu ra mẫu. 

**Mẫu 2** 

| Bước | Ứng viên x | Đồ thị con đỉnh | Xe đạp? | Con đường dài nhất | có thể (x)? | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | [2,4,5,6] | Có | - | Đúng | 

Chu kỳ cho phép lặp đi lặp lại các bước di chuyển, do đó, bất kỳ bước di chuyển lớn nào$k$là có thể. Tối đa tối thiểu là 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Mỗi DFS chạy trong O(n+m) và tìm kiếm nhị phân lặp lại O(log n) trên các giá trị đỉnh duy nhất | 
| Không gian | O(n+m) | Đối với danh sách kề, mảng đã truy cập và ghi nhớ | 

Giải pháp dễ dàng phù hợp với các ràng buộc: DFS và ghi nhớ xử lý 200.000 đỉnh một cách hiệu quả và tìm kiếm nhị phân thêm hệ số logarit trên phạm vi số nguyên 32 bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("""6 7 4
1 10 2 3 4 5
1 2
1 3
3 4
4 5
5 6
6 2
2 5""") == "4", "sample 1"

# Minimum size
assert run("""1 0 1
7""") == "7", "single vertex, 1 move"

# Impossible moves
assert run("""3 2 5
1 2 3
1 2
2 3""") == "-1", "long path impossible"

#
```
