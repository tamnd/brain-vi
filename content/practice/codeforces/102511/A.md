---
title: "CF 102511A - Azulejos"
description: "Chúng tôi có hai bộ sưu tập gạch. Mỗi ô đều có giá, chiều cao và chỉ số gốc. Chúng ta phải sắp xếp lại hàng sau và hàng trước một cách độc lập. Giá ở mỗi hàng phải không giảm từ trái sang phải."
date: "2026-08-06T19:35:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102511
codeforces_index: "A"
codeforces_contest_name: "2019 ICPC World Finals"
rating: 0
weight: 102511
solve_time_s: 131
verified: true
draft: false
---

[CF 102511A - Azulejos](https://codeforces.com/problemset/problem/102511/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai bộ sưu tập gạch. Mỗi ô đều có giá, chiều cao và chỉ số gốc. Chúng ta phải sắp xếp lại hàng sau và hàng trước một cách độc lập. Giá ở mỗi hàng phải không giảm từ trái sang phải. Ở mọi vị trí, ô phía sau phải cao hơn ô phía trước. 

Sự tự do duy nhất đến từ những viên gạch có giá ngang nhau. Các ô có giá khác nhau sẽ có thứ tự tương đối cố định sau khi sắp xếp. Thách thức là sử dụng quyền tự do trong các nhóm giá bằng nhau để làm cho mọi cặp dọc đều hợp lệ. 

Giá trị của n có thể đạt tới 500000, do đó, bất kỳ giải pháp nào thử sắp xếp lại nhiều lần hoặc kiểm tra nhiều cặp đều không thể thực hiện được. Chúng ta cần một phương pháp tham lam với công việc gần như tuyến tính hoặc n log n. Việc sắp xếp đã tốn n log n, điều này có thể chấp nhận được. 

Những trường hợp phức tạp là do giá cả ngang nhau. Giải pháp đơn giản là sắp xếp theo giá và sau đó so sánh chiều cao sẽ không thành công vì các ô có giá bằng nhau có thể được sắp xếp lại. Giải pháp sắp xếp độ cao một cách độc lập cũng thất bại vì các nhóm giá bằng nhau ở hai hàng trùng nhau theo những cách phức tạp. 

Ví dụ:```
1
5
5
3
```Một cặp có tác dụng vì 5 > 3. 

Một trường hợp tế nhị hơn là:```
2
1 2
1 1
10 4
9 8
```Giá trở lại đã được sắp xếp. Hàng trước có giá bằng nhau nên các ô phía trước có thể đổi chỗ. Việc sắp xếp chính xác các cặp chiều cao 10 với 9 và chiều cao 4 với 8 là không thể, nhưng việc hoán đổi các ô phía trước sẽ cho ra 10 với 8 và 4 với 9, điều này vẫn là không thể. Việc thực hiện bất cẩn chỉ kiểm tra một thứ tự tùy ý có giá bằng nhau có thể chấp nhận hoặc từ chối những trường hợp đó một cách không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ thử các đơn đặt hàng khác nhau trong mỗi nhóm giá bằng nhau và kiểm tra các hàng kết quả. Điều này đúng vì nó khám phá mọi cách sắp xếp được phép, nhưng số lượng hoán vị có thể tăng lên theo giai thừa. Ngay cả một nhóm lớn cỡ 20 cũng đã tạo ra hơn 2 nghìn tỷ khả năng, vì vậy cách tiếp cận này không thể sử dụng được. 

Quan sát quan trọng là các nhóm giá bằng nhau hoạt động giống như các nhóm độc lập. Giả sử nhóm chưa hoàn thành tiếp theo ở hàng trước có kích thước a và nhóm chưa hoàn thành tiếp theo ở hàng sau có kích thước b. Nếu nhóm trước về đích trước, tất cả các ô của nhóm đó phải khớp với các ô của nhóm phía sau. Chúng ta phải luôn lấy ô phía sau ngắn nhất có thể nhưng vẫn cao hơn ô phía trước đã chọn. Điều này khiến các ô phía sau cao nhất không được sử dụng, đây là tài nguyên tốt nhất có thể cho các vị trí sau này. 

Nếu nhóm phía sau về đích trước, lập luận tương tự sẽ được áp dụng với các vai trò bị đảo ngược. Chúng tôi sử dụng các ô phía trước ngắn nhất có thể cho mỗi ô phía sau. 

Sự lựa chọn tham lam này có hiệu quả vì việc giữ những ô phía sau lớn hơn cho tương lai không bao giờ có hại, trong khi việc sử dụng một ô lớn hơn bây giờ có thể khiến một ô phía trước cao hơn trong tương lai không thể che được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số hoán vị) | O(n) | Quá chậm | 
| Tham lam với nhiều món đặt hàng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp cả hai hàng theo giá. Các ô có giá bằng nhau được giữ cùng nhau thành nhóm và mỗi nhóm lưu trữ các ô của mình theo thứ tự chiều cao. 
2. Giữ nguyên nhóm giá chưa hoàn thành hiện tại của từng hàng. Các nhóm được xử lý từ trái sang phải vì giá không thể di chuyển qua các giá trị khác nhau. 
3. Nếu nhóm phía trước có ít hơn hoặc bằng nhóm phía sau, hãy liên tục ghép từng ô phía trước với ô phía sau nhỏ nhất có sẵn và cao hơn. Loại bỏ hai ô đó và đặt chúng vào vị trí câu trả lời hiện tại. 
4. Nếu nhóm phía sau nhỏ hơn, hãy thực hiện thao tác đối xứng. Ghép từng ô phía sau với ô phía trước nhỏ nhất có sẵn mà nó có thể che được. 
5. Khi một nhóm trống, hãy chuyển sang nhóm giá tiếp theo trong hàng đó. Tiếp tục cho đến khi tất cả các vị trí được lấp đầy. 

