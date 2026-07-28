---
title: "CF 102787B - Lê TreaP"
description: "Chúng tôi duy trì một chuỗi có thể thay đổi. Chuỗi bắt đầu bằng n chữ cái viết thường và sau đó q thao tác sửa đổi nó hoặc đặt câu hỏi về nó. Việc xóa sẽ xóa toàn bộ khoảng liền kề."
date: "2026-07-27T19:14:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102787
codeforces_index: "B"
codeforces_contest_name: "Algorithms Thread Treaps Contest"
rating: 0
weight: 102787
solve_time_s: 84
verified: true
draft: false
---

[CF 102787B - Pear TreaP](https://codeforces.com/problemset/problem/102787/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một chuỗi có thể thay đổi. Chuỗi bắt đầu bằng`n`chữ thường và sau đó`q`hoạt động sửa đổi nó hoặc đặt câu hỏi về nó. Việc xóa sẽ xóa toàn bộ khoảng liền kề. Việc chèn sẽ đặt một ký tự mới vào một vị trí nhất định và dịch chuyển các ký tự sau sang phải. Một truy vấn hỏi xem chuỗi con hiện tại có bằng chuỗi ngược của nó hay không. 

Khó khăn đến từ việc các vị trí không cố định. Một ký tự hiện đang ở vị trí 100 có thể ở vị trí 20 sau này do bị xóa và chèn vào. Đồng thời, việc kiểm tra trực tiếp một bảng màu đòi hỏi phải xem xét mọi ký tự trong khoảng được truy vấn. 

Giới hạn rất lớn: cả độ dài ban đầu và số lượng thao tác đều có thể đạt tới`3 * 10^5`. Một giải pháp quét một chuỗi con có độ dài`n`cho mọi truy vấn có thể thực hiện xung quanh`n * q`, đó là về`9 * 10^10`kiểm tra ký tự trong trường hợp xấu nhất. Ngay cả một giải pháp tuyến tính đơn giản cho mỗi thao tác cũng vượt xa giới hạn thời gian của cuộc thi cho phép. Chúng ta cần mỗi thao tác thực hiện theo thời gian logarit. 

Có một số trường hợp dễ bỏ sót. Chuỗi con một ký tự luôn là chuỗi palindrome. Ví dụ, đầu vào```
1 1
z
3 1 1
```phải xuất ra```
yes
```Một giải pháp so sánh hai nửa và quên trường hợp này có thể vô tình bác bỏ nó. 

Một trường hợp khác là khi việc chèn xảy ra ở đầu hoặc cuối. Ví dụ,```
3 2
abc
2 a 1
3 1 2
```tạo ra chuỗi`aabc`, và chuỗi con được truy vấn là`aa`, vậy câu trả lời là`yes`. Mã xử lý các vị trí dựa trên 0 không chính xác thường chèn sai vị trí ở đây. 

Việc xóa cũng có thể làm trống phần lớn chuỗi. Ví dụ,```
5 2
abcba
1 2 4
3 1 2
```lá`aa`, đó là một palindrome. Thay vào đó, một cấu trúc giữ các chỉ mục cũ sau khi xóa có thể trả về kết quả cho chuỗi cũ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là lưu trữ chuỗi trong một mảng hoặc danh sách. Việc chèn và xóa yêu cầu dịch chuyển các ký tự và truy vấn palindrome có thể so sánh các ký tự ở cả hai đầu. Bản thân truy vấn có giá`O(length of substring)`, trong khi việc chèn và xóa có thể tốn kém`O(n)`vì nhiều nhân vật có thể di chuyển. Với`3 * 10^5`hoạt động, trường hợp xấu nhất đạt khoảng`O(nq)`làm việc, quá chậm. 

Quan sát quan trọng là tất cả các thao tác đều dựa trên các vị trí bên trong một chuỗi. Chúng ta không cần truy cập ngẫu nhiên vào mọi phần tử, chúng ta cần chia chuỗi tại các vị trí, nối các phần lại với nhau và nhanh chóng yêu cầu thông tin về một phân đoạn. 

Một treap ngầm được thiết kế chính xác cho việc này. Nó lưu trữ chuỗi dưới dạng cây nhị phân cân bằng ngẫu nhiên trong đó việc duyệt theo thứ tự là thứ tự chuỗi hiện tại. Mỗi nút lưu trữ kích thước của cây con của nó, vì vậy chúng ta có thể phân chia tại một vị trí và hợp nhất các phần trong`O(log n)`thời gian dự kiến. 

Để trả lời các truy vấn palindrome, mỗi nút treap duy trì hai giá trị băm luân phiên: một cho chuỗi theo thứ tự bình thường và một cho thứ tự đảo ngược. Nếu một chuỗi con có cùng hàm băm xuôi và ngược thì chuỗi đó được coi là một chuỗi màu nhạt. Việc tách treap sẽ tách biệt chuỗi con được yêu cầu, cho phép các hàm băm được lưu trữ trả lời truy vấn mà không cần quét các ký tự. 

Phương pháp brute-force hoạt động vì nó trực tiếp kiểm tra định nghĩa của một bảng màu nhưng nó liên tục tính toán lại thông tin. Quan sát rằng một chuỗi con có thể được biểu thị bằng hàm băm được duy trì cho phép chúng tôi thay thế các lần quét lặp lại bằng các lần kiểm tra liên tục sau khi phân tách logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n)`mỗi truy vấn,`O(n)`mỗi lần sửa đổi |`O(n)`| Quá chậm | 
| Tối ưu |`O(log n)`dự kiến ​​cho mỗi hoạt động |`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một kho lưu trữ ngầm từ chuỗi ban đầu. Mỗi nút lưu trữ ký tự, kích thước cây con, mức độ ưu tiên, con trái và con phải và hai giá trị băm. Hàm băm tiến đại diện cho chuỗi con khi nó xuất hiện từ trái sang phải, trong khi hàm băm ngược đại diện cho cùng một chuỗi con từ phải sang trái. 
2. Về việc xóa vị trí`l`bởi vì`r`, chia tre thành ba phần: tiền tố trước`l`, phân đoạn bị xóa và hậu tố sau`r`. Phần giữa bị loại bỏ và hai phần còn lại được hợp nhất. 
3. Để chèn vào vị trí`p`, chia phần thưởng thành phần đầu tiên`p-1`ký tự và hậu tố còn lại. Tạo một nút mới cho ký tự được chèn và hợp nhất ba phần lại với nhau. 
4. Đối với truy vấn palindrome về vị trí`l`bởi vì`r`, tách ra phân đoạn được yêu cầu. So sánh hàm băm thuận và hàm băm ngược của nó. Giá trị băm bằng nhau có nghĩa là phân đoạn được đọc giống nhau theo cả hai hướng. Hợp nhất các mảnh lại sau đó để chuỗi ban đầu được khôi phục. 
5. Trong mỗi lần hợp nhất hoặc phân tách, hãy tính toán lại kích thước và giá trị băm được lưu trữ. Điều này giữ cho bản tóm tắt của mỗi nút bằng thông tin chính xác về cây con của nó. 

