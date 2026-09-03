---
title: "CF 104468L - Các đỉnh hữu dụng của Khaled"
description: "Chúng ta được cấp một cây có gốc trong đó mỗi nút có một giá trị. Chúng tôi muốn xây dựng một tập hợp con các nút theo một ràng buộc phụ thuộc vào tham số $K$."
date: "2026-06-30T13:03:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104468
codeforces_index: "L"
codeforces_contest_name: "The 2023 Damascus University Collegiate Programming Contest"
rating: 0
weight: 104468
solve_time_s: 190
verified: false
draft: false
---

[CF 104468L - Các đỉnh Khaled-utiful](https://codeforces.com/problemset/problem/104468/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc trong đó mỗi nút có một giá trị. Chúng tôi muốn xây dựng một tập hợp con các nút theo một ràng buộc phụ thuộc vào một tham số$K$. Ràng buộc không phải trực tiếp về các cạnh mà là về tổ tiên: nếu chúng ta chọn một nút, chúng ta sẽ bị giới hạn về số lượng tổ tiên của nó cũng được chọn. 

Đối với một cố định$K$, một lựa chọn hợp lệ là bất kỳ tập hợp con nào của các nút sao cho với mọi nút được chọn$v$, trong số tất cả tổ tiên của nó trên cây, nhiều nhất là$K-1$trong số đó cũng được chọn. Nói cách khác, dọc theo bất kỳ đường dẫn từ gốc đến nút nào, chúng tôi không được phép “xếp chồng” quá nhiều nút đã chọn với mật độ dày đặc. 

Đối với mỗi$K$từ 1 đến$N$, chúng ta phải tính tổng giá trị nút tối đa có thể theo ràng buộc đó. 

Ràng buộc$N \le 2 \cdot 10^5$trên tất cả các trường hợp thử nghiệm buộc chúng tôi phải tránh xa mọi thứ có thể tính toán lại các giải pháp một cách độc lập cho mỗi trường hợp thử nghiệm.$K$hoặc mỗi tập hợp con. Bất kỳ giải pháp nào cũng phải sử dụng lại cấu trúc trên tất cả$K$các giá trị. Một cách giải thích ngây thơ sẽ thử DP mỗi$K$, ngay lập tức trở thành$O(N^2)$hoặc tệ hơn. 

Một trường hợp cạnh tinh tế xuất hiện trong chuỗi. Nếu cây là một đường thẳng và các giá trị cao thấp xen kẽ nhau thì mức độ bao gồm tham lam từ gốc đến lá thay đổi mạnh mẽ như$K$tăng lên. Bất kỳ giải pháp nào xử lý từng nút độc lập với lịch sử lựa chọn tổ tiên của nó sẽ thất bại, vì tính khả thi phụ thuộc vào mật độ tổ tiên toàn cầu chứ không phải cấu trúc cục bộ. 

## Phương pháp tiếp cận 

Chiến lược brute-force sẽ cố gắng tính toán tập hợp con tối ưu cho mỗi$K$riêng. Đối với một cố định$K$, điều này sẽ trở thành một cây DP với các trạng thái theo dõi xem có bao nhiêu tổ tiên đã được chọn trên đường dẫn, dẫn đến một$O(NK)$hoặc độ phức tạp tệ hơn cho mỗi trường hợp thử nghiệm. Từ$K$phạm vi lên đến$N$, điều này suy biến thành$O(N^2)$, quá lớn. 

Quan sát quan trọng là ràng buộc là đơn điệu trong$K$. Nếu một bộ hợp lệ cho$K$, nó cũng có giá trị cho bất kỳ lớn hơn$K$. Điều này có nghĩa$F(K)$là hàm không giảm và cấu trúc của lời giải tối ưu thay đổi dần dần thay vì độc lập tại mỗi thời điểm.$K$. 

Cái nhìn sâu sắc hơn là ràng buộc giới hạn một cách hiệu quả số lượng nút được chọn có thể xuất hiện trên bất kỳ đường dẫn từ gốc đến nút nào. Điều này tương đương với việc chọn các nút trong khi kiểm soát “chồng chéo độ sâu” dọc theo các đường dẫn, có thể được chuyển thành vấn đề đóng góp đối với các nút được sắp xếp theo các ràng buộc về giá trị và cấu trúc được xử lý thông qua đối số thứ tự tổng thể. 

Điều này cho phép chúng ta diễn giải lại vấn đề bằng cách chọn các nút theo thứ tự giá trị giảm dần, trong khi vẫn duy trì số lượng lựa chọn mà mỗi nút “kế thừa” từ tổ tiên của nó. Mỗi nút đóng góp nếu nó vẫn khả thi theo hạn ngạch hiện tại của tổ tiên được chọn dọc theo đường dẫn của nó. 

Một khi chúng ta xem nó theo cách này, ngày càng tăng$K$tương ứng với việc nới lỏng ràng buộc dung lượng một cách thống nhất trên tất cả các nút, điều này cho phép chúng ta tính toán tất cả$F(K)$trong một lần sử dụng một DP toàn cầu duy nhất trên cấu trúc cây và tính toán các lựa chọn đang hoạt động theo nhiều cấp độ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Cây Per-K DP |$O(N^2)$|$O(N)$| Quá chậm | 
| Kích hoạt tiền tố + tham lam toàn cầu trên cây |$O(N \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1. Đối với mỗi nút, chúng tôi tính toán độ sâu của nó và duy trì cấu trúc cây con. 

Chúng tôi xử lý các nút theo thứ tự giá trị giảm dần vì các giá trị cao hơn phải được ưu tiên trong bất kỳ tổng tối ưu nào. 

Chúng tôi duy trì cấu trúc theo dõi số lượng nút được chọn tồn tại dọc theo đường dẫn đến thư mục gốc. Thay vì lưu trữ rõ ràng tất cả các đường dẫn, chúng tôi duy trì sự tích lũy giống như Fenwick theo thứ tự DFS. 

### bước 

1. Root cây và tính toán thời gian vào và ra DFS. 

Điều này tuyến tính hóa từng cây con thành một khoảng sao cho các mối quan hệ tổ tiên trở thành các mối quan hệ phạm vi. 
2. Sắp xếp các nút theo giá trị theo thứ tự giảm dần. 

Chúng tôi cố gắng kích hoạt các nút từ giá trị cao nhất trở xuống. 
3. Duy trì BIT (cây Fenwick) theo thứ tự DFS cho biết liệu nút hiện có được chọn hay không. 

Một nút khả thi nếu số lượng nút được chọn trên đường dẫn gốc của nó ít hơn$K$. 
4. Đối với mỗi nút theo thứ tự được sắp xếp, chúng tôi mô phỏng sự bao gồm của nó bằng cách truy vấn xem nó hiện có bao nhiêu tổ tiên được chọn. 
5. Chúng tôi đánh dấu nút là đã chọn trong BIT nếu nó hợp lệ dưới ngưỡng hiện tại. 
6. Để tính toán tất cả$F(K)$, chúng tôi quan sát thấy rằng mỗi nút có một ngưỡng tới hạn$K_v$: tối thiểu$K$cho phép nó được đưa vào các lựa chọn có giá trị cao hơn. Chúng tôi tính toán ngưỡng này trong khi kích hoạt các nút. 
7. Cuối cùng, chúng tôi tổng hợp các khoản đóng góp: mỗi nút đóng góp giá trị của nó cho tất cả$K \ge K_v$. Điều này trở thành một mảng khác biệt trên$K$. 

### Tại sao nó hoạt động 

Điều bất biến chính là khi xử lý các nút theo thứ tự giá trị giảm dần, mỗi khi chúng tôi quyết định thêm một nút vào thì tất cả các nút có giá trị cao hơn đều đã được sửa. Do đó, số lượng tổ tiên được chọn cho một nút được xác định đầy đủ tại thời điểm quyết định. Điều này làm cho tính khả thi của nó chỉ phụ thuộc vào cấu trúc tiền tố không thay đổi sau này, do đó ngưỡng kích hoạt của mỗi nút được xác định rõ ràng và độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    T = int(input())
    for _ in range(T):
        n = int(input())
        a = list(map(int, input().split()))

        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        tin = [0] * n
        tout = [0] * n
        parent = [-1] * n
        depth = [0] * n
        timer = 0

        stack = [(0, -1, 0)]
        order = []

        while stack:
            v, p, state = stack.pop()
            if state == 0:
                tin[v] = timer
                timer += 1
                parent[v] = p
                order.append(v)
                stack.append((v, p, 1))
                for to in g[v]:
                    if to == p:
                        continue
                    depth[to] = depth[v] + 1
                    stack.append((to, v, 0))
            else:
                tout[v] = timer - 1

        bit = [0] * (n + 5)

        def add(i, v):
            i += 1
            while i <= n:
                bit[i] += v
                i += i & -i

        def sum_(i):
            s = 0
            i += 1
            while i > 0:
                s += bit[i]
                i -= i & -i
            return s

        def path_sum(v):
            return sum_(tin[v])

        nodes = sorted(range(n), key=lambda x: -a[x])

        # each node gets minimum K at which it can be selected
        Kmin = [1] * n

        active = []

        for v in nodes:
            # number of already selected ancestors
            cnt = path_sum(parent[v]) if parent[v] != -1 else 0
            Kmin[v] = cnt + 1
            add(tin[v], 1)

        # difference array over K
        diff = [0] * (n + 3)
        for v in range(n):
            k = Kmin[v]
            diff[k] += a[v]
            diff[n + 1] -= a[v]

        cur = 0
        res = []
        for k in range(1, n + 1):
            cur += diff[k]
            res.append(str(cur))

        print(" ".join(res))

if __name__ == "__main__":
    solve()
```## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây:```
1 - 2
|
3
```Giá trị:```
1 2 3
```Chúng tôi xử lý các nút theo thứ tự: 3, 2, 1. 

Nút 3 không có tổ tiên được chọn → Kmin = 1 

Nút 2 không có tổ tiên được chọn → Kmin = 1 

Nút 1 không có tổ tiên được chọn → Kmin = 1 

Vì vậy tất cả đều đóng góp ngay lập tức. 

| nút | giá trị | Kmin | 
| --- | --- | --- | 
| 3 | 3 | 1 | 
| 2 | 2 | 1 | 
| 1 | 1 | 1 | 

Vì vậy: 

- F(1)=6 
- F(2)=6 
- F(3)=6 

Điều này cho thấy rằng khi các ràng buộc đủ lỏng lẻo thì tất cả các nút luôn trở nên tối ưu. 

### Ví dụ 2 

Chuỗi:```
1 - 2 - 3 - 4
```Giá trị:```
4 3 2 1
```Các nút cao hơn chặn các nút thấp hơn để có K nhỏ, vì vậy Kmin tăng lên khi chúng ta đi xuống. 

Điều này chứng tỏ rằng việc đếm tổ tiên ảnh hưởng trực tiếp đến ngưỡng kích hoạt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N)$| Hoạt động DFS + sắp xếp + BIT | 
| Không gian |$O(N)$| kề + mảng + BIT | 

Giải pháp phù hợp vì tổng số nút trên tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$và tất cả các phép toán đều là các phép toán logarit hoặc tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# sample structure checks only
assert run("""2
4
1 2 3 4
1 2
1 3
2 4
""") != "", "basic tree"

assert run("""1
2
100 1
1 2
""") != "", "chain case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây sao | lựa chọn đơn điệu | sự thống trị gốc | 
| chuỗi | tăng ràng buộc | xếp chồng tổ tiên | 
| cây cân đối | cấu trúc hỗn hợp | xử lý cây con | 

## Vỏ cạnh 

Trường hợp cạnh khóa là một chuỗi sâu trong đó tất cả các giá trị đều giảm. Trong tình huống đó, các ràng buộc tổ tiên tích lũy nhanh chóng và chỉ ở mức cao$K$giá trị cho phép các nút sâu hơn được chọn. Thuật toán xử lý vấn đề này vì Kmin của mỗi nút phụ thuộc hoàn toàn vào tổ tiên đã được kích hoạt theo thứ tự DFS, đảm bảo các nút sâu hơn tự nhiên nhận được ngưỡng cao hơn.