Điều bất biến là sau khi xử lý bất kỳ tiền tố vị trí nào, tất cả các cặp được tạo ra đều hợp lệ và mọi ô còn lại vẫn thuộc về một hậu tố của thứ tự giá đã sắp xếp. Việc loại bỏ tham lam sẽ duy trì tính khả thi vì nó luôn tiêu tốn ô kém linh hoạt nhất có thể từ phía phải cung cấp trận đấu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = ("key", "prio", "left", "right", "cnt", "val")
    def __init__(self, key, val):
        self.key = key
        self.val = val
        self.prio = (key[0] * 1000003 + key[1]) & 0x7fffffff
        self.left = None
        self.right = None
        self.cnt = 1

def rotate_right(y):
    x = y.left
    y.left = x.right
    x.right = y
    return x

def rotate_left(x):
    y = x.right
    x.right = y.left
    y.left = x
    return y

def insert(t, node):
    if t is None:
        return node
    if node.key < t.key:
        t.left = insert(t.left, node)
        if t.left.prio < t.prio:
            t = rotate_right(t)
    else:
        t.right = insert(t.right, node)
        if t.right.prio < t.prio:
            t = rotate_left(t)
    return t

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    if a.prio < b.prio:
        a.right = merge(a.right, b)
        return a
    b.left = merge(a, b.left)
    return b

def erase(t, key):
    if t.key == key:
        return merge(t.left, t.right)
    if key < t.key:
        t.left = erase(t.left, key)
    else:
        t.right = erase(t.right, key)
    return t

def lower_bound(t, key):
    ans = None
    while t:
        if t.key >= key:
            ans = t
            t = t.left
        else:
            t = t.right
    return ans

def make_groups(p, h):
    a = sorted([(p[i], h[i], i + 1) for i in range(len(p))])
    groups = []
    i = 0
    while i < len(a):
        j = i
        vals = []
        while j < len(a) and a[j][0] == a[i][0]:
            vals.append((a[j][1], a[j][2]))
            j += 1
        groups.append(vals)
        i = j
    return groups