Điều bất biến đằng sau thuật toán là mọi nút treap luôn mô tả chính xác trình tự chứa trong cây con của nó. Việc phân tách giữ nguyên thứ tự các ký tự và chỉ hợp nhất các chuỗi đã đúng. Bởi vì các giá trị băm được lưu trữ được cập nhật sau mỗi thay đổi cấu trúc nên phân đoạn riêng biệt được sử dụng trong truy vấn luôn có các biểu diễn tiến và lùi chính xác. Biểu diễn bằng nhau chứng minh rằng phân đoạn này là một palindrome theo hàm băm được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD1 = 10**9 + 7
MOD2 = 10**9 + 9
BASE = 911382323

MAXN = 700000

pow1 = [1] * (MAXN + 5)
pow2 = [1] * (MAXN + 5)
for i in range(1, MAXN + 5):
    pow1[i] = pow1[i - 1] * BASE % MOD1
    pow2[i] = pow2[i - 1] * BASE % MOD2

import random
random.seed(1)

class Node:
    __slots__ = ("c", "p", "l", "r", "sz", "h1", "h2", "rh1", "rh2")
    def __init__(self, c):
        self.c = ord(c) - 96
        self.p = random.randint(1, 1 << 60)
        self.l = None
        self.r = None
        self.sz = 1
        self.h1 = self.h2 = self.c
        self.rh1 = self.rh2 = self.c

