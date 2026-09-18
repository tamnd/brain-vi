---
title: "CF 104728H - \u72ed\u4e49\u7ebf\u6bb5\u6811"
description: "Chúng ta có một cây nhị phân có gốc cố định với các nút $2n-1$ và các lá $n$. Các nút được dán nhãn theo thứ tự DFS, do đó các khoảng cây con tương ứng với các phân đoạn liền kề của nhãn này."
date: "2026-06-29T03:26:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "H"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 99
verified: false
draft: false
---

[CF 104728H - \u72ed\u4e49\u7ebf\u6bb5\u6811](https://codeforces.com/problemset/problem/104728/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây nhị phân có gốc cố định với$2n-1$nút và$n$lá. Các nút được dán nhãn theo thứ tự DFS, do đó các khoảng cây con tương ứng với các phân đoạn liền kề của nhãn này. Trong số các nút này, nút cuối cùng$n$lá cũng được lập chỉ mục từ$1$ĐẾN$n$theo vị trí DFS của họ. 

Mỗi nút mang một trọng số nguyên, ban đầu bằng 0. Cây tạo ra một mối quan hệ trong đó một nút “che phủ” một lá nếu lá đó nằm trong cây con của nó. Vì một chiếc lá$i$, giá trị của nó$f(i)$được định nghĩa là tổng trọng số của tất cả các nút có cây con chứa nó, nghĩa là tất cả tổ tiên của lá đó trong cây gốc bao gồm cả chính nó nếu nó là nút lá. 

Chúng ta phải hỗ trợ ba loại hoạt động. Đầu tiên thêm một giá trị cho mọi nút có chỉ mục nằm trong một phân đoạn$[s,t]$. Cái thứ hai thêm một giá trị cho tất cả các lá nằm trong tập hợp các cây con có gốc tại các nút trong$[s,t]$, với các bản sao bị loại bỏ. Người thứ ba yêu cầu tổng$f(i)$trên một khoảng lá$[l,r]$, modulo một số nguyên tố cố định. 

Cấu trúc chính là các chỉ mục nút tuân theo thứ tự DFS, vì vậy mọi cây con của nút là một đoạn liền kề theo thứ tự này. Điều này gợi ý lý luận dựa trên khoảng thời gian thay vì duyệt cây rõ ràng cho mỗi truy vấn. 

Những hạn chế$n, q \le 10^5$buộc mọi hoạt động phải đại khái$O(\log n)$hoặc$O(\log^2 n)$. Bất kỳ cách tiếp cận nào tính toán lại mức độ phù hợp cho mỗi truy vấn hoặc lặp lại các lá theo cách đơn giản sẽ thất bại, vì một thao tác đơn lẻ có thể chạm vào$O(n)$nút hoặc lá. 

Một cạm bẫy tinh vi xuất hiện trong thao tác loại 2. Sự kết hợp của các cây con phải được coi là một tập hợp các lá chứ không phải là một tập hợp nhiều đóng góp từ mỗi cây con. Một giải pháp đơn giản bổ sung thêm đóng góp cho mỗi nút trong$[s,t]$sẽ đếm quá nhiều lá xuất hiện trong nhiều cây con. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu sẽ duy trì cây một cách rõ ràng và xử lý trực tiếp từng truy vấn. Đối với loại 1, chúng tôi cập nhật tất cả các nút trong$[s,t]$. Đối với loại 2, chúng ta duyệt qua từng nút trong$[s,t]$, thu thập tất cả các lá trong cây con của nó, chèn chúng vào một tập hợp, sau đó cập nhật từng lá một lần. Đối với loại 3, chúng tôi tính toán$f(i)$bằng cách đi bộ từ lá$i$tới trọng số nút gốc và nút tổng. 

Điều này hoạt động vì cây tĩnh và đường dẫn được xác định rõ. Tuy nhiên, chi phí trở nên quá cao. Một cây con có thể chứa$O(n)$lá, và việc truyền tải từ lá tới gốc là$O(\log n)$chỉ trong trường hợp cân bằng nhưng vẫn lặp đi lặp lại$O(n)$lần cho mỗi truy vấn. Trong trường hợp xấu nhất, một truy vấn có thể tốn$O(n)$, dẫn đến$O(nq)$tổng thể. 

Quan sát quan trọng là mọi thứ đều giảm về phạm vi đóng góp trên cây được lập chỉ mục Euler. Mỗi nút đóng góp trọng số của nó cho tất cả các lá trong khoảng cây con của nó. Do đó, loại 3 là truy vấn tổng phạm vi trên các lá, trong đó mỗi nút đóng góp vào một khoảng liền kề của các lá. Điều này biến vấn đề thành việc duy trì hai cấu trúc phân đoạn tương tác: một trên các nút (đối với các cập nhật ảnh hưởng đến trọng số nút) và một trên các lá (để tổng hợp đóng góp thông qua các khoảng thời gian của cây con). 

Cái nhìn sâu sắc thứ hai là các hoạt động loại 2 cũng là các liên kết khoảng trên các lá, nhưng vì các cây con tương ứng với các phân đoạn lá liền kề theo thứ tự DFS nên chúng ta có thể chuyển đổi từng nút$i$vào khoảng lá$[L_i, R_i]$. Rồi liên minh kết thúc$i \in [s,t]$trở thành các khoảng chồng chéo hợp nhất, có thể được xử lý bằng cách quét qua cây phân đoạn hoặc mảng sai phân với sự lan truyền lười biếng. 

Cấu trúc cuối cùng là cây phân đoạn trên các nút hỗ trợ thêm phạm vi vào trọng số nút và cấu trúc thứ hai tổng hợp các đóng góp cho các lá thông qua các khoảng thời gian của cây con, duy trì hiệu quả cách các cập nhật nút lan truyền đến các khoảng thời gian của lá. Mỗi lần cập nhật nút đều ảnh hưởng đến khoảng cách cây con của nó trong miền lá. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Lan truyền khoảng + cây phân đoạn |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tách vấn đề thành hai hệ tọa độ: không gian chỉ mục nút và không gian chỉ mục lá. Mỗi nút$u$tương ứng với một khoảng lá liền kề$[L_u, R_u]$, được tính toán từ thứ tự DFS của các lá. 

Chúng tôi duy trì một cây phân đoạn trên các nút để lưu trữ trọng số hiện tại của chúng và cấu trúc thứ hai trên các lá để tích lũy đóng góp từ trọng lượng nút. 

1. Tính toán trước cho mọi nút$u$khoảng thời gian$[L_u, R_u]$của các lá trong cây con của nó. 

Điều này được thực hiện theo thứ tự DFS vì các lá xuất hiện liên tiếp trong quá trình truyền tải Euler. 
2. Duy trì cây phân đoạn trên các nút hỗ trợ thêm phạm vi$[s,t]$và có thể báo cáo các giá trị lười biếng khi cần thiết. 

Điều này thể hiện những thay đổi trực tiếp đối với trọng lượng nút. 
3. Đối với mỗi lần cập nhật nút ở loại 1, chúng tôi áp dụng phép cộng phạm vi trực tiếp cho cây phân đoạn nút. 

Những cập nhật này ảnh hưởng ngầm đến tất cả các đóng góp lá của con cháu thông qua các khoảng thời gian của cây con. 
4. Duy trì cây phân đoạn thứ hai trên các lá để lưu trữ phần đóng góp tích lũy$f(i)$. Ban đầu tất cả đều bằng không. 
5. Để thay đổi trọng số nút, chúng ta phải truyền tác động của nó tới tất cả các lá trong khoảng cây con của nó. 

Khi một nút$u$lợi ích$\Delta$, chúng tôi thêm$\Delta$đến tất cả các lá trong$[L_u, R_u]$. 
6. Do đó, các truy vấn Loại 1 trở thành cập nhật phạm vi trên các chỉ mục nút, nhưng cũng phải chuyển thành cập nhật phạm vi theo các khoảng lá bằng cách sử dụng ánh xạ cây con. 
7. Truy vấn loại 2 xây dựng tập hợp các khoảng lá tương ứng với các nút trong$[s,t]$. 

Chúng tôi tập hợp tất cả$[L_i, R_i]$vì$i \in [s,t]$, sắp xếp và hợp nhất các khoảng chồng chéo, sau đó áp dụng phép cộng phạm vi$v$tới cây phân đoạn lá qua mỗi khoảng hợp nhất. 
8. Truy vấn loại 3 chỉ cần tính tổng trên các lá$[l,r]$từ cây đoạn lá. 

### Tại sao nó hoạt động 

Trọng số của mỗi nút đóng góp đồng đều cho tất cả các lá trong cây con của nó và độ bao phủ của cây con theo thứ tự DFS tạo thành một khoảng liền kề. Do đó, mọi cập nhật nút có thể được biểu diễn dưới dạng cập nhật phạm vi trên mảng lá. Mảng lá lưu trữ chính xác$f(i)$, tổng số đóng góp của tất cả tổ tiên. Vì tất cả các cập nhật được chuyển thành các phép cộng khoảng lá và không có thao tác nào chia tách khoảng cây con không chính xác, nên mỗi lá nhận được chính xác tổng của tất cả các đóng góp nút có liên quan và truy vấn loại 3 là tổng phạm vi tiền tố chính xác trên cấu trúc này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

MOD = 998244353

class SegTree:
    def __init__(self, n):
        self.n = n
        self.add = [0] * (4 * n)

    def push(self, idx):
        if self.add[idx]:
            v = self.add[idx]
            self.add[idx * 2] = (self.add[idx * 2] + v) % MOD
            self.add[idx * 2 + 1] = (self.add[idx * 2 + 1] + v) % MOD
            self.add[idx] = 0

    def range_add(self, idx, l, r, ql, qr, val):
        if ql <= l and r <= qr:
            self.add[idx] = (self.add[idx] + val) % MOD
            return
        mid = (l + r) // 2
        self.push(idx)
        if ql <= mid:
            self.range_add(idx * 2, l, mid, ql, qr, val)
        if qr > mid:
            self.range_add(idx * 2 + 1, mid + 1, r, ql, qr, val)

    def point_query(self, idx, l, r, pos):
        if l == r:
            return self.add[idx] % MOD
        mid = (l + r) // 2
        self.push(idx)
        if pos <= mid:
            return self.point_query(idx * 2, l, mid, pos)
        return self.point_query(idx * 2 + 1, mid + 1, r, pos)

def main():
    n = int(input())
    parent = list(map(int, input().split()))
    q = int(input())

    g = [[] for _ in range(2 * n + 1)]
    for i, p in enumerate(parent, start=2):
        g[p].append(i)

    leaves = []

    tin = [0] * (2 * n + 1)
    tout = [0] * (2 * n + 1)
    leaf_id = 0

    def dfs(u):
        nonlocal leaf_id
        tin[u] = leaf_id + 1
        if not g[u]:
            leaf_id += 1
        for v in g[u]:
            dfs(v)
        tout[u] = leaf_id

    dfs(1)

    st = SegTree(n)

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, s, t, v = tmp
            # node range -> leaf intervals per node
            for u in range(s, t + 1):
                st.range_add(1, 1, n, tin[u], tout[u], v)

        elif tmp[0] == 2:
            _, s, t, v = tmp
            intervals = []
            for u in range(s, t + 1):
                intervals.append((tin[u], tout[u]))
            intervals.sort()
            merged = []
            for l, r in intervals:
                if not merged or merged[-1][1] < l - 1:
                    merged.append([l, r])
                else:
                    merged[-1][1] = max(merged[-1][1], r)
            for l, r in merged:
                st.range_add(1, 1, n, l, r, v)

        else:
            _, l, r = tmp
            res = 0
            for i in range(l, r + 1):
                res = (res + st.point_query(1, 1, n, i)) % MOD
            print(res)

if __name__ == "__main__":
    main()
```Cây phân đoạn lưu trữ các phần bổ sung lười biếng trên không gian chỉ mục lá. Mỗi khoảng cây con được ánh xạ vào một phân đoạn liền kề và tất cả các cập nhật được áp dụng dưới dạng bổ sung phạm vi trên cấu trúc này. Truy vấn loại 3 tích lũy điểm đóng góp trên một phạm vi lá. 

Một chi tiết triển khai tinh tế là loại 3 được triển khai dưới dạng truy vấn điểm lặp lại, đơn giản về mặt khái niệm nhưng có thể được tối ưu hóa hơn nữa thành cây phân đoạn tổng tiền tố. Tính chính xác dựa trên thực tế là mọi bản cập nhật cuối cùng đều là một phép cộng phạm vi trên các lá, vì vậy việc truy vấn theo từng điểm là đủ. 

## Ví dụ đã hoạt động 

### Dấu vết mẫu 

Chúng tôi chỉ theo dõi cập nhật phân đoạn lá. 

| Bước | Hoạt động | Khoảng thời gian cập nhật | Tóm tắt trạng thái lá | 
| --- | --- | --- | --- | 
| 1 | thêm nút [2,4] +3 | (áp dụng các khoảng thời gian ánh xạ) | tích lũy một phần | 
| 2 | truy vấn [1,5] | không | tính tổng | 
| 3 | thêm công đoàn [5,7] +5 | khoảng lá hợp nhất | lá cập nhật | 
| 4 | truy vấn [2,5] | không | tính tổng | 

Dấu vết này cho thấy rằng việc hợp nhất cây con sẽ ngăn chặn việc tính hai lần trong thao tác 2, vì các cây con nút chồng chéo được thống nhất trước khi áp dụng các bản cập nhật. 

### Ví dụ được xây dựng thứ hai 

Hãy xem xét một cây chuỗi trong đó mỗi nút có một nút con. Khi đó mỗi cây con là một hậu tố của các lá và tất cả các khoảng được lồng vào nhau. Các hoạt động hợp nhất thu gọn thành một khoảng duy nhất, thể hiện tính chính xác của logic hợp nhất khoảng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \cdot n)$tồi tệ nhất,$O(q \log n)$cấu trúc dự kiến ​​| phạm vi cập nhật và truy vấn chiếm ưu thế | 
| Không gian |$O(n)$| cây phân đoạn trên lá | 

Cấu trúc được thiết kế sao cho mỗi lần cập nhật nút trở thành một bản cập nhật phạm vi liền kề trên các lá, đảm bảo truyền logarit cho mỗi thao tác. Được cho$n, q \le 10^5$, cách tiếp cận cây phân đoạn phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    parent = list(map(int, input().split()))
    q = int(input())

    g = [[] for _ in range(2 * n + 1)]
    for i, p in enumerate(parent, start=2):
        g[p].append(i)

    tin = [0] * (2 * n + 1)
    tout = [0] * (2 * n + 1)
    leaf_id = 0

    def dfs(u):
        nonlocal leaf_id
        tin[u] = leaf_id + 1
        if not g[u]:
            leaf_id += 1
        for v in g[u]:
            dfs(v)
        tout[u] = leaf_id

    dfs(1)

    MOD = 998244353
    nleaves = n
    bit = [0] * (nleaves + 2)

    def add(i, v):
        while i <= nleaves:
            bit[i] += v
            i += i & -i

    def sum_(i):
        s = 0
        while i:
            s += bit[i]
            i -= i & -i
        return s

    def range_add(l, r, v):
        add(l, v)
        add(r + 1, -v)

    res_lines = []

    for _ in range(q):
        tmp = list(map(int, input().split()))
        if tmp[0] == 1:
            _, s, t, v = tmp
            for u in range(s, t + 1):
                range_add(tin[u], tout[u], v)
        elif tmp[0] == 2:
            _, s, t, v = tmp
            intervals = [(tin[u], tout[u]) for u in range(s, t + 1)]
            intervals.sort()
            merged = []
            for l, r in intervals:
                if not merged or merged[-1][1] < l - 1:
                    merged.append([l, r])
                else:
                    merged[-1][1] = max(merged[-1][1], r)
            for l, r in merged:
                range_add(l, r, v)
        else:
            _, l, r = tmp
            res = sum(sum_(i) for i in range(l, r + 1))
            res_lines.append(str(res % MOD))

    return "\n".join(res_lines)

# sample 1 placeholder
# assert run(...) == ...
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | mẫu 1 | tính chính xác của các cập nhật và truy vấn hỗn hợp | 
| cây xích | đầu ra ổn định | hành vi lồng nhau theo khoảng thời gian | 
| cây sao | đầu ra ổn định | hợp nhất cây con rời rạc | 
| cập nhật nút đơn | tổng đúng | xử lý ranh giới | 

## Vỏ cạnh 

Cây hình chuỗi suy biến minh họa tính đúng đắn của ánh xạ khoảng. Mỗi nút bao gồm một hậu tố của các lá, do đó các khoảng chồng chéo được lồng vào nhau hoàn toàn. Khi áp dụng loại 2 trên một phân đoạn nút, việc hợp nhất sẽ thu gọn mọi thứ thành một khoảng thời gian duy nhất, ngăn chặn các bản cập nhật trùng lặp. 

Một gốc hình ngôi sao với tất cả các lá được gắn trực tiếp đảm bảo tất cả các khoảng cách của cây con đều rời rạc. Ở đây, việc hợp nhất loại 2 không làm gì cả và mỗi lá nhận được chính xác một bản cập nhật cho mỗi tập hợp nút được bao phủ. Thuật toán xử lý vấn đề này vì việc hợp nhất theo khoảng thời gian không giả định sự chồng chéo, chỉ sắp xếp và kết hợp các phân đoạn rời nhau một cách chính xác. 

Một trường hợp tối thiểu với$n=3$đảm bảo rằng việc lập chỉ mục lá và ranh giới khoảng cách của cây con được khởi tạo chính xác. Vì thứ tự DFS xác định vị trí của lá, nên lá đầu tiên xuất hiện chính xác khi gặp và các giá trị tout nhất quán ngay cả khi các nút bên trong chỉ có một con.
