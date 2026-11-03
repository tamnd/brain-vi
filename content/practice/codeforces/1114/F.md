---
title: "CF 1114F - Xin vui lòng, một truy vấn khác về mảng?"
description: "Chúng tôi có một dãy số nguyên, mỗi số từ 1 đến 300 và chúng tôi cần xử lý hai loại truy vấn. Truy vấn đầu tiên nhân một phân đoạn liền kề của mảng với một số nhất định."
date: "2026-06-12T04:53:50+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "divide-and-conquer", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1114
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 538 (Div. 2)"
rating: 2400
weight: 1114
solve_time_s: 67
verified: true
draft: false
---

[CF 1114F - Vui lòng cung cấp một Truy vấn khác về mảng?](https://codeforces.com/problemset/problem/1114/F) 

**Đánh giá:** 2400 
**Tags:** bitmasks, cấu trúc dữ liệu, chia để trị, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một dãy số nguyên, mỗi số từ 1 đến 300 và chúng tôi cần xử lý hai loại truy vấn. Truy vấn đầu tiên nhân một phân đoạn liền kề của mảng với một số nhất định. Truy vấn thứ hai yêu cầu tổng Euler của tích một đoạn của mảng, modulo$10^9 + 7$. Hàm tổng Euler,$\varphi(n)$, đếm các số nguyên lên đến$n$đó là nguyên tố cùng nhau với$n$. Vì phép nhân có tính phân phối trên tích nên truy vấn nhân sẽ thay đổi hệ số nguyên tố của từng phần tử trong phân đoạn, từ đó ảnh hưởng đến tổng của bất kỳ phân đoạn nào bao gồm các phần tử đó. 

Các ràng buộc rất đáng kể: mảng có thể có tới 400.000 phần tử và có thể có 200.000 truy vấn. Các phương pháp ngây thơ tính toán lại kết quả mỗi lần hoặc thậm chí tính lại các thừa số nguyên tố cho mỗi phân đoạn sẽ thực hiện quá nhiều thao tác. Cụ thể, việc tính toán lại một sản phẩm cho mỗi truy vấn trong$O(n)$sẽ mang lại$O(nq)\approx 8 \cdot 10^{10}$hoạt động vượt xa khả năng chấp nhận được trong giới hạn thời gian 6 giây. 

Các trường hợp cạnh không rõ ràng bao gồm các phân đoạn chứa 1. Tổng của 1 là 1, do đó, bất kỳ phân đoạn nào hoàn toàn bằng 1 đều phải trả về 1. Tương tự, các phép toán nhân với 1 không thay đổi bất cứ điều gì nhưng vẫn phải được theo dõi trong cấu trúc của chúng ta. Việc triển khai bất cẩn giả sử tất cả các phần tử đều lớn hơn 1 có thể tính toán số 0 không chính xác nếu sử dụng công thức như$\prod (p_i-1)p_i^{k_i-1}$cho số nguyên tố$p_i$chia sản phẩm. 

Một điều tinh tế khác là tích của các phần tử phân đoạn có thể phát triển cực lớn, nhanh chóng vượt quá số nguyên 64 bit. Điều này ngăn cản việc tính toán tích số một cách đơn giản; lời giải phải hoạt động với cấu trúc nhân một cách gián tiếp, lý tưởng nhất là thông qua số mũ nguyên tố. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ xử lý truy vấn NHÂN bằng cách lặp từ$l$ĐẾN$r$và nhân từng phần tử, sau đó xử lý truy vấn TOTIENT bằng cách tính tích của phân đoạn và phân tích nó. Điều này hiệu quả với các mảng nhỏ vì các phần tử mảng được giới hạn bởi 300, do đó hệ số hóa của chúng rẻ. Tuy nhiên, với 400.000 phần tử và 200.000 truy vấn, phương pháp này có thể yêu cầu tới$8 \cdot 10^{10}$phép nhân hoặc phân tích nhân tử, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là cả hai phép toán đều tuyến tính trong không gian số mũ của các thừa số nguyên tố. Mỗi số nguyên từ 1 đến 300 có một tập hợp các thừa số nguyên tố cố định và việc nhân một phân đoạn với một số tương đương với việc cộng các số mũ nguyên tố tương ứng với mỗi phần tử trong phân đoạn đó. Tương tự, việc tính tổng tích số sẽ rút gọn thành việc kết hợp các số mũ nguyên tố:$\varphi(\prod a_i) = \prod p^{k_i-1}(p-1)$cho mọi số nguyên tố$p$chia sản phẩm. Điều này cho phép chúng tôi lưu trữ, đối với mỗi phân đoạn, một bitmask gồm các số nguyên tố hoặc vectơ số mũ và cập nhật/truy vấn nó một cách hiệu quả bằng cách sử dụng cây phân đoạn với khả năng lan truyền lười biếng. 

Chúng tôi tính toán trước các hệ số nguyên tố cho tất cả các số lên tới 300. Sau đó, chúng tôi biểu thị từng phân đoạn trong cây phân đoạn bằng thông tin nguyên tố tổng hợp. Truy vấn nhân thêm vectơ số mũ và truy vấn tổng kết hợp vectơ số mũ và tính toán modulo tổng$10^9 + 7$. Việc sử dụng mặt nạ bit để theo dõi số nguyên tố nào chia phân đoạn cho phép thực hiện các phép tính kết hợp cực nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q * 8) | O(n) | Quá chậm | 
| Cây phân đoạn + Số mũ Bitmask | O((n + q) * log n * P) | O(n * P) | Đã chấp nhận | 