def size(t):
    return t.sz if t else 0

def hashes(t):
    if t:
        return t.h1, t.h2, t.rh1, t.rh2
    return 0, 0, 0, 0

def pull(t):
    if not t:
        return
    ls = size(t.l)
    rs = size(t.r)
    lh1, lh2, lrh1, lrh2 = hashes(t.l)
    rh1, rh2, rrh1, rrh2 = hashes(t.r)

    t.sz = ls + 1 + rs

    t.h1 = (lh1 * pow1[1 + rs] + t.c * pow1[rs] + rh1) % MOD1
    t.h2 = (lh2 * pow2[1 + rs] + t.c * pow2[rs] + rh2) % MOD2

    t.rh1 = (rrh1 * pow1[1 + ls] + t.c * pow1[ls] + lrh1) % MOD1
    t.rh2 = (rrh2 * pow2[1 + ls] + t.c * pow2[ls] + lrh2) % MOD2

def split(t, k):
    if not t:
        return None, None
    if size(t.l) >= k:
        a, b = split(t.l, k)
        t.l = b
        pull(t)
        return a, t
    else:
        a, b = split(t.r, k - size(t.l) - 1)
        t.r = a
        pull(t)
        return t, b

def merge(a, b):
    if not a or not b:
        return a or b
    if a.p > b.p:
        a.r = merge(a.r, b)
        pull(a)
        return a
    else:
        b.l = merge(a, b.l)
        pull(b)
        return b

def build(s):
    root = None
    for ch in s:
        root = merge(root, Node(ch))
    return root

def solve():
    n, q = map(int, input().split())
    root = build(input().strip())
    ans = []

    for _ in range(q):
        query = input().split()
        typ = int(query[0])

        if typ == 1:
            l, r = int(query[1]), int(query[2])
            a, b = split(root, l - 1)
            _, c = split(b, r - l + 1)
            root = merge(a, c)

        elif typ == 2:
            c = query[1]
            p = int(query[2])
            a, b = split(root, p - 1)
            root = merge(merge(a, Node(c)), b)

        else:
            l, r = int(query[1]), int(query[2])
            a, b = split(root, l - 1)
            mid, c = split(b, r - l + 1)
            if mid.h1 == mid.rh1 and mid.h2 == mid.rh2:
                ans.append("yes")
            else:
                ans.append("no")
            root = merge(a, merge(mid, c))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Lớp nút chứa chính xác thông tin cần thiết cho tren. Kích thước cây con cho phép`split`để tìm vị trí mà không lưu trữ các chỉ số rõ ràng. Bốn giá trị băm là các giá trị băm tiến và lùi theo hai mô-đun khác nhau, giúp giảm nguy cơ va chạm. 

các`pull`chức năng là hoạt động bảo trì cốt lõi. Sau khi một cây con thay đổi, nó sẽ tính toán lại toàn bộ bản tóm tắt của cây con. Các công thức đặt con bên trái trước ký tự hiện tại và sau đó trước con bên phải cho hàm băm tiến. Hàm băm ngược áp dụng ý tưởng tương tự theo hướng ngược lại. 

các`split`hàm sử dụng kích thước cây con để phân tách cây đầu tiên`k`các nhân vật từ phần còn lại. Điều kiện biên quan trọng là`k`đại diện cho số lượng ký tự, vì vậy việc chèn vào vị trí`p`có nghĩa là chia tách tại`p - 1`. 

