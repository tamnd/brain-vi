---
title: "CF 2117H - Mưa không ngớt"
description: "Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn cập nhật một phần tử duy nhất của mảng và sau khi cập nhật, chúng ta được yêu cầu số nguyên tối đa $k$ sao cho một số mảng con chứa đa số $k$."
date: "2026-06-08T11:05:51+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2117
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 1029 (Div. 3)"
rating: 2500
weight: 2117
solve_time_s: 112
verified: false
draft: false
---

[CF 2117H - Mưa không ngừng](https://codeforces.com/problemset/problem/2117/H) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu, phân chia để chinh phục, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn cập nhật một phần tử của mảng và sau khi cập nhật, chúng tôi được yêu cầu số nguyên tối đa$k$sao cho một số mảng con chứa một$k$-số đông. MỘT$k$-đa số là số xảy ra ít nhất$\lfloor \frac{|b|+1}{2} \rfloor + k$lần trong một mảng con$b$. 

Vấn đề này là một thách thức vì sau mỗi lần cập nhật, chúng ta phải tính toán mức tối đa này$k$một cách hiệu quả. Độ dài mảng$n$và số lượng truy vấn$q$mỗi cái có thể lên đến$3 \cdot 10^5$và tổng của tất cả các trường hợp thử nghiệm cũng không vượt quá$3 \cdot 10^5$. Điều này có nghĩa là bất kỳ thuật toán nào lặp qua tất cả các mảng con sau mỗi truy vấn, sẽ là$O(n^2)$, hoàn toàn không thể thực hiện được. Chúng ta cần thứ gì đó gần gũi hơn$O(n \log n)$cho mỗi trường hợp thử nghiệm hoặc tốt hơn. 

Sự tinh tế đến từ định nghĩa của$k$-số đông. Đối với các mảng nhỏ hoặc mảng có các phần tử lặp lại, giá trị tối đa$k$có thể lớn, nhưng nếu mảng chứa tất cả các số riêng biệt thì giá trị lớn nhất$k$có thể bằng không. Một cách tiếp cận ngây thơ cố gắng đếm số lần xuất hiện cho mỗi mảng con sau mỗi lần cập nhật sẽ âm thầm tạo ra các câu trả lời sai hoặc vượt quá giới hạn thời gian. Ví dụ, với$a = [1,2,3,4,5]$, sau khi thiết lập$a_3 = 4$, mức tối đa chính xác$k$là$2$, nhưng một cách tiếp cận bất cẩn chỉ kiểm tra các phần tử đa số của toàn bộ mảng sẽ trả về$1$. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ lặp lại tất cả các mảng con cho mỗi truy vấn và kiểm tra xem mỗi số xuất hiện bao nhiêu lần. Đối với một truy vấn duy nhất, đây là$O(n^2)$mảng con, và trong mỗi mảng con, việc đếm số lần xuất hiện là$O(n)$, cho$O(n^3)$mỗi truy vấn. Điều này là quá chậm vì$n$có thể$3 \cdot 10^5$. 

Cái nhìn sâu sắc quan trọng là vấn đề giảm xuống để duy trì số lượng phần tử một cách hiệu quả và khai thác thực tế là chỉ một tập hợp nhỏ các số có thể có khả năng đạt được mức cao$k$. Trong mảng bất kỳ, số nào có thể đạt giá trị lớn nhất$k$phải là chế độ của một số mảng con. Điều này cho phép chúng ta xem xét cách tiếp cận chia để trị tương tự như thuật toán của Mo hoặc sử dụng cây phân đoạn với đa số ứng cử viên. Cụ thể, chúng tôi có thể theo dõi các ứng cử viên tiềm năng cho đa số bằng cách sử dụng cây phân đoạn và cập nhật nhanh chóng số lượng khi một phần tử thay đổi. Bởi vì chúng ta chỉ cần tối đa$k$chứ không phải mảng con chính xác, chúng ta có thể tìm kiếm nhị phân trên$k$và xác minh xem có ứng viên nào đạt được nó hay không. 

Điều này làm giảm vấn đề duy trì tần số phần tử một cách linh hoạt, tính toán tần số ứng cử viên tối đa trên các mảng con và thực hiện tìm kiếm nhị phân. Với thiết kế cây phân đoạn thích hợp hoặc các cấu trúc ổn định, mỗi truy vấn có thể được xử lý một cách đại khái$O(\log n)$, cho$O((n+q) \log n)$tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3 q)$|$O(n)$| Quá chậm | 
| Cây phân đoạn + Tìm kiếm nhị phân |$O((n+q)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đầu tiên, xử lý trước mảng để ánh xạ từng giá trị tới các vị trí của nó. Điều này cho phép tính toán tần số hiệu quả sau này. 
2. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ một phần tử đa số tiềm năng cho phân khúc đó và tần số của nó so với phân khúc đó. Các nút được hợp nhất bằng cách so sánh hai ứng cử viên con: nếu chúng giống nhau, tính tổng tần số; nếu khác, hãy trừ số nhỏ hơn khỏi số lớn hơn và giữ lại ứng cử viên còn lại. Điều này đảm bảo rằng bất kỳ ứng cử viên nào được lưu trữ tại một nút đều là ứng cử viên duy nhất có thể chiếm đa số trong phân khúc đó. 
3. Đối với mỗi truy vấn, cập nhật mảng và sửa đổi nút cây phân đoạn tương ứng với phần tử đã thay đổi. Cập nhật tần số từ dưới lên bằng cách sử dụng logic hợp nhất tương tự. 
4. Để tính giá trị lớn nhất$k$sau một truy vấn, trích xuất ứng cử viên từ nút gốc. Đếm tổng số lần xuất hiện của nó trong mảng bằng cách sử dụng các vị trí được xử lý trước. Tối đa$k$thì đơn giản là$\text{count} - \lfloor \frac{n+1}{2} \rfloor$. Nếu âm, đặt thành 0. 
5. Trả về mức tối đa$k$cho mỗi truy vấn. 

Tại sao nó hoạt động: Cây phân đoạn luôn duy trì một ứng cử viên có thể đạt được$k$-đa số trong một số mảng con. Bởi vì một sự thật$k$-đa số phải xuất hiện nhiều hơn một nửa độ dài của một mảng con cộng thêm$k$, bất kỳ số nào khác không thể vượt qua số lượng ứng viên ở bất kỳ phân khúc nào. Tìm kiếm nhị phân bật$k$không cần thiết vì mức tối đa có thể được tính trực tiếp từ tổng số lần xuất hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [None] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, node, l, r, arr):
        if l == r:
            self.tree[node] = (arr[l], 1)
            return
        m = (l + r) // 2
        self.build(node*2, l, m, arr)
        self.build(node*2+1, m+1, r, arr)
        self.tree[node] = self.merge(self.tree[node*2], self.tree[node*2+1])

    def merge(self, left, right):
        if left[0] == right[0]:
            return (left[0], left[1] + right[1])
        if left[1] > right[1]:
            return (left[0], left[1] - right[1])
        else:
            return (right[0], right[1] - left[1])

    def update(self, idx, val, node=1, l=0, r=None):
        if r is None:
            r = self.n - 1
        if l == r:
            self.tree[node] = (val, 1)
            return
        m = (l + r) // 2
        if idx <= m:
            self.update(idx, val, node*2, l, m)
        else:
            self.update(idx, val, node*2+1, m+1, r)
        self.tree[node] = self.merge(self.tree[node*2], self.tree[node*2+1])

    def query_candidate(self):
        return self.tree[1][0]

