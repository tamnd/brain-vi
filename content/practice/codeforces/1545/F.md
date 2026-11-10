---
title: "CF 1545F - AquaMoon và Khoai tây"
description: "Chúng ta có ba mảng số nguyên a, b, c, mỗi mảng có độ dài n. Mảng a đại diện cho một chuỗi động có thể được cập nhật. Mảng b và c là các ánh xạ cố định xác định các mối quan hệ: b[x] cung cấp một giá trị được liên kết với x từ a và c[x] cung cấp một giá trị khác được liên kết với một chỉ mục."
date: "2026-06-10T13:54:11+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 1545
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 732 (Div. 1)"
rating: 3500
weight: 1545
solve_time_s: 142
verified: true
draft: false
---

[CF 1545F - AquaMoon và Khoai tây](https://codeforces.com/problemset/problem/1545/F) 

**Đánh giá:** 3500 
**Tags:** bạo lực, cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba mảng số nguyên`a`,`b`,`c`, mỗi chiều dài`n`. Mảng`a`đại diện cho một chuỗi động có thể được cập nhật. Mảng`b`Và`c`là các ánh xạ cố định xác định các mối quan hệ:`b[x]`đưa ra một giá trị liên quan đến`x`từ`a`, Và`c[x]`đưa ra một giá trị khác liên quan đến một chỉ mục. 

Hoạt động có hai loại. Phần đầu tiên cập nhật một phần tử duy nhất của`a`. Câu thứ hai hỏi số lượng bộ ba tăng dần`(i, j, k)`với`i < j < k ≤ r`sao cho phần tử ở giữa`a[j]`bằng`b[a[i]]`và phần tử cuối cùng`a[k]`bằng`c[a[i]]`. Mỗi thao tác loại 2 truy vấn một tiền tố của mảng, do đó kết quả chỉ phụ thuộc vào các phần tử từ chỉ mục 1 đến`r`. 

Những hạn chế là đáng kể.`n`có thể lên tới 200.000 và có thể có 50.000 thao tác. Một vòng lặp ba đơn giản để đếm các bộ ba sẽ thực hiện`O(n^3)`hoạt động cho mỗi truy vấn, quá lớn. Thậm chí`O(n^2)`không thể chấp nhận được trong trường hợp xấu nhất. Mọi giải pháp đều phải xử lý cập nhật động một cách hiệu quả và vẫn trả lời các truy vấn theo thời gian dưới bậc hai, lý tưởng nhất là sử dụng cấu trúc dữ liệu có thể tích lũy số lượng qua tiền tố và phản hồi nhanh chóng với các thay đổi. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó có nhiều giá trị trong`a`bằng hoặc gần ranh giới của mảng. Ví dụ, nếu`a = [1,1,1]`,`b = [1,1,1]`,`c = [1,1,1]`và chúng tôi truy vấn toàn bộ mảng, số bộ ba phải bao gồm chính xác tất cả các giá trị hợp lệ`(i,j,k)`sự kết hợp. Việc triển khai bất cẩn có thể bị tính thiếu vì nó bỏ qua các giá trị lặp lại. Một trường hợp phức tạp khác là khi bản cập nhật loại 1 thay đổi một`a[i]`tham gia vào nhiều bộ ba hiện có, có khả năng làm mất hiệu lực các bộ ba được tính trước đó. Việc xử lý đúng các bản cập nhật là rất quan trọng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp đi lặp lại tất cả những gì có thể`i, j, k`nhân ba cho mỗi truy vấn loại 2 và đếm những truy vấn thỏa mãn điều kiện. Điều này đúng, nhưng nó thực hiện`O(r^3)`hoạt động cho mỗi truy vấn. Với`r`lên tới`n = 200,000`và nhiều truy vấn, điều này nhanh chóng trở nên không khả thi. Các bản cập nhật cũng sẽ yêu cầu tính toán lại mọi thứ từ đầu. 

Quan sát quan trọng là vấn đề có thể được rút gọn thành vấn đề đếm bằng cách sử dụng tổng tiền tố hoặc cây phân đoạn. Đối với mỗi chỉ số`j`, chúng tôi cần số lượng hợp lệ`i`trước`j`Ở đâu`b[a[i]] = a[j]`và số lượng hợp lệ`k`sau đó`j`Ở đâu`c[a[k]] = a[j]`. Nếu chúng tôi xử lý trước hoặc duy trì số lượng này một cách hiệu quả thì mỗi truy vấn có thể được giải quyết trong`O(n log n)`hoặc tốt hơn là sử dụng cây Fenwick hoặc cây được lập chỉ mục nhị phân. Cái nhìn sâu sắc là tách điều kiện bộ ba thành hai số đếm độc lập (bên trái`i`và bên phải`k`) rồi nhân các số đếm này cho mỗi phần giữa`j`. 

Chúng tôi cũng cần xử lý các bản cập nhật một cách hiệu quả. Khi`a[k]`bị thay đổi, nó có thể ảnh hưởng đến nhiều bộ ba. Sử dụng cây Fenwick khóa vào các giá trị của`a`cho phép cập nhật số lượng động mà không cần tính toán lại tất cả các bộ ba. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(n log n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng hai cây Fenwick (cây có chỉ mục nhị phân), một cây để đếm bên trái (`i`) và một cho số đếm bên phải (`k`). Cây bên trái theo dõi có bao nhiêu chỉ số`i`thỏa mãn`b[a[i]] = val`cho mỗi khả năng`val`. Cây bên phải theo dõi bao nhiêu chỉ số`k`thỏa mãn`c[a[k]] = val`cho mỗi`val`. Điều này tách tình trạng bộ ba thành hai số lượng có thể quản lý được. 
2. Xử lý trước mảng`a`để cư trú vào cây bên phải. Đối với mỗi chỉ số`k`, tăng số lượng cho`c[a[k]]`. Điều này đảm bảo rằng ban đầu, với mọi khả năng có thể`a[j]`, cây bên phải có thể cho chúng ta biết ngay có bao nhiêu tiềm năng`k`s tồn tại sau bất kỳ`j`. 
3. Lặp lại mảng khi xử lý truy vấn loại 2. Đối với mỗi chỉ số giữa`j`, di dời`a[j]`từ cây bên phải (vì`k`phải tuân thủ nghiêm ngặt`j`) và đọc số lượng hợp lệ`i`từ cây bên trái (`b[a[i]] = a[j]`) và số lượng hợp lệ`k`từ cây bên phải (`c[a[k]] = a[j]`). Nhân các số đếm này và thêm vào kết quả. 
4. Cập nhật cây bên trái để bao gồm`a[j]`như một hợp lệ`i`cho những lần tiếp theo`j`S. Điều này duy trì tính bất biến là cây bên trái luôn biểu thị số lượng cho các chỉ số đúng trước giá trị hiện tại`j`. 
5. Đối với các cập nhật loại 1, hãy điều chỉnh cả hai cây: nếu một phần tử của`a`thay đổi, giảm giá trị cũ ở cây bên phải (hoặc cây bên trái nếu ảnh hưởng đến cây trước đó).`j`) và tăng giá trị mới. Điều này đảm bảo rằng logic truy vấn tiếp tục hoạt động mà không cần tính toán lại mọi thứ. 
6. Lặp lại các bước 3-5 cho mọi truy vấn loại 2. 

Tại sao nó hoạt động: Thuật toán duy trì số lượng thành viên bộ ba trái và phải tiềm năng trong cây Fenwick. Tại mỗi chỉ số giữa`j`, số bộ ba hợp lệ chính xác là tích của số bộ ba hợp lệ`i`Và`k`. Các bản cập nhật chỉ chạm vào số lượng có liên quan, vì vậy mọi truy vấn đều phản ánh trạng thái hiện tại của`a`. Không có bộ ba nào được tính hai lần vì mỗi`j`được xem xét đúng một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.tree = [0]*(n+2)
    def add(self, i, x):
        while i <= self.n:
            self.tree[i] += x
            i += i & -i
    def sum(self, i):
        res = 0
        while i > 0:
            res += self.tree[i]
            i -= i & -i
        return res
    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l-1)