Thao tác truy vấn tạm thời loại bỏ khoảng thời gian được yêu cầu. Điều này không sao chép các ký tự, nó chỉ thay đổi các liên kết cây, vì vậy nó vẫn giữ nguyên logarit. Sau khi kiểm tra giá trị băm, treap ban đầu được xây dựng lại bằng cách hợp nhất các phần giống nhau. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 4
aaaa
1 3 4
3 1 1
3 1 1
2 a 3
```| Bước | Hoạt động | Chuỗi hiện tại | Phân khúc biệt lập | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Ban đầu | aaa | | | 
| 1 | Xóa 3 đến 4 | aa | | | 
| 2 | Truy vấn 1 đến 1 | aa | một | vâng | 
| 3 | Truy vấn 1 đến 1 | aa | một | vâng | 
| 4 | Chèn một lúc 3 | aaa | | | 

Dấu vết cho thấy khoảng một ký tự được xử lý một cách tự nhiên vì hàm băm tiến và lùi của nó giống hệt nhau. 

Đối với mẫu thứ hai:```
5 5
aaaaa
2 b 3
1 1 1
3 5 5
1 5 5
1 3 3
```| Bước | Hoạt động | Chuỗi hiện tại | Phân khúc biệt lập | Kết quả | 
| --- | --- | --- | --- | --- | 
| 0 | Ban đầu | aaa | | | 
| 1 | Chèn b ở số 3 | aabaa | | | 
| 2 | Xóa 1 đến 1 | aaa | | | 
| 3 | Truy vấn 5 đến 5 | aaa | một | vâng | 
| 4 | Xóa 5 đến 5 | aba | | | 
| 5 | Xóa 3 đến 3 | aba | | | 

Ví dụ này chứng minh rằng cấu trúc giữ vị trí chính xác sau nhiều lần chèn và xóa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(log n)`dự kiến ​​cho mỗi hoạt động | Mỗi thao tác thực hiện một số lần chia tách và hợp nhất treap không đổi. | 
| Không gian |`O(n + q)`| Treap lưu trữ các ký tự hiện tại và tối đa một nút mới được tạo cho mỗi lần chèn. | 

Với tối đa`3 * 10^5`hoạt động, cập nhật logarit được yêu cầu. Treap tránh việc xây dựng lại chuỗi và giữ mọi thao tác trong giới hạn đã định. 

## Trường hợp thử nghiệm```python
import sys
import io

# These tests assume the submitted solution is wrapped so solve() can be called.
# They are examples of the cases that should be checked.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""4 4
aaaa
1 3 4
3 1 1
3 1 1
2 a 3
""") == "yes\nyes\n", "sample 1"

assert run("""5 5
aaaaa
2 b 3
1 1 1
3 5 5
1 5 5
1 3 3
""") == "yes\n", "sample 2"

assert run("""1 1
z
3 1 1
""") == "yes\n", "single character"

assert run("""3 3
abc
2 a 1
3 1 2
3 1 4
""") == "yes\nno\n", "insertion boundary"

assert run("""5 2
abcba
1 2 4
3 1 2
""") == "yes\n", "deletion leaving palindrome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ký tự đơn |`yes`| Xử lý truy vấn palindrome nhỏ nhất có thể | 
| Chèn vào vị trí 1 |`yes`sau đó`no`| Kiểm tra lập chỉ mục chèn | 
| Xóa phần giữa |`yes`| Kiểm tra việc xây dựng lại sau khi gỡ bỏ | 
| Cung cấp mẫu | Kết quả đầu ra mẫu | Khẳng định hành vi chuẩn | 

## Vỏ cạnh 

Truy vấn một ký tự được xử lý bằng cách so sánh hàm băm giống như mọi truy vấn khác. Vì```
1 1
z
3 1 1
```tre biệt lập chỉ chứa`z`. Cả hàm băm tiến và hàm băm ngược của nó đều`26`, vậy câu trả lời là`yes`. 

Việc chèn ở vị trí đầu tiên là nguyên nhân phổ biến gây ra các lỗi riêng lẻ. Vì```
3 2
abc
2 a 1
3 1 2
```điểm phân chia là 0 ký tự. Cây mới trở thành`aabc`, và hai ký tự đầu tiên là`aa`, vì vậy cả hai giá trị băm đều khớp và câu trả lời là`yes`. 

Việc xóa lớn có hiệu quả vì khoảng thời gian bị xóa được thể hiện dưới dạng toàn bộ phân đoạn trep. Vì```
5 2
abcba
1 2 4
3 1 2
```sự phân chia loại bỏ`bcb`, rời đi`aa`. Truy vấn tách biệt hai ký tự đó và so sánh các giá trị băm bằng nhau, tạo ra`yes`. Không có vị trí ký tự nào cần điều chỉnh thủ công vì cấu trúc tre đã thể hiện trình tự mới.