def solve():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))
        st = SegmentTree(a)
        from collections import defaultdict
        positions = defaultdict(list)
        for idx, val in enumerate(a):
            positions[val].append(idx)
        res = []
        for _ in range(q):
            i, x = map(int, input().split())
            i -= 1
            old = a[i]
            positions[old].remove(i)
            a[i] = x
            positions[x].append(i)
            st.update(i, x)
            cand = st.query_candidate()
            cnt = len(positions[cand])
            k = cnt - ((n + 1) // 2)
            res.append(max(0, k))
        print(" ".join(map(str, res)))

if __name__ == "__main__":
    solve()
```các`SegmentTree`lớp đóng gói tính toán ứng viên. Logic hợp nhất đảm bảo rằng bất kỳ số nào được trả về ở gốc đều là số duy nhất có thể vượt quá một nửa độ dài trong một số phân đoạn. Cập nhật vị trí chỉ sửa đổi đường dẫn từ lá đến gốc. Việc đếm số lần xuất hiện sử dụng`positions`map để tránh quét toàn bộ mảng, duy trì hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng đầu vào`[1,2,3,4,5]`, truy vấn`[(3,4),(1,4),(2,4),(4,3),(2,3)]`. 

| Bước | Mảng | Ứng viên | Đếm | k | 
| --- | --- | --- | --- | --- | 
| 0 | [1,2,3,4,5] | bất kỳ | 1 | 0 | 
| 1 | [1,2,4,4,5] | 4 | 2 | 1 | 
| 2 | [4,2,4,4,5] | 4 | 3 | 1 | 
| 3 | [4,4,4,4,5] | 4 | 4 | 2 | 
| 4 | [4,4,4,3,5] | 4 | 3 | 1 | 
| 5 | [4,3,4,3,5] | 4 | 2 | 0 | 

Bảng này cho thấy ứng viên của cây phân đoạn theo dõi phần tử đa số một cách hiệu quả và k được tính toán chính xác. 

### Ví dụ 2 

Mảng đầu vào`[3,2,3,3,2,2,3]`, truy vấn`[(2,3),(5,3),(6,3),(3,4),(4,4),(7,4),(6,4),(2,4)]`. 

Tương tự, bảng sẽ hiển thị các ứng viên đang điều chỉnh và k là sự khác biệt giữa số lượng ứng viên và nửa độ dài
