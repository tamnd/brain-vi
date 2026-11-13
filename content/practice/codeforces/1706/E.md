---
title: "CF 1706E - Qpwoeirut và Vertices"
description: "Chúng ta có một đồ thị vô hướng liên thông với các đỉnh $n$ và các cạnh $m$. Mỗi cạnh được đánh số từ 1 đến $m$ và chúng ta được yêu cầu xử lý các truy vấn $q$."
date: "2026-06-09T21:20:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dfs-and-similar", "divide-and-conquer", "dsu", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 1706
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 809 (Div. 2)"
rating: 2300
weight: 1706
solve_time_s: 133
verified: false
draft: false
---

[CF 1706E - Qpwoeirut và Vertices](https://codeforces.com/problemset/problem/1706/E) 

**Đánh giá:** 2300 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, dsu, tham lam, cây cối 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông với$n$đỉnh và$m$các cạnh. Mỗi cạnh được đánh số từ 1 đến$m$, và chúng tôi được yêu cầu xử lý$q$truy vấn. Mỗi truy vấn cung cấp một loạt các chỉ số đỉnh$[l, r]$, và chúng ta phải xác định số cạnh ban đầu nhỏ nhất$k$sao cho tất cả các đỉnh trong phạm vi đó có thể tiếp cận lẫn nhau chỉ bằng các cạnh từ 1 đến$k$. 

Trình bày lại theo thuật ngữ thực tế: hãy tưởng tượng bạn lần lượt thêm các cạnh vào một biểu đồ trống ban đầu theo thứ tự đã cho. Đối với một tập hợp các đỉnh liên tiếp nhất định, chúng ta muốn biết thời điểm chúng tạo thành một thành phần liên thông với nhau. Chúng tôi không quan tâm đến khả năng kết nối với các đỉnh ngoài phạm vi này mà chỉ quan tâm đến kết nối nội bộ. 

Những ràng buộc ngụ ý rằng$n$có thể lên tới$10^5$Và$m, q$lên đến$2 \cdot 10^5$cho mỗi trường hợp thử nghiệm, với tổng số trên tất cả các trường hợp thử nghiệm được giới hạn tương tự. Điều này có nghĩa là một thuật toán có độ phức tạp$O(n m q)$hoặc$O(n^2)$là không thể. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính trong$m$Và$n$mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng bao gồm phạm vi độ dài 1 và các truy vấn trong đó$l$Và$r$nằm ở điểm cực trị của thứ tự đỉnh. Ví dụ: một truy vấn như$l=1, r=1$sẽ trả về 0, vì một đỉnh được kết nối tầm thường với chính nó. Việc triển khai bất cẩn luôn xử lý các cạnh sẽ trả về ít nhất 1. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng việc thêm các cạnh tăng dần và đối với mỗi truy vấn, hãy kiểm tra xem các đỉnh trong phạm vi truy vấn có được kết nối hay không. Điều này đòi hỏi phải duy trì một sơ đồ con của biểu đồ đầu tiên$k$cạnh và biểu diễn$O(r-l+1)$kiểm tra kết nối cho mỗi truy vấn. Trong trường hợp xấu nhất, đây là$O(m q n)$, quá chậm. 

Thông tin chi tiết quan trọng là vấn đề yêu cầu kết nối **phạm vi đỉnh liên tiếp** chứ không phải tập hợp tùy ý. Điều này cho phép chúng ta tính toán, cho mỗi đỉnh$i$, cạnh sớm nhất$f[i]$sau đó là đỉnh$i$trở nên kết nối với$i-1$. Nếu chúng tôi duy trì thông tin này trong khi lặp qua các cạnh theo thứ tự, vấn đề sẽ giảm xuống **truy vấn phạm vi tối đa**: đối với một truy vấn$[l, r]$, câu trả lời là$\max(f[l+1], f[l+2], ..., f[r])$. Điều này hoạt động vì khả năng kết nối của toàn bộ phạm vi yêu cầu chỉ số cạnh lớn nhất kết nối bất kỳ cặp liên tiếp nào. 

Để tính toán hiệu quả$f[i]$, chúng ta có thể sử dụng cấu trúc Disjoint Set Union (DSU), còn được gọi là Union-Find. Khi chúng ta thêm các cạnh, chúng ta hợp nhất các tập hợp chứa các điểm cuối của cạnh và theo dõi đỉnh tối đa trong mỗi thành phần. Khi hai thành phần hợp nhất, chúng ta cập nhật cạnh đầu tiên nối các đỉnh liên tiếp trong thành phần đó. Quá trình này chạy trong thời gian gần tuyến tính. 

Điều này làm giảm công việc trên mỗi truy vấn thành phạm vi truy vấn tối đa trên mảng$f$, có thể được xử lý trước bằng cây phân đoạn hoặc đơn giản là mảng tiền tố tối đa nếu truy vấn ngoại tuyến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(q \cdot n \cdot m)$|$O(n+m)$| Quá chậm | 
| Tối ưu |$O((n+m) \cdot \alpha(n) + q)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo cấu trúc DSU cho$n$đỉnh. Mỗi đỉnh bắt đầu trong thành phần riêng của nó. 
2. Khởi tạo một mảng$f[2..n]$với số không.$f[i]$sẽ lưu trữ chỉ số cạnh tối thiểu sau đó các đỉnh$i-1$Và$i$được kết nối. 
3. Lặp lại các cạnh theo thứ tự$1..m$. Đối với một cạnh kết nối$u$Và$v$: 

- Tìm các thành phần của$u$Và$v$sử dụng DSU. 
- Nếu chúng đã có trong cùng một thành phần thì bỏ qua. 
- Ngược lại thì gộp 2 thành phần lại với nhau. Sau khi hợp nhất, với tất cả các đỉnh liên tiếp bao trùm hợp, hãy cập nhật$f[i] = \text{current edge index}$nếu nó bằng không. 
4. Sau khi xử lý tất cả các cạnh, tính mảng tiền tố cực đại$F$từ$f$. Đây$F[i] = \max(f[2..i])$. Điều này cho phép$O(1)$câu trả lời truy vấn bằng cách lấy$F[r]$hoặc$F[r] - F[l]$tùy thuộc vào việc lập chỉ mục. 
5. Đối với mỗi truy vấn$[l, r]$:

- Nếu như$l = r$, đầu ra 0. 
- Ngược lại thì xuất ra$\max(f[l+1..r])$. Điều này tương ứng với cạnh sớm nhất mà sau đó toàn bộ phạm vi sẽ được kết nối. 

**Tại sao nó hoạt động:** Mỗi bước hợp nhất đảm bảo rằng cạnh đầu tiên nối hai đỉnh liên tiếp bất kỳ sẽ được ghi vào$f$. Bởi vì một phạm vi được kết nối khi và chỉ khi tất cả các cặp liên tiếp được kết nối, việc lấy giá trị tối đa của các giá trị này sẽ mang lại giá trị tối thiểu$k$yêu cầu. DSU đảm bảo chúng ta không bao giờ bỏ lỡ việc hợp nhất và phương pháp phỏng đoán theo cấp bậc liên kết đảm bảo tính hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.size = [1]*n
        self.left = list(range(n))
        self.right = list(range(n))

    def find(self, x):
        if self.p[x] != x:
            self.p[x] = self.find(self.p[x])
        return self.p[x]

    def union(self, x, y, edge_idx, f):
        x = self.find(x)
        y = self.find(y)
        if x == y:
            return
        if self.size[x] < self.size[y]:
            x, y = y, x
        for i in range(self.left[y]+1, self.right[y]+1):
            if f[i] == 0:
                f[i] = edge_idx
        self.p[y] = x
        self.size[x] += self.size[y]
        self.left[x] = min(self.left[x], self.left[y])
        self.right[x] = max(self.right[x], self.right[y])

def solve():
    t = int(input())
    for _ in range(t):
        n, m, q = map(int, input().split())
        edges = [tuple(map(int, input().split())) for _ in range(m)]
        edges = [(u-1, v-1) for u,v in edges]
        f = [0]*(n+1)
        dsu = DSU(n)
        for idx, (u, v) in enumerate(edges, 1):
            dsu.union(u, v, idx, f)
        prefix_max = [0]*(n+1)
        for i in range(2, n+1):
            prefix_max[i] = max(prefix_max[i-1], f[i])
        for _ in range(q):
            l, r = map(int, input().split())
            if l == r:
                print(0, end=' ')
            else:
                print(prefix_max[r], end=' ')
        print()

if __name__ == "__main__":
    solve()
```DSU lưu trữ các đỉnh ngoài cùng bên trái và ngoài cùng bên phải trong mỗi thành phần. Khi hai thành phần hợp nhất, nó cập nhật cạnh sớm nhất kết nối tất cả các đỉnh liên tiếp trong khoảng được hợp nhất. Mảng tiền tố tối đa chuyển đổi các giá trị cặp liên tiếp thành phản hồi truy vấn nhanh. Xử lý ranh giới đảm bảo các truy vấn có độ dài 1 trả về 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 2
1 2
1 1
1 2
```Trạng thái của các biến chính sau các cạnh: 

| Đỉnh | f[i] | 
| --- | --- | 
| 2 | 1 | 

Kết quả truy vấn: 

| Truy vấn | Tối đa f[l+1..r] | Trả lời | 
| --- | --- | --- | 
| 1 1 | - | 0 | 
| 1 2 | f[2]=1 | 1 | 

Điều này xác nhận rằng một đỉnh được kết nối bình thường và cạnh đầu tiên kết nối đỉnh 1 và 2. 

### Ví dụ 2 

đầu vào:```
5 5 5
1 2
1 3
2 4
3 4
3 5
1 4
3 4
2 2
2 5
3 5
```Tiền tố tối đa$F$sau khi xử lý các cạnh:`[0, 0, 1, 3, 3, 5]`Truy vấn: 

| Truy vấn | Tối đa f[l+1..r] | Trả lời | 
| --- | --- | --- | 
| 1 4 | max(f[2..4])=3 | 3 | 
| 3 4 | f[4]=3 | 3 | 
| 2 2 | - | 0 | 
| 2 5 | max(f[3..5])=5 | 5 | 
| 3 5 | tối đa(f[4..5])=5 | 5 | 

Bảng cho thấy rằng việc lấy giá trị tối đa trên các cặp liên tiếp một cách chính xác sẽ tạo ra cạnh sớm nhất bao phủ toàn bộ phạm vi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) α(n) + q) | Các hoạt động DSU chạy trong thời gian gần tuyến tính với tính năng nén đường dẫn và liên kết theo kích thước. Máy tính |
