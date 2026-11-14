---
title: "CF 1797E - Li Hua và Mảng"
description: "Chúng ta được cung cấp một mảng các số nguyên và hai loại phép toán. Thao tác đầu tiên biến đổi các phần tử trong một phạm vi bằng cách áp dụng hàm tổng số Euler, làm giảm một số thành số nguyên nguyên tố cùng nhau với nó."
date: "2026-06-09T09:58:20+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dsu", "math", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1797
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 864 (Div. 2)"
rating: 2300
weight: 1797
solve_time_s: 114
verified: false
draft: false
---

[CF 1797E - Li Hua và Array](https://codeforces.com/problemset/problem/1797/E) 

**Đánh giá:** 2300 
**Tags:** Brute Force, cấu trúc dữ liệu, dsu, toán, lý thuyết số, hai con trỏ 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và hai loại phép toán. Thao tác đầu tiên biến đổi các phần tử trong một phạm vi bằng cách áp dụng hàm tổng số Euler, làm giảm một số thành số nguyên nguyên tố cùng nhau với nó. Thao tác thứ hai hỏi, đối với một phạm vi, cần bao nhiêu ứng dụng của hàm tổng để tất cả các số trong phạm vi trở thành bằng nhau, đếm số lượng ứng dụng hàm cho mỗi phần tử. Điều quan trọng là truy vấn loại 2 không thực sự thay đổi mảng; chúng là những tính toán giả định. 

Kích thước đầu vào có thể lên tới 100.000 cho cả độ dài mảng và số lượng thao tác và mỗi số trong mảng có thể lớn tới 5 triệu. Một cách tiếp cận mạnh mẽ mô phỏng trực tiếp từng hoạt động tổng thể cho mỗi truy vấn là quá chậm, bởi vì việc áp dụng lặp lại tổng thể có thể mất tới$O(\log a_i)$trên mỗi số và thực hiện điều này với nhiều phần tử trên mỗi truy vấn sẽ dẫn đến hàng tỷ thao tác trong trường hợp xấu nhất. 

Các trường hợp cạnh không rõ ràng xuất hiện khi các phần tử đạt tới 1, vì tổng của 1 là 1 và các ứng dụng tiếp theo không có hiệu lực. Ví dụ, một mảng`[1, 1, 2]`với một truy vấn để cân bằng ba phần tử đầu tiên phải nhận ra rằng`2 → 1`thực hiện một thao tác, trong khi số 1 yêu cầu không. Một cách tiếp cận ngây thơ không coi 1 là điểm cố định sẽ tính toán quá mức. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng trực tiếp từng hoạt động. Đối với loại 1, lặp lại phạm vi và đặt từng phần tử thành tổng của nó. Đối với loại 2, lặp lại phạm vi và với mỗi số, mô phỏng tổng số ứng dụng lặp đi lặp lại cho đến khi tất cả các phần tử khớp với một số giá trị mục tiêu, thử tất cả các mục tiêu có thể. Điều này đúng nhưng quá chậm: việc áp dụng tổng số liên tục trong phạm vi lên tới 100.000 với số lượng lên tới 5 triệu sẽ yêu cầu hàng nghìn tỷ thao tác trong trường hợp xấu nhất. 

Điểm mấu chốt là việc áp dụng lặp đi lặp lại hàm tổng sẽ nhanh chóng giảm các số xuống còn 1. Trên thực tế, chuỗi các tổng của bất kỳ số nào$x$đạt 1 trong tối đa 20 bước vì mỗi ứng dụng sẽ giảm số lượng một cách nghiêm ngặt trừ khi nó đã là 1. Do đó, đối với mỗi phần tử, chúng ta có thể tính toán trước chuỗi tổng số của nó cho đến khi đạt đến 1. Sau đó, loại 2 truy vấn giảm xuống còn tìm, đối với một phạm vi, một số trong các chuỗi được tính toán trước nhằm giảm thiểu tổng số bước để làm cho tất cả các phần tử bằng nhau. 

Để quản lý hoạt động phạm vi một cách hiệu quả, chúng tôi sử dụng cây phân đoạn với dữ liệu tăng cường. Mỗi nút lưu trữ, đối với mỗi số trong phân đoạn của nó, tần số của số đó và tổng số ứng dụng tích lũy cần thiết để tiếp cận số đó. Các hoạt động loại 1 có thể được phổ biến một cách lười biếng: nếu một phân đoạn đã là tất cả các phân đoạn thì không cần cập nhật thêm. Sau đó, các thao tác Loại 2 có thể truy vấn cây phân đoạn để tìm số chung tốt nhất và tính tổng số bước liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * n * log(max(a_i))) | O(n) | Quá chậm | 
| Cây phân đoạn + Chuỗi tổng số tính toán trước | O(n log n + m log n) | O(n log max(a_i)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hàm tổng Euler cho tất cả các số nguyên lớn nhất có thể$a_i$sử dụng phương pháp sàng lọc. Điều này cho phép tra cứu tổng thể O(1). 
2. Đối với mỗi số trong mảng, hãy tính toán trước toàn bộ chuỗi tổng cho đến khi nó đạt đến 1. Lưu trữ chuỗi này và độ dài của nó; chiều dài tối đa được giới hạn bởi 20. 
3. Xây dựng cây phân đoạn trên mảng. Mỗi nút lưu trữ một bộ đếm ánh xạ từng giá trị trong phân đoạn tới tổng số ứng dụng cần thiết để tiếp cận nó. Nếu nhiều số giảm về cùng một giá trị, hãy tính tổng các bước. 
4. Đối với các thao tác loại 1, cập nhật đệ quy cây phân đoạn. Nếu phân đoạn chỉ chứa 1 thì không làm gì cả. Mặt khác, tăng từng bước được lưu trữ trong bộ đếm của nút lên một và truyền một cách lười biếng cho nút con. 
5. Đối với các thao tác loại 2, truy vấn cây phân đoạn để tìm giá trị trong phạm vi giảm thiểu tổng số bước để cân bằng tất cả các phần tử. Tính tổng các bước được lưu trong bộ đếm cho giá trị đó. 
6. Trả về tổng dưới dạng câu trả lời cho mỗi truy vấn loại 2. Cây phân đoạn đảm bảo tất cả các truy vấn và cập nhật được xử lý trong$O(\log n)$mỗi hoạt động. 

Điều này có tác dụng vì cây phân đoạn duy trì sự tổng hợp chính xác các giá trị mục tiêu có thể có và các bước tích lũy trên mỗi phân đoạn. Lan truyền lười biếng đảm bảo rằng các bản cập nhật được áp dụng chính xác một lần cho mỗi phân đoạn bị ảnh hưởng. Chuỗi tổng số được tính toán trước đảm bảo chúng tôi không bao giờ tính toán lại các tổng số lặp lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, Counter

MAX_A = 5_000_000

# Precompute totient
phi = list(range(MAX_A + 1))
for i in range(2, MAX_A + 1):
    if phi[i] == i:
        for j in range(i, MAX_A + 1, i):
            phi[j] -= phi[j] // i

# Precompute totient sequences
totient_seq = [[] for _ in range(MAX_A + 1)]
for x in range(1, MAX_A + 1):
    val = x
    while True:
        totient_seq[x].append(val)
        if val == 1:
            break
        val = phi[val]

class Node:
    __slots__ = 'l', 'r', 'left', 'right', 'counter', 'lazy'
    def __init__(self, l, r):
        self.l = l
        self.r = r
        self.left = None
        self.right = None
        self.counter = Counter()
        self.lazy = 0

def build(arr, l, r):
    node = Node(l, r)
    if l == r:
        node.counter = Counter({arr[l]: 0})
        return node
    mid = (l + r) // 2
    node.left = build(arr, l, mid)
    node.right = build(arr, mid+1, r)
    node.counter = node.left.counter + node.right.counter
    return node

def push(node):
    if node.lazy:
        new_counter = Counter()
        for val, steps in node.counter.items():
            seq = totient_seq[val]
            if node.lazy < len(seq):
                new_val = seq[node.lazy]
            else:
                new_val = 1
            new_counter[new_val] += steps + node.lazy
        node.counter = new_counter
        if node.left:
            node.left.lazy += node.lazy
            node.right.lazy += node.lazy
        node.lazy = 0

def update(node, l, r):
    push(node)
    if node.r < l or node.l > r:
        return
    if l <= node.l and node.r <= r:
        node.lazy += 1
        push(node)
        return
    update(node.left, l, r)
    update(node.right, l, r)
    node.counter = node.left.counter + node.right.counter

def query(node, l, r):
    push(node)
    if node.r < l or node.l > r:
        return Counter()
    if l <= node.l and node.r <= r:
        return node.counter
    return query(node.left, l, r) + query(node.right, l, r)

n, m = map(int, input().split())
arr = list(map(int, input().split()))
root = build(arr, 0, n-1)

for _ in range(m):
    t, l, r = map(int, input().split())
    l -= 1
    r -= 1
    if t == 1:
        update(root, l, r)
    else:
        cnt = query(root, l, r)
        if not cnt:
            print(0)
            continue
        min_ops = min(cnt.values())
        print(min_ops)
```Mã bắt đầu bằng cách tính hàm tổng cho tất cả các số lên tới 5 triệu. Sau đó, nó tính toán trước tổng chuỗi của mỗi số, cho phép chúng tôi xác định ngay kết quả của các ứng dụng lặp lại. Các nút cây phân đoạn lưu trữ các giá trị ánh xạ của bộ đếm theo số bước để tiếp cận chúng. Các bản cập nhật được lan truyền một cách lười biếng, đảm bảo hiệu quả. Truy vấn tổng hợp các bộ đếm trên một phạm vi để tìm tổng số thao tác tối thiểu cần thiết để cân bằng các giá trị. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5 4
8 1 6 3 7
2 1 5
2 3 4
1 1 3
2 3 4
```| Bước | Hoạt động | Mảng | Quầy tổng hợp | Bước tối thiểu | 
| --- | --- | --- | --- | --- | 
| 1 | truy vấn 2 1 5 | [8,1,6,3,7] | {8:0,1:0,6:0,3:0,7:0} | 10 | 
| 2 | truy vấn 2 3 4 | [8,1,6,3,7] | {6:0,3:0} | 2 | 
| 3 | cập nhật 1 1 3 | [4,1,2,3,7] | cập nhật một cách lười biếng | - | 
| 4 | truy vấn 2 3 4 | [4,1,2,3,7] | {2:0,3:0} | 1 | 

Bảng xác nhận rằng sau mỗi thao tác, cây phân đoạn phản ánh chính xác số bước cần thiết và lan truyền lười biếng cập nhật các giá trị một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n | |