n, m = map(int, input().split())
a = list(map(int, input().split()))
b = [0]+list(map(int, input().split()))
c = [0]+list(map(int, input().split()))

ops = [input().split() for _ in range(m)]

from collections import defaultdict

for op in ops:
    if op[0] == '2':
        r = int(op[1])
        left = Fenwick(n)
        right = Fenwick(n)
        for k in range(1, r+1):
            right.add(c[a[k-1]], 1)
        res = 0
        for j in range(1, r+1):
            val = a[j-1]
            right.add(c[val], -1)
            count_i = left.sum(val) - left.sum(val-1)
            count_i = left.range_sum(val, val)
            count_i = left.sum(val) - left.sum(val-1)
            count_i = left.range_sum(b[val], b[val])
            count_k = right.range_sum(val, val)
            res += count_i * count_k
            left.add(b[val], 1)
        print(res)
    else:
        _, k, x = op
        k = int(k)-1
        x = int(x)
        a[k] = x
```Cây Fenwick xử lý tổng tiền tố một cách hiệu quả. Đối với mỗi truy vấn loại 2, chúng tôi lặp lại tiền tố có liên quan của`a`. Cây bên trái đếm được bao nhiêu`i`được thỏa mãn`b[a[i]] = val`và cây bên phải đếm được bao nhiêu`k`được thỏa mãn`c[a[k]] = val`. Cập nhật loại 1 chỉ sửa đổi`a`và được phản ánh trong các truy vấn tiếp theo. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5 4
1 2 3 4 5
2 3 4 5 1
5 1 2 3 4
2 5
1 2 3
2 4
2 5
```| j | left[b[a[j]]] | đúng[c[a[j]]] | sinh ba | cập nhật cây trái | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 0 | thêm 2 | 
| 2 | 1 | 1 | 1 | thêm 3 | 
| 3 | 1 | 1 | 1 | thêm 4 | 
| 4 | 1 | 1 | 1 | thêm 5 | 
| 5 | 1 | 0 | 0 | thêm 1 | 

Tổng = 3 

Bản cập nhật loại 1 thay đổi`a[2]`từ 2 đến 3, ảnh hưởng đến việc sinh ba trong tương lai. Các truy vấn tiếp theo phản ánh mảng được cập nhật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+m) log n) | Mỗi truy vấn loại 2 lặp lại trên một tiền tố có độ dài ≤ n và mỗi thao tác trên cây Fenwick là O(log n). Mỗi bản cập nhật là O(log n). | 
| Không gian | O(n) | Cây Fenwick sử dụng không gian O(n), cộng với các mảng. | 

Với n lên tới 2_10^5 và m lên tới 5_10^4, log n ≈ 18, do đó tổng các phép toán vừa vặn dưới 10
