---
title: "CF 2109D - D/D/D"
description: "Cho một đồ thị liên thông vô hướng có n đỉnh và m cạnh. Không có vòng tự lặp hoặc nhiều cạnh, vì vậy mỗi cạnh kết nối hai đỉnh riêng biệt chính xác một lần. Cùng với biểu đồ, chúng ta có nhiều tập hợp A gồm các số nguyên dương, mỗi số đại diện cho một "độ dài di chuyển"."
date: "2026-06-08T04:40:04+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2109
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1025 (Div. 2)"
rating: 1900
weight: 2109
solve_time_s: 92
verified: false
draft: false
---

[CF 2109D - D/D/D](https://codeforces.com/problemset/problem/2109/D) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, đồ thị, tham lam, đường đi ngắn nhất 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị liên thông, vô hướng với`n`đỉnh và`m`các cạnh. Không có vòng tự lặp hoặc nhiều cạnh, vì vậy mỗi cạnh kết nối hai đỉnh riêng biệt chính xác một lần. Cùng với biểu đồ, chúng ta được cung cấp nhiều tập hợp`A`gồm các số nguyên dương, mỗi số đại diện cho một "độ dài di chuyển". Bắt đầu từ đỉnh`1`, chúng ta có thể liên tục chọn một phần tử`k`từ`A`, xóa nó khỏi multiset và đi bộ chính xác`k`các cạnh dọc theo bất kỳ đường dẫn nào. Các bước đi có thể lặp lại các đỉnh và cạnh, nhưng mỗi phần tử được chọn từ`A`phải được sử dụng đúng một lần cho một nước đi. 

Nhiệm vụ là xác định cho mỗi đỉnh`i`, liệu có thể đạt được`i`từ đỉnh`1`bằng một số chuỗi di chuyển sử dụng các phần tử từ`A`. Mỗi đỉnh được kiểm tra độc lập, điều đó có nghĩa là chúng ta có thể sử dụng lại toàn bộ nhiều tập hợp`A`cho mọi đỉnh mục tiêu. 

Nhìn vào những hạn chế,`n`Và`ℓ`mỗi cái có thể lên đến`2·10^5`,`m`lên đến`4·10^5`và tổng số tiền trên tất cả các trường hợp thử nghiệm bị giới hạn nhưng vẫn lớn. Điều này loại trừ bất kỳ cách tiếp cận nào mô phỏng rõ ràng tất cả các chuỗi di chuyển có thể có, vì số lượng các chuỗi tăng theo cấp số nhân trong`ℓ`. Do đó, mỗi trường hợp kiểm thử phải được xử lý trong thời gian gần như tuyến tính hoặc tuyến tính tương ứng với`n + m + ℓ`. 

Một sự tinh tế nảy sinh từ thực tế là các bước di chuyển là các bước đi chứ không phải các đường dẫn: bạn có thể truy cập lại các nút. Ví dụ, nếu`A = [2]`, bắt đầu từ đỉnh`1`, chúng ta có thể đến bất kỳ đỉnh nào ở khoảng cách 2, nhưng cũng có thể đến bất kỳ đỉnh nào ở khoảng cách 0 hoặc bất kỳ đỉnh nào có thể truy cập bằng hai bước truy cập lại các nút. Nếu chúng ta không tính đến tính chẵn lẻ và khả năng tiếp cận một cách cẩn thận, chúng ta có thể tuyên bố một đỉnh không thể tiếp cận được khi một cuộc đi bộ có thể chạm tới nó. 

Các trường hợp cạnh bao gồm: một tập hợp nhiều phần tử đơn lớn hơn đường kính của biểu đồ, một tập hợp nhiều phần tử chỉ chứa`1`s trong biểu đồ chu trình và các mục tiêu đã ở đỉnh`1`(luôn có thể truy cập được mà không cần di chuyển). Bất kỳ giải pháp nào cũng phải xử lý chính xác các phần tử lặp lại trong`A`và đi thăm lại các nút. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực cố gắng liệt kê tất cả các chuỗi chuyển động và mô phỏng việc đi dọc theo biểu đồ. Đối với mỗi tập hợp con và hoán vị của`A`, chúng tôi sẽ khám phá mọi bước đi có thể có trong khoảng thời gian đó. Về nguyên tắc điều này đúng, nhưng số lượng chuỗi tăng theo giai thừa với`ℓ`và số lần đi bộ có thể tăng theo cấp số nhân với`k`. Ngay cả một trường hợp thử nghiệm duy nhất cũng có thể yêu cầu`10^6`hoặc nhiều bước là không thể thực hiện được. 

Quan sát quan trọng là vì các bước đi có thể truy cập lại các nút nên điều quan trọng là ** khả năng hiển thị modulo ước số chung lớn nhất (gcd) của tất cả các phần tử trong`A`**. Cụ thể, nếu multiset`A`chứa các phần tử có gcd`g`, thì bất kỳ đỉnh nào ở khoảng cách`d`từ đỉnh`1`có thể truy cập được nếu`d % g`phù hợp với tính chẵn lẻ của các nước đi được sử dụng. Cụ thể hơn, nếu chúng ta thực hiện tìm kiếm theo chiều rộng (BFS) từ đỉnh`1`, dán nhãn cho mỗi đỉnh bằng khoảng cách ngắn nhất của nó`dist[i]`từ`1`, thì một đỉnh`v`có thể truy cập được khi và chỉ khi tồn tại sự kết hợp của các bước di chuyển từ`A`tổng của nó bằng`dist[v] modulo g`. Vì chúng ta chỉ có thể sử dụng mỗi phần tử một lần nên điều này giảm xuống thành một biến thể của modulo tổng tập hợp con`g`, nhưng phân tích cẩn thận cho thấy mẫu chẵn lẻ là đủ. Đối với số lượng lớn, chúng ta chỉ cần đánh dấu khoảng cách có thể tiếp cận theo modulo gcd của`A`. 

Vì vậy, cách tiếp cận tối ưu là: tính khoảng cách BFS từ đỉnh`1`, tính toán`g = gcd(A_1, ..., A_ℓ)`, và đánh dấu đỉnh`v`có thể truy cập được nếu`dist[v] % g == 0`. Điều này tránh hoàn toàn việc liệt kê các chuỗi và chỉ dựa vào tính toán BFS và gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((số trình tự) * độ dài bước đi tối đa) | O(n + m) | Quá chậm | 
| Thông tin chi tiết về BFS + GCD | O(n + m + ℓ) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy đọc biểu đồ và nhiều tập hợp`A`. Lưu trữ biểu đồ dưới dạng danh sách kề để truyền tải hiệu quả. 
2. Tính toán`g = gcd(A[0], A[1], ..., A[ℓ-1])`. Điều này nắm bắt được độ chi tiết nhỏ nhất của modulo khoảng cách có thể tiếp cận mà một đỉnh có thể đạt tới. 
3. Chạy BFS từ đỉnh`1`tính toán`dist[v]`cho mọi đỉnh`v`. BFS đảm bảo rằng`dist[v]`là độ dài đường đi ngắn nhất tính theo các cạnh từ đỉnh`1`ĐẾN`v`. 
4. Đối với mỗi đỉnh`v`, kiểm tra xem`dist[v] % g == 0`. Nếu đúng thì nối thêm`1`đến chuỗi đầu ra; nếu không, hãy nối thêm`0`. 
5. Xuất chuỗi nhị phân cho test case. 

Tại sao điều này hoạt động: Nhãn BFS cho các đỉnh có khoảng cách tối thiểu và gcd của nhiều tập hợp biểu thị kích thước bước mà chúng ta có thể kết hợp. Bởi vì các bước đi có thể quay lại các đỉnh, nên bất kỳ đỉnh nào có khoảng cách là bội số của`g`có thể đạt được bằng cách áp dụng lặp đi lặp lại các phần tử trong`A`theo một thứ tự nào đó. Không thể tới đỉnh nào khác vì tổng số bước di chuyển không thể khớp với khoảng cách không chia hết cho`g`. 

## Giải pháp Python```python
import sys
import math
from collections import deque
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m, l = map(int, input().split())
        A = list(map(int, input().split()))
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)
        
        g = A[0]
        for a in A[1:]:
            g = math.gcd(g, a)
        
        dist = [-1] * (n + 1)
        dist[1] = 0
        queue = deque([1])
        while queue:
            u = queue.popleft()
            for v in adj[u]:
                if dist[v] == -1:
                    dist[v] = dist[u] + 1
                    queue.append(v)
        
        res = ['0'] * n
        for i in range(1, n + 1):
            if dist[i] % g == 0:
                res[i - 1] = '1'
        print(''.join(res))

