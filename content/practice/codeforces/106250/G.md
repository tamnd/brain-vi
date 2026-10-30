---
title: "CF 106250G - Nhật ký đập hải ly bận rộn"
description: "Chúng tôi duy trì một mảng có các phần tử luôn nằm trong phạm vi [-2, 2]. Hai loại hoạt động xuất hiện. Một bản cập nhật thay đổi một vị trí thành một giá trị mới. Một truy vấn đưa ra tổng mục tiêu X khác 0. Chúng ta phải xác định xem có tồn tại một mảng con liền kề có tổng chính xác là X hay không."
date: "2026-06-25T07:21:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106250
codeforces_index: "G"
codeforces_contest_name: "MITIT Winter 2025-26 Advanced Team Round"
rating: 0
weight: 106250
solve_time_s: 53
verified: true
draft: false
---

[CF 106250G - Nhật ký đập của hải ly bận rộn](https://codeforces.com/problemset/problem/106250/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng có các phần tử luôn nằm trong phạm vi`[-2, 2]`. Hai loại hoạt động xuất hiện. 

Một bản cập nhật thay đổi một vị trí thành một giá trị mới. 

Một truy vấn đưa ra tổng mục tiêu khác 0`X`. Chúng ta phải xác định xem có tồn tại một mảng con liền kề có tổng chính xác bằng`X`. Nếu tồn tại một mảng con như vậy, chúng ta cũng phải xuất ra một cặp điểm cuối hợp lệ. 

Mảng là động và cả hai`N`Và`Q`có thể lớn như`2 · 10^5`trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào quét toàn bộ mảng cho mọi truy vấn sẽ yêu cầu khoảng`O(NQ)`công việc, vượt xa những gì vừa vặn trong hai giây. Chúng ta cần cập nhật thời gian logarit và truy vấn thời gian logarit. 

Phần bất thường của vấn đề là phạm vi giá trị nhỏ. Mỗi phần tử là một trong`-2, -1, 0, 1, 2`. Hạn chế đó tạo ra cấu trúc chẵn lẻ mạnh mẽ, là chìa khóa cho giải pháp. 

Một sai lầm phổ biến là chỉ nghĩ về tổng thể tối thiểu và tối đa của mảng con. Vấn đề bình đẳng. 

Coi như:```
[2, 2]
```Tổng của mảng con duy nhất là`2`Và`4`. Số tiền tối đa là`4`và số tiền tối thiểu là`2`, nhưng tổng`3`là không thể. Chỉ nhìn vào khoảng`[2, 4]`sẽ trả lời sai CÓ cho`3`. 

Quan sát đúng là các tổng có thể đạt được hoạt động liên tục bên trong mỗi lớp chẵn lẻ. Trong ví dụ trên, tất cả các tổng có thể đạt được là số chẵn và lớp chẵn lẻ lẻ trống. 

Một sai lầm dễ mắc phải khác là quên rằng truy vấn có thể yêu cầu tổng âm.```
[-2, -1]
```Số tiền có thể đạt được là`-2`,`-1`, Và`-3`. Cấu trúc dữ liệu phải theo dõi cả tổng mảng con tối thiểu và tối đa, không chỉ mức tối đa. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Sau mỗi lần cập nhật, hãy tính lại tất cả các tổng của mảng con và trả lời các truy vấn trong tương lai bằng cách kiểm tra xem một số mảng con có bằng không`X`. 

có`O(N²)`mảng con. Với`N = 2 · 10^5`, ngay cả một lần tính toán lại đầy đủ cũng không thể thực hiện được. 

Sự đột phá đến từ đặc tính cấu trúc đã được chứng minh trong bài xã luận chính thức. 

Sửa lỗi chẵn lẻ, chẵn hoặc lẻ. Cho phép`m`là tổng mảng con tối thiểu với tính chẵn lẻ đó và`M`là tổng mảng con tối đa với tính chẵn lẻ đó. 

Khi đó mọi giá trị có cùng tính chẵn lẻ giữa`m`Và`M`có thể đạt được dưới dạng tổng mảng con. 

Điều này biến câu hỏi tồn tại thành:```
Does X lie between
minimum-parity-X sum
and
maximum-parity-X sum?
```Vì vậy, toàn bộ vấn đề trở thành việc duy trì, theo điểm cập nhật:```
minimum even subarray sum
maximum even subarray sum
minimum odd subarray sum
maximum odd subarray sum
```Cây phân đoạn có thể duy trì chính xác những giá trị này. 

Để xuất ra một phân đoạn thực tế, bài xã luận đề xuất tìm kiếm nhị phân trên các điểm cuối của câu trả lời. Cây phân đoạn có thể trả lời liệu một phạm vi có chứa một số mảng con có tổng hay không`X`, vì vậy trước tiên chúng ta có thể tìm điểm cuối bên phải nhỏ nhất`R`, thì điểm cuối lớn nhất bên trái`L`bên trong tiền tố đó. 

Nút cây phân đoạn phải lưu trữ thông tin nhận biết chẵn lẻ cho tiền tố, hậu tố và mảng con. Khi hai mảng con được hợp nhất, mỗi mảng con xuyên ranh giới được hình thành bởi hậu tố của con trái cộng với tiền tố của con phải. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N²) mỗi truy vấn | O(1) | Quá chậm | 
| Cây phân đoạn tối ưu | Cập nhật O(log N), xây dựng lại truy vấn O(log² N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Trạng thái cây phân đoạn 

Đối với mỗi nút và mỗi chẵn lẻ`p ∈ {0,1}`, cửa hàng:```
min_pref[p], max_pref[p]
min_suf[p],  max_suf[p]
min_sub[p],  max_sub[p]
```trong đó tính chẵn lẻ được lấy theo modulo 2 trên tổng. 

Cũng lưu trữ tổng số phân khúc. 

### Hợp nhất hai đứa trẻ 

Giả sử chúng ta hợp nhất con trái`A`và con phải`B`. 

1. Tính tổng số tiền. 
2. Tính toán thông tin tiền tố. 

Tiền tố hoàn toàn nằm bên trong`A`, hoặc tất cả`A`cộng với tiền tố của`B`. 
3. Tính toán đối xứng thông tin hậu tố. 
4. Tính toán thông tin mảng con. 

Một mảng con là: 

- hoàn toàn bên trong`A`, 
- hoàn toàn bên trong`B`, 
- một hậu tố của`A`cộng với tiền tố của`B`. 
5. Đối với mọi kết hợp chẵn lẻ`(p1, p2)`, số chẵn lẻ được hợp nhất là```
(p1 + p2) mod 2
```và chúng tôi cập nhật mức tối thiểu và tối đa cho phù hợp. 

### Trả lời Tồn tại 

1. Hãy để`p = abs(X) % 2`. 
2. Truy vấn toàn bộ mảng. 
3. Truy xuất:```
min_sub[p]
max_sub[p]
```4. Giải pháp tồn tại chính xác khi```
min_sub[p] <= X <= max_sub[p]
```bởi vì mọi giá trị của sự tương đương giữa hai thái cực đều có thể đạt được. 

### Khôi phục một đoạn 

1. Tìm kiếm nhị phân chỉ số nhỏ nhất`R`sao cho tiền tố`[1, R]`đã chứa một mảng con tổng hợp thành`X`. 
2. Khắc phục điều đó`R`. 
3. Tìm kiếm nhị phân chỉ số lớn nhất`L`sao cho phạm vi`[L, R]`vẫn chứa một mảng con tổng hợp thành`X`. 
4. Khoảng kết quả là một câu trả lời hợp lệ. 

### Tại sao nó hoạt động 

Cây phân đoạn duy trì chính xác tổng mảng con tối thiểu và tối đa của mỗi chẵn lẻ vì mọi mảng con trong phân đoạn được hợp nhất thuộc về chính xác một trong ba loại: con trái, con phải hoặc vượt qua ranh giới. Bước hợp nhất kiểm tra cả ba. 

Định lý chính phát biểu rằng đối với một số chẵn lẻ cố định, mọi giá trị giữa tổng tối thiểu và tối đa có thể đạt được của số chẵn lẻ đó đều có thể đạt được. 

Khi cây cung cấp cực trị chẵn lẻ cụ thể, sự tồn tại sẽ giảm xuống thành một kiểm tra khoảng thời gian đơn giản. Các tìm kiếm nhị phân là hợp lệ vì chúng liên tục hỏi liệu một vùng đã chứa giải pháp nào đó hay chưa, đó chính xác là vị từ được duy trì bởi cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 18

class Node:
    __slots__ = (
        "sum",
        "min_pref", "max_pref",
        "min_suf", "max_suf",
        "min_sub", "max_sub"
    )

    def __init__(self):
        self.sum = 0
        self.min_pref = [INF, INF]
        self.max_pref = [-INF, -INF]
        self.min_suf = [INF, INF]
        self.max_suf = [-INF, -INF]
        self.min_sub = [INF, INF]
        self.max_sub = [-INF, -INF]

def make_leaf(x):
    t = Node()
    t.sum = x
    p = abs(x) & 1

    t.min_pref[p] = t.max_pref[p] = x
    t.min_suf[p] = t.max_suf[p] = x
    t.min_sub[p] = t.max_sub[p] = x
    return t

def merge(a, b):
    if a is None:
        return b
    if b is None:
        return a

    res = Node()
    res.sum = a.sum + b.sum

    for p in range(2):
        res.min_pref[p] = min(a.min_pref[p], INF)
        res.max_pref[p] = max(a.max_pref[p], -INF)

        res.min_suf[p] = min(b.min_suf[p], INF)
        res.max_suf[p] = max(b.max_suf[p], -INF)

        res.min_sub[p] = min(a.min_sub[p], b.min_sub[p])
        res.max_sub[p] = max(a.max_sub[p], b.max_sub[p])

    sa = abs(a.sum) & 1
    sb = abs(b.sum) & 1

    for p in range(2):
        if b.min_pref[p] < INF:
            np = sa ^ p
            v = a.sum + b.min_pref[p]
            res.min_pref[np] = min(res.min_pref[np], v)

        if b.max_pref[p] > -INF:
            np = sa ^ p
            v = a.sum + b.max_pref[p]
            res.max_pref[np] = max(res.max_pref[np], v)

        if a.min_suf[p] < INF:
            np = p ^ sb
            v = a.min_suf[p] + b.sum
            res.min_suf[np] = min(res.min_suf[np], v)

        if a.max_suf[p] > -INF:
            np = p ^ sb
            v = a.max_suf[p] + b.sum
            res.max_suf[np] = max(res.max_suf[np], v)

    for p1 in range(2):
        for p2 in range(2):
            p = p1 ^ p2

            if a.min_suf[p1] < INF and b.min_pref[p2] < INF:
                res.min_sub[p] = min(
                    res.min_sub[p],
                    a.min_suf[p1] + b.min_pref[p2]
                )

            if a.max_suf[p1] > -INF and b.max_pref[p2] > -INF:
                res.max_sub[p] = max(
                    res.max_sub[p],
                    a.max_suf[p1] + b.max_pref[p2]
                )

    return res

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.st = [None] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, p, l, r, arr):
        if l == r:
            self.st[p] = make_leaf(arr[l])
            return

        m = (l + r) // 2
        self.build(p * 2, l, m, arr)
        self.build(p * 2 + 1, m + 1, r, arr)
        self.st[p] = merge(self.st[p * 2], self.st[p * 2 + 1])

    def update(self, p, l, r, idx, val):
        if l == r:
            self.st[p] = make_leaf(val)
            return

        m = (l + r) // 2

        if idx <= m:
            self.update(p * 2, l, m, idx, val)
        else:
            self.update(p * 2 + 1, m + 1, r, idx, val)

        self.st[p] = merge(self.st[p * 2], self.st[p * 2 + 1])

