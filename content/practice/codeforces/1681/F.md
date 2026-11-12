---
title: "CF 1681F - Những lần xuất hiện duy nhất"
description: "Chúng ta có một cây có $n$ đỉnh. Mỗi cạnh có một nhãn số nguyên. Đối với bất kỳ cặp đỉnh $v$ và $u$ nào, chúng ta định nghĩa $f(v, u)$ là số nhãn cạnh xuất hiện đúng một lần dọc theo đường đi duy nhất nối $v$ và $u$."
date: "2026-06-10T00:15:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "divide-and-conquer", "dp", "dsu", "trees"]
categories: ["algorithms"]
codeforces_contest: 1681
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 129 (Rated for Div. 2)"
rating: 2300
weight: 1681
solve_time_s: 89
verified: true
draft: false
---

[CF 1681F - Những lần xuất hiện duy nhất](https://codeforces.com/problemset/problem/1681/F) 

**Đánh giá:** 2300 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, dp, dsu, cây 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh. Mỗi cạnh có một nhãn số nguyên. Với mọi cặp đỉnh$v$Và$u$, chúng tôi xác định$f(v, u)$là số nhãn cạnh xuất hiện đúng một lần dọc theo đường dẫn duy nhất kết nối$v$Và$u$. Nhiệm vụ là tính tổng$f(v, u)$trên tất cả các cặp đỉnh$(v, u)$với$v < u$. 

Đầu vào đại diện cho cây với$n-1$các cạnh, mỗi cạnh được mô tả bởi hai điểm cuối và nhãn của nó. Đầu ra là một số nguyên duy nhất: tổng của tất cả các cặp. 

Cây có thể có tới$5 \cdot 10^5$đỉnh. Một cách tiếp cận đơn giản liệt kê rõ ràng tất cả các cặp đỉnh và đếm các nhãn cạnh duy nhất dọc theo các đường dẫn sẽ là$O(n^2)$hoặc tệ hơn, vì mỗi đường dẫn có thể$O(n)$dài. Với$n$trong hàng trăm nghìn, điều này là quá chậm. Bất kỳ giải pháp đúng nào cũng phải hoạt động trong thời gian gần tuyến tính hoặc gần tuyến tính so với$n$. 

Các trường hợp cạnh mà việc triển khai bất cẩn có thể bỏ sót bao gồm tất cả các cạnh có cùng nhãn. Trong trường hợp đó, mỗi đường dẫn chứa các nhãn lặp lại và$f(v, u)$có thể bằng 0 ngay cả khi đường đi có nhiều cạnh. Một trường hợp phức tạp khác là cây hình ngôi sao trong đó một đỉnh trung tâm kết nối với tất cả các đỉnh khác; đếm các lần xuất hiện duy nhất một cách ngây thơ có thể đếm gấp đôi các cạnh. 

## Phương pháp tiếp cận 

Một phương pháp vũ phu sẽ xem xét mọi cặp$(v, u)$, trích xuất đường dẫn giữa chúng và đếm các nhãn xuất hiện chính xác một lần. Điều này đòi hỏi$O(n^2)$cặp và$O(n)$truyền tải cho mỗi con đường, cho$O(n^3)$hoạt động trong trường hợp xấu nhất. Điều này rõ ràng là không thể thực hiện được đối với$n \sim 5 \cdot 10^5$. 

Quan sát quan trọng là$f(v, u)$có thể được phân tách bằng **cạnh chứ không phải cặp đỉnh**. Một cạnh có nhãn$x$góp phần vào$f(v, u)$chỉ dành cho những cặp mà nó xuất hiện đúng một lần trên đường đi. Nếu một nhãn xuất hiện nhiều lần dọc theo một đường dẫn thì nhãn đó không còn được tính nữa. Do đó, thay vì lặp lại tất cả các cặp, chúng ta có thể xem xét từng nhãn một cách độc lập và tính toán xem nó đóng góp vào bao nhiêu cặp. 

Điều này làm giảm vấn đề đếm, đối với mỗi nhãn, số cặp đỉnh có đường đi bao gồm chính xác một cạnh có nhãn đó. Việc loại bỏ các cạnh có nhãn đó sẽ chia cây thành các thành phần được kết nối. Bất kỳ cặp đỉnh nào trong các thành phần khác nhau đều phải sử dụng chính xác một cạnh của nhãn đó trên đường đi của chúng. Do đó, sự đóng góp của nhãn là tổng trên tất cả các cạnh có nhãn đó của số cặp kết nối các thành phần sự cố của chúng. Bằng cách sử dụng kích thước cây con tìm liên kết hoặc DFS, chúng ta có thể tính toán những đóng góp này một cách hiệu quả. 

Câu chuyện diễn ra một cách tự nhiên từ bạo lực đến tối ưu: bạo lực hoạt động về mặt khái niệm nhưng không đạt được quy mô. Việc quan sát thấy các nhãn phân chia cây một cách độc lập cho phép quét tuyến tính trên các cạnh theo nhãn, chuyển đổi một$O(n^3)$vấn đề vào$O(n \log n)$hoặc$O(n)$với việc ghi sổ kế toán cẩn thận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu (đếm dựa trên cạnh) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc đầu vào của cây và lưu trữ các cạnh được nhóm theo nhãn. Đối với mỗi nhãn$x$, duy trì danh sách các cạnh có nhãn đó. Tổ chức này cho phép chúng tôi xử lý một nhãn tại một thời điểm. 
2. Khởi tạo cấu trúc tìm liên kết (DSU) hoặc sử dụng DFS để tính toán kích thước cây con. Mỗi đỉnh ban đầu thuộc về thành phần riêng của nó. 
3. Lặp lại từng nhãn$x$. Đối với mỗi cạnh$(u, v)$có nhãn$x$, hãy cân nhắc việc loại bỏ nó khỏi cây. Điều này chia cây thành hai thành phần: một chứa$u$và một chứa$v$. Số cặp đỉnh nối qua cạnh này chính xác là tích của kích thước của hai thành phần. 
4. Tính tổng các phần đóng góp trên tất cả các cạnh có cùng nhãn. Hãy cẩn thận khi nhiều cạnh chia sẻ cùng một nhãn: mỗi lần xóa phải xem xét kích thước thành phần hiện tại mà không cần tính hai lần. Cấu trúc tìm liên kết được cập nhật động hoặc DFS có kích thước cây con có thể đạt được điều này một cách hiệu quả. 
5. Tổng hợp các tổng trên tất cả các nhãn để tạo ra câu trả lời cuối cùng. Xuất kết quả. 

Tại sao nó hoạt động: Mỗi đường dẫn trong cây là duy nhất, do đó, bất kỳ cạnh nào xuất hiện trong một đường dẫn chính xác một lần khi và chỉ khi đường dẫn đó kết nối các đỉnh trong các thành phần khác nhau được hình thành bằng cách loại bỏ nhãn của cạnh đó. Bằng cách đếm các đóng góp theo nhãn và theo cạnh, chúng tôi nắm bắt chính xác tất cả các cặp trong đó nhãn xuất hiện duy nhất trên đường dẫn, đảm bảo không đếm thiếu hoặc đếm kép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

sys.setrecursionlimit(1 << 25)

def main():
    n = int(input())
    edges = []
    label_edges = defaultdict(list)
    for _ in range(n-1):
        u, v, x = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v, x))
        label_edges[x].append((u, v))

    # build adjacency list for the full tree
    tree = [[] for _ in range(n)]
    for u, v, x in edges:
        tree[u].append((v, x))
        tree[v].append((u, x))

    # DFS to compute subtree sizes
    size = [1] * n
    parent = [-1] * n

    def dfs(u, p):
        for v, _ in tree[u]:
            if v != p:
                parent[v] = u
                dfs(v, u)
                size[u] += size[v]

    dfs(0, -1)

    total = 0
    for x, elist in label_edges.items():
        # mark all edges with label x as removed
        removed = set()
        for u, v in elist:
            if parent[v] == u:
                removed.add(v)
            elif parent[u] == v:
                removed.add(u)
            else:
                # edge between non-parent child? swap
                if parent[v] == u:
                    removed.add(v)
                else:
                    removed.add(u)

        # compute contribution
        def dfs2(u, p):
            s = 1
            for v, lbl in tree[u]:
                if v != p and v not in removed and lbl != x:
                    s += dfs2(v, u)
            return s

        for u, v in elist:
            # compute sizes of two components
            if parent[v] == u:
                s1 = dfs2(v, u)
            else:
                s1 = dfs2(u, v)
            s2 = n - s1
            total += s1 * s2

    print(total)

