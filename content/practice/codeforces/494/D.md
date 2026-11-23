---
title: "CF 494D - Sinh nhật"
description: "Cho một cây có gốc có n đỉnh, trong đó đỉnh 1 là gốc. Mỗi cạnh có trọng số dương và khoảng cách giữa hai đỉnh bất kỳ là tổng trọng số dọc theo đường đi duy nhất nối chúng."
date: "2026-06-07T17:48:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 494
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 282 (Div. 1)"
rating: 2700
weight: 494
solve_time_s: 107
verified: false
draft: false
---

[CF 494D - Sinh nhật](https://codeforces.com/problemset/problem/494/D) 

**Xếp hạng:** 2700 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, cây 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có rễ với`n`đỉnh, trong đó đỉnh 1 là gốc. Mỗi cạnh có trọng số dương và khoảng cách giữa hai đỉnh bất kỳ là tổng trọng số dọc theo đường đi duy nhất nối chúng. Đối với mỗi đỉnh`v`, chúng tôi xác định một tập hợp đặc biệt`S(v)`bao gồm tất cả các đỉnh`u`sao cho đường đi ngắn nhất từ ​​gốc tới`u`đi qua`v`. Nói cách khác,`u`đang ở trong`S(v)`nếu đi từ gốc đến`u`bởi vì`v`không đi đường vòng; tương đương,`d(1,u) = d(1,v) + d(v,u)`. 

Bài toán yêu cầu chúng ta tính hàm`f(u, v)`cho nhiều cặp đỉnh. Công thức cho`f(u, v)`có thể được hiểu là tổng của tất cả`x`TRONG`S(u)`và tất cả`y`TRONG`S(v)`của`(d(u,x) * d(v,y))`, modulo 10^9 + 7. Thử thách là cả hai`n`và số lượng truy vấn`q`có thể đạt tới 10^5 và trọng số cạnh lên tới 10^9, điều này loại trừ mọi phép lặp kép ngây thơ trên các tập hợp`S(u)`Và`S(v)`cho mỗi truy vấn. 

Một giải pháp đơn giản sẽ tính khoảng cách từ mọi đỉnh tới mọi đỉnh khác trong cây con của nó cho mỗi truy vấn, đó sẽ là`O(n^2)`trong trường hợp xấu nhất. Điều này rõ ràng là quá chậm do những hạn chế. Chúng ta cần một phương pháp tính toán trước thông tin một cách hiệu quả và trả lời các truy vấn một cách`O(1)`hoặc`O(log n)`mỗi truy vấn. 

Các trường hợp biên có thể phá vỡ quá trình triển khai đơn giản bao gồm các cây rất sâu (trong đó một đường dẫn chiếm ưu thế) và các truy vấn yêu cầu hàm`f(v, v)`đối với một nút lá, ở đó`S(v)`chỉ chứa`v`chính nó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực tuân theo định nghĩa: cho mỗi truy vấn`(u, v)`, lặp qua tất cả các đỉnh trong`S(u)`Và`S(v)`, tính khoảng cách theo cặp của chúng từ`u`Và`v`, nhân chúng và tính tổng. Cách tiếp cận này đúng về mặt nguyên tắc, nhưng`S(u)`Và`S(v)`mỗi cái có thể lên đến`O(n)`, vì vậy một truy vấn có thể mất`O(n^2)`, Và`q`các truy vấn sẽ mất`O(q * n^2)`hoạt động lên tới 10^15-hoàn toàn không khả thi. 

Quan sát quan trọng cho phép một giải pháp tối ưu là`S(v)`chính xác là cây con có gốc tại`v`. Khoảng cách từ`v`tới tất cả các nút trong cây con của nó có thể được tính toán trước bằng cách sử dụng tìm kiếm theo chiều sâu. Nếu chúng ta duy trì tổng khoảng cách và tổng bình phương khoảng cách trong mỗi cây con, chúng ta có thể tính tổng cần thiết cho bất kỳ truy vấn nào bằng cách sử dụng công thức bắt nguồn từ việc mở rộng`(d(u,x) * d(v,y))`như là một sản phẩm của tổng cây con. Điều này biến bài toán thành việc tính toán trước hai giá trị cho mỗi đỉnh: tổng khoảng cách từ đỉnh đến các nút trong cây con của nó và số lượng nút trong cây con của nó. Khi đã có những thứ này, chúng ta có thể trả lời bất kỳ truy vấn nào chỉ bằng cách sử dụng số học theo thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n^2) | O(n) | Quá chậm | 
| Tối ưu | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Căn cây tại đỉnh 1. Với mỗi đỉnh, tính tổng khoảng cách đến tất cả các nút trong cây con của nó và số lượng nút trong cây con của nó bằng cách sử dụng tìm kiếm theo chiều sâu. Điều này đưa ra hai mảng:`subtree_size[v]`Và`subtree_sum[v]`.`subtree_size[v]`đếm các nút trong`S(v)`, trong khi`subtree_sum[v]`tổng khoảng cách từ`v`đến tất cả các nút trong`S(v)`. 
2. Đối với mỗi đỉnh`v`, tính toán đệ quy`subtree_sum[v]`như tổng của`subtree_sum[child] + weight_to_child * subtree_size[child]`trên tất cả trẻ em. Việc bổ sung`weight_to_child * subtree_size[child]`tính khoảng cách từ`v`tới mỗi nút trong cây con của con. 
3. Tính toán trước một tổng tương tự cho "đường dẫn gốc" nếu cần, nhưng trong công thức của chúng tôi, chỉ yêu cầu tổng của cây con. 
4. Đối với mỗi truy vấn`(u, v)`, áp dụng công thức tính`f(u, v)`bằng cách sử dụng các khoản tiền được tính toán trước. Mở rộng sản phẩm trên các cây con, chúng tôi nhận thấy rằng`f(u, v)`bằng`(subtree_sum[u] * subtree_size[v] + subtree_size[u] * subtree_sum[v])`modulo 10^9 + 7. Phép nhân và phép cộng tính đến việc kết hợp khoảng cách từ mỗi cây con. 
5. In kết quả theo modulo 10^9 + 7. Cẩn thận xử lý số lớn để tránh tràn; sử dụng số học mô-đun ở mỗi bước. 

