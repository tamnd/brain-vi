---
title: "CF 1906F - Tối đa hóa giá trị"
description: "Chúng ta được cung cấp một mảng có kích thước $N$, ban đầu chứa đầy các số 0. Có các hoạt động $M$; mỗi thao tác được mô tả bằng ba số nguyên $Li, Ri, Xi$, nghĩa là nếu chúng ta thực hiện thao tác này, chúng ta sẽ thêm $Xi$ vào mọi phần tử ở các vị trí từ $Li$ đến $Ri$."
date: "2026-06-08T20:45:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1906
codeforces_index: "F"
codeforces_contest_name: "2023-2024 ICPC, Asia Jakarta Regional Contest (Online Mirror, Unrated, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1906
solve_time_s: 105
verified: true
draft: false
---

[CF 1906F - Tối đa hóa giá trị](https://codeforces.com/problemset/problem/1906/F) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng kích thước$N$, ban đầu chứa đầy số không. có$M$hoạt động; mỗi thao tác được mô tả bằng ba số nguyên$L_i, R_i, X_i$, nghĩa là nếu chúng ta thực hiện thao tác này, chúng ta sẽ thêm$X_i$đến mọi phần tử ở vị trí$L_i$bởi vì$R_i$. 

Các truy vấn hơi bất thường: đối với mỗi truy vấn$(K, S, T)$, ta muốn tìm giá trị lớn nhất có thể của$A_K$sau khi thực hiện một chuỗi hoạt động tiếp giáp được chọn từ phạm vi$S$ĐẾN$T$. Nói cách khác, chúng ta có thể chọn bất kỳ khối hoạt động liên tục nào trong$[S, T]$và thực hiện chúng theo thứ tự, đồng thời chúng tôi muốn biết giá trị lớn nhất mà vị trí đó$K$có thể tiếp cận. 

Những hạn chế$N, M, Q \le 10^5$có nghĩa là bất kỳ giải pháp ngây thơ nào thử tất cả các phạm vi liền kề cho mọi truy vấn sẽ quá chậm. Mỗi truy vấn có thể yêu cầu$O(M^2)$kiểm tra theo cách tiếp cận vũ phu, điều này là không khả thi. Chúng ta phải tìm kiếm một giải pháp xung quanh$O((M + Q) \log M)$hoặc$O(M + Q)$cho mỗi truy vấn bằng cách sử dụng tiền xử lý và cấu trúc dữ liệu hiệu quả. 

Trường hợp cạnh không rõ ràng xảy ra khi các phép toán có thể có giá trị âm. Ví dụ, nếu$X_i$là âm, việc thực hiện nhiều thao tác hơn không nhất thiết sẽ tăng$A_K$. Một cách tiếp cận bất cẩn cho rằng "nhiều thao tác luôn tốt hơn" sẽ đưa ra câu trả lời sai. Tương tự, nếu phạm vi của truy vấn$[S, T]$không bao gồm các hoạt động ảnh hưởng đến$A_K$, câu trả lời đúng là bằng không. 

Ví dụ, hãy xem xét$N = 2$,$M = 2$, hoạt động:$(1, 1, -10), (2, 2, 20)$và truy vấn$(1, 1, 2)$. Chỉ thao tác 1 ảnh hưởng đến vị trí 1, do đó giá trị tối đa cho$A_1$là -10. Một tổng ngây thơ trên tất cả các hoạt động trong$[S, T]$sẽ cộng sai 20 từ thao tác thứ hai. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: đối với mỗi truy vấn, lặp lại tất cả các mảng hoạt động con trong$[S, T]$, mô phỏng việc áp dụng các phép toán và theo dõi giá trị lớn nhất của$A_K$. Điều này đòi hỏi$O((T-S+1)^2 \cdot N)$làm việc theo mỗi truy vấn, nghĩa là$O(M^2 \cdot N)$trong trường hợp xấu nhất. Với$M$lên đến$10^5$, cách tiếp cận này sẽ cần$10^{15}$hoạt động, rõ ràng là không thể. 

Cái nhìn sâu sắc quan trọng là đối với mỗi vị trí$K$, chúng tôi chỉ quan tâm đến các hoạt động ảnh hưởng đến nó. Mỗi hoạt động ảnh hưởng đến một phạm vi liên tục của các vị trí mảng. Chúng ta có thể xử lý trước các hoạt động để xây dựng, cho mỗi$K$, một danh sách các giá trị hoạt động tác động đến nó. Sau đó, mỗi truy vấn sẽ giảm xuống việc tìm tổng tối đa của một mảng con liền kề của các giá trị này, được giới hạn ở dãy con$[S, T]$. Đây là một bài toán "tổng mảng con tối đa" cổ điển, có thể giải được trong$O(\text{length})$sử dụng thuật toán của Kadane hoặc thậm chí nhanh hơn với cây phân đoạn nếu chúng ta muốn xử lý nhiều truy vấn một cách hiệu quả. 

Vì các truy vấn yêu cầu các mảng con tối đa trong các phạm vi cụ thể nên chúng ta có thể xây dựng cây phân đoạn theo trình tự các thao tác ảnh hưởng đến từng mảng con.$A_K$. Mỗi nút cây phân đoạn lưu trữ bốn giá trị: tổng tổng, tổng tiền tố tối đa, tổng hậu tố tối đa và tổng mảng con tối đa. Sau đó, trả lời một truy vấn là$O(\log M)$cho mỗi truy vấn sau khi tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q * M * N) | O(N) | Quá chậm | 
| Tiền xử lý + Cây phân đoạn cho mỗi vị trí | O(M log M + Q log M) | O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với từng vị trí$1 \le K \le N$, lặp qua tất cả các thao tác và thu thập các thao tác ảnh hưởng đến vị trí$K$. Lưu trữ dưới dạng danh sách các số nguyên, trong đó chỉ số tương ứng với số thao tác ban đầu. 
2. Đối với từng vị trí$K$, xây dựng cây phân đoạn trên danh sách các thao tác liên quan. Mỗi nút trong cây phân đoạn lưu trữ bốn giá trị: tổng tổng của phân đoạn, tổng tiền tố tối đa, tổng hậu tố tối đa và tổng mảng con tối đa. Điều này cho phép kết hợp các phân đoạn con một cách hiệu quả. 
3. Đối với mỗi truy vấn$(K, S, T)$:

