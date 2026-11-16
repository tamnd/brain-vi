---
title: "CF 1895D - XOR Xây dựng"
description: "Chúng ta được cung cấp các giá trị XOR giữa các phần tử liên tiếp của một hoán vị chưa xác định. Đặt hoán vị cần thiết là $b$. Đối với mọi cặp liền kề, chúng ta biết $$bi oplus b{i+1} = ai.$$ Mảng $b$ phải chứa mọi số nguyên từ $0$ đến $n-1$ đúng một lần."
date: "2026-06-08T21:46:38+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "data-structures", "math", "string-suffix-structures", "trees"]
categories: ["algorithms"]
codeforces_contest: 1895
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 157 (Rated for Div. 2)"
rating: 1900
weight: 1895
solve_time_s: 169
verified: true
draft: false
---

[CF 1895D - Xây dựng XOR](https://codeforces.com/problemset/problem/1895/D) 

**Xếp hạng:** 1900 
**Tags:** bitmask, thuật toán xây dựng, cấu trúc dữ liệu, toán học, cấu trúc hậu tố chuỗi, cây 
**Thời gian giải:** 2m 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp các giá trị XOR giữa các phần tử liên tiếp của một hoán vị chưa xác định. 

Đặt hoán vị cần thiết là$b$. Với mọi cặp liền kề, chúng ta biết$$b_i \oplus b_{i+1} = a_i.$$Mảng$b$phải chứa mọi số nguyên từ$0$ĐẾN$n-1$đúng một lần. Đầu vào đảm bảo rằng tồn tại ít nhất một hoán vị như vậy. 

Nhận xét đầu tiên là có lần$b_1$cố định thì mọi phần tử khác sẽ được xác định. Từ$$b_2 = b_1 \oplus a_1,$$và sau đó$$b_3 = b_2 \oplus a_2,$$và vân vân. Toàn bộ mảng là một hàm của một giá trị bắt đầu không xác định. 

Ràng buộc$n \le 2 \cdot 10^5$ngay lập tức loại trừ bất cứ điều gì bậc hai. MỘT$O(n^2)$quá trình xác minh sẽ yêu cầu khoảng$4 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa thời hạn. Chúng ta nên mong đợi một$O(n)$hoặc$O(n \log n)$giải pháp. 

Phần khó khăn là tìm giá trị chính xác của$b_1$. Việc triển khai đơn giản có thể thử mọi giá trị bắt đầu có thể và kiểm tra xem chuỗi kết quả có phải là hoán vị hay không. Điều đó hoạt động về mặt khái niệm nhưng trở nên quá chậm. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
n = 2
a = [1]
```Hoán vị hợp lệ là$[0,1]$hoặc$[1,0]$. Một giải pháp giả định$b_1=0$không xác minh sẽ thất bại bất cứ khi nào giá trị bắt đầu thực sự khác nhau. 

Coi như:```
n = 4
a = [2,1,2]
```Nếu chúng ta chọn$b_1=0$, chúng tôi có được```
0 2 3 1
```đó là hợp lệ. Nhưng trong nhiều đầu vào, việc chọn$0$tạo ra các giá trị trùng lặp. Việc các số được tạo nằm trong một phạm vi nhỏ không có nghĩa là chúng tạo thành một hoán vị. 

Một trường hợp tinh vi khác xuất hiện khi tiền tố XOR lặp lại:```
prefixes = [0, 5, 2, 5]
```Một lập luận bất cẩn có thể kết luận rằng sự trùng lặp là không thể tránh khỏi. Trong thực tế, việc dịch chuyển mọi tiền tố bằng một giá trị XOR phù hợp sẽ thay đổi đồng thời tất cả các số và các số trùng lặp phụ thuộc vào giá trị bắt đầu đã chọn. Giải pháp đúng phải suy luận về toàn bộ tập hợp các giá trị chứ không phải các vị trí riêng lẻ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực xuất phát trực tiếp từ quan sát rằng việc sửa chữa$b_1$xác định toàn bộ hoán vị. 

Xác định tiền tố XOR:$$p_1 = 0,$$Và$$p_i = a_1 \oplus a_2 \oplus \cdots \oplus a_{i-1}.$$Khi đó mọi phần tử đều thỏa mãn$$b_i = b_1 \oplus p_i.$$Nếu chúng ta thử mọi giá trị có thể có của$b_1$từ$0$ĐẾN$n-1$, chúng ta có thể tạo ra tất cả$n$các giá trị và kiểm tra xem chúng có tạo thành chính xác tập hợp không$\{0,1,\dots,n-1\}$. 

Lý do này đúng vì mọi hoán vị hợp lệ đều phải phát sinh từ một trong những giá trị ban đầu đó. Vấn đề là sự phức tạp. có$n$ứng cử viên cho$b_1$và mỗi chi phí xác minh$O(n)$, dẫn đến$O(n^2)$. 

Quan sát quan trọng là đầu vào được đảm bảo có giải pháp. Thay vì kiểm tra tất cả các lần khởi động có thể xảy ra, chúng ta có thể trực tiếp rút ra kết quả đúng. 

Cho phép$$p_1,p_2,\dots,p_n$$là các giá trị XOR tiền tố với$p_1=0$. 

Từ$$b_i=b_1\oplus p_i,$$bộ$$\{b_1\oplus p_i\}$$phải bằng chính xác$$\{0,1,\dots,n-1\}.$$Bây giờ tập trung vào việc giải thích trie nhị phân. 

Đối với giá trị ứng cử viên$x$, số lớn nhất trong số$$x \oplus p_i$$có thể được tìm thấy một cách hiệu quả với một lần thử. Nếu tập kết quả chính xác$[0,n-1]$, thì mọi giá trị phải ở dưới$n$. Kể từ đây$$\max_i (x \oplus p_i) < n.$$Vì nghiệm được đảm bảo tồn tại nên giá trị ban đầu đúng chính xác là giá trị$x$mà XOR tối đa với bất kỳ tiền tố nào trở thành$n-1$. 

Giải pháp Codeforces ban đầu chèn tất cả các XOR tiền tố vào một trie nhị phân và tìm giá trị$x$có XOR tối đa so với tập hợp bằng$n-1$. Giá trị đó trở thành$b_1$. Một lần$b_1$đã biết thì việc xây dựng lại toàn bộ hoán vị là ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log V)$|$O(n \log V)$| Đã chấp nhận | 

Đây$V$là giá trị XOR tối đa có thể có, thấp hơn$2^{18}$cho vấn đề này. 

## Hướng dẫn thuật toán 

1. Tính toán XOR tiền tố. 

Cho phép$p_1=0$. Đối với mỗi vị trí, hãy thêm XOR tích lũy của tất cả các vị trí trước đó$a$-giá trị. 
2. Chèn tất cả các XOR tiền tố vào một trie nhị phân. 

Trie cho phép chúng ta trả lời các truy vấn XOR tối đa trong$O(\log V)$. 
3. Với mọi giá trị$x$từ$0$ĐẾN$n-1$, truy vấn thử cho$$\max_i (x \oplus p_i).$$Đây là truy vấn XOR tối đa tiêu chuẩn. 
4. Tìm giá trị duy nhất$x$có XOR tối đa bằng$n-1$. 

Bởi vì hoán vị cuối cùng phải chứa mọi số từ$0$ĐẾN$n-1$, giá trị được tạo lớn nhất chính xác là$n-1$. Sự tồn tại được đảm bảo của nghiệm ngụ ý rằng giá trị ban đầu đúng thỏa mãn điều kiện này. 
5. Đặt$b_1=x$. 
6. Xây dựng lại hoán vị bằng cách sử dụng$$b_i = x \oplus p_i.$$7. Xuất ra chuỗi kết quả. 

### Tại sao nó hoạt động 

Biểu diễn XOR tiền tố cho$$b_i=b_1\oplus p_i.$$Do đó, mọi hoán vị hợp lệ đều có được bằng cách dịch chuyển XOR toàn bộ tiền tố được đặt theo một giá trị duy nhất. 

Giả định$x=b_1$. Vì các số kết quả chính xác$\{0,\dots,n-1\}$, giá trị cực đại của chúng bằng$n-1$. Kể từ đây$$\max_i(x\oplus p_i)=n-1.$$Đối với bất kỳ ứng cử viên nào khác, tập dịch chuyển không thể bằng hoán vị mong muốn, do đó giá trị XOR tối đa sẽ khác. Việc thử cho phép chúng tôi xác định sự thay đổi chính xác một cách hiệu quả. Sau khi hồi phục$b_1$, mọi phần tử còn lại đều tuân theo các quan hệ XOR xác định, do đó mảng được xây dựng lại chính xác là hoán vị được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class TrieNode:
    __slots__ = ("ch",)

    def __init__(self):
        self.ch = [-1, -1]

trie = [TrieNode()]

def add(x):
    node = 0
    for bit in range(20, -1, -1):
        b = (x >> bit) & 1
        if trie[node].ch[b] == -1:
            trie[node].ch[b] = len(trie)
            trie.append(TrieNode())
        node = trie[node].ch[b]

def max_xor(x):
    node = 0
    ans = 0
    for bit in range(20, -1, -1):
        b = (x >> bit) & 1
        want = b ^ 1
        if trie[node].ch[want] != -1:
            ans |= 1 << bit
            node = trie[node].ch[want]
        else:
            node = trie[node].ch[b]
    return ans

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    pref = [0]
    cur = 0
    for x in a:
        cur ^= x
        pref.append(cur)

    for x in pref:
        add(x)

    start = 0
    for x in range(n):
        if max_xor(x) == n - 1:
            start = x
            break

    ans = [start ^ p for p in pref]
    print(*ans)

solve()
```Phần đầu tiên xây dựng mảng XOR tiền tố. Mối quan hệ$$b_i=b_1\oplus p_i$$là thực tế cấu trúc trung tâm của vấn đề, vì vậy mọi thứ sau đó hoạt động dựa trên các giá trị tiền tố. 

Trie lưu trữ tất cả các tiền tố. Mỗi cấp độ tương ứng với một bit. Truy vấn XOR tối đa tham lam theo sau bit đối diện bất cứ khi nào có thể, đây là bằng chứng tiêu chuẩn cho XOR tối đa trong bộ ba nhị phân. 

Vòng lặp trên tất cả các ứng cử viên$x \in [0,n-1]$tìm kiếm giá trị bắt đầu có XOR tối đa so với bộ tiền tố bằng$n-1$. Bảo đảm vấn đề đảm bảo giá trị như vậy tồn tại. 

Cuối cùng, câu trả lời được xây dựng lại trực tiếp như$x \oplus p_i$. Không cần xác minh thêm. 

Việc triển khai sử dụng 21 bit, bao phủ thoải mái tất cả các giá trị trong vấn đề. Các hoạt động XOR không bao giờ tràn trong Python, do đó không có mối lo ngại về kích thước số nguyên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 1 2
```Tiền tố XOR: 

| Bước | Hiện tại | Tiền tố XOR | 
| --- | --- | --- | 
| Bắt đầu | - | 0 | 
| 1 | 2 | 2 | 
| 2 | 1 | 3 | 
| 3 | 2 | 1 | 

Vì thế:```
pref = [0, 2, 3, 1]
```Tìm kiếm ứng viên: 

| x | max(x XOR pre[i]) | 
| --- | --- | 
| 0 | 3 | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 3 | 

Lựa chọn hợp lệ đầu tiên là$x=0$. 

Tái thiết: 

| tôi | trước[i] | b[i] = 0 XOR pref[i] | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 2 | 2 | 
| 3 | 3 | 3 | 
| 4 | 1 | 1 | 

Đầu ra:```
0 2 3 1
```Dấu vết này thể hiện ý tưởng cốt lõi rằng toàn bộ hoán vị chỉ đơn giản là một phiên bản được dịch chuyển của bộ XOR tiền tố. 

### Ví dụ 2 

đầu vào:```
2
1
```Tiền tố XOR: 

| Bước | Tiền tố XOR | 
| --- | --- | 
| Bắt đầu | 0 | 
| Sau một[1] | 1 | 

Vì thế:```
pref = [0, 1]
```Tìm kiếm ứng viên: 

| x | max(x XOR pre[i]) | 
| --- | --- | 
| 0 | 1 | 
| 1 | 1 | 

Chọn$x=0$. 

Tái thiết: 

| tôi | trước[i] | Kết quả | 
| --- | --- | --- | 
| 1 | 0 | 0 | 
| 2 | 1 | 1 | 

Đầu ra:```
0 1
```Ví dụ này hiển thị đầu vào nhỏ nhất có thể và xác nhận rằng công thức tái cấu trúc hoạt động ngay cả khi chỉ tồn tại một ràng buộc XOR. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log V)$| Mỗi lần chèn tiền tố và mỗi truy vấn đều vượt qua tất cả các cấp độ trie | 
| Không gian |$O(n \log V)$| Các nút Trie được tạo trong khi lưu trữ tất cả các tiền tố | 

Đối với vấn đề này,$V$ở dưới$2^{21}$, Vì thế$\log V$thực chất là một hằng số nhỏ. Với$n \le 2 \cdot 10^5$, giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    class TrieNode:
        __slots__ = ("ch",)

        def __init__(self):
            self.ch = [-1, -1]

    trie = [TrieNode()]

    def add(x):
        node = 0
        for bit in range(20, -1, -1):
            b = (x >> bit) & 1
            if trie[node].ch[b] == -1:
                trie[node].ch[b] = len(trie)
                trie.append(TrieNode())
            node = trie[node].ch[b]

    def max_xor(x):
        node = 0
        ans = 0
        for bit in range(20, -1, -1):
            b = (x >> bit) & 1
            want = b ^ 1
            if trie[node].ch[want] != -1:
                ans |= 1 << bit
                node = trie[node].ch[want]
            else:
                node = trie[node].ch[b]
        return ans

    n = int(input())
    a = list(map(int, input().split()))

    pref = [0]
    cur = 0
    for x in a:
        cur ^= x
        pref.append(cur)

    for x in pref:
        add(x)

    start = 0
    for x in range(n):
        if max_xor(x) == n - 1:
            start = x
            break

    ans = [start ^ p for p in pref]
    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    global input
    input = sys.stdin.readline

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("4\n2 1 2\n") == "0 2 3 1"

# minimum size
assert run
```