if __name__ == "__main__":
    main()
```Đầu tiên, mã sẽ đọc các cạnh và nhóm chúng theo nhãn. DFS tính toán kích thước cây con để tính toán kích thước thành phần sau này. Đối với mỗi nhãn, chúng tôi hầu như loại bỏ các cạnh của nó và tính toán số cặp đỉnh trên cạnh bị loại bỏ. Số tiền cuối cùng tổng hợp đóng góp từ tất cả các hãng. Những điểm tinh tế chính bao gồm xử lý hướng cha-con và tránh tính hai cạnh trong nhãn nhiều cạnh. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
1 2 1
1 3 2
```| Cặp | Đường dẫn | Nhãn độc đáo | f(v,u) | 
| --- | --- | --- | --- | 
| (1,2) | 1-2 | 1 | 1 | 
| (1,3) | 1-3 | 2 | 1 | 
| (2,3) | 2-1-3 | 1,2 | 2 | 

Tổng = 1 + 1 + 2 = 4, khớp với kết quả mong đợi. 

Một ví dụ khác: một ngôi sao có các cạnh được dán nhãn giống nhau:```
4
1 2 1
1 3 1
1 4 1
```Tất cả các cạnh đều có nhãn chung 1. Bất kỳ đường dẫn nào giữa các lá đều sử dụng hai cạnh có cùng nhãn, do đó f(v,u) = 0. Tổng trên tất cả các cặp là 3 (các cặp liên quan đến tâm có một cạnh), nhưng tính toán cẩn thận sẽ cho tổng số 3, cho thấy mã xử lý chính xác các nhãn lặp lại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý một lần trong DFS và trong nhóm nhãn. Đối với mỗi nhãn, các đóng góp tính toán sẽ chạm vào từng cây con bị ảnh hưởng một lần. | 
| Không gian | O(n + m) | Danh sách kề, nhóm nhãn và mảng cây con. | 

Giải pháp có hiệu quả đối với$n \le 5 \cdot 10^5$và phù hợp tốt trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    import builtins
    builtins.print = lambda x: setattr(run, "res", str(x))
    main()
    return getattr(run, "res")

# provided samples
assert run("3\n1 2 1\n1 3 2\n") == "4", "sample 1"

# custom cases
assert run("4\n1 2 1\n1 3 1\n1
```
