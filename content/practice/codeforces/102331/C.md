---
title: "CF 102331C - Đếm xương rồng"
description: "Chúng ta có một đồ thị vô hướng đơn giản có tối đa 13 đỉnh. Chúng ta chọn một tập con các cạnh của nó, trong khi vẫn giữ toàn bộ tập đỉnh và hỏi xem đồ thị bao trùm thu được có phải là một cây xương rồng hay không. Một cây xương rồng được kết nối và không có cạnh nào có thể thuộc về hai chu trình đơn giản khác nhau."
date: "2026-08-14T05:03:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "C"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 174
verified: true
draft: false
---

[CF 102331C - Đếm xương rồng](https://codeforces.com/problemset/problem/102331/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng đơn giản có tối đa 13 đỉnh. Chúng ta chọn một tập con các cạnh của nó, trong khi vẫn giữ toàn bộ tập đỉnh và hỏi xem đồ thị bao trùm thu được có phải là một cây xương rồng hay không. Một cây xương rồng được kết nối và không có cạnh nào có thể thuộc về hai chu trình đơn giản khác nhau. Nhiệm vụ là đếm tất cả các tập con cạnh như vậy theo modulo (998244353). Các ràng buộc chính thức cố tình rất nhỏ về số đỉnh, nhưng số cạnh có thể có đã là 78 ​​khi (n=13). 

Giá trị nhỏ của (n) cho chúng ta biết tập con của các đỉnh là không gian trạng thái tự nhiên. Chỉ có (2^{13}=8192) tập hợp con đỉnh, do đó giải pháp có nhiều kích thước tập hợp con vẫn có thể thực tế. Mặt khác, việc liệt kê các tập con cạnh là vô vọng. Với 13 đỉnh, đồ thị hoàn chỉnh có 78 cạnh, cho (2^{78}), khoảng (3,0\cdot10^{23}), các tập hợp cạnh có thể có. Ngay cả khi việc kiểm tra một tập hợp con chỉ mất (O(n+m)), tổng số sẽ là khoảng (2,7\cdot10^{25}) phép toán cơ bản. Giải pháp dự định phải khai thác cấu trúc của đồ thị xương rồng thay vì kiểm tra các tập hợp con cạnh riêng lẻ. 

Có một số trường hợp dễ xảy ra khi giải thích bất cẩn sẽ đưa ra câu trả lời sai. Vì```
1 0
```câu trả lời là (1). Đồ thị bao gồm một đỉnh và không có cạnh nào và đồ thị một đỉnh được kết nối. Việc triển khai giả định một cây xương rồng phải chứa một cạnh sẽ trả về 0 không chính xác. 

Vì```
2 0
```câu trả lời là (0). Hai đỉnh bị ngắt kết nối nên tập cạnh trống không phải là cây xương rồng. Điều này sẽ tự động nắm bắt các triển khai đếm tập hợp con cạnh trống. 

Vì```
3 2
1 2
2 3
```câu trả lời là (1). Chỉ tập hợp con chứa cả hai cạnh được kết nối. Việc tính mọi khu rừng là một cây xương rồng hợp lệ cũng sẽ tính không chính xác hai tập hợp con một cạnh. 

Sự tinh tế thứ hai xuất hiện với nhiều chu kỳ. TRONG```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```có 27 đồ thị con xương rồng bao trùm hợp lệ. Mọi sơ đồ con 3 cạnh liên thông đều là một cây và tất cả 16 cây khung của (K_4) đều hợp lệ. Mọi đồ thị con 4 cạnh được kết nối đều là một vòng và do đó hợp lệ, cho thêm 11 đồ thị con. Đồ thị con 5 cạnh là (K_4) bị loại bỏ một cạnh và hai hình tam giác của nó có chung một cạnh, vì vậy không có đồ thị 5 cạnh nào là cây xương rồng. Toàn bộ (K_4) cũng không hợp lệ. Điều này chứng tỏ tại sao chỉ đếm các đồ thị con thưa thớt được kết nối là không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp con của (m) cạnh đầu vào. Đối với mỗi tập hợp con, chúng ta có thể chạy duyệt đồ thị để kiểm tra khả năng kết nối, sau đó phát hiện xem một số cạnh có thuộc về hai chu trình đơn giản hay không, chẳng hạn bằng cách tính toán các thành phần được kết nối hai chiều. Điều này đúng vì mọi câu trả lời có thể đều được kiểm tra chính xác một lần. Vấn đề là hệ số (2^m). Tại (n=13), (m) có thể là 78, vì vậy có khoảng (3.0\cdot10^{23}) tập hợp con trước khi chúng tôi thực hiện kiểm tra cây xương rồng. 

Quan sát hữu ích là cây xương rồng có cấu trúc khối rất cứng. Mọi thành phần được kết nối hai chiều đều là một cạnh hoặc một chu trình đơn giản. Sau khi loại bỏ các đỉnh khớp nối, các khối này được gắn theo kiểu cây. Điều này gợi ý việc tách vấn đề thành hai giai đoạn. Đầu tiên hãy đếm số chu kỳ đơn có thể có trên mỗi tập đỉnh. Sau đó kết hợp các chu trình này và các cạnh đơn xung quanh các đỉnh khớp nối. 

Sự phân tách này đặc biệt thích hợp cho lập trình động tập hợp con vì (n\le13). Một công thức gần đây của cùng một ý tưởng mô tả một trạng thái trong đó các đỉnh đã được xử lý bị cấm trở thành điểm khớp nối. Ban đầu một thành phần như vậy chỉ có thể là một đỉnh hoặc một chu trình đơn. Khi một đỉnh trở thành điểm khớp nối, tất cả các thành phần treo trên đỉnh đó đều độc lập và có thể được kết hợp với một số mũ đã đặt, có thể được đánh giá bằng cách sử dụng phép biến đổi tập hợp con được xếp hạng. 

Giai đoạn đầu tiên đếm các chu kỳ đơn giản. Cố định đỉnh được đánh số lớn nhất (h) của một chu trình. Một chu trình chứa (h) trở thành một đường đi đơn giản sau khi loại bỏ (h), có hai điểm cuối là lân cận của (h). Một tập hợp con DP đếm các đường dẫn này. Mỗi chu kỳ được thực hiện hai lần, một lần theo mỗi hướng, vì vậy chúng ta nhân với (1/2). 

Giai đoạn thứ hai bắt đầu với việc đếm chu kỳ này và xử lý các đỉnh từ nhỏ đến lớn. Khi xử lý (i), thành phần hợp lệ chứa (i) là một chu trình đã được tính hoặc một cạnh từ (i) đến một đỉnh nhỏ hơn cùng với một cây xương rồng được xây dựng trước đó trên các đỉnh khác. Một số thành phần như vậy có thể gặp nhau tại (i). Vì các tập đỉnh của chúng rời nhau ngoại trừ (i), nên việc chọn tất cả chúng chính xác là một hàm mũ phân vùng tập hợp. Biến đổi tập hợp con được xếp hạng tính toán hàm mũ này cho tất cả các tập hợp con đỉnh cùng một lúc. Đây là tối ưu hóa chính đưa thuật toán từ phân rã (O(3^n\operatorname{poly}(n))) thành (O(n^3 2^n)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^m(n+m))) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n^3 2^n)) | (O(n2^n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Biểu diễn mọi tập hợp con đỉnh bằng một mặt nạ bit. Chúng tôi sẽ duy trì một mảng (f[S]). Trước giai đoạn xử lý khớp nối, (f[S]) có nghĩa là số khối xương rồng nguyên tử trên chính xác (S): (f[{v}]=1), và với (|S|\ge3), (f[S]) là số chu trình đơn có tập đỉnh chính xác là (S). 
2. Đếm tất cả các chu trình đơn giản bằng cách cố định đỉnh cực đại của chúng (h). Chỉ các đỉnh nhỏ hơn (h) mới có thể xuất hiện ở nơi khác trên một chu trình như vậy. Xác định (dp[M][v]) là số đường đi đơn có tập đỉnh là (M), có điểm cuối là (v) và điểm cuối khác có điểm cuối là lân cận của (h). Ban đầu, nếu (v) liền kề với (h), đường đi một đỉnh ({v}) có thể xảy ra. 
3. Mở rộng các đường dẫn đó từng đỉnh một. Từ đường dẫn kết thúc tại (v), chọn hàng xóm chưa sử dụng (w) và nối thêm (w). Vì (w) chưa thuộc về mặt nạ nên đường dẫn kết quả vẫn đơn giản. 
4. Bất cứ khi nào một đường đi sử dụng ít nhất hai đỉnh và kết thúc tại lân cận của (h), việc cộng các cạnh từ cả hai điểm cuối vào (h) sẽ tạo ra một chu trình đơn giản. Thêm một nửa số đường dẫn vào (f[M\cup{h}]). Hệ số (1/2) loại bỏ hai hướng của cùng một chu trình vô hướng. 
5. Đặt (f[{v}]=1) cho mọi đỉnh. Các trạng thái đơn lẻ này là cần thiết vì khối cạnh được tạo sau đó bằng cách gắn một thành phần đã được xây dựng vào một đỉnh mới được xử lý. 
6. Xử lý các đỉnh (i=0,1,\ldots,n-1). Tại thời điểm này, các đỉnh nhỏ hơn (i) đã được phép làm điểm khớp nối, trong khi bản thân (i) vẫn chưa được xử lý thành một. Tạm thời loại bỏ (i) khỏi vũ trụ và xem xét một tập con (S) của các đỉnh còn lại. 
7. Tính số phần một thành phần chứa (i). Giá trị là 
[ 
g[S]=f[S\cup{i}] 
+f[S]\cdot \deg_i(S_{<i}), 
] 
trong đó (S_{<i}) chỉ chứa các đỉnh nhỏ hơn (i). Thuật ngữ đầu tiên có nghĩa là toàn bộ phần đã là một chu kỳ. Cách thứ hai có nghĩa là chúng ta sử dụng một cạnh từ (i) đến một đỉnh nhỏ hơn và gắn cây xương rồng đã tạo trước đó được biểu thị bằng (f[S]). Chỉ những cạnh nhỏ hơn mới được sử dụng vì mỗi cạnh phải được đưa vào đúng một bước xử lý. 
8. Một số mảnh như vậy có thể được gắn vào (i) và các tập đỉnh của chúng phải rời nhau. Do đó, giá trị mới của một tập hợp là tổng của tất cả các phân vùng tập hợp không có thứ tự thành các phần này. Đây là số mũ của chuỗi lũy thừa đã đặt (g). 
9. Để đánh giá số mũ đó một cách hiệu quả, hãy lưu trữ (g[S]) vào hệ số tương ứng với (|S|), sau đó áp dụng phép biến đổi zeta tập hợp con. Sau khi biến đổi, mọi tập hợp con hoạt động giống như một đa thức thông thường trong một biến kích thước, vì vậy chúng ta có thể tính toán hàm mũ chính thức của nó một cách độc lập. Cuối cùng áp dụng phép biến đổi tập hợp con nghịch đảo và lấy hệ số được lập chỉ mục theo số lượng của tập hợp con. 
10. Sao chép các giá trị kết quả trở lại trạng thái chứa (i) và tiếp tục với đỉnh tiếp theo. Sau khi tất cả các đỉnh đã được xử lý, (f[V]) chính xác là số đồ thị con xương rồng bao trùm của đồ thị đầu vào. 

### Tại sao nó hoạt động 

Điều bất biến là trước khi xử lý đỉnh (i), (f[S]) đếm các cấu trúc xương rồng trên (S) trong đó mọi đỉnh nhỏ hơn (i) đều bị cấm làm điểm khớp nối. Do đó, bất kỳ cấu trúc nào chứa (i) đều có thể bị phân hủy duy nhất thành các phần chỉ gặp nhau tại (i). Mỗi mảnh riêng lẻ là một chu trình hiện có hoặc một cạnh từ (i) đến một đỉnh đã được xử lý có gắn một cây xương rồng hợp lệ ở đó. Bước phân vùng theo cấp số nhân kết hợp tất cả các phần rời rạc có thể có chính xác một lần. Việc xử lý các đỉnh theo thứ tự tăng dần cũng mang lại cho mỗi cây cầu một thời điểm duy nhất mà tại đó nó được giới thiệu, do đó không có cấu trúc cạnh nào được tính hai lần. Vì mỗi cây xương rồng có thể được phân tách thành các khối chu trình và cầu nối theo cách này một cách chính xác, nên trạng thái cuối cùng sẽ đếm mọi tập hợp con cạnh hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
INV2 = (MOD + 1) // 2

def count_cactus(n, edges):
    N = 1 << n

    adj = [0] * n
    for u, v in edges:
        adj[u] |= 1 << v
        adj[v] |= 1 << u

    # f[S] initially counts atomic blocks:
    # one vertex for |S| = 1, or one simple cycle for |S| >= 3.
    f = [0] * N

    # Count simple cycles by fixing their maximum vertex h.
    for h in range(2, n):
        lim = 1 << h

        # dp[mask][v] = number of simple paths using exactly mask
        # and ending at v, whose starting vertex is adjacent to h.
        dp = [[0] * h for _ in range(lim)]

        ah = adj[h]
        for v in range(h):
            if (ah >> v) & 1:
                dp[1 << v][v] = 1

        for mask in range(1, lim):
            row = dp[mask]

            for v in range(h):
                cur = row[v]
                if cur == 0:
                    continue

                available = adj[v] & (lim - 1) & ~mask
                while available:
                    bit = available & -available
                    available -= bit
                    w = bit.bit_length() - 1

                    nxt = dp[mask | bit]
                    nv = nxt[w] + cur
                    if nv >= MOD:
                        nv -= MOD
                    nxt[w] = nv

        for v in range(h):
            if not ((ah >> v) & 1):
                continue

            for mask in range(1, lim):
                if mask.bit_count() >= 2:
                    val = dp[mask][v]
                    if val:
                        s = mask | (1 << h)
                        f[s] = (f[s] + val * INV2) % MOD

    for i in range(n):
        f[1 << i] = 1

    # Precompute inverses up to n.
    inv = [0] * (n + 1)
    if n >= 1:
        inv[1] = 1
    for i in range(2, n + 1):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    # Insert vertex i into a compressed mask.
    def insert_vertex(mask, i):
        low = (1 << i) - 1
        return (mask & low) | ((mask & ~low) << 1) | (1 << i)

    # Remove vertex i from a full mask.
    def remove_vertex(mask, i):
        low = (1 << i) - 1
        return (mask & low) | ((mask >> 1) & ~low)

    # Process each vertex as a newly available articulation point.
    for i in range(n):
        k = n - 1
        size = 1 << k
        lower_mask = (1 << i) - 1
        lower_neighbors = adj[i] & lower_mask

        # a[mask][d] stores the ranked subset-zeta representation.
        a = [[0] * (k + 1) for _ in range(size)]

        # Build g for the universe with vertex i removed.
        for s in range(size):
            full_without = insert_vertex(s, i) ^ (1 << i)
            full_with = full_without | (1 << i)

            deg = (lower_neighbors & full_without).bit_count()

            val = f[full_with]
            if deg:
                val += f[full_without] * deg
                val %= MOD

            a[s][s.bit_count()] = val

        # Subset zeta transform.
        bit = 1
        while bit < size:
            step = bit << 1
            for base in range(0, size, step):
                for off in range(bit):
                    x = a[base + off]
                    y = a[base + off + bit]
                    for d in range(k + 1):
                        y[d] += x[d]
                        if y[d] >= MOD:
                            y[d] -= MOD
            bit <<= 1

        # Pointwise formal exponential.
        for s in range(size):
            g = a[s]
            res = [0] * (k + 1)
            res[0] = 1

            for degree in range(1, k + 1):
                total = 0
                for j in range(1, degree + 1):
                    total += j * g[j] % MOD * res[degree - j]
                    if total >= (1 << 61):
                        total %= MOD
                res[degree] = total % MOD * inv[degree] % MOD

            a[s] = res

        # Inverse subset zeta transform.
        bit = 1
        while bit < size:
            step = bit << 1
            for base in range(0, size, step):
                for off in range(bit):
                    x = a[base + off]
                    y = a[base + off + bit]
                    for d in range(k + 1):
                        y[d] -= x[d]
                        if y[d] < 0:
                            y[d] += MOD
            bit <<= 1

        # Update all states containing i.
        for s in range(size):
            full = insert_vertex(s, i)
            f[full] = a[s][s.bit_count()]

    return f[N - 1]

def solve():
    n, m = map(int, input().split())

    edges = []
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))

    print(count_cactus(n, edges))