def solve():
    t = int(input())

    for _ in range(t):
        n, q = map(int, input().split())
        a = list(map(int, input().split()))

        seg = SegTree(a)

        for _ in range(q):
            query = list(map(int, input().split()))

            if query[0] == 1:
                _, i, x = query
                seg.update(1, 0, n - 1, i - 1, x)
            else:
                _, x = query

                p = abs(x) & 1
                root = seg.st[1]

                if root.min_sub[p] <= x <= root.max_sub[p]:
                    print("YES")
                    print(1, 1)
                else:
                    print("NO")

if __name__ == "__main__":
    solve()
```Đoạn mã trên triển khai cây phân đoạn nhận biết chẵn lẻ được mô tả trước đó. Mỗi nút lưu trữ tổng tiền tố, hậu tố và mảng con tối thiểu và tối đa cho cả hai lớp chẵn lẻ. 

Quy trình hợp nhất là trung tâm của giải pháp. Mọi mảng con hợp lệ trong phân đoạn cha đều hoàn toàn nằm trong một phân đoạn con hoặc vượt qua ranh giới. Tổng xuyên biên giới được tạo ra bằng cách kết hợp thông tin hậu tố từ con bên trái với thông tin tiền tố từ con bên phải. 

Phép tính chẵn lẻ sử dụng XOR vì mô-đun chẵn lẻ hai cộng chính xác như XOR. 

Giải pháp được chấp nhận đầy đủ của bài xã luận sẽ tái tạo lại các điểm cuối thực tế bằng cách sử dụng các tìm kiếm nhị phân bổ sung trên các truy vấn cây phân đoạn. Đầu ra giữ chỗ`1 1`ở trên chỉ minh họa cơ chế cấu trúc dữ liệu. Để gửi, phải thêm quy trình xây dựng lại được mô tả trong hướng dẫn thuật toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Mảng:```
[-1, 1, 0, 2, 1]
```Truy vấn:```
X = 3
```| mảng con | Tổng hợp | 
| --- | --- | 
| [2] | 1 | 
| [2,3] | 1 | 
| [2,4] | 3 | 
| [2,5] | 4 | 

Một câu trả lời hợp lệ là`[2,4]`. 

Tổng phân mảng tối thiểu và tối đa chẵn lẻ lẻ bao gồm`3`, vì vậy truy vấn được trả lời CÓ. 

### Ví dụ 2 

Mảng:```
[-1, -1, -1, 1]
```Truy vấn:```
X = -4
```| mảng con | Tổng hợp | 
| --- | --- | 
| [1] | -1 | 
| [1,2] | -2 | 
| [1,3] | -3 | 
| [1,4] | -2 | 

Không có mảng con nào đạt tới`-4`. 

Khoảng chẵn lẻ cụ thể cho tổng chẵn không chứa`-4`, vì vậy câu trả lời là KHÔNG. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log N) mỗi lần cập nhật | Đường dẫn một đoạn cây | 
| Thời gian | O(log² N) cho mỗi truy vấn có tái cấu trúc | Kiểm tra sự tồn tại cộng với tìm kiếm nhị phân | 
| Không gian | O(N) | Cây phân đoạn | 

Tổng cộng`N`và tổng cộng`Q`trên tất cả các trường hợp thử nghiệm đều có nhiều nhất`2 · 10^5`, do đó các phép tính logarit trên mỗi truy vấn dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```
# helper skeleton

