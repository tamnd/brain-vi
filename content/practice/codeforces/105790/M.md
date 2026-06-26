---
title: "CF 105790M - Giun khổng lồ"
description: "Đa vũ trụ tạo thành một cây định hướng có gốc với vũ trụ gốc 1. Mọi cạnh đều hướng từ vũ trụ có nhiều sao đến vũ trụ có ít sao hơn."
date: "2026-06-26T03:51:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "M"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 51
verified: true
draft: false
---

[CF 105790M - Giun khổng lồ](https://codeforces.com/problemset/problem/105790/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đa vũ trụ tạo thành một cây định hướng có gốc với vũ trụ gốc`1`. Mọi cạnh đều hướng từ vũ trụ có nhiều sao đến vũ trụ có ít sao hơn. Bởi vì số lượng sao giảm nghiêm trọng dọc theo mọi đường dẫn từ gốc đến lá, trong số tất cả các vũ trụ có thể tiếp cận một tập hợp các nút nhất định, vũ trụ có số lượng sao nhỏ nhất đơn giản là tổ tiên chung sâu nhất của các nút đó. Trong thuật ngữ cây, đó là Tổ tiên chung thấp nhất (LCA) của chúng. 

Đối với mỗi truy vấn, chúng tôi được cung cấp một danh sách có thứ tự các vũ trụ riêng biệt$$a_1,a_2,\dots,a_K$$và chúng ta phải tính toán$$\sum_{1 \le i \le j \le K} f(i,j)$$Ở đâu$f(i,j)$là mã định danh vũ trụ của nút sâu nhất có thể tiếp cận mọi vũ trụ trong đoạn liền kề$a_i,\dots,a_j$. Vì nút đó chính xác là LCA của tất cả các nút trong phân đoạn nên nhiệm vụ sẽ trở thành:$$\sum_{1 \le i \le j \le K}
\text{LCA}(a_i,a_{i+1},\dots,a_j)$$sử dụng chính số vũ trụ làm giá trị được thêm vào. 

Cây chứa tới$10^5$các nút, có tới$10^5$truy vấn và tổng của tất cả độ dài truy vấn nhiều nhất là$3 \cdot 10^5$. Một giải pháp kiểm tra từng mảng con một cách độc lập sẽ yêu cầu$O(K^2)$hoạt động trên mỗi truy vấn, vượt xa những gì phù hợp với giới hạn. 

Một quan sát tinh tế là nhãn nút không phải là độ sâu hoặc số lượng sao. Câu trả lời bổ sung các mã định danh vũ trụ được hoạt động LCA trả về. 

Hãy xem xét truy vấn:```
2 4 5
```trong cây mẫu nơi cả 4 và 5 đều là con của 3. 

Các khoảng là:```
[4] -> 4
[5] -> 5
[4,5] -> 3
```Câu trả lời là:```
4 + 5 + 3 = 12
```Việc triển khai bất cẩn tính LCA thay vì tính tổng các mã định danh của chúng sẽ tạo ra kết quả sai. 

Một trường hợp khác là truy vấn có độ dài bằng một:```
1 7
```Khoảng cách duy nhất là`[7]`và LCA của một nút đơn chính là nút đó. Câu trả lời phải là`7`, không phải cha mẹ hoặc gốc của nó. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi khoảng thời gian$[i,j]$, tính LCA của tất cả các nút bên trong nó và cộng kết quả vào câu trả lời. 

Một cách để làm điều này là kéo dài khoảng thời gian từ mỗi vị trí bắt đầu. Nếu chúng ta giữ LCA hiện tại trong khi di chuyển điểm cuối bên phải thì mọi khoảng thời gian có thể được xử lý theo$O(\log N)$sử dụng nâng nhị phân. Tổng độ phức tạp trở thành$O(K^2 \log N)$. Với$K$lớn như$10^5$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là LCA sụp đổ rất nhanh. 

Cố định một vị trí$r$. Nhìn vào tất cả các mảng con kết thúc tại$r$:```
[a_r]
[a_{r-1}, a_r]
[a_{r-2}, a_r]
...
```Khi chúng ta kéo dài khoảng sang trái, LCA chỉ có thể di chuyển lên trên cây. Khi LCA thay đổi, nó sẽ trở thành tổ tiên chặt chẽ của LCA trước đó. 

Dọc theo bất kỳ đường đi từ gốc tới lá nào cũng chỉ có$O(\log N)$tổ tiên riêng biệt có thể xuất hiện sau khi sáp nhập LCA lặp đi lặp lại. Đây là thuộc tính tương tự được sử dụng trong các bài toán cổ điển "gcds riêng biệt của mảng con". Tập hợp các LCA riêng biệt cho các mảng con kết thúc ở một vị trí cố định vẫn còn nhỏ. 

Giả sử chúng ta đã biết tất cả LCA riêng biệt của các mảng con kết thúc ở vị trí$r-1$. Đối với một nút mới$a_r$, mọi giá trị LCA cũ$v$trở thành:$$\text{LCA}(v,a_r)$$cho mảng con mở rộng. Chúng tôi cũng thêm mảng con một phần tử mới có LCA là$a_r$. 

Nhiều giá trị kết quả bằng nhau nên chúng tôi hợp nhất các LCA bằng nhau và chỉ giữ lại số lượng của chúng. Số lượng LCA riêng biệt vẫn còn nhỏ, mang lại giải pháp hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(K^2 \log N)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(K \log^2 N)$mỗi truy vấn |$O(\log N)$mỗi truy vấn | Đã chấp nhận | 

Tổng độ phức tạp của tất cả các truy vấn là$O((\sum K)\log^2 N)$, điều này dễ dàng phù hợp vì$\sum K \le 3 \cdot 10^5$. 

## Hướng dẫn thuật toán 

### Tiền xử lý 

1. Gốc cây tại nút`1`. 
2. Chạy DFS để tính toán độ sâu và bảng nâng nhị phân`up[v][j]`, Ở đâu`up[v][j]`là$2^j$-tổ tiên của`v`. 
3. Thực hiện một`lca(u,v)`chức năng sử dụng nâng nhị phân. 

### Đang xử lý một truy vấn 

1. Duy trì một danh sách`cur`chứa cặp`(lca_value, count)`. 

Ý nghĩa là: trong số tất cả các mảng con kết thúc ở vị trí trước đó, chính xác`count`trong số chúng có LCA bằng`lca_value`. 
2. Đối với nút tiếp theo`x`, bắt đầu một danh sách mới`nxt`. 
3. Chèn`(x,1)`vào trong`nxt`. 

Điều này đại diện cho mảng con một phần tử`[x]`. 
4. Cho mỗi cặp`(v,c)`TRONG`cur`, tính toán`w = lca(v,x)`. 

Mỗi mảng con được biểu diễn bởi`(v,c)`trở thành một mảng con dài hơn kết thúc tại`x`, và LCA mới của nó là`w`. 
5. Nếu cặp cuối cùng đã được lưu trong`nxt`có LCA`w`, thêm vào`c`theo số lượng của nó. Nếu không thì nối thêm`(w,c)`. 

Các LCA bằng nhau liên tiếp được hợp nhất để chỉ còn lại các giá trị riêng biệt. 
6. Thay thế`cur`với`nxt`. 
7. Thêm$$\sum (\text{lca\_value} \times \text{count})$$trên tất cả các cặp trong`cur`đến câu trả lời truy vấn. 
8. Lặp lại cho mọi nút của chuỗi truy vấn. 

### Tại sao nó hoạt động 

Sau khi xử lý vị trí`r`, danh sách`cur`đại diện cho tất cả các mảng con kết thúc tại`r`, được nhóm theo LCA của họ. Mỗi mảng con kết thúc tại`r`là khoảng một phần tử`[a_r]`hoặc phần mở rộng của mảng con kết thúc tại`r-1`. 

Trong một khoảng thời gian dài, LCA mới chính xác là$$\text{LCA}(\text{old LCA}, a_r)$$bởi vì LCA của một tập hợp các nút có thể được tích lũy tăng dần thông qua các hoạt động LCA theo cặp lặp đi lặp lại. 

Như vậy mọi mảng con kết thúc tại`r`đóng góp cho đúng một nhóm trong`cur`và mỗi nhóm tương ứng với các mảng con thực tế. Tổng hợp`lca_value × count`đưa ra tổng đóng góp của tất cả các khoảng kết thúc tại`r`. Tính tổng tất cả các vị trí mỗi khoảng thời gian chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    n, q = map(int, input().split())

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = (n + 1).bit_length()

    depth = [0] * (n + 1)
    up = [[0] * (n + 1) for _ in range(LOG)]

    stack = [1]
    parent = [0] * (n + 1)
    parent[1] = 1

    order = [1]
    while stack:
        v = stack.pop()
        for to in g[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            depth[to] = depth[v] + 1
            stack.append(to)
            order.append(to)

    for v in range(1, n + 1):
        up[0][v] = parent[v]

    for j in range(1, LOG):
        prev = up[j - 1]
        cur = up[j]
        for v in range(1, n + 1):
            cur[v] = prev[prev[v]]

    def lca(a, b):
        if depth[a] < depth[b]:
            a, b = b, a

        diff = depth[a] - depth[b]
        bit = 0
        while diff:
            if diff & 1:
                a = up[bit][a]
            diff >>= 1
            bit += 1

        if a == b:
            return a

        for j in range(LOG - 1, -1, -1):
            if up[j][a] != up[j][b]:
                a = up[j][a]
                b = up[j][b]

        return up[0][a]

    answers = []

    for _ in range(q):
        arr = list(map(int, input().split()))
        k = arr[0]
        nodes = arr[1:]

        cur = []
        ans = 0

        for x in nodes:
            nxt = [(x, 1)]

            for v, cnt in cur:
                w = lca(v, x)

                if nxt[-1][0] == w:
                    nxt[-1] = (w, nxt[-1][1] + cnt)
                else:
                    nxt.append((w, cnt))

            cur = nxt

            for v, cnt in cur:
                ans += v * cnt

        answers.append(str(ans))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    main()
```Quá trình tiền xử lý xây dựng bảng nâng nhị phân một lần cho toàn bộ cây. Mọi truy vấn LCA sau đó sẽ chạy trong$O(\log N)$. 

Quá trình xử lý truy vấn chỉ giữ lại các LCA riêng biệt cho các mảng con kết thúc ở vị trí hiện tại. Khi mở rộng tất cả các mảng con trước đó bằng một nút mới, một số LCA thường thu gọn vào cùng một tổ tiên. Việc hợp nhất các kết quả bằng nhau liền kề là rất quan trọng, nếu không danh sách có thể tăng kích thước tuyến tính. 

Câu trả lời sử dụng mã định danh vũ trụ được hoạt động LCA trả về. Vì có thể có$O(K^2)$khoảng thời gian, tổng cuối cùng phải được lưu trữ dưới dạng số nguyên 64 bit. Số nguyên Python tự động xử lý việc này. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây:```
1
├─2
└─3
  ├─4
  └─5
```Truy vấn:```
2 4 5
```| Vị trí | Nút | cur sau khi xử lý | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 4 | (4,1) | 4 | 
| 2 | 5 | (5,1), (3,1) | 8 | 

Tổng cộng:```
4 + 8 = 12
```Hai nhóm ở vị trí 2 tương ứng với các khoảng`[5]`Và`[4,5]`. 

### Ví dụ 2 

Cây hình ngôi sao:```
1
├─2
├─3
└─4
```Truy vấn:```
3 2 3 4
```| Vị trí | Nút | cur sau khi xử lý | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2 | (2,1) | 2 | 
| 2 | 3 | (3,1), (1,1) | 4 | 
| 3 | 4 | (4,1), (1,2) | 6 | 

Trả lời:```
2 + 4 + 6 = 12
```Ví dụ này cho thấy một số khoảng có thể chia sẻ cùng một LCA và được biểu diễn bằng một`(value,count)`đôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + (\sum K)\log^2 N)$| Tiền xử lý cộng với xử lý truy vấn | 
| Không gian |$O(N \log N)$| Bàn nâng nhị phân | 

Chi phí tiền xử lý được thanh toán một lần. Vì tổng độ dài truy vấn tối đa là$3 \cdot 10^5$, tổng thời gian chạy thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    import sys as _sys
    old_stdout = _sys.stdout
    _sys.stdout = out

    try:
        main()
    finally:
        _sys.stdout = old_stdout

    return out.getvalue().strip()

# sample 1
assert run(
"""5 2
1 2
1 3
3 4
3 5
1 2
2 4 5
"""
) == "2\n12"

# sample 2
assert run(
"""4 1
1 2
1 3
1 4
3 2 3 4
"""
) == "12"

# sample 3
assert run(
"""1 1
1 1
"""
) == "1"

# single node query
assert run(
"""2 1
1 2
1 2
"""
) == "2"

# chain
assert run(
"""4 1
1 2
2 3
3 4
3 2 3 4
"""
) == "16"

# all LCAs become root
assert run(
"""5 1
1 2
1 3
1 4
1 5
4 2 3 4 5
"""
) == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nút đơn | 1 | Kích thước tối thiểu | 
| Truy vấn có độ dài 1 | Id nút | LCA của một nút bằng chính nó | 
| Cây xích | 16 | LCA con cháu tổ tiên | 
| Cây sao | 20 | Nhiều khoảng chia sẻ gốc | 
| Mẫu 1 | 2, 12 | Tính đúng đắn chung | 

## Vỏ cạnh 

Một truy vấn chỉ chứa một vũ trụ:```
2 1
1 2
1 2
```Khoảng cách duy nhất là`[2]`. Thuật toán tạo ra`cur = [(2,1)]`, thêm`2 * 1`, và trả về`2`. Không cần tính toán LCA với nút khác. 

Một chuỗi:```
1 - 2 - 3 - 4
query: 2 3 4
```Khoảng LCA là:```
[2] = 2
[3] = 3
[4] = 4
[2,3] = 2
[3,4] = 3
[2,3,4] = 2
```Câu trả lời là:```
2 + 3 + 4 + 2 + 3 + 2 = 16
```Biểu diễn LCA được nhóm xử lý việc này một cách tự nhiên vì LCA chỉ di chuyển lên trên khi cần thiết. 

Một ngôi sao bắt nguồn từ`1`:```
1 connected to 2,3,4,5
query: 2 3 4 5
```Mỗi khoảng có độ dài ít nhất hai có LCA`1`. Trong quá trình xử lý, nhiều tiện ích mở rộng sẽ thu gọn về cùng một giá trị và bước hợp nhất sẽ kết hợp chúng thành một`(1,count)`đôi. Đây chính xác là tình huống khiến số lượng trạng thái được lưu trữ ở mức nhỏ.