Đây,$P$là số số nguyên tố ≤ 300, tức là 62. 

## Hướng dẫn thuật toán 

1. Tính trước tất cả các số nguyên tố đến 300 bằng sàng. Mỗi số$1 \le x \le 300$được liên kết với một bitmask gồm các số nguyên tố chia nó. Cửa hàng$\varphi(x)$dưới dạng bảng riêng cho các số lên tới 300 nếu cần. 
2. Đối với mỗi phần tử trong mảng, hãy chuyển đổi giá trị của nó thành một bitmask biểu thị các thừa số nguyên tố và vectơ số mũ của mỗi số nguyên tố. 
3. Xây dựng cây phân đoạn trên mảng, trong đó mỗi nút lưu trữ hai phần thông tin: vectơ số mũ tích lũy của các số nguyên tố trong phân đoạn đó và mặt nạ bit biểu thị số nguyên tố nào chia bất kỳ phần tử nào trong phân đoạn đó. 
4. Đối với truy vấn NHÂN, hãy chuyển đổi số nhân thành vectơ số mũ và mặt nạ bit của nó. Áp dụng bản cập nhật lười biếng cho cây phân đoạn, thêm số mũ vào phạm vi bị ảnh hưởng. Sử dụng bitwise OR cho mặt nạ bit để biểu thị sự hiện diện của số nguyên tố. 
5. Đối với truy vấn TOTIENT, hãy truy vấn cây phân đoạn để lấy vectơ số mũ tích lũy và mặt nạ bit cho phạm vi. Tính tổng bằng cách sử dụng$\varphi(\prod a_i) = \prod_{p} p^{k_p - 1} (p-1)$modulo$10^9 + 7$, Ở đâu$k_p$là tổng số mũ của số nguyên tố$p$trong phân khúc. 
6. Xuất kết quả theo thứ tự. 

**Tại sao nó hoạt động:** Cây phân đoạn với vectơ số mũ đảm bảo rằng đối với bất kỳ phạm vi truy vấn nào, chúng tôi biết hệ số nguyên tố kết hợp chính xác của phân đoạn. Lan truyền lười biếng đảm bảo rằng nhiều bội số được tích lũy chính xác trước tổng số truy vấn, duy trì tính bất biến mà mỗi nút thể hiện chính xác phân đoạn của nó. Mặt nạ bit cho phép kết hợp nhanh chóng và tránh tính toán lại các số nguyên tố có mặt. Tính đúng đắn phụ thuộc vào tính nhân của hàm tổng và tính chất cộng của số mũ trong phép nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

# Precompute primes and factorizations
MAX_A = 300
primes = []
is_prime = [True] * (MAX_A+1)
is_prime[0] = is_prime[1] = False
for i in range(2, MAX_A+1):
    if is_prime[i]:
        primes.append(i)
        for j in range(i*i, MAX_A+1, i):
            is_prime[j] = False

prime_index = {p:i for i,p in enumerate(primes)}
P = len(primes)

# Factorization as exponent vector
def factorize(x):
    exps = [0]*P
    for i,p in enumerate(primes):
        while x % p == 0:
            x //= p
            exps[i] += 1
    return exps

class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.tree = [[0]*P for _ in range(2*self.size)]
        self.lazy = [[0]*P for _ in range(2*self.size)]
        for i in range(self.n):
            self.tree[self.size + i] = data[i]
        for i in range(self.size-1, 0, -1):
            for j in range(P):
                self.tree[i][j] = self.tree[2*i][j] + self.tree[2*i+1][j]

    def push(self, v, l, r):
        for j in range(P):
            if self.lazy[v][j]:
                self.tree[v][j] += self.lazy[v][j]*(r-l+1)
                if v < self.size:
                    self.lazy[2*v][j] += self.lazy[v][j]
                    self.lazy[2*v+1][j] += self.lazy[v][j]
                self.lazy[v][j] = 0

    def update(self, ql, qr, add, v=1, l=0, r=None):
        if r is None:
            r = self.size-1
        self.push(v,l,r)
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            for j in range(P):
                self.lazy[v][j] += add[j]
            self.push(v,l,r)
            return
        m = (l+r)//2
        self.update(ql, qr, add, 2*v, l, m)
        self.update(ql, qr, add, 2*v+1, m+1, r)
        for j in range(P):
            self.tree[v][j] = self.tree[2*v][j] + self.tree[2*v+1][j]

    def query(self, ql, qr, v=1, l=0, r=None):
        if r is None:
            r = self.size-1
        self.push(v,l,r)
        if qr < l or r < ql:
            return [0]*P
        if ql <= l and r <= qr:
            return self.tree[v][:]
        m = (l+r)//2
        left = self.query(ql, qr, 2*v, l, m)
        right = self.query(ql, qr, 2*v+1, m+1, r)
        return [left[j]+right[j] for j in range(P)]

def mod_pow(a,b):
    res=1
    while b>0:
        if b&1:
            res=res*a%MOD
        a=a*a%MOD
        b>>=1
    return res

def totient(exps):
    res = 1
    for i,p in enumerate(primes):
        k = exps[i]
        if k > 0:
            res = res *
```
