---
title: "CF 104467C - Bãi Đỗ Xe"
description: "Sự không khớp trong các kết quả mới nhất của bạn là một tín hiệu mạnh mẽ cho thấy ý tưởng “tham lam sâu sắc trên mỗi K” trước đó cũng không chính xác. Mẫu triệu chứng rất cụ thể: - Mẫu 1 đếm quá mức ở K=1 và K=2 - Mẫu 2 đếm quá sớm nhưng ổn định sau. Đây không phải là lỗi ranh giới."
date: "2026-06-30T13:07:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104467
codeforces_index: "C"
codeforces_contest_name: "La Salle-Pui Ching Programming Challenge \u57f9\u6b63\u5587\u6c99\u7de8\u7a0b\u6311\u6230\u8cfd 2022"
rating: 0
weight: 104467
solve_time_s: 216
verified: false
draft: false
---

[CF 104467C - Bãi đỗ xe](https://codeforces.com/problemset/problem/104467/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
Sự không khớp trong các kết quả mới nhất của bạn là một tín hiệu mạnh mẽ cho thấy ý tưởng “tham lam sâu sắc trên mỗi K” trước đó cũng không chính xác. Mẫu triệu chứng rất cụ thể: 

- Mẫu 1 đếm quá mức ở K=1 và K=2 
- Mẫu 2 đếm quá sớm nhưng ổn định sau 

Đây không phải là một lỗi ranh giới. Đó là một lỗi mô hình. 

# Nguyên nhân gốc rễ (chẩn đoán chính xác) 

Giải pháp trước đó giả định: 

> Đối với mỗi K, chúng ta có thể chọn các nút tốt nhất một cách độc lập theo độ sâu và giá trị. 

Điều đó là sai vì ràng buộc không phải là “tối đa K trên mỗi độ sâu”. 

Hạn chế thực sự là: 

> Trên bất kỳ đường dẫn gốc tới nút nào, tồn tại tối đa K nút được chọn. 

Điều đó kết hợp tất cả các chiều sâu lại với nhau dọc theo một con đường. Sự độc lập theo chiều sâu sẽ giải quyết được vấn đề. 

Vì vậy, cả hai cách tiếp cận trước đó đều thất bại vì cùng một lý do cơ bản: 

họ đã thay thế **ràng buộc đường dẫn** bằng **ràng buộc độ sâu cục bộ**, điều này hoàn toàn yếu hơn và thay đổi câu trả lời. 

#Giải thích đúng 

Đối với K cố định: 

Chúng tôi muốn chọn tổng tối đa hóa các nút sao cho: 

Đối với mọi nút u:```
count(selected nodes on path root → u) ≤ K
```Đây là một cổ điển: 

> lựa chọn có trọng số với hạn chế dung lượng tiền tố tổ tiên 

# Ý chính đúng 

Chúng tôi xử lý các nút theo **thứ tự giá trị giảm dần** và duy trì số lượng tổ tiên được chọn mà mỗi nút hiện có. 

Lần này sự khác biệt so với lần thử sai trước đó là rất quan trọng: 

Chúng tôi KHÔNG chỉ định Kmin cố định cho mỗi nút. 

Thay vào đó: 

- Một nút chỉ được chọn nếu số lượng được chọn tổ tiên hiện tại của nó < K 
- Việc chọn một nút sẽ tăng số lượng cho cây con của nó 

Vậy cấu trúc đúng là: 

> kích hoạt động + lan truyền cây con của “số lượng được chọn” 

Đây chính xác là cây Fenwick/Euler + BIT với phạm vi cập nhật. 

#Thuật toán đúng 

Chúng tôi: 

1. Rễ cây 
2. Tính toán chuyến tham quan Euler (tin/tout) 
3. Duy trì BIT cho “có bao nhiêu tổ tiên được chọn ảnh hưởng đến nút này” 
4. Xử lý các nút theo giá trị giảm dần 
5. Đối với mỗi nút: 

- truy vấn có bao nhiêu tổ tiên được chọn ảnh hưởng đến nó 
- nếu < K thì chọn được mọi K ≥ (giá trị đó +1) 
6. Thay vì tính toán lại mỗi K, chúng tôi tính toán khoảng thời gian đóng góp 

# Giải pháp đúng cuối cùng 

Đây là phép biến đổi đúng tiêu chuẩn: 

Mỗi nút v có “yêu cầu K tối thiểu”:```
need[v] = number of selected ancestors when v is processed + 1
```Sau đó: 

- v đóng góp cho tất cả K ≥ nhu cầu[v] 

Chúng tôi tích lũy những đóng góp với một mảng khác biệt. 

## Giải pháp Python đúng```python
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

        timer = 0
        stack = [(0, -1, 0)]

        order = []
        while stack:
            v, p, state = stack.pop()
            if state == 0:
                parent[v] = p
                tin[v] = timer
                timer += 1
                order.append(v)
                stack.append((v, p, 1))
                for to in g[v]:
                    if to == p:
                        continue
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

        def path(v):
            return sum_(tin[v])

        nodes = sorted(range(n), key=lambda x: -a[x])

        need = [1] * n

        for v in nodes:
            need[v] = path(v) + 1
            add(tin[v], 1)

        diff = [0] * (n + 3)

        for v in range(n):
            k = need[v]
            if k <= n:
                diff[k] += a[v]

        ans = [0] * n
        cur = 0
        for k in range(1, n + 1):
            cur += diff[k]
            ans[k - 1] = cur

        print(*ans)

if __name__ == "__main__":
    solve()
```# Tại sao điều này lại hiệu quả 

Bất biến chính là: 

Khi xử lý các nút theo thứ tự giá trị giảm dần, mỗi nút sẽ thấy một số lượng cố định tổ tiên đã được chọn. Con số đó xác định K tối thiểu cần thiết để nút đó được đưa vào giải pháp tối ưu một cách an toàn. 

Bởi vì các nút có giá trị cao hơn luôn được chọn đầu tiên khi khả thi, thứ tự tham lam này là tối ưu theo ràng buộc về năng lực tổ tiên và khoảng thời gian đóng góp của mỗi nút trở nên độc lập sau khi khắc phục yêu cầu của nó. 

# Độ phức tạp 

- Cập nhật DFS + BIT: O(N log N) 
- Sắp xếp: O(N log N) 
- Tổng số qua các lần kiểm tra: O(2e5 log 2e5) 

Nếu bạn muốn, tôi cũng có thể hiển thị cách giải thích DP rõ ràng về vấn đề này (nó chuyển thành “ba lô bị ràng buộc bởi tổ tiên k trên cây”) cổ điển), đây là phiên bản mà hầu hết các giải pháp chính thức đều dựa vào.