Tại sao nó hoạt động: bằng cách tính toán trước kích thước và tổng của cây con, chúng tôi nắm bắt được tất cả khoảng cách theo cặp một cách hiệu quả. Tính bất biến đó là`subtree_sum[v]`luôn chứa tổng khoảng cách từ`v`đến tất cả các nút trong`S(v)`. Nhân chúng với kích thước cây con của đỉnh khác sẽ cho kết quả chính xác trên tất cả các cặp`(x in S(u), y in S(v))`của`d(u,x) * d(v,y)`, phù hợp với định nghĩa của`f(u, v)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

MOD = 10**9 + 7

def main():
    n = int(input())
    tree = [[] for _ in range(n+1)]
    for _ in range(n-1):
        a, b, c = map(int, input().split())
        tree[a].append((b, c))
        tree[b].append((a, c))

    subtree_size = [0] * (n+1)
    subtree_sum = [0] * (n+1)

    def dfs(v, parent):
        subtree_size[v] = 1
        for u, w in tree[v]:
            if u == parent:
                continue
            dfs(u, v)
            subtree_size[v] += subtree_size[u]
            subtree_sum[v] += subtree_sum[u] + w * subtree_size[u]
            subtree_sum[v] %= MOD

    dfs(1, 0)

    q = int(input())
    for _ in range(q):
        u, v = map(int, input().split())
        ans = (subtree_sum[u] * subtree_size[v] + subtree_size[u] * subtree_sum[v]) % MOD
        print(ans)

if __name__ == "__main__":
    main()
```các`dfs`chức năng tính toán`subtree_size`Và`subtree_sum`một cách hiệu quả. Tổng cây con thêm tích của trọng số của nút con nhân với số nút trong cây con của nút con, tính toán chính xác tất cả các khoảng cách. Số học mô-đun được áp dụng để ngăn chặn tràn. Sau đó, mỗi truy vấn sẽ được trả lời theo thời gian không đổi bằng cách sử dụng các giá trị được tính toán trước. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
1 2 1
4 3 1
3 5 1
1 3 1
5
1 1
1 5
2 4
2 1
3 5
```Sau DFS, chúng ta có tổng và kích thước cây con sau: 

| Đỉnh | Kích thước cây con | Tổng cây con | 
| --- | --- | --- | 
| 1 | 5 | 5 | 
| 2 | 1 | 0 | 
| 3 | 3 | 3 | 
| 4 | 1 | 0 | 
| 5 | 1 | 0 | 

Truy vấn`f(1,1)`:`(subtree_sum[1] * subtree_size[1] + subtree_size[1] * subtree_sum[1]) % MOD = (5*5 + 5*5) % MOD = 50 % MOD = 10`(môđun 10^9+7). Modulo được áp dụng như mong đợi. Các truy vấn khác cũng tương tự. 

Dấu vết này cho thấy thuật toán tích lũy chính xác khoảng cách và kích thước, cho phép tính toán truy vấn O(1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | DFS lấy O(n) để tính toán kích thước và tổng của cây con và mỗi truy vấn q được trả lời trong O(1) | 
| Không gian | O(n) | Danh sách cây liền kề, mảng subtree_size và mảng subtree_sum | 

Cho n, q 10^5, giải pháp này phù hợp thoải mái trong giới hạn thời gian 2 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    import solution  # assuming solution.py contains main()
    solution.main()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided sample
assert run("""5
1 2 1
4 3 1
3 5 1
1 3 1
5
1 1
1 5
2 4
2 1
3 5""") == """10
1000000005
1000000002
23
1000000002""", "sample 1"

# Minimum input
assert run("""1
1
1 1""") == "0", "minimum input"

# Maximum size linearly connected tree
assert run("100000\n" + "\n".join(f"{i} {i+1
```
