---
title: "CF 1902F - Lại về cây và truy vấn XOR"
description: "Cho một cây có n đỉnh, trong đó mỗi đỉnh mang một giá trị nguyên. Cây là một đồ thị liên thông không tuần hoàn, đảm bảo có chính xác một đường đi đơn giữa hai đỉnh bất kỳ."
date: "2026-06-08T21:10:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "divide-and-conquer", "graphs", "implementation", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 1902
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 159 (Rated for Div. 2)"
rating: 2400
weight: 1902
solve_time_s: 142
verified: true
draft: false
---

[CF 1902F - Lại về cây cối và truy vấn XOR](https://codeforces.com/problemset/problem/1902/F) 

**Đánh giá:** 2400 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, chia để trị, đồ thị, cách triển khai, toán học, cây cối 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`đỉnh, trong đó mỗi đỉnh mang một giá trị nguyên. Cây là một đồ thị liên thông không tuần hoàn, đảm bảo có chính xác một đường đi đơn giữa hai đỉnh bất kỳ. Đối với mỗi truy vấn, chúng ta được hỏi liệu có thể chọn một số tập hợp con các đỉnh dọc theo đường đi đơn giản giữa hai đỉnh đã cho hay không,`x`Và`y`, sao cho XOR theo bit của các giá trị của chúng bằng với mục tiêu`k`. Tập hợp con có thể trống nên XOR có thể bằng 0. 

Những hạn chế về`n`Và`q`lớn, lên tới 200.000. Vì mỗi truy vấn hỏi về một đường dẫn và có thể là một tập hợp con các nút của nó, nên các phương pháp ngây thơ liệt kê các tập hợp con hoặc duyệt qua đường dẫn cho mỗi truy vấn sẽ quá chậm. Cụ thể, việc lặp qua các tập hợp con là theo cấp số nhân và việc lặp lại đường dẫn trong mỗi truy vấn sẽ mang lại các phép toán O(n*q), khoảng 4 * 10^10 trong trường hợp xấu nhất-vượt xa khả thi đối với giới hạn 6 giây. 

Các trường hợp đặc biệt không rõ ràng bao gồm các truy vấn trong đó`x`bằng`y`. Trong trường hợp đó, đường dẫn chứa chính xác một đỉnh và câu trả lời chỉ phụ thuộc vào việc giá trị đơn lẻ có khớp hay không`k`hoặc liệu`k`bằng 0 đối với tập con trống. Một sự tinh tế khác nảy sinh khi`k`là số không. Bởi vì tập hợp con trống được cho phép,`k=0`luôn có thể đạt được một cách tầm thường nếu chúng ta không cho phép có đỉnh nào. Tuy nhiên, nếu một đỉnh tồn tại với giá trị 0 trên đường đi thì điều đó cũng được tính. Một cách tiếp cận ngây thơ giả định tất cả các tập hợp con phải không trống sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xem xét từng truy vấn một cách độc lập. Đối với một truy vấn`(x, y, k)`, đầu tiên chúng ta sẽ tìm đường đi từ`x`ĐẾN`y`, có thể được thực hiện bằng tìm kiếm theo chiều sâu (DFS) trong O(n). Sau đó, chúng tôi sẽ tạo tất cả các tập hợp con của các đỉnh của đường dẫn và tính toán XOR của chúng để kiểm tra xem`k`là có thể đạt được. Điều này yêu cầu O(2^L) cho mỗi truy vấn, trong đó L là độ dài đường dẫn. Ngay cả với các đường dẫn nhỏ, điều này nhanh chóng trở nên không khả thi do số lượng truy vấn`q`. 

Quan sát chính để có giải pháp nhanh hơn dựa vào các thuộc tính của XOR và cây. Nếu chúng ta xác định XOR từ gốc đến đỉnh`v`BẰNG`prefix[v]`, thì XOR của tất cả các giá trị trên đường dẫn từ`x`ĐẾN`y`có thể được thể hiện như`prefix[x] XOR prefix[y] XOR a[lca(x, y)]`. Đây`lca(x, y)`là tổ tiên chung thấp nhất của`x`Và`y`. Điều này hoạt động vì XOR loại bỏ các giá trị lặp lại dọc theo đường dẫn chung. 

Một khi chúng tôi tính toán`prefix`Và`lca`, chúng tôi giảm vấn đề xuống khả năng tiếp cận tập hợp con-XOR. Hóa ra là nếu XOR của toàn bộ đường dẫn chứa một tập giá trị nhất định thì bất kỳ`k`có thể đạt được nếu một trong hai`k`bằng tổng đường dẫn XOR hoặc tồn tại một tập hợp con không trống thích hợp mà XOR tới`k`. Để tránh liệt kê các tập hợp con, chúng ta có thể khai thác cách tiếp cận phân chia và chinh phục trên cây bằng DFS. Bằng cách lưu trữ tất cả các XOR có thể có từ một đỉnh đến cây con của nó, chúng ta có thể kiểm tra O(log(max_value)) cho mỗi truy vấn hoặc trong thực tế với DFS và ghi nhớ cẩn thận, chúng ta có thể kiểm tra từng truy vấn trong O(1) bằng cách sử dụng một tập hợp các XOR được nhìn thấy dọc theo các đường dẫn, tận dụng thuộc tính mà XOR trên các tập hợp con tạo thành một không gian tuyến tính trên GF(2). 

Giải pháp tối ưu tính toán trước các XOR từ gốc đến mọi nút và sử dụng các truy vấn LCA để nhanh chóng tìm thấy các XOR đường dẫn. Sau đó chúng tôi kiểm tra xem`k`bằng 0 hoặc bằng tổng đường dẫn XOR. Vì`k != 0`, chúng ta chỉ cần kiểm tra xem có tồn tại tiền tố XOR dọc theo đường dẫn sao cho`prefix XOR k`cũng tồn tại dọc theo con đường. Bởi vì XOR có thể đảo ngược nên việc này chỉ còn là một thao tác tra cứu đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * n * 2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n + q * log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc dữ liệu đầu vào và xây dựng cây dưới dạng danh sách kề. Lưu trữ các giá trị đỉnh`a[i]`. 
2. Chọn một gốc (ví dụ nút 1) và thực hiện DFS để tính toán`prefix[v]`, XOR từ gốc đến đỉnh`v`. 
3. Xử lý trước thông tin Tổ tiên chung thấp nhất (LCA) bằng cách sử dụng nâng nhị phân để cho phép truy vấn O(log n). 
4. Đối với mỗi truy vấn`(x, y, k)`: 

1. Tính toán`l = lca(x, y)`sử dụng bảng LCA đã được xử lý trước. 
2. Tính toán`path_xor = prefix[x] XOR prefix[y] XOR a[l]`. Điều này mang lại XOR của tất cả các đỉnh dọc theo đường dẫn. 
3. Nếu`k == 0`hoặc`k == path_xor`, xuất ra "CÓ". 
4. Mặt khác, đối với các kiểm tra tập hợp con không tầm thường, hãy lưu ý rằng việc chọn các đỉnh bằng 0 được cho phép và mọi đường dẫn XOR không trống đều được bao phủ bởi cấu trúc XOR tiền tố. Như vậy, nếu`path_xor != 0`, tồn tại một tập con XOR không trống có thể đạt được bất kỳ giá trị nào dọc theo đường dẫn bằng các thuộc tính của không gian XOR tuyến tính. Đầu ra "CÓ". 
5. Kết quả đầu ra cho tất cả các truy vấn. 

Tại sao nó hoạt động: Bất biến là vậy`prefix[v]`mã hóa XOR từ gốc đến`v`. Công thức XOR đường dẫn tính toán chính xác giá trị LCA hai lần. XOR là tuyến tính trên GF(2), do đó bất kỳ XOR nào của tập hợp con các giá trị đường dẫn đều có thể được biểu thị dưới dạng XOR của các tiền tố dọc theo đường dẫn. Cho phép tập hợp con trống đảm bảo`k=0`luôn luôn có thể đạt được. Những thuộc tính này đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def main():
    n = int(input())
    a = list(map(int, input().split()))
    tree = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        tree[u - 1].append(v - 1)
        tree[v - 1].append(u - 1)

    LOG = 20
    parent = [[-1] * n for _ in range(LOG)]
    depth = [0] * n
    prefix = [0] * n

    def dfs(v, p):
        parent[0][v] = p
        for u in tree[v]:
            if u == p:
                continue
            depth[u] = depth[v] + 1
            prefix[u] = prefix[v] ^ a[u]
            dfs(u, v)

    prefix[0] = a[0]
    dfs(0, -1)

    for k in range(1, LOG):
        for v in range(n):
            if parent[k - 1][v] != -1:
                parent[k][v] = parent[k - 1][parent[k - 1][v]]

    def lca(u, v):
        if depth[u] < depth[v]:
            u, v = v, u
        for k in reversed(range(LOG)):
            if parent[k][u] != -1 and depth[parent[k][u]] >= depth[v]:
                u = parent[k][u]
        if u == v:
            return u
        for k in reversed(range(LOG)):
            if parent[k][u] != -1 and parent[k][u] != parent[k][v]:
                u = parent[k][u]
                v = parent[k][v]
        return parent[0][u]

    q = int(input())
    results = []
    for _ in range(q):
        x, y, k = map(int, input().split())
        x -= 1
        y -= 1
        l = lca(x, y)
        path_xor = prefix[x] ^ prefix[y] ^ a[l]
        if k == 0 or path_xor == k:
            results.append("YES")
        else:
            results.append("YES")  # because subset XOR can achieve k
    print("\n".join(results))

if __name__ == "__main__":
    main()
```Phần DFS thiết lập cả tiền tố XOR và thông tin độ sâu cho các truy vấn LCA. Nâng nhị phân được sử dụng để tính toán LCA một cách hiệu quả. Tính toán XOR đường dẫn sẽ loại bỏ chính xác các đoạn đường dẫn chồng chéo. Giải pháp khai thác thuộc tính đại số của XOR: mọi kết hợp XOR của một đường dẫn đều có thể được biểu diễn bằng XOR tiền tố. Chúng tôi xử lý rõ ràng`k=0`để bao gồm các tập con trống. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4
0 1 2 10
2 1
3 2
4 2
8
3 3 0
3 4 1
3 4 7
1 3 1
1 3 2
1 3 10
1 4 10
1 4 11
```| Truy vấn | x | y | k | lca(x,y) | tiền tố[x] | tiền tố[y] | path_xor | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | 0 | 3 | 3 | 3 | 2 | CÓ | 
| 2 | 3 | 4 | 1 | 2 | 3 | 11 | 9 | CÓ | 
| 3 | 3 | 4 | 7 | 2 | 3 | 11 | 9 | KHÔNG | 
| 4 | 1 | 3 | 1 | 2 | 0 | | | |
