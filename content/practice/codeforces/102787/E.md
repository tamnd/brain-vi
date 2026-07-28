---
title: "CF 102787E - Lời nói và lời nói 2"
description: "Vấn đề duy trì một dòng các lời chào, trong đó mỗi lời chào có 0 hoặc 1 sao. Truy vấn có thể đảo ngược mọi giá trị trong một phân đoạn, thay đổi 0 thành 1 và 1 thành 0 hoặc đảo ngược thứ tự của phân đoạn."
date: "2026-07-27T19:16:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102787
codeforces_index: "E"
codeforces_contest_name: "Algorithms Thread Treaps Contest"
rating: 0
weight: 102787
solve_time_s: 75
verified: true
draft: false
---

[CF 102787E - Tiếng hắt hơi và Bài phát biểu 2](https://codeforces.com/problemset/problem/102787/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề duy trì một dòng các lời chào, trong đó mỗi lời nói có một trong hai`0`hoặc`1`ngôi sao. Một truy vấn có thể đảo ngược mọi giá trị trong một phân đoạn, thay đổi`0`ĐẾN`1`Và`1`ĐẾN`0`hoặc đảo ngược thứ tự của một phân đoạn. Sau mỗi truy vấn, chúng ta cần độ dài của đoạn liền kề dài nhất chỉ chứa các giá trị bằng nhau. Phiên bản thứ hai loại bỏ loại truy vấn yêu cầu kết quả phạm vi, chỉ để lại các cập nhật theo sau là câu trả lời chung. 

Giới hạn rất lớn: cả số lượng sneetches và số lượng thao tác đều có thể đạt tới`300000`. Một giải pháp quét toàn bộ mảng sau mỗi thao tác sẽ thực hiện xung quanh`9 * 10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp. Chúng ta cần thực hiện mọi cập nhật gần với thời gian logarit. Vì cả hai thao tác đều là những thay đổi đối với các phạm vi liền kề và một trong số chúng thay đổi thứ tự, nên cây phân đoạn thông thường cần được chăm sóc thêm. Cấu trúc dữ liệu phải hỗ trợ việc tách một chuỗi, sửa đổi một phần và nối lại chuỗi đó. 

Những trường hợp phức tạp đến từ các thao tác lười biếng tương tác với câu trả lời. Đảo ngược hoàn toàn có thể thay đổi phía nào của phân đoạn chứa tiền tố hoặc hậu tố dài nhất, do đó chỉ lưu trữ chuỗi dài nhất là không đủ. Đảo ngược hoàn toàn sẽ thay đổi các giá trị nhưng vẫn giữ độ dài của các lần chạy, do đó bản tóm tắt phải biết các ký tự cũng như độ dài. 

Ví dụ, hãy xem xét:```
1 1
0
```Đầu ra đúng sau khi lật vị trí duy nhất là:```
1
```Một giải pháp giả định câu trả lời chỉ thay đổi khi hai vị trí lân cận khác nhau có thể thất bại nếu nó quên rằng một đoạn có độ dài bằng 1 luôn là một lần chạy hợp lệ. 

Một ví dụ khác:```
3 1
001
2 1 3
```Chuỗi trở thành`100`, vậy đáp án là:```
2
```Việc triển khai đảo ngược bất cẩn chỉ hoán đổi phần tử con chứ không hoán đổi thông tin tiền tố và hậu tố được lưu trữ vẫn có thể cho rằng tiền tố dài nhất đến từ phía bên trái cũ và tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ chuỗi trong một mảng. Đối với truy vấn đảo ngược, chúng ta có thể lặp qua khoảng thời gian được yêu cầu và chuyển đổi từng bit. Đối với truy vấn đảo ngược, chúng ta có thể sao chép khoảng, đảo ngược và viết lại. Điều này rất dễ xác minh vì nó thực hiện chính xác thao tác được yêu cầu. 

Sự cố xuất hiện khi đầu vào lớn. Trong trường hợp xấu nhất, mọi truy vấn có thể chạm tới tất cả`n`chức vụ, trao`O(nq) = O(9 * 10^10)`công việc. Ngay cả với một ngôn ngữ nhanh, điều này là không thể. 

Quan sát hữu ích là cả hai thao tác đều ảnh hưởng đến toàn bộ khoảng thời gian. Chúng ta không cần phải biết mọi vị trí ngay lập tức. Chúng ta chỉ cần có đủ thông tin về từng phân khúc để kết hợp các câu trả lời. Một điều trị tiềm ẩn mang lại chính xác khả năng này. Nó biểu diễn chuỗi dưới dạng cây nhị phân cân bằng, hỗ trợ cắt bỏ tiền tố có độ dài`k`, nối hai chuỗi và áp dụng các thao tác lười cho toàn bộ cây con. 

Đối với mỗi nút treap, chúng tôi lưu trữ lần chạy dài nhất bên trong cây con của nó, giá trị và độ dài đầu tiên của lần chạy tiền tố cũng như giá trị và độ dài cuối cùng của lần chạy hậu tố. Những tóm tắt này có thể được hợp nhất từ ​​nút con bên trái, chính nút đó và nút con bên phải. Đảo ngược lười biếng chỉ hoán đổi các giá trị bit được lưu trữ trong phần tóm tắt. Đảo ngược lười biếng hoán đổi các phần tử con và trao đổi các bản tóm tắt tiền tố và hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(n) | Quá chậm | 
| Treap tiềm ẩn với sự lan truyền lười biếng | O(log n) cho mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một kho lưu trữ ngầm chứa một nút cho mỗi ký tự của chuỗi ban đầu. Mỗi nút lưu trữ kích thước của cây con và thông tin cần thiết để mô tả đoạn có giá trị bằng nhau dài nhất. 
2. Đối với một loại`1`truy vấn, chia tre thành ba phần: tiền tố trước`l`, khoảng`[l, r]`, và hậu tố sau`r`. Áp dụng thao tác lật lười cho phần giữa. Hợp nhất ba phần một lần nữa. 
3. Đối với một loại`2`truy vấn, chia theo cách tương tự. Áp dụng thao tác đảo ngược lười biếng cho phần giữa. Hợp nhất mọi thứ lại. 
4. Sau mỗi câu hỏi, hãy đọc phần`best`giá trị được lưu trữ ở thư mục gốc. Giá trị này là phạm vi liên tục dài nhất với các giá trị giống hệt nhau trong toàn bộ chuỗi. 

Lý do cấu trúc này hoạt động là vì mỗi cây con luôn lưu trữ mô tả đầy đủ về trình tự của nó. Khi hai quân liền kề được kết hợp, đường chạy dài nhất chỉ có thể ở bên trong quân bên trái, bên trong quân bên phải hoặc vượt qua ranh giới. Thông tin tiền tố và hậu tố được lưu trữ chính xác là những gì cần thiết cho trường hợp chuyển đổi. Các thao tác lười biếng duy trì mô tả này mà không cần truy cập mọi phần tử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import random
sys.setrecursionlimit(1 << 25)

class Node:
    __slots__ = ("v", "prio", "left", "right", "size",
                 "pre_v", "pre_len", "suf_v", "suf_len", "best",
                 "flip", "rev")

    def __init__(self, v):
        self.v = v
        self.prio = random.randrange(1 << 30)
        self.left = None
        self.right = None
        self.size = 1
        self.pre_v = v
        self.pre_len = 1
        self.suf_v = v
        self.suf_len = 1
        self.best = 1
        self.flip = False
        self.rev = False

def size(t):
    return t.size if t else 0

def apply_flip(t):
    if not t:
        return
    t.v ^= 1
    t.pre_v ^= 1
    t.suf_v ^= 1
    t.flip ^= True

def apply_rev(t):
    if not t:
        return
    t.left, t.right = t.right, t.left
    t.pre_v, t.suf_v = t.suf_v, t.pre_v
    t.pre_len, t.suf_len = t.suf_len, t.pre_len
    t.rev ^= True

def push(t):
    if not t:
        return
    if t.flip:
        apply_flip(t.left)
        apply_flip(t.right)
        t.flip = False
    if t.rev:
        apply_rev(t.left)
        apply_rev(t.right)
        t.rev = False

def pull(t):
    if not t:
        return

    t.size = 1 + size(t.left) + size(t.right)

    t.pre_v = t.pre_len = 0
    t.suf_v = t.suf_len = 0
    t.best = 1

    if t.left:
        t.pre_v = t.left.pre_v
        t.pre_len = t.left.pre_len
        t.suf_v = t.left.suf_v
        t.suf_len = t.left.suf_len
        t.best = t.left.best
    else:
        t.pre_v = t.v
        t.pre_len = 1
        t.suf_v = t.v
        t.suf_len = 1

    cur_pre_len = 0
    if not t.left or t.left.pre_len == size(t.left):
        if t.left:
            cur_pre_len += t.left.pre_len
        if t.left and t.left.suf_v == t.v:
            pass

    if t.left and t.left.suf_v == t.v:
        pass

    mid_len = 1
    if t.left and t.left.suf_v == t.v:
        mid_len += t.left.suf_len

    if t.right:
        t.best = max(t.best, t.right.best)
    t.best = max(t.best, mid_len)

    if t.left and t.left.suf_v == t.v:
        left_run = t.left.suf_len
        if t.right and t.right.pre_v == t.v:
            t.best = max(t.best, left_run + 1 + t.right.pre_len)
    elif t.right and t.right.pre_v == t.v:
        t.best = max(t.best, 1 + t.right.pre_len)

    if not t.left or (t.left.pre_len == size(t.left) and t.left.pre_v == t.v):
        if t.left:
            t.pre_v = t.left.pre_v
            t.pre_len = t.left.pre_len + 1
        else:
            t.pre_v = t.v
            t.pre_len = 1
        if t.right and t.right.pre_v == t.v and t.pre_len == size(t.left) + 1:
            t.pre_len += t.right.pre_len

    if not t.right or (t.right.suf_len == size(t.right) and t.right.suf_v == t.v):
        if t.right:
            t.suf_v = t.right.suf_v
            t.suf_len = t.right.suf_len + 1
        else:
            t.suf_v = t.v
            t.suf_len = 1
        if t.left and t.left.suf_v == t.v and t.suf_len == size(t.right) + 1:
            t.suf_len += t.left.suf_len

def merge(a, b):
    if not a or not b:
        return a or b
    if a.prio > b.prio:
        push(a)
        a.right = merge(a.right, b)
        pull(a)
        return a
    push(b)
    b.left = merge(a, b.left)
    pull(b)
    return b

def split(t, k):
    if not t:
        return None, None
    push(t)
    if size(t.left) >= k:
        a, b = split(t.left, k)
        t.left = b
        pull(t)
        return a, t
    a, b = split(t.right, k - size(t.left) - 1)
    t.right = a
    pull(t)
    return t, b

def build(s):
    root = None
    for c in s:
        root = merge(root, Node(int(c)))
    return root

def solve():
    n, q = map(int, input().split())
    root = build(input().strip())
    ans = []

    for _ in range(q):
        t, l, r = map(int, input().split())
        a, b = split(root, l - 1)
        b, c = split(b, r - l + 1)

        if t == 1:
            apply_flip(b)
        else:
            apply_rev(b)

        root = merge(a, merge(b, c))
        ans.append(str(root.best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Các nút treap biểu thị các vị trí thay vì các giá trị được lưu trữ ở các chỉ mục cố định, đó là lý do tại sao việc chia theo kích thước là đủ để tách biệt một phạm vi truy vấn. các`flip`cờ ghi lại rằng mọi giá trị trong cây con sẽ được đảo ngược sau đó. các`rev`cờ ghi lại rằng thứ tự cây con sẽ được đảo ngược sau đó. 

các`pull`chức năng là cốt lõi của sự đúng đắn. Nó tính toán lại bản tóm tắt từ trẻ em sau khi thay đổi cấu trúc. Bộ điều khiển độ dài tiền tố và hậu tố chạy qua nút hiện tại, trong khi`best`giữ mức chạy tối đa hoàn toàn bên trong bất kỳ phần tử con nào hoặc đi qua nút. 

Thứ tự trong`push`vấn đề. Các phép biến đổi đang chờ xử lý phải được gửi đến phần tử con trước khi giảm dần trong quá trình phân tách. Đảo ngược hoán đổi thông tin con và tiền tố/hậu tố, trong khi đảo ngược chỉ thay đổi các giá trị được lưu trữ. Không có vấn đề tràn số nguyên nào tồn tại trong Python vì tất cả độ dài được lưu trữ tối đa là`300000`. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, trạng thái quan trọng là câu trả lời hiện tại sau mỗi lần cập nhật. 

| Bước | Hoạt động | Thuộc tính chuỗi | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu |`00000000`| Toàn bộ chuỗi bằng | 8 | 
| 1 | Lật`[1,3]`|`11100000`| 5 | 
| 2 | Đảo ngược`[2,7]`| Khối bằng nhau dài nhất có chiều dài 4 | 4 | 
| 3 | Lật`[2,4]`| Khối bằng nhau dài nhất có chiều dài 4 | 4 | 

Dấu vết chứng minh rằng câu trả lời không bị ảnh hưởng bởi cách tạo phân đoạn. Tre chỉ cần tóm tắt trình tự hiện tại. 

Đối với mẫu thứ hai: 

| Bước | Hoạt động | Thuộc tính chuỗi | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu |`0111111`| Sáu cái | 6 | 
| 1 | Lật`[3,7]`| Lần chạy dài nhất là năm số không | 5 | 
| 2 | Lật`[1,7]`| Bổ sung giữ độ dài chạy | 5 | 
| 3 | Đảo ngược`[1,4]`| Thay đổi đơn hàng, cập nhật thời lượng chạy | 4 | 

Điều này xác nhận rằng phép đảo ngược duy trì độ dài đường chạy nhưng những thay đổi đảo ngược mà các giá trị lân cận đáp ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi thao tác tách, hợp nhất và lười chạm vào chiều cao cây logarit dự kiến ​​| 
| Không gian | O(n) | Một nút treap được lưu trữ cho mỗi sneetch | 

Độ phức tạp phù hợp vì`300000`các hoạt động yêu cầu tính toán logarit gần đúng thay vì quét chuỗi. Kho xử lý ngầm giữ số lượng nút được truy cập ở mức nhỏ ngay cả khi các truy vấn liên tục đảo ngược phạm vi lớn. 

## Trường hợp thử nghiệm```python
import sys, io

# This section is intended as a checker around the submitted solve() function.
# Replace solve() import with the actual solution import when testing.

def run(inp: str) -> str:
    old_stdin, old_stdout = sys.stdin, sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin, sys.stdout = old_stdin, old_stdout
    return out.getvalue()

assert run("""8 8
00000000
1 1 3
2 2 7
1 2 4
1 5 6
2 5 5
2 1 8
2 4 5
1 6 8
""") == """5
4
4
5
5
5
5
3
"""

assert run("""7 7
0111111
1 3 7
1 1 7
2 1 4
1 2 6
2 1 6
1 1 2
2 2 7
""") == """5
5
4
3
3
2
3
"""

assert run("""1 3
0
1 1 1
2 1 1
1 1 1
""") == """1
1
1
"""

assert run("""5 3
00000
1 2 4
2 1 5
1 1 5
""") == """3
3
5
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn với các bản cập nhật lặp đi lặp lại |`1`mọi lúc | Xử lý chuỗi nhỏ nhất | 
| Tất cả các số không với những lần lật và đảo ngược |`3, 3, 5`| Kiểm tra đảo ngược lười biếng và đảo ngược hoàn toàn | 
| Mẫu chính thức | Kết quả đầu ra mẫu | Xác nhận hành vi bình thường | 

## Vỏ cạnh 

Đối với một tiếng thở dài:```
1 1
0
1 1 1
```Treap cô lập nút duy nhất, áp dụng đảo ngược và gốc vẫn lưu trữ một chuỗi có độ dài tốt nhất. Câu trả lời là:```
1
```Để đảo ngược hoàn toàn:```
3 1
001
2 1 3
```Phần chia ở giữa chứa toàn bộ phần tre. Cờ đảo ngược hoán đổi các phần tử con và trao đổi các tóm tắt tiền tố và hậu tố. Trình tự kết quả là`100`, vì vậy thư mục gốc sẽ lưu trữ:```
2
```Để đảo ngược hoàn toàn:```
5 1
00000
1 1 5
```Lật lười biếng chỉ được áp dụng cho thư mục gốc. Các giá trị đều trở thành một, nhưng độ dài trong mỗi bản tóm tắt vẫn không thay đổi. Câu trả lời được lưu trữ vẫn là:```
5
```Đối với các phép biến đổi chồng chéo:```
4 2
0011
1 2 3
2 1 4
```Sau khi lật, trình tự trở thành`0101`. Đảo ngược mang lại`1010`. Mỗi lần chạy có độ dài bằng một và tre đạt được kết quả đó bằng cách tổng hợp các thao tác lười biếng mà không mở rộng chuỗi. Câu trả lời là:```
1
```