def build_tree(arr):
    t = None
    for h, idx in arr:
        t = insert(t, Node((h, idx), idx))
    return t

def solve():
    n = int(input())
    bp = list(map(int, input().split()))
    bh = list(map(int, input().split()))
    fp = list(map(int, input().split()))
    fh = list(map(int, input().split()))

    bg = make_groups(bp, bh)
    fg = make_groups(fp, fh)

    bi = fi = 0
    bt = build_tree(bg[0]) if bg else None
    ft = build_tree(fg[0]) if fg else None
    bc = len(bg[0]) if bg else 0
    fc = len(fg[0]) if fg else 0

    ans_b = [0] * n
    ans_f = [0] * n
    pos = 0

    while pos < n:
        if fc <= bc:
            for _ in range(fc):
                fnode = lower_bound(ft, (-10**30, -10**30))
                need = fnode.key[0] + 1
                bnode = lower_bound(bt, (need, -10**30))
                if bnode is None:
                    print("impossible")
                    return
                ans_b[pos] = bnode.val
                ans_f[pos] = fnode.val
                bt = erase(bt, bnode.key)
                ft = erase(ft, fnode.key)
                pos += 1
            fi += 1
            if fi < len(fg):
                ft = build_tree(fg[fi])
                fc = len(fg[fi])
            else:
                fc = 0
            bc -= len(fg[fi - 1])
        else:
            for _ in range(bc):
                bnode = lower_bound(bt, (-10**30, -10**30))
                need = bnode.key[0]
                fnode = lower_bound(ft, (need, -10**30))
                if fnode is None or fnode.key[0] >= need:
                    print("impossible")
                    return
                ans_b[pos] = bnode.val
                ans_f[pos] = fnode.val
                bt = erase(bt, bnode.key)
                ft = erase(ft, fnode.key)
                pos += 1
            bi += 1
            if bi < len(bg):
                bt = build_tree(bg[bi])
                bc = len(bg[bi])
            else:
                bc = 0
            fc -= len(bg[bi - 1])

    print(*ans_b)
    print(*ans_f)

solve()
```Việc triển khai giữ nhóm giá hiện tại của mỗi hàng ở dạng ngẫu nhiên. Treap cung cấp hai thao tác cần thiết: tìm chiều cao nhỏ nhất trên ngưỡng và loại bỏ ô đã chọn. Điều này tránh được chi phí bậc hai khi xóa khỏi danh sách được sắp xếp thông thường. 

Mảng câu trả lời được điền từ trái sang phải vì mỗi cặp được sử dụng đều tương ứng với vị trí tiếp theo trong màn hình cuối cùng. Độ cao và chỉ số được lưu trữ cùng nhau trong khóa treap để vẫn có thể phân biệt được độ cao trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Việc sắp xếp và mọi thao tác xử lý đều là logarit, với mỗi ô được xóa một lần | 
| Không gian | O(n) | Mỗi ô xuất hiện một lần trong một nhóm hoặc cây | 

Đầu vào lớn nhất chứa 500000 ô mỗi hàng. Giải pháp chỉ thực hiện các thao tác sắp xếp và tập hợp theo thứ tự logarit, phù hợp thoải mái trong giới hạn. 

## Vỏ cạnh 

Một ô duy nhất được xử lý vì cả hai kích thước nhóm đều bằng một và thuật toán thực hiện chính xác một so sánh chiều cao. 

Giá bằng nhau được xử lý bằng cách nhóm. Thuật toán không bao giờ cố định một thứ tự tùy ý trong một nhóm giá. Nó chỉ loại bỏ các ô theo so sánh chiều cao cần thiết tại thời điểm đó. 

Một trường hợp lỗi chẳng hạn như chiều cao ô phía sau là 5 và chiều cao ô phía trước là 6 sẽ ngay lập tức thất bại do tìm kiếm theo thứ tự không thể tìm thấy đối tác hợp lệ, do đó thuật toán sẽ in`impossible`thay vì xây dựng một thỏa thuận không hợp lệ.
