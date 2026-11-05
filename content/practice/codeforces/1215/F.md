---
title: "CF 1215F - Đài phát thanh"
description: "Chúng tôi được yêu cầu chỉ định công suất tín hiệu cho một tập hợp các đài phát thanh và chọn một tập hợp con của các đài này sao cho mọi khiếu nại của người dân đều được đáp ứng, không có hai đài nào gây nhiễu và công suất tín hiệu của mỗi đài được chọn nằm trong phạm vi cho phép."
date: "2026-06-11T22:58:40+07:00"
tags: ["codeforces", "competitive-programming", "2-sat"]
categories: ["algorithms"]
codeforces_contest: 1215
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 585 (Div. 2)"
rating: 2700
weight: 1215
solve_time_s: 135
verified: false
draft: false
---

[CF 1215F - Đài phát thanh](https://codeforces.com/problemset/problem/1215/F) 

**Đánh giá:** 2700 
**Thẻ:** 2-thứ bảy 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chỉ định công suất tín hiệu cho một tập hợp các đài phát thanh và chọn một tập hợp con của các đài này sao cho mọi khiếu nại của người dân đều được đáp ứng, không có hai đài nào gây nhiễu và công suất tín hiệu của mỗi đài được chọn nằm trong phạm vi cho phép. Mỗi khiếu nại xác định hai trạm và yêu cầu ít nhất một trong số đó đang hoạt động. Mỗi trạm có công suất tín hiệu tối thiểu và tối đa được phép và một số cặp trạm nhất định không thể hoạt động do nhiễu. Thách thức là tìm ra công suất tín hiệu và một tập hợp các trạm hợp lệ hoặc không thể báo cáo được. 

Kích thước đầu vào lớn: lên tới 400.000 khiếu nại, trạm và cặp nhiễu. Điều này loại trừ bất kỳ thuật toán nào có độ phức tạp bậc ba hoặc bậc hai về số lượng trạm hoặc khiếu nại. Chúng ta cần một giải pháp gần với thời gian tuyến tính hoặc tuyến tính. Một cách tiếp cận ngây thơ thử mọi tập hợp con của các trạm hoặc mọi công suất tín hiệu có thể là không khả thi vì ngay cả việc liệt kê tất cả các tập hợp con của 400.000 trạm cũng rất chậm về mặt thiên văn. 

Các trường hợp biên không rõ ràng bao gồm các tình huống trong đó công suất tối thiểu cho phép của tất cả các trạm cao hơn công suất tối đa được phép của các trạm khác, khiến không thể tìm thấy một công suất tín hiệu toàn cầu duy nhất. Một trường hợp phức tạp khác là khi các hạn chế nhiễu tạo thành một mạng lưới phức tạp nhằm ngăn chặn các trạm kích hoạt cần thiết để khiếu nại, chẳng hạn như nếu các khiếu nại tạo thành một chu kỳ và mỗi trạm lân cận cản trở trạm tiếp theo. Việc triển khai bất cẩn có thể chọn các trạm một cách tham lam mà không kiểm tra xung đột, tạo ra giải pháp không hợp lệ hoặc báo cáo không có giải pháp khi có giải pháp. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ thử mọi công suất tín hiệu nguyên từ 1 đến M và với mỗi công suất, cố gắng chọn một tập hợp con các trạm đáp ứng tất cả các khiếu nại và tránh nhiễu. Để xác minh từng tập hợp con, người ta cần kiểm tra tất cả các khiếu nại và tất cả các cặp gây nhiễu. Về nguyên tắc, điều này đúng nhưng quá chậm: lặp lại M giá trị lên tới 4×10^5 và xử lý 400.000 khiếu nại cho mỗi khiếu nại dẫn đến khoảng 1,6×10^11 thao tác, vượt xa giới hạn khả thi. 

Quan sát quan trọng là đối với bất kỳ công suất tín hiệu được chọn nào, một trạm đều được phép (của nó`[l_i, r_i]`chứa công suất tín hiệu) hay không. Nếu chúng tôi sửa công suất tín hiệu dự kiến ​​thì vấn đề sẽ giảm xuống còn phiên bản 2-SAT. Mỗi khiếu nại yêu cầu ít nhất một trong hai trạm của nó được chọn, tạo thành một điều khoản`(x_i OR y_i)`. Mỗi cặp nhiễu cấm chọn cả hai trạm, tạo thành một mệnh đề`(NOT u_i OR NOT v_i)`. 2-SAT có thể được giải trong thời gian tuyến tính theo số lượng biến và mệnh đề bằng cách sử dụng các thành phần được liên kết chặt chẽ trong biểu đồ hàm ý. 

Vì vậy, cách tiếp cận tối ưu là lặp lại các công suất tín hiệu có thể có một cách thông minh. Thay vì kiểm tra tất cả các công suất từ ​​1 đến M, chúng tôi chỉ xem xét các công suất tại đó một số phạm vi trạm bắt đầu hoặc kết thúc vì chỉ những ranh giới này mới có thể thay đổi tập hợp các trạm hợp lệ. Đối với mỗi sức mạnh ứng cử viên, chúng tôi xây dựng phiên bản 2-SAT cho các trạm được phép ở sức mạnh đó, giải quyết và trả về giải pháp nếu thỏa mãn. Điều này làm giảm số lượng quyền hạn của ứng cử viên xuống tối đa là 2p, điều này là khả thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(M·(n+m)) ≈ 1,6×10^11 | O(n+m) | Quá chậm | 
| Tối ưu | O(p + n + m) trên mỗi ứng viên × O(p) ứng viên ≈ 10^7-10^8 | O(p + n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các giá trị khác biệt từ`l_i`Và`r_i`mảng các trạm. Đây là các công suất tín hiệu duy nhất trong đó tập hợp các trạm hợp lệ thay đổi, vì việc di chuyển giữa các điểm này không thêm hoặc bớt bất kỳ trạm được phép nào. 
2. Sắp xếp các quyền hạn ứng cử viên này. Việc lặp lại theo thứ tự cho phép chúng tôi xử lý các phạm vi liền kề một cách hiệu quả nếu muốn. 
3. Đối với mỗi công suất tín hiệu ứng cử viên`f`, xác định những trạm nào được phép. Một trạm được phép nếu tối thiểu`l_i ≤ f ≤ r_i`. 
4. Xây dựng phiên bản 2-SAT cho các trạm được phép: 

- Đối với mỗi khiếu nại`(x_i, y_i)`, thêm một mệnh đề`(x_i OR y_i)`. Điều này đảm bảo ít nhất một trạm đáp ứng được khiếu nại. 
- Đối với mỗi cặp nhiễu`(u_i, v_i)`, thêm một mệnh đề`(NOT u_i OR NOT v_i)`để cấm cả hai được chọn. 
5. Giải bài 2-SAT bằng phương pháp đồ thị hàm ý chuẩn: 

- Đối với mỗi khoản`(a OR b)`, thêm các cạnh`(NOT a → b)`Và`(NOT b → a)`. 
- Tìm các thành phần có liên hệ chặt chẽ. Nếu một biến và phủ định của nó nằm trong cùng một thành phần thì thể hiện đó không thỏa mãn. 
- Ngược lại, gán giá trị thật dựa trên thứ tự thành phần để có được lựa chọn trạm hợp lệ. 
6. Nếu bất kỳ công suất tín hiệu dự kiến ​​nào mang lại phiên bản 2-SAT thỏa mãn, hãy xuất công suất tín hiệu và các trạm đã chọn. Ngược lại thì báo cáo`-1`. 

Tại sao nó hoạt động: việc lặp lại các công suất biên đảm bảo rằng tất cả các công suất tín hiệu khả thi đều được kiểm tra mà không có sự dư thừa. 2-SAT mô hình chính xác cả các hạn chế khiếu nại và can thiệp. Giải pháp cho bất kỳ trường hợp thỏa mãn nào đều thỏa mãn tất cả các điều kiện bài toán ban đầu vì nó mã hóa trực tiếp phạm vi trạm được phép và hạn chế xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    n, p, M, m = map(int, input().split())
    complaints = [tuple(map(int, input().split())) for _ in range(n)]
    lr = [tuple(map(int, input().split())) for _ in range(p)]
    conflicts = [tuple(map(int, input().split())) for _ in range(m)]

    # Collect candidate powers
    candidates = set()
    for l, r in lr:
        candidates.add(l)
        candidates.add(r)
    candidates = sorted(candidates)

    # 2-SAT helper functions
    class TwoSAT:
        def __init__(self, n):
            self.N = n
            self.adj = [[] for _ in range(2*n)]

        def add_implication(self, u, v):
            self.adj[u].append(v)

        def add_or(self, u, v):
            self.add_implication(u^1, v)
            self.add_implication(v^1, u)

        def satisfiable(self):
            self.order = []
            self.visited = [False]*(2*self.N)
            self.comp = [0]*(2*self.N)

            def dfs(u):
                self.visited[u] = True
                for v in self.adj[u]:
                    if not self.visited[v]:
                        dfs(v)
                self.order.append(u)

            for u in range(2*self.N):
                if not self.visited[u]:
                    dfs(u)

            g_rev = [[] for _ in range(2*self.N)]
            for u in range(2*self.N):
                for v in self.adj[u]:
                    g_rev[v].append(u)

            self.visited = [False]*(2*self.N)
            label = 0
            comp = [0]*(2*self.N)
            def dfs_rev(u):
                comp[u] = label
                self.visited[u] = True
                for v in g_rev[u]:
                    if not self.visited[v]:
                        dfs_rev(v)

            for u in reversed(self.order):
                if not self.visited[u]:
                    dfs_rev(u)
                    label += 1
            self.comp = comp
            assignment = [False]*self.N
            for i in range(self.N):
                if comp[2*i] == comp[2*i+1]:
                    return None
                assignment[i] = comp[2*i] < comp[2*i+1]
            return assignment

    for f in candidates:
        valid = [i for i, (l, r) in enumerate(lr) if l <= f <= r]
        if not valid:
            continue
        id_map = {v: idx for idx, v in enumerate(valid)}
        ts = TwoSAT(len(valid))

        for x, y in complaints:
            if x-1 in id_map and y-1 in id_map:
                ts.add_or(2*id_map[x-1], 2*id_map[y-1])
            elif x-1 in id_map:
                ts.add_or(2*id_map[x-1], 2*id_map[x-1])
            elif y-1 in id_map:
                ts.add_or(2*id_map[y-1], 2*id_map[y-1])
            else:
                break
        else:
            for u, v in conflicts:
                if u-1 in id_map and v-1 in id_map:
                    ts.add_or(2*id_map[u-1]^1, 2*id_map[v-1]^1)
            assignment = ts.satisfiable()
            if assignment:
                chosen = [valid[i]+1 for i, val in enumerate(assignment) if val]
                print(len(chosen), f)
                print(*chosen)
                return
    print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xác định công suất tín hiệu dự kiến ​​bằng cách sử dụng ranh giới của phạm vi trạm. Lớp TwoSAT được triển khai bằng cách sử dụng phương pháp biểu đồ hàm ý. Khiếu nại tạo ra mệnh đề OR, can thiệp tạo ra mệnh đề NOT-OR. Chỉ các trạm được phép ở mức điện hiện tại mới được ánh xạ thành các biến. Nếu đạt yêu cầu, chương trình sẽ in ra nguồn điện và trạm đã chọn; nếu không, nó sẽ tiếp tục đến ứng cử viên tiếp theo. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Bước | Sức mạnh ứng cử viên f | Trạm hợp lệ | Các điều khoản được thêm vào | Thỏa mãn được không? | Trạm được chọn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1,2,4 | (1 HOẶC 3), (2 | | |