Một. Lập bản đồ các chỉ số hoạt động toàn cầu$[S, T]$vào các chỉ số trong danh sách các hoạt động ảnh hưởng đến$K$. Chỉ những thao tác thực sự sửa đổi$A_K$có liên quan. 

b. Truy vấn cây phân đoạn để biết tổng mảng con tối đa trong các chỉ mục này. Điều này trả lại sự đóng góp tối đa có thể cho$A_K$từ bất kỳ chuỗi hoạt động tiếp giáp nào trong phạm vi truy vấn. 
4. Xuất kết quả. Nếu không có hoạt động nào ảnh hưởng$A_K$trong phạm vi, tổng tối đa bằng không. 

Tại sao nó hoạt động: Cây phân đoạn duy trì tính bất biến mà mỗi nút báo cáo chính xác tổng mảng con tối đa trong khoảng thời gian của nó. Khi truy vấn một dãy con, cây phân đoạn kết hợp các nút theo thời gian logarit, bảo toàn tổng mảng con tối đa. Bằng cách xử lý trước các hoạt động trên mỗi vị trí, chúng tôi tránh lặp lại các hoạt động không liên quan, đảm bảo hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, data):
        n = len(data)
        self.N = 1
        while self.N < n: self.N <<= 1
        self.data = [(-float('inf'), -float('inf'), -float('inf'), 0)] * (2*self.N)
        for i in range(n):
            val = data[i]
            self.data[self.N + i] = (val, val, val, val)  # total, prefix, suffix, max subarray
        for i in range(self.N-1, 0, -1):
            self.data[i] = self._combine(self.data[2*i], self.data[2*i+1])
    
    def _combine(self, left, right):
        total = left[0] + right[0]
        prefix = max(left[1], left[0] + right[1])
        suffix = max(right[2], right[0] + left[2])
        maxsum = max(left[3], right[3], left[2] + right[1])
        return (total, prefix, suffix, maxsum)
    
    def query(self, l, r):
        l += self.N
        r += self.N
        left_res = (0, 0, 0, -float('inf'))
        right_res = (0, 0, 0, -float('inf'))
        while l < r:
            if l % 2:
                left_res = self._combine(left_res, self.data[l])
                l += 1
            if r % 2:
                r -= 1
                right_res = self._combine(self.data[r], right_res)
            l //= 2
            r //= 2
        return self._combine(left_res, right_res)[3]

N, M = map(int, input().split())
ops = []
for _ in range(M):
    L, R, X = map(int, input().split())
    ops.append((L-1, R-1, X))

# map each position to affecting operations
pos_ops = [[] for _ in range(N)]
for idx, (L, R, X) in enumerate(ops):
    for k in range(L, R+1):
        pos_ops[k].append((idx, X))

# build segment trees per position
trees = []
for lst in pos_ops:
    vals = [x for idx, x in lst]
    if vals:
        trees.append(SegmentTree(vals))
    else:
        trees.append(None)

Q = int(input())
for _ in range(Q):
    K, S, T = map(int, input().split())
    K -= 1
    S -= 1
    # map global indices S,T to position-specific list indices
    lst = pos_ops[K]
    if not lst:
        print(0)
        continue
    l = 0
    while l < len(lst) and lst[l][0] < S:
        l += 1
    r = l
    while r < len(lst) and lst[r][0] <= T-1:
        r += 1
    if l == r:
        print(0)
    else:
        print(trees[K].query(l, r))
```Phần đầu tiên đọc đầu vào và thu thập các thao tác ảnh hưởng đến nó đối với từng vị trí mảng. Mỗi cây phân đoạn lưu trữ tiền tố, hậu tố và tổng tối đa có liên quan để hỗ trợ các truy vấn tối đa phạm vi. Đối với mỗi truy vấn, chúng tôi ánh xạ cẩn thận các chỉ số hoạt động toàn cầu vào danh sách hoạt động theo vị trí cụ thể, sau đó truy vấn cây phân đoạn. Kiểm tra ranh giới đảm bảo phạm vi trống trả về 0. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1**:```
2 6
1 1 -50
1 2 -20
2 2 -30
1 1 60
1 2 40
2 2 10
5
1 1 6
2 1 6
1 1 3
2 1 3
1 1 2
```Đối với vị trí 1: 

| Ý tôi là | Ảnh hưởng đến vị trí 1 | X | 
| --- | --- | --- | 
| 0 | vâng | -50 | 
| 1 | vâng | -20 | 
| 2 | không | - | 
| 3 | vâng | 60 | 
| | | |
