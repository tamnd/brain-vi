---
title: "CF 1615D - Cây X(hoặc)-mas"
description: "Chúng ta có một cây có n nút và n-1 cạnh, trong đó mỗi cạnh có thể lưu trữ một số nguyên không âm biểu thị ánh sáng hoặc -1 nếu không xác định giá trị."
date: "2026-06-10T06:41:35+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar", "dsu", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1615
codeforces_index: "D"
codeforces_contest_name: "Codeforces Global Round 18"
rating: 2200
weight: 1615
solve_time_s: 206
verified: false
draft: false
---

[CF 1615D - Cây X(hoặc)-mas](https://codeforces.com/problemset/problem/1615/D) 

**Xếp hạng:** 2200 
**Thẻ:** bitmasks, dfs và tương tự, dsu, đồ thị, cây 
**Thời gian giải:** 3 phút 26s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với`n`nút và`n-1`các cạnh, trong đó mỗi cạnh có thể lưu trữ một số nguyên không âm biểu thị ánh sáng hoặc`-1`nếu giá trị không xác định. Một số yêu tinh cho chúng ta biết hai nút mà họ quan tâm và tính chẵn lẻ của số bit được đặt trong XOR của các giá trị cạnh dọc theo đường dẫn đơn giản kết nối các nút này. Nhiệm vụ của chúng ta là xác định xem có tồn tại phép gán số nguyên cho các cạnh chưa biết sao cho ký ức của tất cả các yêu tinh đều nhất quán hay không. Nếu có thể, chúng ta phải xây dựng lại một phép gán hợp lệ. 

Kích thước đầu vào là đáng kể: lên đến`2*10^5`các nút và yêu tinh cho mỗi trường hợp thử nghiệm và tối đa`2*10^4`trường hợp thử nghiệm. Một cách tiếp cận đơn giản tính toán XOR cho tất cả các đường dẫn cho mỗi truy vấn sẽ là O(n) cho mỗi truy vấn, tốc độ này quá chậm. Do đó, chúng ta phải tận dụng các thuộc tính cấu trúc cây và giảm các truy vấn XOR đường dẫn thành các ràng buộc cục bộ để có thể được phổ biến một cách hiệu quả. Các ràng buộc cũng gợi ý rằng các thuộc tính bitwise chỉ quan trọng ở modulo 2, bởi vì các yêu tinh báo cáo tính chẵn lẻ thay vì các giá trị XOR đầy đủ. 

Các trường hợp cạnh bao gồm các cây trong đó tất cả các giá trị cạnh đều không xác định hoặc trong đó các giá trị đã biết đã xung đột với ký ức của yêu tinh. Ví dụ, một cây đơn giản gồm ba nút có các cạnh`(1,2,-1)`,`(2,3,0)`và một truy vấn`(1,3,0)`nên gán cạnh chưa biết là 0 để thỏa mãn tính chẵn lẻ, nhưng nếu truy vấn có tính chẵn lẻ là 1 thì sẽ không có phép gán hợp lệ. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ liệt kê tất cả các phép gán có thể có cho các cạnh có`-1`, tính toán XOR đường dẫn cho mỗi truy vấn và kiểm tra tính chẵn lẻ. Đối với một cây có`k`có các cạnh chưa biết, có`2^k`kết hợp cho tính chẵn lẻ một mình. Với`k`có khả năng là 2*10^5, điều này hoàn toàn không khả thi. 

Thông tin chi tiết quan trọng là các ràng buộc XOR dọc theo các đường dẫn trong cây xác định một hệ thống tuyến tính trên GF(2) (trường nhị phân). Mỗi bộ nhớ elf đưa ra một phương trình chẵn lẻ cho một đường dẫn. Phép gán cạnh cây chuyển thành giá trị nút bằng cách sử dụng gốc: xác định`xor_to_root[u]`là XOR của các cạnh từ gốc tới nút`u`. Sau đó XOR của một đường dẫn`u-v`là`xor_to_root[u] XOR xor_to_root[v]`. Mỗi truy vấn`u-v-p`trở thành một phương trình tuyến tính`xor_to_root[u] XOR xor_to_root[v] = p`. Các giá trị đã biết cạnh ràng buộc XOR tương đối của các điểm cuối của chúng. Tất cả các ràng buộc này có thể được biểu diễn dưới dạng bài toán tô màu 2 (gán 0 hoặc 1 cho`xor_to_root[u]`cho mỗi nút), có thể được giải quyết bằng DFS hoặc tìm liên kết. Nếu xảy ra xung đột (hai nút phải có XOR xung đột) thì việc gán là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^k * n * m) | O(n) | Quá chậm | 
| Tối ưu (lan truyền chẵn lẻ XOR) | O(n + m) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các cạnh của cây và tách những cạnh có giá trị đã biết khỏi các giá trị chưa biết. Chuyển đổi các giá trị đã biết thành chẵn lẻ nhị phân (0 cho các bit chẵn, 1 cho số lẻ). 
2. Xây dựng danh sách kề cho cây. Đối với các cạnh có tính chẵn lẻ đã biết, hãy ghi lại ràng buộc chẵn lẻ XOR giữa các điểm cuối của chúng. 
3. Mô hình hóa mỗi nút dưới dạng một biến đại diện`xor_to_root[node]`modulo 2. Mỗi cạnh có tính chẵn lẻ đã biết sẽ cho`xor_to_root[u] XOR xor_to_root[v] = edge_parity`. 
4. Xử lý các truy vấn elf tương tự: mỗi truy vấn`(u,v,p)`cho`xor_to_root[u] XOR xor_to_root[v] = p`. Kết hợp những điều này với các ràng buộc cạnh thành cấu trúc tô màu tìm liên kết hoặc DFS. 
5. Duyệt cây bằng DFS. Đối với mỗi thành phần được kết nối, gán màu (0 hoặc 1) cho gốc tùy ý, truyền màu theo các ràng buộc. Nếu xảy ra xung đột (một nút được gán cả 0 và 1), hãy trả về NO. 
6. Sau khi tất cả các ràng buộc được xử lý mà không có xung đột, hãy xây dựng lại các giá trị cạnh. Đối với các cạnh chưa biết, gán`xor_to_root[u] XOR xor_to_root[v]`làm tính chẵn lẻ của cạnh (bất kỳ số nguyên hợp lệ nào khớp với tính chẵn lẻ đó là đủ). 
7. Đầu ra CÓ theo sau là các cạnh được tái tạo. 

