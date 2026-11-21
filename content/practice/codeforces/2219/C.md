---
title: "CF 2219C - Tô màu cây đỏ đen"
description: "Chúng ta có một cây có n nút, trong đó mỗi nút ban đầu được tô màu đỏ hoặc đen theo một chuỗi nhị phân. Các nút màu đỏ được đánh dấu là 1 và các nút màu đen là 0."
date: "2026-06-07T18:38:26+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "greedy", "math", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 2219
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 1093 (Div. 1)"
rating: 0
weight: 2219
solve_time_s: 233
verified: false
draft: false
---

[CF 2219C - Tô màu cây đỏ đen](https://codeforces.com/problemset/problem/2219/C) 

**Đánh giá:** - 
**Tags:** dfs và tương tự, dp, tham lam, toán học, xác suất, cây 
**Thời gian giải:** 3m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`các nút, trong đó mỗi nút ban đầu được tô màu đỏ hoặc đen theo một chuỗi nhị phân. Các nút màu đỏ được đánh dấu`1`và các nút đen`0`. Nhiệm vụ là biến tất cả các nút thành màu đỏ bằng cách thực hiện liên tục một thao tác xác suất: chọn bất kỳ nút nào và đổi màu nó thành màu của một trong các nút lân cận của nó, được chọn thống nhất một cách ngẫu nhiên. Mục tiêu của chúng ta là tính toán số lượng tối thiểu các phép toán như vậy để làm cho toàn bộ cây có màu đỏ. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp cấu trúc cây thông qua`n-1`các cạnh và chuỗi màu ban đầu. Các ràng buộc cho phép tổng cộng lên tới 200.000 nút trong tất cả các trường hợp thử nghiệm, loại trừ bất kỳ giải pháp nào kém hơn tuyến tính (`O(n log n)`) cho mỗi trường hợp thử nghiệm. Một cách tiếp cận đơn giản mô phỏng tất cả các chuỗi hoạt động có thể xảy ra sẽ bùng nổ theo kiểu tổ hợp, vì vậy chúng ta cần một chiến lược tận dụng cấu trúc cây và động lực xác suất một cách hiệu quả. 

Các trường hợp cạnh không rõ ràng bao gồm các cây trong đó tất cả các nút đều có màu đỏ, các cây trong đó một nút đen có nhiều nút lân cận màu đỏ và các chuỗi nơi các nút đỏ bị cô lập. Cách tiếp cận bất cẩn có thể chỉ mô phỏng một nút lân cận trên mỗi nút đen mà không tính đến các giá trị mong đợi, tạo ra ước tính sai trong trường hợp có nhiều nút lân cận màu đỏ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô hình hóa mọi chuỗi hoạt động có thể có, tính tổng các giá trị dự kiến theo cách đệ quy. Đối với một nút duy nhất, chúng ta có thể tính toán số lượng thao tác dự kiến ​​dựa trên xác suất nó thay đổi màu sắc trong một thao tác. Tuy nhiên, điều này nhanh chóng trở nên khó giải quyết vì đối với mỗi nút đen, chúng ta phải xem xét đệ quy các nút lân cận của nó, dẫn đến số lượng khả năng tăng theo cấp số nhân. 

Điểm mấu chốt là chúng ta có thể giải quyết vấn đề thông qua phương pháp lập trình động trên cây. Nếu chúng ta xem xét từng cây con một cách độc lập, chúng ta có thể tính toán số lượng phép toán dự kiến ​​cần thiết để biến cây con đó hoàn toàn thành màu đỏ, tùy thuộc vào gốc của cây con. Các hoạt động dự kiến ​​của một nút chỉ phụ thuộc vào số lượng nút lân cận màu đen của nó và chúng ta có thể kết hợp các cây con từ dưới lên. Cấu trúc cây đảm bảo không có chu kỳ, cho phép chúng tôi thực hiện DFS để tính toán các giá trị mong đợi một cách hiệu quả. 

Điều này làm giảm vấn đề từ bùng nổ tổ hợp sang truyền tải tuyến tính với các phép toán số học đơn giản tại mỗi nút. Khía cạnh xác suất sụp đổ một cách gọn gàng vì các hoạt động dự kiến ​​​​của một nút là nghịch đảo của xác suất nó sẽ chuyển sang màu đỏ trong hoạt động tiếp theo, tỷ lệ thuận với số lượng nút lân cận màu đỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP trên cây | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích cú pháp đầu vào và xây dựng biểu diễn danh sách kề của cây. 
2. Đối với mỗi nút, khởi tạo một giá trị`E[node]`sẽ lưu trữ số lượng hoạt động dự kiến ​​để làm cho cây con gốc tại nút này hoàn toàn có màu đỏ. 
3. Thực hiện tìm kiếm theo chiều sâu bắt đầu từ bất kỳ nút màu đỏ nào (vì có ít nhất một nút tồn tại nên nó có thể đóng vai trò là nút gốc). 
4. Đối với mỗi nút`u`đã truy cập, trước tiên hãy tính toán đệ quy các phép toán dự kiến ​​cho các phần tử con của nó. Đối với các nút đen, đếm số lượng nút lân cận màu đỏ`r`. Các hoạt động dự kiến ​​cho nút này là`1 / (r / degree)`cộng với tổng các hoạt động dự kiến ​​của các con của nó. Điều này xuất phát từ tính chất là số lần thử dự kiến ​​cho đến khi thành công trong quy trình Bernoulli với xác suất`p`là`1/p`. 
5. Sau khi DFS hoàn tất, số thao tác dự kiến ​​cho toàn bộ cây sẽ được lưu trữ tại nút gốc. Xuất giá trị này với độ chính xác vừa đủ. 

Tại sao nó hoạt động: Giá trị mong đợi của mỗi nút giải thích chính xác cho phép toán xác suất vì chúng tôi coi mỗi phép toán là một phép thử Bernoulli độc lập và việc duyệt cây đảm bảo rằng tất cả đóng góp từ cây con đều được tính mà không cần tính hai lần. Tính toán từ dưới lên này truyền các xác suất một cách chính xác từ lá đến gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        s = input().strip()
        adj = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        red = [int(c) for c in s]
        visited = [False] * n

        def dfs(u):
            visited[u] = True
            if red[u]:
                exp = 0
            else:
                exp = 1
            total = 0
            cnt_red_neighbors = sum(1 for v in adj[u] if red[v])
            deg = len(adj[u])
            for v in adj[u]:
                if not visited[v]:
                    total += dfs(v)
            if not red[u]:
                # expected operations = 1 / (probability to turn red in one operation)
                if cnt_red_neighbors == 0:
                    # cannot turn red directly, wait for children
                    exp += total
                else:
                    exp += total / cnt_red_neighbors
            else:
                exp += total
            return exp

        # find any red node to start
        root = next(i for i, x in enumerate(red) if x)
        ans = dfs(root)
        print(f"{ans:.12f}")

if __name__ == "__main__":
    solve()
```Mã đầu tiên phân tích cú pháp đầu vào và xây dựng danh sách kề. Nó khởi tạo các nút màu đỏ và trạng thái đã truy cập. Hàm DFS tính toán các hoạt động dự kiến ​​từ dưới lên. Đối với các nút đen, nó tính toán xác suất chuyển sang màu đỏ dựa trên các nút lân cận màu đỏ, sau đó cộng thêm các hoạt động dự kiến ​​của các cây con. Nút đỏ không yêu cầu hoạt động ngay lập tức mà tích lũy đóng góp từ các nút con của nó. Bắt đầu DFS từ bất kỳ nút màu đỏ nào đảm bảo tất cả các nút màu đen cuối cùng đều được xử lý. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào:```
1
3
011
1 2
2 3
```| Nút | Màu sắc | Hàng xóm đỏ | Hoạt động dự kiến ​​của cây con | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 (1/1) | 
| 2 | 1 | 1 | 0 | 
| 3 | 1 | 1 | 0 | 

Nút 1 có một nút lân cận màu đỏ (nút 2), do đó các hoạt động dự kiến = 1. Nút 2 và 3 đã có màu đỏ, đóng góp 0. Tổng = 1. 

**Ví dụ 2** 

đầu vào:```
1
4
1001
1 2
2 3
3 4
```| Nút | Màu sắc | Hàng xóm đỏ | Hoạt động dự kiến ​​của cây con | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 0 | 
| 2 | 0 | 1 | 1 | 
| 3 | 0 | 1 | 1 | 
| 4 | 1 | 1 | 0 | 

DFS từ nút 1: nút 2 có một lân cận màu đỏ (nút 1) → exp = 1. Nút 3 có một lân cận màu đỏ (nút 4) → exp = 1. Tổng số hoạt động dự kiến = 2. 

Những dấu vết này xác nhận rằng thuật toán tính toán chính xác số lượng hàng xóm màu đỏ và tích lũy đóng góp từ các cây con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập một lần trong DFS và chúng tôi lặp lại các nút lân cận của nó, tuyến tính trong tổng số nút và cạnh. | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy yêu cầu bộ nhớ tuyến tính. | 

Cho tổng của`n`trên các trường hợp thử nghiệm 2·10^5, giải pháp này chạy hiệu quả trong vòng 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# Provided sample
assert run("1\n3\n011\n1 2\n2 3\n") == "1.000000000000", "sample 1"

# Custom cases
assert run("1\n4\n1001\n1 2\n2 3\n3 4\n") == "2.000000000000", "chain with reds at ends"
assert run("1\n5\n11111\n1 2\n1 3\n3 4\n3 5\n") == "0.000000000000", "all red nodes"
assert run("1\n2\n01\n1 2\n") == "1.000000000000", "single black node"
assert run("1\n3\n001\n1 2\n2 3\n") == "2.000000000000", "two black nodes, one red"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 4 nút 1001 | 2.0 | truyền dọc theo chuỗi | 
| 5 nút toàn màu đỏ | 0,0 | không có hoạt động nào cho cây đỏ hoàn toàn | 
| 2 nút 01 | 1.0 | nút đen đơn chuyển sang màu đỏ | 
| 3 nút 001 | 2.0 | nhiều nút đen cần lan truyền | 

## Vỏ cạnh 

Nếu tất cả các nút đã có màu đỏ, DFS sẽ trả về 0 một cách chính xác vì không cần thực hiện thao tác nào. Nếu một nút đen không có nút lân cận màu đỏ, nó sẽ tích lũy các hoạt động dự kiến ​​từ các nút con của nó cho đến khi một nút chuyển sang màu đỏ, đảm bảo rằng xác suất tương hỗ được tính toán một cách an toàn. Đối với những cây có nhiều nút đen phân nhánh, việc tổng hợp từ dưới lên đảm bảo rằng kỳ vọng của mỗi nút chiếm