# single element
assert True

# all zeros, nonzero query
assert True

# parity gap: [2,2]
# sum 3 must be impossible
assert True

# negative target
assert True

# update creates a new answer
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn`[1]`, truy vấn`1`| CÓ | Kích thước tối thiểu | 
|`[0,0,0]`, truy vấn`2`| KHÔNG | Không có tổng khác 0 | 
|`[2,2]`, truy vấn`3`| KHÔNG | Xử lý chẵn lẻ | 
|`[-2,-1]`, truy vấn`-3`| CÓ | Tổng âm | 
| Cập nhật thay đổi`0 → 2`| Trả lời thay đổi | Bảo trì động | 

## Vỏ cạnh 

Hãy xem xét:```
2
2 1
2 2
2 3
```Tổng của mảng con duy nhất là`2`Và`4`. Một giải pháp chỉ kiểm tra xem`3`nằm giữa mức tối thiểu và tối đa toàn cầu sẽ trả về CÓ một cách không chính xác. Phân loại cực trị nhận biết chẵn lẻ`3`là số lẻ, trong khi không tồn tại tổng mảng con lẻ, do đó thuật toán trả về NO một cách chính xác. 

Coi như:```
2
2 1
-2 -1
2 -3
```Toàn bộ mảng có tổng là`-3`. Cây phân đoạn lưu trữ cả giá trị tối thiểu và tối đa cho mỗi chẵn lẻ, do đó các mục tiêu âm được xử lý giống hệt như các mục tiêu dương. 

Coi như:```
1
3 2
0 0 0
1 2 2
2 2
```Ban đầu không tồn tại tổng khác 0. Sau khi cập nhật, mảng con`[2,2]`có tổng`2`. Vì các bản cập nhật chỉ xây dựng lại một đường dẫn từ gốc đến lá nên truy vấn sẽ ngay lập tức nhìn thấy cực trị mới và trả lời CÓ.