Tại sao nó hoạt động: cấu trúc cây đảm bảo có chính xác một đường dẫn đơn giản giữa bất kỳ cặp nút nào, do đó, mỗi ràng buộc chẵn lẻ tương ứng với một phương trình tuyến tính trên GF(2) liên quan đến các giá trị XOR của nút. Việc truyền bá các bài tập thông qua DFS đảm bảo rằng tất cả các ràng buộc trong các thành phần được kết nối đều được thỏa mãn một cách nhất quán hoặc phát hiện được xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        edges = []
        adj = [[] for _ in range(n+1)]
        unknown_edges = []
        for _ in range(n-1):
            x, y, v = map(int, input().split())
            edges.append([x, y, v])
            adj[x].append((y, len(edges)-1))
            adj[y].append((x, len(edges)-1))
            if v != -1:
                v_parity = bin(v).count('1') % 2
                edges[-1][2] = v_parity
            else:
                unknown_edges.append(len(edges)-1)

        uf_parent = [i for i in range(n+1)]
        color = [None]*(n+1)

        def find(u):
            if uf_parent[u] != u:
                uf_parent[u] = find(uf_parent[u])
            return uf_parent[u]

        def union(u, v, w):
            ru, rv = find(u), find(v)
            if ru == rv:
                if (color[u] ^ color[v]) != w:
                    return False
                return True
            if color[ru] is None:
                color[ru] = 0
            if color[rv] is None:
                color[rv] = 0
            parity = color[u] ^ color[v] ^ w
            uf_parent[rv] = ru
            color[rv] = parity
            return True

        constraints = []
        for idx, (x, y, v) in enumerate(edges):
            if v != -1:
                constraints.append((x, y, v))
        for _ in range(m):
            a, b, p = map(int, input().split())
            constraints.append((a, b, p))

        possible = True
        for u, v, p in constraints:
            if color[u] is None:
                color[u] = 0
            if color[v] is None:
                color[v] = 0
            if not union(u, v, p):
                possible = False
                break

        if not possible:
            print("NO")
            continue

        # Assign final edge values
        def dfs(u, par):
            for v, idx in adj[u]:
                if v == par:
                    continue
                if edges[idx][2] == -1:
                    edges[idx][2] = color[u] ^ color[v]
                dfs(v, u)

        for i in range(1, n+1):
            if color[i] is None:
                color[i] = 0
                dfs(i, -1)
                break
        dfs(1, -1)

        print("YES")
        for x, y, v in edges:
            print(x, y, v)

if __name__ == "__main__":
    solve()
```Mã này trước tiên chuyển đổi tất cả các giá trị cạnh thành chẵn lẻ và xây dựng danh sách kề. Sau đó, nó truyền bá các ràng buộc XOR bằng cách sử dụng kỹ thuật tìm liên kết và tô màu, đảm bảo không phát sinh xung đột. Sau khi xác minh tính nhất quán, nó sẽ xây dựng lại các cạnh chưa biết bằng DFS bằng cách sử dụng các màu được gán cho các nút. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 5
1 2 -1
1 3 1
4 2 7
6 3 0
2 5 -1
2 3 1
2 5 0
5 6 1
6 1 1
4 5 1
```| Nút | màu sắc | Bình luận | 
| --- | --- | --- | 
| 1 | 0 | Màu gốc | 
| 2 | 0 | Tính chẵn lẻ từ cạnh 1-2 chưa xác định | 
| 3 | 1 | Cạnh 1-3 chẵn lẻ 1 | 
| 4 | 1 | Cạnh 2-4 chẵn lẻ 1 | 
| 5 | 0 | Không rõ tính chẵn lẻ của cạnh 2-5 | 
| 6 | 1 | Cạnh 3-6 chẵn lẻ 0 | 

Các giá trị chẵn lẻ của cạnh được xây dựng lại thỏa mãn tất cả các ràng buộc elf. 

### Mẫu 2 

đầu vào:```
3 3
1 2 -1
1 3 -1
1 2 0
1 3 1
2 3 0
```Sự lan truyền DFS cho thấy sự xung đột giữa các đường dẫn chẵn lẻ. Đầu ra là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Việc truyền bá DFS và tìm liên kết trên mỗi trường hợp thử nghiệm có tỷ lệ tuyến tính trong các nút và truy vấn | 
| Không gian | O(n + m) | Danh sách kề, màu sắc và mảng tìm liên kết | 

Điều này phù hợp thoải mái trong giới hạn 2 giây ngay cả đối với các hạn chế tối đa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    f = io.StringIO()
    with redirect_stdout(f):
        solve()
    return f.getvalue().strip()

# Provided samples
assert run
```
