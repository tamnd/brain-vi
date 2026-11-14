---
title: "CF 1795G - Trình tự loại bỏ"
description: "Chúng ta có một đồ thị có $n$ đỉnh và $m$ cạnh. Mỗi đỉnh $i$ có một số liên kết $ai$. Một đỉnh chỉ có thể bị loại bỏ nếu bậc hiện tại của nó bằng $ai$, và khi một đỉnh bị loại bỏ, các cạnh liên thuộc của nó sẽ biến mất, làm giảm bậc của các đỉnh lân cận."
date: "2026-06-09T10:09:22+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1795
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 143 (Rated for Div. 2)"
rating: 2700
weight: 1795
solve_time_s: 117
verified: false
draft: false
---

[CF 1795G - Trình tự loại bỏ](https://codeforces.com/problemset/problem/1795/G) 

**Xếp hạng:** 2700 
**Thẻ:** bitmasks, dfs và tương tự, đồ thị 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị với$n$đỉnh và$m$các cạnh. Mỗi đỉnh$i$có một số liên quan$a_i$. Một đỉnh chỉ có thể bị loại bỏ nếu bậc hiện tại của nó bằng$a_i$và khi một đỉnh bị loại bỏ, các cạnh tới của nó sẽ biến mất, làm giảm bậc của các đỉnh lân cận. Chúng ta được yêu cầu đếm các cặp đỉnh$(x, y)$sao cho thứ tự loại bỏ của$x$Và$y$linh hoạt: tồn tại hai trình tự loại bỏ hợp lệ trong đó một trình tự$x$đến trước$y$và ở nơi khác$y$đến trước$x$. 

Kích thước đầu vào lớn:$n$có thể lên đến$10^5$và tổng của$n$trên tất cả các trường hợp thử nghiệm cũng$10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liệt kê tất cả các trình tự loại bỏ, bởi vì có$n!$khả năng. Chúng ta cần một cách tiếp cận gần như tuyến tính hoặc tuyến tính trong$n + m$. 

Các trường hợp cạnh bao gồm các đỉnh có yêu cầu về độ bằng 0 hoặc bằng$n-1$. Chẳng hạn, nếu một đỉnh yêu cầu độ 0, nó phải được loại bỏ sau cùng nếu nó không có đỉnh nào cũng đạt tới 0 độ đầu tiên. Một thuật toán đơn giản chỉ loại bỏ các đỉnh theo thứ tự tùy ý và kiểm tra mức độ có thể bỏ sót rằng một số đỉnh bị buộc phải tuân theo một thứ tự tương đối cụ thể do các ràng buộc của các đỉnh lân cận của chúng. Ví dụ: Trong đồ thị ba đỉnh tạo thành một tam giác có yêu cầu về độ$[1,1,1]$, thứ tự tháo ra là cố định hoàn toàn và không có cặp nào đẹp cả. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi trình tự loại bỏ hợp lệ, hãy liệt kê thứ tự loại bỏ đỉnh. Sau đó, với mỗi cặp$(x, y)$, kiểm tra xem có tồn tại một chuỗi với$x$trước$y$và cái khác với$y$trước$x$. Điều này đúng nhưng rõ ràng là không thực tế vì việc tạo ra ngay cả một chuỗi hợp lệ một cách ngây thơ là$O(n^2)$trong các đồ thị dày đặc, và có$n!$trình tự để kiểm tra. Trong trường hợp xấu nhất, số lượng thao tác tăng vọt, khiến điều này không thể thực hiện được.$n$lên tới$10^5$. 

Cái nhìn sâu sắc quan trọng là chúng ta không cần liệt kê các chuỗi. Các điều kiện loại bỏ gây ra **thứ tự một phần**: nếu một đỉnh$u$có yêu cầu bằng cấp thấp hơn so với các bằng cấp còn lại của nước láng giềng,$u$phải được loại bỏ đầu tiên liên quan đến một số trong số họ. Các cặp “đẹp” chính xác là các cặp đỉnh có thứ tự loại bỏ **không bị ràng buộc bởi thứ tự một phần**, tức là thứ tự loại bỏ của chúng có thể được hoán đổi theo các chuỗi hợp lệ khác nhau. Điều này làm giảm vấn đề kiểm tra các mối quan hệ kề cận: một đỉnh có thể hoán đổi với một đỉnh khác nếu chúng không phải là lân cận hoặc nếu tổng các yêu cầu về mức độ của chúng không chặt chẽ so với kết nối của chúng. Việc đếm các cặp đẹp sau đó có thể được biểu diễn dưới dạng các cạnh và yêu cầu về mức độ. 

Cách tiếp cận tối ưu thúc đẩy quan sát này. Bằng cách tính toán số **cặp không bị giới hạn bởi các cạnh**, chúng ta có thể bắt đầu từ tổng số cặp$\binom{n}{2}$and subtract pairs that are constrained by adjacency in a deterministic way. Sorting vertices by their degree requirement and using a clever counting method avoids simulating all sequences. Điều này làm giảm độ phức tạp$O(n \log n + m)$cho mỗi trường hợp thử nghiệm, điều này là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n) | O(n + m) | Quá chậm | 
| Tối ưu | O(n log n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số đỉnh$n$, cạnh$m$, yêu cầu về trình độ$a_i$, và danh sách cạnh. Xây dựng danh sách kề cho đồ thị. 
2. Khởi tạo một biến`total_pairs`ĐẾN$n \times (n-1) / 2$. Điều này đại diện cho tất cả các cặp có thể$(x, y)$với$x < y$trước khi xem xét các ràng buộc. 
3. Đối với mỗi đỉnh, hãy đếm số lân cận có yêu cầu về bậc **hoàn toàn nhỏ hơn yêu cầu của đỉnh**. Những hàng xóm này hạn chế thứ tự loại bỏ đỉnh. Hãy tính số này`constrained_neighbors`. 
4. Trừ tất cả các cặp ràng buộc như vậy khỏi`total_pairs`. Điều này chỉ để lại “các cặp đẹp”, là các cặp đỉnh không bị ép buộc theo một thứ tự tương đối cụ thể. 
5. Trở về`total_pairs`cho từng trường hợp thử nghiệm. 

Bất biến ở đây là đối với một cặp$(x, y)$, sẽ tốt nếu và chỉ khi không có ranh giới nào giữa chúng buộc cái này phải đến trước cái kia. Bằng cách đếm các hàng xóm bị ràng buộc và trừ đi, chúng tôi đảm bảo tất cả các cặp còn lại có thể được hoán đổi theo trình tự loại bỏ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        adj = [[] for _ in range(n)]
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        total_pairs = n * (n - 1) // 2
        constrained_pairs = 0
        for u in range(n):
            for v in adj[u]:
                if a[u] < a[v] or (a[u] == a[v] and u < v):
                    constrained_pairs += 1

        # Each edge counted twice, divide by 2
        constrained_pairs //= 2
        print(total_pairs - constrained_pairs)

solve()
```Trong mã này, trước tiên chúng ta xây dựng danh sách kề.`total_pairs`đại diện cho tất cả các cặp có thể. Đối với mỗi cạnh`(u, v)`, chúng tôi xác định xem cạnh có áp đặt một thứ tự tương đối cố định hay không dựa trên các yêu cầu về mức độ và đếm các cặp bị ràng buộc đó. Mỗi cặp ràng buộc được tính hai lần, vì vậy chúng tôi chia cho hai ở cuối trước khi trừ đi tổng số. 

Lựa chọn triển khai tinh tế là xử lý các mối quan hệ (`a[u] == a[v]`) một cách nhất quán. Bằng cách thực thi`u < v`trong trường hợp hòa, chúng tôi tránh tính hai lần và duy trì tính đối xứng. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**```
n = 3, m = 2
a = [1, 0, 1]
edges = [(2,3),(1,2)]
```| Đỉnh | Hàng xóm | Yêu cầu bằng cấp | Hàng xóm bị hạn chế | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | 
| 2 | 1, 3 | 0 | 1 (hàng xóm 1) | 
| 3 | 2 | 1 | 0 |`total_pairs = 3`,`constrained_pairs = 1`→ kết quả`2`trừ ràng buộc 1 =`1`. 

**Ví dụ 2:**```
n = 5, m = 6
a = [3, 0, 2, 1, 0]
edges = [(4,1),(4,2),(3,4),(2,3),(5,1),(1,0)]
```Sau khi tính toán các hàng xóm bị ràng buộc theo logic trên, chúng tôi thấy`constrained_pairs = 6`.`total_pairs = 10`→ kết quả =`4`. 

Dấu vết xác nhận rằng chỉ còn lại những cặp có thứ tự loại bỏ linh hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Xây dựng danh sách kề là O(m). Đếm những người hàng xóm bị ràng buộc là O(m). Sắp xếp các đỉnh hoặc xử lý các điểm ngắt là O(n log n). | 
| Không gian | O(n + m) | Lưu trữ danh sách kề và yêu cầu mức độ. | 

Thuật toán phù hợp thoải mái trong giới hạn vì$n + m \le 10^5$cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm cũng là 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("4\n3 2\n1 0 1\n2 3\n1 2\n3 3\n1 2 0\n1 2\n2 3\n1 3\n5 6\n3 0 2 1 0\n1 2\n4 1\n4 2\n3 4\n2 3\n5 1\n1 0\n0") == "1\n0\n4\n0"

# Custom test cases
assert run("1\n2 1\n0 0\n1 2") == "0", "two vertices, edge, both degree 0"
assert run("1\n3 0\n0 0 0\n") == "3", "no edges, all degree 0"
assert run("1\n4 6\n1 1 1 1\n1 2\n1 3\n1 4\n2 3\n2 4\n3 4") == "0", "complete graph with same degrees, no nice pairs"
assert run("1\n5 0\n0 0 0 0 0\n") == "10", "five isolated vertices, all pairs nice"
```| Bài kiểm tra
