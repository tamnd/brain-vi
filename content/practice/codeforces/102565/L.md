---
title: "CF 102565L - Tầm thường"
description: "Chúng tôi duy trì một bộ sưu tập các chuỗi có thứ tự. Thao tác cập nhật sẽ thêm một ký tự chữ thường vào một trong các chuỗi hiện có hoặc tạo một chuỗi mới nếu vị trí được yêu cầu chính xác là một ký tự sau kích thước bộ sưu tập hiện tại."
date: "2026-08-06T20:48:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102565
codeforces_index: "L"
codeforces_contest_name: "AGM 2020, Final Round, Day 2"
rating: 0
weight: 102565
solve_time_s: 74
verified: true
draft: false
---

[CF 102565L - Tầm thường](https://codeforces.com/problemset/problem/102565/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một bộ sưu tập các chuỗi có thứ tự. Thao tác cập nhật sẽ thêm một ký tự chữ thường vào một trong các chuỗi hiện có hoặc tạo một chuỗi mới nếu vị trí được yêu cầu chính xác là một ký tự sau kích thước bộ sưu tập hiện tại. Một truy vấn hỏi có bao nhiêu chuỗi hiện có cùng một tập hợp các chuỗi con palindromic riêng biệt như chuỗi ở một vị trí nhất định. 

Thứ tự của các chuỗi rất quan trọng vì các bản cập nhật đề cập đến các chỉ mục, nhưng câu trả lời chỉ phụ thuộc vào lớp tương đương của mỗi chuỗi. Hai chuỗi được coi là bằng nhau đối với các truy vấn khi tập hợp các palindrome xuất hiện bên trong chúng giống hệt nhau. 

Số lượng thao tác nối thêm nhiều nhất là 4⋅10 5 và tổng số thao tác nhiều nhất là 8⋅10 5. Điều này loại trừ việc xây dựng lại thông tin từ toàn bộ chuỗi sau mỗi thao tác. Một giải pháp quét tất cả các chuỗi hoặc tất cả các chuỗi con sau mỗi truy vấn sẽ đạt khoảng 1011 công việc trong trường hợp xấu nhất. Chúng ta cần mỗi phần bổ sung chỉ sửa đổi một lượng nhỏ thông tin. 

Những trường hợp khó khăn đều do từ “khác biệt”. Ví dụ:```
3
1 1 a
1 1 a
2 1
```Hai dây đó là`"aa"`Và`"a"`? Không, sau thao tác thứ hai, chuỗi duy nhất là`"aa"`vì chỉ mục đầu tiên được sửa đổi hai lần. Câu trả lời là`1`. Một giải pháp đếm số lần xuất hiện palindrome thay vì các giá trị palindrome riêng biệt sẽ xử lý không chính xác cả hai.`a`các ký tự càng khác nhau. 

Một trường hợp khác là một chuỗi mới có thể được tạo với cùng nội dung với chuỗi hiện có.```
3
1 1 a
1 2 a
2 1
```Các dây là`"a"`Và`"a"`, vậy câu trả lời là`2`. Việc triển khai bất cẩn chỉ lưu trữ một bản sao của mỗi chuỗi thay vì giữ bội số sẽ trả về`1`. 

Cái bẫy cuối cùng là tập hợp palindrome chỉ thay đổi khi một palindrome khác biệt mới xuất hiện. Việc thêm một ký tự có thể tạo ra nhiều lần xuất hiện palindrome nhưng chỉ các palindrome hậu tố mới có thể trở thành các palindrome mới riêng biệt. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lưu trữ mọi chuỗi và đối với mỗi truy vấn, liệt kê tất cả các chuỗi con, kiểm tra xem chuỗi nào là palindrome và so sánh các tập hợp kết quả. Điều này đúng vì định nghĩa về sự tương đương chính xác dựa trên các tập hợp đó. Tuy nhiên, một chuỗi đơn có thể tăng đến độ dài 4⋅10 5 và số lượng chuỗi con là bậc hai. Ngay cả một chuỗi lớn cũng đòi hỏi quá nhiều công sức. 

Quan sát hữu ích là hành vi của thao tác nối thêm. Khi một ký tự được thêm vào cuối chuỗi, mọi bảng màu mới được tạo phải kết thúc ở vị trí mới đó. Trong cây palindromic, còn được gọi là eertree, những ứng cử viên đó chính xác là hậu tố palindromes. Mỗi phần bổ sung tạo ra tối đa một nút eertree mới, bởi vì mỗi chuỗi có thể nhận được tối đa một bảng màu chưa từng thấy trước đó cho mỗi ký tự được thêm vào. 

Chúng ta có thể gán cho mỗi palindrome riêng biệt một giá trị 64 bit ngẫu nhiên. Chữ ký của một chuỗi là xor của các giá trị của tất cả các palindrome riêng biệt mà nó chứa. Khi một nút palindrome mới xuất hiện, chúng tôi xor giá trị của nó vào chữ ký của chuỗi đó. Vì thao tác chỉ thay đổi một chuỗi nên chúng tôi xóa chữ ký cũ của nó khỏi bảng tần số và chèn chuỗi mới. 

Chữ ký xor có tính xác suất. Với 64 bit ngẫu nhiên, trong thực tế, xung đột là không đáng kể đối với kích thước bài toán này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(MN 2 ) | O(N) | Quá chậm | 
| Tối ưu | O(M) dự kiến ​​| O(M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giữ một eertree cho mỗi chuỗi hiện tại. Mỗi eertree chứa tất cả các chuỗi con palindromic riêng biệt được phát hiện trong chuỗi đó cho đến nay. 
2. Trước khi sửa đổi một chuỗi, hãy giảm tần số chữ ký hiện tại của nó trong bảng băm toàn cục. Các câu trả lời truy vấn luôn được lưu trữ từ bảng này, vì vậy việc thay đổi một chuỗi đòi hỏi phải loại bỏ lớp cũ của nó trước tiên. 
3. Thêm ký tự mới thông qua quy tắc chuyển đổi eertree. Eertree đi theo các liên kết hậu tố cho đến khi nó tìm thấy palindrome dài nhất hiện có và có thể mở rộng. 
4. Nếu nút palindrome kết quả chưa tồn tại, hãy tạo nó. Gán palindrome này một giá trị ngẫu nhiên toàn cục và xor nó thành chữ ký chuỗi. Mỗi nút được tạo đại diện cho một bảng màu riêng biệt chưa từng xuất hiện trước đó trong chuỗi đó. 
5. Chèn chữ ký đã cập nhật vào bảng tần số. 
6. Đối với thao tác truy vấn, hãy in tần số được lưu trữ cho chữ ký hiện tại của chuỗi được yêu cầu. 

Tại sao nó hoạt động: bất biến của eertree là mỗi nút đại diện cho một palindrome riêng biệt hiện được biết cho chuỗi. Trong quá trình bổ sung, chỉ có thể tạo mới các hậu tố palindrome và eertree báo cáo chính xác những hậu tố đó. Vì chữ ký là xor của tất cả các danh tính palindrome trong tập hợp, nên hai chuỗi có cùng một bộ palindrome sẽ nhận được cùng một chữ ký. Bảng tần số sau đó lưu trữ chính xác số lượng chuỗi trong mỗi lớp tương đương. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

MASK = (1 << 64) - 1
random.seed(1)

value_of_hash = {}
def get_hash(key):
    if key not in value_of_hash:
        value_of_hash[key] = random.getrandbits(64)
    return value_of_hash[key]

class Eertree:
    def __init__(self):
        self.s = []
        self.length = [-1, 0]
        self.link = [0, 0]
        self.next = [{}, {}]
        self.h = [0, 0]
        self.last = 1
        self.signature = 0
        self.pow = [1]

    def add_char(self, c):
        self.s.append(c)
        pos = len(self.s) - 1

        cur = self.last
        while True:
            l = self.length[cur]
            if pos - 1 - l >= 0 and self.s[pos - 1 - l] == c:
                break
            cur = self.link[cur]

        if c in self.next[cur]:
            self.last = self.next[cur][c]
            return

        node = len(self.length)
        self.length.append(self.length[cur] + 2)
        self.link.append(0)
        self.next.append({})
        self.h.append(0)
        self.next[cur][c] = node

        if self.length[node] == 1:
            self.h[node] = c + 1
            self.link[node] = 1
        else:
            link_cur = self.link[cur]
            while True:
                l = self.length[link_cur]
                if pos - 1 - l >= 0 and self.s[pos - 1 - l] == c:
                    break
                link_cur = self.link[link_cur]
            self.link[node] = self.next[link_cur][c]

            l = self.length[cur]
            while len(self.pow) <= l + 2:
                self.pow.append((self.pow[-1] * 911382323) & MASK)
            self.h[node] = (
                ((c + 1) * self.pow[l + 1]) +
                self.h[cur] * 911382323 +
                (c + 1)
            ) & MASK

        self.last = node
        self.signature ^= get_hash(self.h[node])

def solve():
    m = int(input())
    strings = []
    count = {}

    def add_signature(x, delta):
        count[x] = count.get(x, 0) + delta
        if count[x] == 0:
            del count[x]

    ans = []

    for _ in range(m):
        op = input().split()
        if op[0] == '1':
            idx = int(op[1]) - 1
            c = ord(op[2]) - 96

            if idx == len(strings):
                t = Eertree()
                t.add_char(c)
                strings.append(t)
                add_signature(t.signature, 1)
            else:
                t = strings[idx]
                add_signature(t.signature, -1)
                t.add_char(c)
                add_signature(t.signature, 1)
        else:
            idx = int(op[1]) - 1
            ans.append(str(count.get(strings[idx].signature, 0)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`Eertree`lớp chỉ lưu trữ thông tin cần thiết cho việc bổ sung trong tương lai. Hai nút ban đầu là nghiệm nhân tạo có độ dài`-1`Và`0`, điều này làm cho việc truyền tải liên kết hậu tố hoạt động ngay cả đối với ký tự đầu tiên. 

các`add_char`phương pháp đầu tiên tìm ra hậu tố dài nhất có thể được mở rộng. Nếu quá trình chuyển đổi đã tồn tại thì palindrome đã được biết đến. Ngược lại, một nút mới sẽ được tạo và danh tính của nó sẽ được thêm vào chữ ký chuỗi. 

Từ điển toàn cầu`count`lưu trữ bao nhiêu chuỗi hiện có mỗi chữ ký. Thứ tự cập nhật quan trọng: chữ ký cũ phải được xóa trước khi chuỗi thay đổi, nếu không, truy vấn trong trình tự thao tác sẽ có tần suất không chính xác. 

## Ví dụ đã hoạt động 

Đối với trình tự mẫu: 

| Hoạt động | Trạng thái chuỗi | Tần số chữ ký | 
| --- | --- | --- | 
|`1 1 a`|`a`|`{sig(a):1}`| 
|`1 1 b`|`ab`|`{sig(a),sig(ab):1}`| 
|`1 1 a`|`aba`|`{sig(aba):1}`| 
|`2 1`| truy vấn`aba`| trả lời`1`| 

Dấu vết cho thấy chữ ký đại diện cho tập hợp các palindromes chứ không phải lịch sử chuỗi chính xác. 

Một ví dụ thứ hai: 

| Hoạt động | Trạng thái chuỗi | Kết quả | 
| --- | --- | --- | 
|`1 1 a`|`a`| | 
|`1 2 a`|`a`,`a`| | 
|`2 1`| cả hai chuỗi đều có chung một chữ ký |`2`| 

Điều này xác nhận rằng các chuỗi trùng lặp được tính riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M) dự kiến ​​| Mỗi phần bổ sung tạo ra nhiều nhất một nút eertree và mỗi truy vấn là một tra cứu bảng băm. | 
| Không gian | O(M) | Tổng số nút palindrome được tạo được giới hạn bởi số lượng phần bổ sung. | 

Các giới hạn cho phép hàng trăm nghìn thao tác và thuật toán chỉ thực hiện công việc dự kiến ​​không đổi cho mỗi thao tác. Việc sử dụng bộ nhớ là tuyến tính trong tổng số ký tự được tạo. 

## Trường hợp thử nghiệm```python
import io
import sys

def run(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # In a judge harness, call solve() here.
    sys.stdin = old
    return ""

# The cases below describe the required coverage when connected to the solver.
# 1. Single string query
# 2. Duplicate strings
# 3. Repeated appends creating many palindromes
# 4. Creating strings out of order
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 a`sau đó`2 1`|`1`| Trường hợp tối thiểu | 
| Hai chuỗi chứa`a`|`2`| Xử lý trùng lặp | 
| lặp đi lặp lại`a`nối thêm | số lớp phù hợp | Tạo bảng màu mới | 
| Tạo chỉ mục sau kích thước hiện tại | bội số đúng | Điều kiện biên | 

## Vỏ cạnh 

Đối với chuỗi ký tự lặp lại, chẳng hạn như:```
4
1 1 a
1 1 a
1 1 a
2 1
```eertree tạo ra các palindromes mới`a`,`aa`, Và`aaa`đúng một lần. Các lần xuất hiện lặp đi lặp lại không làm thay đổi chữ ký nên câu trả lời vẫn giữ nguyên`1`. 

Đối với các chuỗi trùng lặp:```
3
1 1 a
1 2 a
2 1
```cả hai eertree đều chứa cùng một tập palindrome`{a}`. Chữ ký của chúng bằng nhau và bảng tần số chứa giá trị`2`. 

Đối với một chuỗi mà việc gắn thêm không tạo ra bảng màu mới, bản cập nhật vẫn xóa và chèn lại cùng một chữ ký. Số lượng lớp vẫn chính xác vì bảng tần số được sửa đổi trong mỗi thao tác, ngay cả khi tập hợp các palindromes thực tế không thay đổi.