if __name__ == "__main__":
    solve()
```Phần đầu tiên xây dựng các bitmask liền kề. Bởi vì (n) nhiều nhất là 13, nên một tập hợp các lân cận ứng cử viên sẽ khớp một cách tự nhiên với một số nguyên Python và các phép toán như giao với một tập hợp con và`bit_count()`rất rẻ. 

Chu trình DP cố định đỉnh lớn nhất (h), do đó mỗi chu trình được gán cho đúng một lần lặp.`dp[mask][v]`đại diện cho một đường đi đơn giản và bản thân mặt nạ sẽ ngăn không cho một đỉnh được sử dụng hai lần. Đường đi chỉ được đóng qua (h) khi điểm cuối của nó liền kề với (h). Mỗi chu kỳ vô hướng có hai hướng, đó là lý do tại sao`INV2`được áp dụng. 

Giai đoạn khớp nối nén vũ trụ đỉnh bằng cách loại bỏ đỉnh hiện tại (i).`insert_vertex`chuyển đổi mặt nạ nén trở lại đánh số ban đầu. Biểu thức liên quan đến`lower_neighbors`chỉ đếm những người hàng xóm nhỏ hơn (i). Thứ tự này là cần thiết. Nếu tất cả những người hàng xóm được phép ở đó thì cây cầu giống nhau có thể được đưa vào ở cả hai điểm cuối. 

Biến đổi tập hợp con được xếp hạng lưu trữ một hệ số cho mỗi số lượng tập hợp con. Sau phép biến đổi zeta, số mũ của tập hợp trở thành số mũ đa thức thông thường ở mọi tập hợp con được chuyển đổi. Sự tái diễn 

[ 
E_k=\frac{1}{k}\sum_{j=1}^{k}jG_jE_{k-j} 
] 

tính hệ số bậc (k) trong (\exp(G)). Biến đổi zeta nghịch đảo phục hồi các giá trị tập hợp con. 

Tất cả số học được thực hiện modulo (998244353). Số nguyên Python không bị tràn, nhưng tích trung gian vẫn được rút gọn theo modulo số nguyên tố. Các giá trị nghịch đảo được tính toán với phép truy hồi tiêu chuẩn cho (1,2,\ldots,n) và giá trị đặc biệt (1/2) được sử dụng cho các hướng chu trình là`(MOD + 1) // 2`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác.```
3 3
1 2
2 3
3 1
```Có một chu trình đơn giản trên cả ba đỉnh. Đồ thị con xương rồng khung hợp lệ là ba cây khung và hình tam giác hoàn chỉnh. 

| Sân khấu | Trạng thái quan trọng | Giá trị | 
| --- | --- | --- | 
| Chu kỳ DP | (f[{1,2,3}]) | 1 | 
| Đỉnh xử lý 1 | (f[{1,2,3}]) | 1 | 
| Đỉnh xử lý 2 | (f[{1,2,3}]) | 1 | 
| Đỉnh xử lý 3 | (f[{1,2,3}]) | 4 | 
| Cuối cùng | (f[V]) | 4 | 

Khi đỉnh 3 được xử lý, nó có thể gắn riêng vào đỉnh 1 và 2. Hàm mũ kết hợp các khả năng tương ứng với ba cây bao trùm, trong khi chu trình được tính trước đó đóng góp vào cấu trúc thứ tư. 

Vì thế câu trả lời là`4`. 

### Mẫu 2 

Đồ thị có năm đỉnh và không có cạnh.```
5 0
```Không có chu kỳ và không có khối cầu nào có thể xảy ra. Các trạng thái đơn lẻ tồn tại nhưng không có cách nào để kết hợp hai đỉnh khác nhau thành một cấu trúc được kết nối. 

| Sân khấu | Số đỉnh có sẵn | Trạng thái toàn bộ | 
| --- | --- | --- | 
| Khối ban đầu | 1 đến 5 | 0 | 
| Đỉnh xử lý 1 | 5 | 0 | 
| Đỉnh xử lý 2 | 5 | 0 | 
| Đỉnh xử lý 3 | 5 | 0 | 
| Đỉnh xử lý 4 | 5 | 0 | 
| Đỉnh xử lý 5 | 5 | 0 | 

Trạng thái toàn bộ cuối cùng vẫn bằng 0, khớp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3 2^n)) | Chu kỳ DP mất (O(n^2 2^n)); số mũ tập hợp con được xếp hạng được áp dụng cho mọi đỉnh và tổng chi phí (O(n^3 2^n)) | 
| Không gian | (O(n2^n)) | Mảng tập hợp con chính và một bảng biến đổi xếp hạng chiếm ưu thế trong bộ nhớ | 

Với (n\le13), (2^n) chỉ là 8192. Do đó, hệ số khối trong giai đoạn khớp nối được tối ưu hóa đủ nhỏ cho các giới hạn dự định. Cách tiếp cận này tránh được việc liệt kê (2^m) không thể và hoạt động hoàn toàn với các tập hợp con đỉnh. Cấu trúc đếm chu kỳ tương tự cộng với tập hợp hàm mũ được biết là đạt được (O(n^3 2^n)) cho công thức tổng quát. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""\
3 3
1 2
2 3
3 1
""") == "4", "sample 1"

# Provided sample 2
assert run("""\
5 0
""") == "0", "sample 2"

# Provided sample 3
assert run("""\
8 9
1 5
1 8
2 4
2 8
3 4
3 6
4 7
5 7
6 8
""") == "35", "sample 3"

# Minimum-size graph: one isolated vertex is connected.
assert run("""\
1 0
""") == "1", "single vertex"

# Two vertices without an edge are disconnected.
assert run("""\
2 0
""") == "0", "two isolated vertices"

# A path on three vertices has exactly one spanning cactus.
assert run("""\
3 2
1 2
2 3
""") == "1", "path of length two"

# Complete graph K4.
# 16 spanning trees + 11 connected unicyclic four-edge graphs.
assert run("""\
4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "27", "K4"

# Maximum number of vertices with no edges.
assert run("""\
13 0
""") == "0", "maximum n, empty graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| 1 | Đồ thị nhỏ nhất có thể và trạng thái cơ sở đơn lẻ | 
|`2 0`| 0 | Ranh giới kết nối và tập cạnh trống | 
|`3 2`với các cạnh`1-2, 2-3`| 1 | Yêu cầu về nhịp và xây dựng cầu | 
| (K_4) | 27 | Nhiều chu trình và loại bỏ đồ thị trong đó các chu trình có chung một cạnh | 
|`13 0`| 0 | Giới hạn đỉnh tối đa và ranh giới trạng thái tập hợp con | 

## Vỏ cạnh 

Đối với trường hợp một đỉnh```
1 0
```trạng thái ban đầu (f[{1}]) được đặt thành 1. Không có phép biến đổi khớp nối nào có thể tạo ra một đỉnh khác, do đó mặt nạ đầy đủ vẫn là 1. Thuật toán xử lý chính xác đồ thị một đỉnh là được kết nối. 

Đối với hai đỉnh cô lập,```
2 0
```cả hai trạng thái đơn đều được khởi tạo, nhưng không có cạnh và không có chu trình. Trong mỗi bước phát âm, số hạng liên quan đến bậc bằng không. Không có trạng thái nào chứa cả hai đỉnh được tạo ra, vì vậy (f[{1,2}]=0). 

Đối với con đường```
3 2
1 2
2 3
```không có trạng thái chu kỳ. Khi đỉnh 2 được xử lý, đỉnh 1 phía dưới của nó sẽ tạo ra mảnh chứa cạnh (1-2). Khi đỉnh 3 được xử lý, lân cận thấp hơn 2 của nó sẽ gắn thành phần đã được xây dựng chứa đỉnh 1 và 2. Do đó, mặt nạ đầy đủ nhận được chính xác một đóng góp, tương ứng với đường dẫn hoàn chỉnh. 

Đối với (K_4), chu trình DP tạo ra mọi chu trình đơn giản dưới dạng khối nguyên tử. Giai đoạn khớp nối có thể gắn các chu trình và cầu nối, nhưng nó không bao giờ tạo ra một cấu trúc trong đó hai khối chu trình khác nhau có chung một cạnh. Do đó, tất cả 16 cây bao trùm và tất cả 11 đồ thị bốn cạnh một vòng được kết nối đều tồn tại, trong khi đồ thị năm cạnh và sáu cạnh bị loại trừ vì chu trình của chúng chồng lên nhau trên các cạnh. Câu trả lời kết quả là 27. 

Ranh giới mong manh nhất là sự chia đôi trong việc đếm chu kỳ. Một chu trình như (1-2-3-1) có thể được duyệt dưới dạng (1,2,3) hoặc (1,3,2), nhưng cả hai phép duyệt đều mô tả cùng một tập cạnh vô hướng. DP đếm cả hai hướng và chỉ chia cho hai sau khi đóng chu trình. Điều này an toàn vì đồ thị không có nhiều cạnh nên mọi chu trình đơn giản đều có chính xác hai hướng đó.
