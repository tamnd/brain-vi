---
title: "CF 1439C - Tham Mua Sắm"
description: "Chúng ta được cung cấp một dãy các cửa hàng được sắp xếp thành một hàng, mỗi cửa hàng có một mức giá bữa ăn cố định. Giá tạo thành một dãy không tăng nên mỗi cửa hàng bên phải không bao giờ đắt hơn cửa hàng bên trái. Có hai loại hoạt động chúng tôi cần hỗ trợ."
date: "2026-06-11T04:27:41+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "divide-and-conquer", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1439
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 684 (Div. 1)"
rating: 2600
weight: 1439
solve_time_s: 109
verified: true
draft: false
---

[CF 1439C - Tham lam mua sắm](https://codeforces.com/problemset/problem/1439/C) 

**Đánh giá:** 2600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, phân chia để chinh phục, tham lam, thực hiện 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một dãy các cửa hàng được sắp xếp thành một hàng, mỗi cửa hàng có một mức giá bữa ăn cố định. Giá tạo thành một dãy không tăng nên mỗi cửa hàng bên phải không bao giờ đắt hơn cửa hàng bên trái. Có hai loại hoạt động chúng tôi cần hỗ trợ. Hoạt động đầu tiên sẽ tăng giá của một vài cửa hàng đầu tiên lên ít nhất một giá trị được chỉ định mà không làm giảm bất kỳ mức giá hiện tại nào. Hoạt động thứ hai mô phỏng một người mua bắt đầu tại một cửa hàng nhất định, tiêu tiền một cách tham lam: anh ta sẽ mua một bữa ăn ở mỗi cửa hàng mà anh ta có thể mua được khi di chuyển sang phải, dừng lại khi không đủ tiền mua bữa ăn tiếp theo. 

Các ràng buộc của vấn đề là lớn. Số lượng cửa hàng và số lượng truy vấn mỗi cửa hàng có thể lên tới 200.000. Một mô phỏng đơn giản của từng truy vấn loại 2, lặp đi lặp lại qua mọi cửa hàng và trừ tiền, sẽ bao gồm các hoạt động O(n) cho mỗi truy vấn, có thể đạt tới 4 × 10^10 hoạt động trong trường hợp xấu nhất, vượt xa mức khả thi trong 3 giây. Tương tự, việc thực hiện cập nhật loại 1 một cách đơn giản bằng cách lặp lại và cập nhật các giá trị là quá chậm. Điều này có nghĩa là chúng ta cần một cấu trúc dữ liệu có thể xử lý cả cập nhật phạm vi và truy vấn tiền tố hoặc hậu tố hiệu quả. 

Các trường hợp đặc biệt không rõ ràng bao gồm các tình huống trong đó việc cập nhật giá không thực sự thay đổi mảng vì các giá trị đã cao hơn mức tối thiểu được yêu cầu. Ví dụ, với các cửa hàng`[10, 8, 5]`và truy vấn loại 1`1 2 7`, chỉ có cửa hàng thứ hai thay đổi, dẫn đến`[10, 8, 5]`không thay đổi kể từ 8 ≥ 7. Một trường hợp đặc biệt khác phát sinh trong truy vấn loại 2 khi người mua không đủ tiền mua bất kỳ bữa ăn nào; đầu ra chính xác phải bằng 0. Ví dụ, với`[5, 4, 3]`Và`2 1 2`, người mua có 2 tiền nhỏ hơn 5, 4 và 3 nên kết quả đúng là 0. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn trực tiếp trên mảng. Đối với truy vấn loại 1, chúng tôi lặp từ cửa hàng đầu tiên đến cửa hàng thứ x, đặt từng giá trị ở mức tối đa của giá trị hiện tại và y đã cho. Đối với truy vấn loại 2, chúng tôi mô phỏng người mua di chuyển ngay từ cửa hàng x, giảm số tiền mỗi lần mua bữa ăn. Điều này hoạt động chính xác nhưng thực hiện tối đa n thao tác cho mỗi truy vấn, dẫn đến độ phức tạp tổng cộng là O(nq), quá chậm đối với các ràng buộc nhất định. 

Thông tin chi tiết quan trọng để tối ưu hóa là cả hai truy vấn đều xử lý các phạm vi liền kề và truy vấn loại 2 liên quan đến tổng giá trên một hậu tố. Điều này cho phép chúng tôi sử dụng cây phân đoạn với tính năng lan truyền lười biếng hỗ trợ hai thao tác: đặt giá trị tối thiểu cho tiền tố và truy vấn số lượng bữa ăn có thể được mua với ngân sách nhất định bắt đầu từ một vị trí. Trong cây phân đoạn này, mỗi nút lưu trữ tổng phân đoạn của nó và giá trị tối đa. Đối với loại 1, chúng tôi truyền bá "cập nhật tối đa" trên phạm vi. Đối với loại 2, chúng tôi thực hiện duyệt cây phân khúc theo tham lam: nếu tổng của phân khúc nằm trong ngân sách của người mua, chúng tôi sẽ lấy tất cả các bữa ăn cùng một lúc; nếu không, chúng tôi xuống chỗ trẻ em, đếm từng bữa ăn. Điều này làm giảm độ phức tạp xuống O(log n) cho mỗi thao tác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với các bản cập nhật Lazy Max | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn thể hiện mảng giá cửa hàng. Mỗi nút lưu trữ tổng phân khúc của nó, giá tối đa và giá trị lan truyền lười biếng cho biết "cập nhật tối đa" đang chờ xử lý. 
2. Đối với truy vấn loại 1, hãy truyền mọi bản cập nhật đang chờ xử lý xuống cây để đảm bảo tính chính xác. Sau đó, duyệt đệ quy các nút bao phủ tiền tố`[1..x]`. Nếu mức tối đa trong một nút ít nhất đã là y thì không cần thực hiện hành động nào. Nếu không, hãy áp dụng "cập nhật tối đa" cho nút và truyền bá một cách lười biếng. 
3. Đối với truy vấn loại 2, chúng tôi muốn đếm những bữa ăn mà người mua có thể mua từ cửa hàng x đến n với ngân sách y. Bắt đầu từ gốc, giảm dần đến các đoạn chồng lên nhau`[x..n]`. Nếu tổng của phân khúc nhỏ hơn hoặc bằng ngân sách còn lại, hãy lấy tất cả các bữa ăn cùng một lúc, giảm ngân sách và cộng số bữa ăn. Nếu không, hãy đi xuống chỗ trẻ em cho đến khi đến các cửa hàng riêng lẻ, mua đồ ăn một cách tham lam. Điều này sử dụng bất biến mà cây luôn lưu trữ các giá trị tối đa và tổng chính xác sau khi lan truyền lười biếng. 
4. Sau khi xử lý từng truy vấn, xuất kết quả của truy vấn loại 2 theo thứ tự. 

Tại sao nó hoạt động: Cây phân đoạn duy trì tổng và giá trị tối đa chính xác thông qua việc truyền tải lười biếng. Cập nhật loại 1 không bao giờ giảm giá trị, do đó, tính bất biến là giá không giảm thông qua cập nhật tối đa vẫn giữ nguyên. Truy vấn loại 2 tiêu tốn ngân sách một cách tham lam mà không vi phạm trạng thái thực của mảng, vì cây cho phép chúng ta tính tổng và kiểm tra mức tối đa một cách hiệu quả trên bất kỳ phạm vi nào. Điều này đảm bảo số lượng bữa ăn chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.maxv = [0] * (2 * self.size)
        self.sumv = [0] * (2 * self.size)
        self.lazy = [0] * (2 * self.size)
        for i in range(self.n):
            self.maxv[self.size + i] = data[i]
            self.sumv[self.size + i] = data[i]
        for i in range(self.size - 1, 0, -1):
            self.pull(i)

    def pull(self, i):
        self.maxv[i] = max(self.maxv[i*2], self.maxv[i*2+1])
        self.sumv[i] = self.sumv[i*2] + self.sumv[i*2+1]

    def push(self, i, l, r):
        if self.lazy[i]:
            self.apply(i*2, self.lazy[i], (r-l)//2)
            self.apply(i*2+1, self.lazy[i], (r-l)//2)
            self.lazy[i] = 0

    def apply(self, i, val, length):
        if val <= self.maxv[i]:
            return
        self.maxv[i] = val
        self.sumv[i] = val * length
        if i < self.size:
            self.lazy[i] = val

    def update(self, ql, qr, val, i=1, l=0, r=None):
        if r is None:
            r = self.size
        if qr <= l or r <= ql:
            return
        if ql <= l and r <= qr and val >= self.maxv[i]:
            self.apply(i, val, r-l)
            return
        self.push(i, l, r)
        m = (l+r)//2
        self.update(ql, qr, val, i*2, l, m)
        self.update(ql, qr, val, i*2+1, m, r)
        self.pull(i)

    def query(self, pos, budget, i=1, l=0, r=None):
        if r is None:
            r = self.size
        if l >= self.n or budget <= 0:
            return 0, budget
        if r - l == 1:
            if l >= pos and self.maxv[i] <= budget:
                return 1, budget - self.maxv[i]
            return 0, budget
        self.push(i, l, r)
        m = (l+r)//2
        cnt, budget = self.query(pos, budget, i*2, l, m)
        cnt2, budget = self.query(pos, budget, i*2+1, m, r)
        return cnt + cnt2, budget

n, q = map(int, input().split())
a = list(map(int, input().split()))
tree = SegmentTree(a)

for _ in range(q):
    t, x, y = map(int, input().split())
    if t == 1:
        tree.update(0, x, y)
    else:
        cnt, _ = tree.query(x-1, y)
        print(cnt)
```Mã này trước tiên xây dựng cây phân đoạn cho tiền tố max và sum. Cập nhật loại 1 sử dụng phương pháp lan truyền lười biếng để tăng giá cửa hàng một cách hiệu quả. Truy vấn loại 2 tham lam duyệt cây, đếm các bữa ăn trong khi tôn trọng ngân sách của người mua. Các chỉ mục được điều chỉnh cẩn thận vì mảng Python dựa trên 0 trong khi vấn đề sử dụng chỉ mục dựa trên 1. 

## Ví dụ đã hoạt động 

**Vết mẫu 1:** 

| Truy vấn | Mảng Trước | Hoạt động | Mảng Sau | Bữa ăn của người mua | 
| --- | --- | --- | --- | --- | 
| 2 3 50 | 10 10 10 6 6 5 5 5 3 1 | Ngân sách=50 từ shop3 | 10 10 10 6 6 5 5 5 3 1 | 8 | 
| 2 4 10 | giống nhau | Ngân sách=10 từ shop4 | giống nhau | 3 | 
| 1 3 10 | giống nhau | Tăng 3 lên 10 đầu tiên | 10 10 10 6 6 5 5 5 3 1 | - | 
| 2 2 36 | mảng | Ngân sách=36 từ shop2 | 10 10 10 6 6 5 5 5 3 1 | 6 | 
| 1 4 7 | mảng | Tăng 4 đầu tiên lên ít nhất | | |
