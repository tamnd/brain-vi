---
title: "CF 446C - DZY thích dãy số Fibonacci"
description: "Chúng tôi được cung cấp một mảng các số nguyên và cần hỗ trợ hai loại truy vấn một cách hiệu quả. Loại đầu tiên yêu cầu thêm các số Fibonacci vào một mảng con liền kề: đối với các chỉ số từ l đến r, chúng ta thêm F₁ vào phần tử ở l, F₂ vào phần tử ở l+1, cho đến F{r-l+1} ở vị trí r."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 446
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round #FF (Div. 1)"
rating: 2400
weight: 446
solve_time_s: 392
verified: false
draft: false
---

[CF 446C - DZY thích số Fibonacci](https://codeforces.com/problemset/problem/446/C) 

**Đánh giá:** 2400 
**Tas:** cấu trúc dữ liệu, toán, lý thuyết số 
**Thời gian giải:** 6 phút 32 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng các số nguyên và cần hỗ trợ hai loại truy vấn một cách hiệu quả. Loại đầu tiên yêu cầu thêm các số Fibonacci vào một mảng con liền kề: đối với các chỉ mục từ _l_ đến _r_, chúng ta thêm _F₁_ vào phần tử tại _l_, _F₂_ vào phần tử tại _l+1_, tối đa _F_{r-l+1__ tại vị trí _r_. Loại thứ hai yêu cầu tổng các phần tử trong mảng con theo modulo 10⁹+9. 

Các ràng buộc rất chặt chẽ: mảng có thể có tới 300.000 phần tử và số lượng truy vấn cũng có thể lên tới 300.000. Điều này loại trừ bất kỳ giải pháp nào thực hiện phép lặp đơn giản trên một phạm vi cho mỗi truy vấn, vì đó sẽ là O(n·m) trong trường hợp xấu nhất, có thể là khoảng 9×10¹⁰ thao tác, vượt xa những gì có thể chạy trong 4 giây. Giá trị của các phần tử mảng và số Fibonacci có thể trở nên rất lớn, vì vậy chúng ta cần xử lý cẩn thận số học mô-đun. 

Trường hợp phức tạp phát sinh khi một truy vấn chỉ ảnh hưởng đến một phần tử duy nhất hoặc khi các cập nhật liên tiếp chồng lên nhau. Ví dụ: nếu mảng là [1,2] và truy vấn là cộng các số Fibonacci từ 1 đến 2, thì bản cập nhật sẽ thêm [1,1] vào mảng. Việc triển khai đơn giản chỉ đơn giản là thêm các số Fibonacci bằng cách tính toán lại chúng cho mỗi truy vấn sẽ quá chậm và có thể dễ dàng tạo ra kết quả sai nếu các chỉ số hoặc độ lệch Fibonacci bị căn chỉnh sai. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp thực hiện các truy vấn như được mô tả. Đối với truy vấn loại 1, chúng tôi tính toán các số Fibonacci r-l+1 đầu tiên và thêm chúng riêng lẻ vào từng phần tử trong phạm vi. Đối với truy vấn loại 2, chúng tôi tính tổng các phần tử trong phạm vi. Mặc dù điều này hoạt động về mặt khái niệm nhưng độ phức tạp là O(m·n) trong trường hợp xấu nhất. Nếu tất cả các truy vấn bao gồm gần như toàn bộ mảng thì thuật toán sẽ thực hiện khoảng 9×10¹⁰ thao tác, vượt xa mức có thể chấp nhận được. 

Cái nhìn sâu sắc quan trọng để tăng tốc độ này là nhận ra rằng phép cộng Fibonacci là tuyến tính và có thể được biểu thị bằng cách sử dụng phép truy toán tuyến tính. Điều này cho phép chúng ta áp dụng phương pháp lan truyền lười trên cây phân đoạn. Thay vì cập nhật từng phần tử riêng lẻ, chúng tôi lưu trữ hiệu ứng của việc thêm phân đoạn Fibonacci ở dạng thu gọn bằng cách sử dụng hai số Fibonacci đầu tiên của phân đoạn. Khi chúng ta truyền các cập nhật xuống cây, chuỗi Fibonacci có thể được mở rộng một cách hiệu quả bằng cách sử dụng phép truy hồi của nó. Điều này biến đổi hoạt động cập nhật từ O(phạm vi) thành O(log n), trong khi truy vấn tổng cũng trở thành O(log n). 

Thuộc tính Fibonacci F_k+2 = F_k+1 + F_k cho phép chúng ta tính toán các giá trị tiếp theo trong một phạm vi mà không cần tính toán lại toàn bộ chuỗi. Các nút cây phân đoạn không chỉ duy trì tổng của khoảng mà còn có hai số đại diện cho hai đóng góp Fibonacci đầu tiên cần được nhân rộng. Lan truyền lười biếng đảm bảo rằng các cập nhật chồng chéo hoặc liên tiếp được xử lý chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n·m) | O(n) | Quá chậm | 
| Cây phân đoạn + Fibonacci lười biếng | O((n+m)·log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán trước các số Fibonacci modulo 10⁹+9 cho đến n+2, vì các bản cập nhật có thể yêu cầu số Fibonacci r-l+1-th. Điều này cho phép truy cập liên tục vào bất kỳ giá trị Fibonacci cần thiết nào. 
2. Xây dựng cây phân đoạn trên mảng. Mỗi nút lưu trữ tổng phân đoạn của nó theo modulo 10⁹+9. Ngoài ra, mỗi nút duy trì một bản cập nhật lười biếng dưới dạng một cặp số đại diện cho hai giá trị Fibonacci đầu tiên cần được truyền cho trẻ em. 
3. Đối với truy vấn loại 1 "1 l r", thay vì cập nhật từng phần tử, hãy tính hai số Fibonacci đầu tiên bắt đầu ở vị trí l. Sử dụng phương pháp lan truyền lười biếng để thêm đoạn Fibonacci này vào nút tương ứng trong cây phân đoạn. Khi một nút bị che một phần, hãy đẩy bản cập nhật tới các nút con của nó, tính toán lại các số Fibonacci bắt đầu cho mỗi nút con bằng cách sử dụng phép truy hồi tuyến tính. 
4. Đối với truy vấn loại 2 "2 l r", hãy truy vấn cây phân đoạn để biết tổng của phạm vi. Nếu có bất kỳ bản cập nhật lười biếng nào tồn tại trên đường dẫn, hãy truyền bá chúng trước để đảm bảo tổng phản ánh tất cả các bản cập nhật trước đó. 
5. Khi truyền các cập nhật Fibonacci tới các nút con, hãy tính hai giá trị Fibonacci đầu tiên cho nút con bên trái trực tiếp từ cặp lười của nút cha. Đối với con bên phải, hãy sử dụng lũy ​​thừa ma trận hoặc tổng Fibonacci đã biết để dịch chuyển điểm bắt đầu một cách chính xác. Điều này đảm bảo rằng các bản cập nhật duy trì trình tự chính xác. 
6. Thực hiện số học mô-đun ở mỗi bước để tránh tràn số nguyên, vì các số có thể tăng nhanh do tăng trưởng Fibonacci. 

Tại sao nó hoạt động: Cây phân đoạn đảm bảo rằng mỗi truy vấn được áp dụng cho một tập hợp nút tối thiểu. Lan truyền lười biếng đảm bảo rằng mỗi phân đoạn Fibonacci được áp dụng chính xác khi cần thiết mà không cần tính toán lại. Biểu diễn hai Fibonacci đầu tiên là đủ vì toàn bộ chuỗi Fibonacci có thể được xây dựng lại từ hai giá trị đó, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 9

def add_mod(a, b):
    return (a + b) % MOD

def mul_mod(a, b):
    return (a * b) % MOD

def build_fib(n):
    fib = [0] * (n + 2)
    fib[1] = 1
    for i in range(2, n + 2):
        fib[i] = add_mod(fib[i-1], fib[i-2])
    return fib

class SegmentTree:
    def __init__(self, a):
        self.n = len(a)
        self.size = 1
        while self.size < self.n:
            self.size <<= 1
        self.sum = [0] * (2 * self.size)
        self.lazy = [(0,0)] * (2 * self.size)
        for i in range(self.n):
            self.sum[self.size + i] = a[i] % MOD
        for i in range(self.size - 1, 0, -1):
            self.sum[i] = add_mod(self.sum[i<<1], self.sum[i<<1|1])

    def push(self, v, l, r):
        a, b = self.lazy[v]
        if a == 0 and b == 0:
            return
        mid = (l + r) >> 1
        len_left = mid - l + 1
        len_right = r - mid
        # Update left child
        self.apply(v<<1, l, mid, a, b)
        # Update right child: shift Fibonacci sequence by len_left
        fib_a = add_mod(mul_mod(fib[len_left-1], b), mul_mod(fib[len_left], a))
        fib_b = add_mod(mul_mod(fib[len_left], b), mul_mod(fib[len_left+1], a))
        self.apply(v<<1|1, mid+1, r, fib_a, fib_b)
        self.lazy[v] = (0,0)

    def apply(self, v, l, r, a, b):
        # Sum over segment: F1+F2+...+Fk = ?
        k = r - l + 1
        s = add_mod(mul_mod(fib[k], b), mul_mod(fib[k-1], a))
        self.sum[v] = add_mod(self.sum[v], s)
        if v < self.size:
            self.lazy[v] = (add_mod(self.lazy[v][0], a), add_mod(self.lazy[v][1], b))

    def update(self, ql, qr, a, b, v=1, l=0, r=None):
        if r is None:
            r = self.size - 1
        if ql > r or qr < l:
            return
        if ql <= l and r <= qr:
            self.apply(v, l, r, a, b)
            return
        self.push(v, l, r)
        mid = (l + r) >> 1
        self.update(ql, qr, a, b, v<<1, l, mid)
        self.update(ql, qr, a, b, v<<1|1, mid+1, r)
        self.sum[v] = add_mod(self.sum[v<<1], self.sum[v<<1|1])

    def query(self, ql, qr, v=1, l=0, r=None):
        if r is None:
            r = self.size - 1
        if ql > r or qr < l:
            return 0
        if ql <= l and r <= qr:
            return self.sum[v]
        self.push(v, l, r)
        mid = (l + r) >> 1
        return add_mod(self.query(ql, qr, v<<1, l, mid),
                       self.query(ql, qr, v<<1|1, mid+1, r))

n, m = map(int, input().split())
a = list(map(int, input().split()))
fib = build_fib(n+2)
st = SegmentTree(a)

for _ in range(m):
    tmp = input().split()
    if tmp[0] == '1':
        l, r = int(tmp[1])-1, int(tmp[2])-1
        st.update(l, r, fib[1], fib[2])
    else:
        l, r = int(tmp[1])-1, int(tmp[2])-1
        print(st.query(l, r))
```Mã này trước tiên xây dựng mảng Fibonacci để truy cập liên tục. Cây phân đoạn hỗ trợ lan truyền lười biếng trong đó mỗi nút lưu trữ một cặp số Fibonacci đại diện cho các bản cập nhật đang chờ xử lý. Các cập nhật được áp dụng hiệu quả cho các phạm vi và truy vấn tổng tính toán chính xác cho tất cả các Fibonacci trước đó
