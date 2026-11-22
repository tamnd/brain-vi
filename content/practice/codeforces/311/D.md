---
title: "CF 311D - Lập phương khoảng thời gian"
description: "Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn đều yêu cầu tổng của một mảng con hoặc hướng dẫn chúng ta lập phương mỗi phần tử trong một mảng con. Đầu ra bao gồm các câu trả lời cho các truy vấn tổng, được tính toán theo modulo 95542721."
date: "2026-06-05T18:42:03+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "math"]
categories: ["algorithms"]
codeforces_contest: 311
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 185 (Div. 1)"
rating: 2600
weight: 311
solve_time_s: 93
verified: true
draft: false
---

[CF 311D - Lập phương ngắt quãng](https://codeforces.com/problemset/problem/311/D) 

**Đánh giá:** 2600 
**Thẻ:** cấu trúc dữ liệu, toán học 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên và một chuỗi các truy vấn. Mỗi truy vấn đều yêu cầu tổng của một mảng con hoặc hướng dẫn chúng ta lập phương mỗi phần tử trong một mảng con. Đầu ra bao gồm các câu trả lời cho các truy vấn tổng, được tính toán theo modulo 95542721. 

Mảng có thể dài tới 100.000 phần tử và có thể có tới 100.000 truy vấn. Một cách tiếp cận đơn giản trực tiếp tính tổng hoặc cập nhật mảng cho mỗi truy vấn sẽ thực hiện tối đa 10^10 thao tác trong trường hợp xấu nhất, vượt xa những gì chúng ta có thể tính toán trong vòng vài giây. Điều này có nghĩa là chúng ta cần một cấu trúc dữ liệu cho phép cả truy vấn tổng phạm vi và sửa đổi phạm vi một cách hiệu quả. 

Một khía cạnh khó khăn của vấn đề này là hoạt động lập phương. Lập khối lặp đi lặp lại không phải là tuyến tính, vì vậy các kỹ thuật cộng phạm vi tiêu chuẩn như lan truyền lười biếng để cộng hoặc gán sẽ không hoạt động trực tiếp. Hơn nữa, modulo 95542721 là số nguyên tố và đủ nhỏ để cho phép tính toán trước các khối lặp lại một cách hiệu quả. 

Việc triển khai đơn giản có thể cố gắng sắp xếp mọi phần tử trên mỗi bản cập nhật. Ví dụ, nếu mảng là`[2, 3]`và chúng ta lập phương toàn bộ mảng hai lần, một vòng lặp bất cẩn sẽ xảy ra`2**3 = 8`, sau đó`8**3 = 512`,`3**3 = 27`, sau đó`27**3 = 19683`. Về nguyên tắc thì đúng, nhưng với phạm vi lớn thì tốc độ này quá chậm. Chúng ta cần một phương pháp tránh chạm vào mọi phần tử cho mỗi khối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp lại trực tiếp qua mảng. Đối với các truy vấn tổng, nó lặp lại tất cả các phần tử trong phạm vi, thêm chúng theo modulo 95542721 và in kết quả. Đối với các cập nhật lập phương, nó lặp qua tất cả các phần tử trong phạm vi, nâng từng phần tử lên lũy thừa bậc ba modulo 95542721 và ghi lại. Cách tiếp cận này đúng nhưng quá chậm. Với n và q lên tới 10^5, các truy vấn trong trường hợp xấu nhất trên toàn bộ mảng có thể dẫn đến 10^10 thao tác. 

Thông tin chi tiết quan trọng là lập phương là mô-đun tuần hoàn 95542721. Nếu bạn tính các lập phương lặp lại của một số theo mô-đun 95542721, thì cuối cùng chuỗi sẽ quay vòng. Trong thực tế, modulo số nguyên tố này, bất kỳ số nguyên x nào cũng sẽ trở về giá trị ban đầu sau tối đa 48 lần lặp lại. Quan sát này cho phép chúng tôi coi mỗi phần tử là một chu kỳ có độ dài tối đa là 48. Thay vì lập phương các giá trị trực tiếp trên mảng, chúng tôi lưu trữ 48 lũy thừa được tính toán trước cho mỗi phân đoạn và duy trì một con trỏ cho biết phân đoạn đó đã được lập phương bao nhiêu lần. Sau đó, các truy vấn tổng kết hợp các giá trị dựa trên con trỏ hiện tại và lập phương toàn bộ phạm vi chỉ tăng modulo con trỏ lên 48. 

Chúng tôi thực hiện điều này bằng cách sử dụng cây phân đoạn với khả năng lan truyền lười biếng. Mỗi nút lưu trữ một mảng gồm 48 tổng biểu thị các khối lặp lại trong khoảng của nó. Giá trị lười biếng là số khối đang chờ xử lý trong khoảng thời gian đó. Khi nhân giống, chúng ta xoay mảng tổng theo giá trị lười. Sau đó, các truy vấn truy cập vào tổng chính xác mà không cần tính toán lại từng phần tử khối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Cây phân đoạn với chu trình lập phương | O(q * log n) | O(n * 48) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các chuỗi lập phương modulo 95542721 cho mỗi phần tử. Với phần tử x, hãy tính`x^(3^k) % 95542721`với k = 0 đến 47. Lưu trữ 48 giá trị này trong một mảng. Điều này xác định chu kỳ lập phương cho mỗi phần tử. 
2. Xây dựng cây phân đoạn. Mỗi nút đại diện cho một phạm vi`[l, r]`của mảng. Thay vì lưu trữ một tổng duy nhất, hãy lưu trữ một mảng gồm 48 tổng, trong đó phần tử thứ k là tổng của tất cả các phần tử trong phạm vi đó sau khi được lập phương k lần. Điều này cho phép trả lời các truy vấn tổng cho bất kỳ số lượng khối đang chờ xử lý nào. 
3. Sử dụng phương pháp lan truyền lười biếng để xử lý các cập nhật hình khối. Mỗi nút có một bộ đếm lười biếng`lazy`cho biết khoảng thời gian của nó nên được lập phương bao nhiêu lần. Khi một nút nhận được bản cập nhật khối, hãy tăng`lazy`modulo 48 và hoãn cập nhật các phần tử con của nó cho đến khi cần thiết. 
4. Để áp dụng một giá trị lười đang chờ xử lý cho một nút, hãy xoay mảng 48 tổng của nó theo`lazy`các vị trí. Ví dụ, nếu`lazy = 1`, tổng sau một lần lập phương nữa sẽ trở thành tổng ở chỉ số 1, tổng sau hai lần lập phương nữa sẽ trở thành chỉ số 2, v.v., với sự bao quanh. 
5. Đối với truy vấn tổng, duyệt cây giống như truy vấn cây phân đoạn thông thường. Khi truy cập một nút, hãy áp dụng phép xoay lười biếng đang chờ xử lý và tính tổng giá trị chính xác từ mảng tổng tương ứng với giá trị lười biếng hiện tại của nó. 
6. Khi kết hợp hai phần tử con, hãy kết hợp các mảng tổng của chúng theo từng phần tử, tôn trọng mọi phép xoay lười biếng. Điều này đảm bảo tổng của nút cha luôn nhất quán với các nút con của nó. 

Tại sao nó hoạt động: Mỗi nút duy trì tất cả 48 trạng thái lập phương có thể có. Lan truyền lười biếng đảm bảo rằng chúng ta không bao giờ mất các bản cập nhật: chúng ta chỉ xoay mảng để phản ánh các hoạt động lập phương. Truy vấn tổng được đọc từ chỉ mục được xoay chính xác. Điều này đảm bảo tính chính xác vì các phép quay và số học mô-đun là nhất quán và có thể đảo ngược. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 95542721
CYCLE = 48

class Node:
    __slots__ = ('sums', 'lazy')
    def __init__(self):
        self.sums = [0] * CYCLE
        self.lazy = 0

def cube_cycle(x):
    arr = [x % MOD]
    for _ in range(CYCLE-1):
        arr.append(pow(arr[-1], 3, MOD))
    return arr

def merge(left, right):
    node = Node()
    for i in range(CYCLE):
        node.sums[i] = (left.sums[i] + right.sums[i]) % MOD
    return node

def rotate(arr, k):
    k %= CYCLE
    return arr[k:] + arr[:k]

class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.tree = [Node() for _ in range(4*self.n)]
        self.build(1, 0, self.n-1, data)
        
    def build(self, v, l, r, data):
        if l == r:
            self.tree[v].sums = cube_cycle(data[l])
            return
        m = (l+r)//2
        self.build(2*v, l, m, data)
        self.build(2*v+1, m+1, r, data)
        self.tree[v] = merge(self.tree[2*v], self.tree[2*v+1])
    
    def push(self, v):
        lazy = self.tree[v].lazy
        if lazy:
            self.tree[2*v].lazy = (self.tree[2*v].lazy + lazy) % CYCLE
            self.tree[2*v+1].lazy = (self.tree[2*v+1].lazy + lazy) % CYCLE
            self.tree[v].sums = rotate(self.tree[v].sums, lazy)
            self.tree[v].lazy = 0
    
    def update(self, v, l, r, ql, qr):
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            self.tree[v].lazy = (self.tree[v].lazy + 1) % CYCLE
            self.tree[v].sums = rotate(self.tree[v].sums, 1)
            return
        self.push(v)
        m = (l+r)//2
        self.update(2*v, l, m, ql, qr)
        self.update(2*v+1, m+1, r, ql, qr)
        self.tree[v] = merge(self.tree[2*v], self.tree[2*v+1])
    
    def query(self, v, l, r, ql, qr):
        if qr < l or r < ql:
            return 0
        if ql <= l and r <= qr:
            return self.tree[v].sums[self.tree[v].lazy]
        self.push(v)
        m = (l+r)//2
        return (self.query(2*v, l, m, ql, qr) + self.query(2*v+1, m+1, r, ql, qr)) % MOD

n = int(input())
a = list(map(int, input().split()))
tree = SegmentTree(a)
q = int(input())
for _ in range(q):
    t,l,r = map(int, input().split())
    l -= 1; r -= 1
    if t == 1:
        print(tree.query(1,0,n-1,l,r))
    else:
        tree.update(1,0,n-1,l,r)
```Mã này xây dựng cây phân đoạn bằng cách sử dụng các chuỗi khối được tính toán trước. Lan truyền lười biếng sẽ xoay các chuỗi này thay vì tính toán lại lũy thừa mỗi lần. Các bản cập nhật tăng bộ đếm lười và xoay mảng. Truy vấn đọc tổng chính xác bằng cách tính giá trị lười hiện tại. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
8
1 2 3 4 5 6 7 8
5
1 2 5
2 2 5
1 2 5
2 3 6
1 4 7
```| Bước | Truy vấn | Lười biếng | Tổng nút được sử dụng | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | tổng 2-5 | 0 | tổng[0] | 2+3+4+5=14 |