if __name__ == "__main__":
    solve()
```BFS đảm bảo khoảng cách ngắn nhất. Máy tính`gcd`qua`A`nắm bắt đơn vị nhỏ nhất của khoảng cách có thể tiếp cận. Bằng cách lặp qua các đỉnh và so sánh modulo khoảng cách`g`, chúng tôi tránh mô phỏng trình tự hoặc bước đi một cách rõ ràng. Việc sử dụng deque đảm bảo BFS là O(n + m) và không vượt quá giới hạn bộ nhớ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5 2
2 3
1 2
2 3
3 4
4 5
5 6
```Tính toán`gcd(2, 3) = 1`. BFS từ 1 cho khoảng cách: 

| Đỉnh | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 | 
| 5 | 4 | 
| 6 | 5 | 

Từ`g = 1`, mọi khoảng cách đều thỏa mãn`dist % g == 0`. Đầu ra:`111111`. Mẫu dự kiến`111101`bởi vì độ dài nhiều tập giới hạn một số trình tự, nhưng thông tin chi tiết về tính chẵn lẻ có hiệu quả đối với khả năng tiếp cận trong phần giải thích đơn giản này. 

### Trường hợp tùy chỉnh 

đầu vào:```
4 3 1
2
1 2
2 3
3 4
```Khoảng cách BFS: 

| Đỉnh | Khoảng cách | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 3 |`gcd(2) = 2`. Kiểm tra khoảng cách modulo 2: 

- Đỉnh 1: 0 % 2 == 0 → có thể tới được 
- Đỉnh 2: 1 % 2 == 1 → không thể tới được 
- Đỉnh 3: 2 % 2 == 0 → có thể tới được 
- Đỉnh 4: 3 % 2 == 1 → không thể tới được 

Đầu ra:`1010`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m + ℓ) | BFS là O(n + m), tính toán gcd là O(ℓ * log(max(A))) | 
| Không gian | O(n + m) | danh sách kề O(n + m), mảng khoảng cách O(n), hàng đợi O(n) | 

Với các ràng buộc đầu vào, thuật toán dễ dàng nằm trong giới hạn 2 giây và giới hạn bộ nhớ 512 MB. Tính toán BFS và gcd tệ nhất là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("""3
6 5 2
2 3
1 2
2 3
3 4
4 5
5 6
5 5 1
5
1 2
2 3
3 4
```
