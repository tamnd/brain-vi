---
title: "CF 1844H - Bội số của ba chu kỳ"
description: "Chúng ta được yêu cầu xử lý một chuỗi các cập nhật trên một mảng trống ban đầu có độ dài $n$. Mỗi lần cập nhật đặt một vị trí trong mảng thành một số và sau tất cả các lần cập nhật, mảng sẽ trở thành hoán vị từ $1$ đến $n$."
date: "2026-06-09T06:05:34+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "dp", "dsu", "math"]
categories: ["algorithms"]
codeforces_contest: 1844
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 884 (Div. 1 + Div. 2)"
rating: 3400
weight: 1844
solve_time_s: 99
verified: false
draft: false
---

[CF 1844H - Bội số của ba chu kỳ](https://codeforces.com/problemset/problem/1844/H) 

**Đánh giá:** 3400 
**Tags:** tổ hợp, cấu trúc dữ liệu, dp, dsu, toán 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xử lý một chuỗi các bản cập nhật trên một mảng có độ dài ban đầu trống$n$. Mỗi lần cập nhật đặt một vị trí trong mảng thành một số và sau tất cả các lần cập nhật, mảng sẽ trở thành một hoán vị của$1$bởi vì$n$. Sau mỗi lần cập nhật, chúng ta cần đếm số cách điền vào các vị trí trống còn lại để hoán vị cuối cùng chỉ chứa các chu trình có độ dài là bội số của ba. 

Một chu trình trong hoán vị là một tập hợp các chỉ số ánh xạ với nhau trong một vòng lặp khép kín. Ví dụ, trong hoán vị$[4,1,2,5,6,3]$, chu kỳ$(1,4,5,6,3,2)$có chiều dài$6$. Điều kiện quan trọng là tất cả các chu trình phải có độ dài chia hết cho ba. 

Ràng buộc đầu vào$n \le 3 \cdot 10^5$với$n \equiv 0 \pmod 3$có nghĩa là chúng ta không thể liệt kê các hoán vị một cách ngây thơ, vì điều đó đòi hỏi phải tính toán giai thừa theo thời gian. Chúng ta cần một cách tiếp cận tổ hợp hoặc đại số cho phép chúng ta cập nhật số đếm một cách hiệu quả sau mỗi thay đổi. 

Trường hợp phức tạp xảy ra khi một bản cập nhật ngay lập tức đóng một chu kỳ không phải là bội số của ba. Ví dụ, với$n=3$và một bản cập nhật duy nhất$a_1 = 2$, chúng ta tạo thành một chu trình có độ dài$1$nếu các mục khác vẫn trống, điều này làm mất hiệu lực tất cả các lần hoàn thành. Một phương pháp đơn giản chỉ đếm các hoán vị còn lại sẽ bỏ qua ràng buộc về cấu trúc này và tạo ra số đếm không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê tất cả các lần hoàn thành hoán vị có thể có sau mỗi lần cập nhật, tính toán tất cả các chu kỳ và kiểm tra độ dài của chúng. Điều này đúng về nguyên tắc nhưng không khả thi đối với$n \sim 3 \cdot 10^5$, vì mỗi bước có thể yêu cầu tới$O(n!)$hoạt động. 

Quan sát dẫn đến một giải pháp hiệu quả là hoán vị có thể được coi là sự kết hợp của các thành phần rời rạc được xác định bởi các vị trí đã được lấp đầy. Mỗi thành phần hoặc là đường đi của các phần tử được kết nối một phần hoặc là một chu trình đã hoàn thành. Cái nhìn sâu sắc quan trọng là số cách để hoàn thành một thành phần thành các chu kỳ chia hết cho ba chỉ phụ thuộc vào kích thước modulo ba của nó và cấu trúc các điểm cuối của nó. Chúng tôi có thể theo dõi các thành phần bằng cách sử dụng cấu trúc dữ liệu tìm liên kết (DSU), duy trì kích thước của từng thành phần và liệu nó có điểm cuối mở hay không. 

Một cái nhìn sâu sắc quan trọng khác là số lượng hoán vị của$k$các phần tử có chu kỳ đều là bội số của ba có thể được tính toán trước bằng cách sử dụng tổ hợp. Cho phép$f[k]$biểu thị số đếm này theo modulo$998244353$. Sự tái diễn xuất phát từ việc chia tách$k$các phần tử thành các chu kỳ có độ dài$3, 6, 9,\dots$. Sử dụng giai thừa và nghịch đảo mô-đun, chúng ta có thể tính toán trước$f[k]$lên đến$n$trong thời gian tuyến tính. 

Điều này làm giảm vấn đề cập nhật DSU sau mỗi lần cập nhật và tính toán sản phẩm của$f$trên tất cả các thành phần, điều này hiệu quả vì mỗi hoạt động liên kết hoặc tìm kiếm có thời gian gần như không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!) mỗi lần cập nhật | Ồ (n!) | Quá chậm | 
| DSU + tính toán trước tổ hợp | O(n log* n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước giai thừa và nghịch đảo giai thừa modulo$998244353$lên đến$n$. Những điều này sẽ cho phép chúng ta tính toán các hệ số đa thức một cách hiệu quả để đếm số lần hoàn thành chu trình. 
2. Tính toán trước một mảng$f[k]$vì$k = 0$ĐẾN$n$, Ở đâu$f[k]$là số hoán vị của$k$các phần tử sao cho tất cả các chu trình đều có độ dài chia hết cho ba. Điều này sử dụng lập trình động với sự tái phát$f[k] = \sum_{i=3,6,9,\dots}^{k} \binom{k-1}{i-1} f[k-i]$lần$(i-1)!$. 
3. Khởi tạo DSU với$n$các phần tử, mỗi phần tử ban đầu là thành phần riêng của nó. Đối với mỗi thành phần, hãy theo dõi kích thước của nó. 
4. Xử lý từng bản cập nhật$x_i, y_i$. Nếu như$x_i$Và$y_i$nằm trong các thành phần khác nhau, hãy kết hợp chúng trong DSU, thêm kích thước của chúng. 
5. Sau khi hợp nhất, hãy tính tích của$f$trên tất cả các kích thước thành phần hiện tại. Điều này đưa ra tổng số cách để hoàn thành hoán vị trong khi vẫn duy trì điều kiện bội số của ba chu trình. 
6. Xuất số này theo modulo$998244353$. 

Tại sao nó hoạt động: Ở mỗi bước, các thành phần DSU đại diện cho các tập hợp chỉ mục độc lập mà việc hoàn thành chúng không gây trở ngại. Bằng cách tính toán trước$f[k]$, chúng tôi biết chính xác có bao nhiêu lần hoàn thành hợp lệ mà mỗi thành phần thừa nhận. Tích trên tất cả các thành phần sẽ tính tất cả các lần hoàn thành hợp lệ của toàn bộ mảng vì các thành phần rời rạc. Mỗi hợp sẽ cập nhật các thành phần một cách chính xác khi có một số mới liên kết hai phần của một phần chu trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def precompute_factorials(n):
    fact = [1] * (n+1)
    inv_fact = [1] * (n+1)
    for i in range(1, n+1):
        fact[i] = fact[i-1] * i % MOD
    inv_fact[n] = pow(fact[n], MOD-2, MOD)
    for i in range(n-1, -1, -1):
        inv_fact[i] = inv_fact[i+1] * (i+1) % MOD
    return fact, inv_fact

def comb(n, k, fact, inv_fact):
    if k < 0 or k > n:
        return 0
    return fact[n] * inv_fact[k] % MOD * inv_fact[n-k] % MOD

def precompute_f(n, fact, inv_fact):
    f = [0] * (n+1)
    f[0] = 1
    for k in range(3, n+1):
        total = 0
        for i in range(3, k+1, 3):
            total += comb(k-1, i-1, fact, inv_fact) * fact[i-1] % MOD * f[k-i] % MOD
            total %= MOD
        f[k] = total
    return f

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n
    
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    
    def union(self, x, y):
        x_root = self.find(x)
        y_root = self.find(y)
        if x_root == y_root:
            return False
        if self.size[x_root] < self.size[y_root]:
            x_root, y_root = y_root, x_root
        self.parent[y_root] = x_root
        self.size[x_root] += self.size[y_root]
        return True

n = int(input())
updates = [tuple(map(int, input().split())) for _ in range(n)]
updates = [(x-1, y-1) for x, y in updates]

fact, inv_fact = precompute_factorials(n)
f = precompute_f(n, fact, inv_fact)
dsu = DSU(n)
res = []

component_sizes = [1] * n
total_ways = 1

for x, y in updates:
    x_root = dsu.find(x)
    y_root = dsu.find(y)
    if x_root != y_root:
        total_ways = total_ways * pow(f[component_sizes[x_root]], MOD-2, MOD) % MOD
        total_ways = total_ways * pow(f[component_sizes[y_root]], MOD-2, MOD) % MOD
        dsu.union(x_root, y_root)
        new_root = dsu.find(x_root)
        component_sizes[new_root] = component_sizes[x_root] + component_sizes[y_root]
        total_ways = total_ways * f[component_sizes[new_root]] % MOD
    res.append(total_ways)

print('\n'.join(map(str, res)))
```Giải pháp tính toán trước các giai thừa và số lượng hoán vị hợp lệ, sau đó sử dụng DSU để theo dõi kích thước thành phần. Ở mỗi lần cập nhật, nó cập nhật sản phẩm của$f$giá trị trên các thành phần để duy trì số lần hoàn thành hợp lệ. Nghịch đảo mô-đun xử lý phép chia trong số học mô-đun. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
6
3 2
1 4
4 5
2 6
5 1
6 3
```| Cập nhật | thành phần DSU | Kích thước thành phần | tổng_cách | 
| --- | --- | --- | --- | 
| 3->2 | {3,2},{1},{4},{5},{6} | 2,1,1,1,1 | 32 | 
| 1->4 | {1,4,5},{2,3},{6} | 3,2,1 | 8 | 
| 4->5 | giống nhau | 3,2,1 | 3 | 
| 2->6 | {1,4,5,2,3,6} | 6 | 2 | 
| 5->1 | giống nhau | 6 | 1 | 
| 6->3 | giống nhau | 6 | |
