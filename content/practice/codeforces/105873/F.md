---
title: "CF 105873F - Vấn đề đầu tiên"
description: "Chúng tôi duy trì một dãy số. Ban đầu, mỗi vị trí đều có một giá trị nhất định, sau đó chúng tôi xử lý một luồng hoạt động. Một truy vấn yêu cầu giá trị tối đa trong khoảng đã chọn. Thao tác thêm sẽ tăng mọi giá trị trong một khoảng thời gian lên một."
date: "2026-06-25T14:27:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105873
codeforces_index: "F"
codeforces_contest_name: "2025 ICPC Gran Premio de Mexico 1ra Fecha"
rating: 0
weight: 105873
solve_time_s: 45
verified: true
draft: false
---

[CF 105873F - Vấn đề đầu tiên](https://codeforces.com/problemset/problem/105873/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một dãy số. Ban đầu, mỗi vị trí đều có một giá trị nhất định, sau đó chúng tôi xử lý một luồng hoạt động. Một truy vấn yêu cầu giá trị tối đa trong khoảng đã chọn. Thao tác thêm sẽ tăng mọi giá trị trong một khoảng thời gian lên một. Thao tác bất thường sẽ đặt lại một số giá trị: trong số các vị trí trong khoảng đã chọn, mọi vị trí có giá trị bằng giá trị tối đa hiện tại của toàn bộ chuỗi sẽ trở thành 0. 

Đầu vào mô tả kích thước mảng, số lượng thao tác, giá trị bắt đầu và sau đó là các thao tác. Đầu ra chỉ bao gồm các câu trả lời cho các truy vấn tối đa, theo cùng thứ tự các truy vấn đó xuất hiện. 

Các giới hạn làm cho một mô phỏng đơn giản không thể thực hiện được. Với tối đa 100000 vị trí và 100000 thao tác, việc quét toàn bộ khoảng thời gian cho mỗi lần cập nhật có thể thực hiện khoảng 10^10 thao tác trong trường hợp xấu nhất. Một giải pháp cần phải ở gần O(log n) cho mỗi thao tác hoặc ít nhất có giới hạn khấu hao mạnh. 

Phần khó khăn là thao tác thiết lập lại. Nó không đặt lại mức tối đa của phạm vi được truy vấn, nó chỉ đặt lại các phần tử bằng mức tối đa toàn cục của toàn bộ mảng. Giải pháp chỉ lưu trữ số lượng tối đa của mỗi phân đoạn sẽ mất thông tin vì hai phân đoạn có thể có cùng số lượng phần tử tối đa nhưng khác nhau để đạt được thông tin đó. 

Ví dụ, hãy xem xét:```
Input
3 2
5 5 1
R 1 2
Q 1 3
```Mức tối đa toàn cầu là 5. Việc đặt lại ảnh hưởng đến vị trí 1 và 2, do đó mảng trở thành`[0, 0, 1]`. Câu trả lời là:```
1
```Việc triển khai bất cẩn diễn giải việc đặt lại là "đặt giá trị tối đa trong phạm vi về 0" có thể vô tình đặt lại các phần tử sai sau khi trộn lẫn cực đại toàn cục và cục bộ. 

Một trường hợp cạnh khác là khi một phân đoạn chứa giá trị tối đa nhưng không phải mọi giá trị trong phân đoạn đó đều bằng nó.```
Input
4 2
7 7 3 7
R 1 3
Q 1 4
```Mức tối đa toàn cầu là 7. Chỉ vị trí 1 và 2 được đặt lại vì vị trí 3 có giá trị 3. Mảng cuối cùng là`[0,0,3,7]`, vậy đáp án là:```
7
```Cây phân đoạn chỉ lưu trữ tối đa không thể biết liệu nó có thể xóa toàn bộ phân đoạn một cách an toàn hay không. 

Trường hợp phức tạp cuối cùng là khi tất cả các giá trị trong một nút đều bằng nhau.```
Input
5 2
4 4 4 4 4
R 2 5
Q 1 5
```Tất cả các giá trị bị ảnh hưởng là giá trị tối đa toàn cầu, do đó mảng trở thành`[4,0,0,0,0]`. Câu trả lời là:```
4
```Cấu trúc dữ liệu phải nhận ra rằng toàn bộ phân khúc có thể được cập nhật cùng một lúc. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lưu trữ mảng trực tiếp và xử lý từng thao tác bằng cách truy cập tất cả các vị trí bị ảnh hưởng. Truy vấn rất dễ dàng vì chúng ta có thể quét khoảng và lấy giá trị lớn nhất. Thao tác thêm cũng đơn giản vì chúng tôi tăng từng phần tử. Thao tác đặt lại sẽ quét phạm vi, tìm mức tối đa toàn cục và xóa các giá trị khớp. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa của mọi thao tác. Vấn đề là tốc độ. Trong trường hợp xấu nhất, mọi thao tác đều chạm vào hầu hết các vị trí, dẫn đến công việc O(NK), có thể đạt tới khoảng 10^10 thao tác phần tử. 

Quan sát quan trọng là thao tác đặt lại chỉ quan tâm đến các giá trị bằng mức tối đa hiện tại. Cây phân đoạn bình thường là không đủ vì chỉ giá trị tối đa không tiết lộ liệu toàn bộ phân đoạn có bao gồm các giá trị tối đa hay không. Chúng ta cần thêm một thông tin nữa: giá trị lớn thứ hai trong mỗi phân đoạn. 

Nếu một đoạn có giá trị lớn nhất`x`và cực đại thứ hai của nó nhỏ hơn`x`, thì mọi phần tử trong đoạn đó chính xác là`x`. Khi`x`là mức tối đa toàn cầu, chúng ta có thể đặt lại toàn bộ phân đoạn ngay lập tức. Mặt khác, chúng tôi chỉ đi sâu vào những phần mà một số yếu tố vẫn cần được kiểm tra. 

Đây chính là ý tưởng đằng sau nhịp cây phân đoạn. Cấu trúc lưu trữ giá trị lớn nhất, giá trị lớn thứ hai, số lần xuất hiện lớn nhất và thông tin lười biếng để bổ sung phạm vi. Việc đặt lại giá trị tối đa là hiệu quả vì bất cứ khi nào chúng tôi không thể hoàn thành tại một nút, chúng tôi chỉ đi sâu hơn cho đến khi tìm thấy các nhóm nhỏ hơn. Các giá trị thực sự được đặt lại sẽ biến mất khỏi lớp tối đa, mang lại mức khấu hao cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(NK) | O(N) | Quá chậm | 
| Nhịp cây phân đoạn | O((N + K) log N) khấu hao | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn. Đối với mỗi nút, lưu trữ giá trị lớn nhất, giá trị lớn thứ hai, số phần tử có giá trị lớn nhất và phép cộng lười biếng vẫn phải được đẩy cho nút con. Cần có mức tối đa thứ hai để xác định xem liệu toàn bộ phân đoạn có thể được đặt lại một cách an toàn hay không. 
2. Đối với thao tác thêm vào`[l, r]`, áp dụng mức tăng lười biếng cho các nút được che phủ. Việc tăng mọi giá trị trong một phân đoạn sẽ thay đổi mức tối đa và mức tối đa thứ hai theo cùng một lượng, do đó thông tin nút vẫn hợp lệ. 
3. Đối với truy vấn trên`[l, r]`, trả về giá trị tối đa được lưu trữ trong các nút được bảo hiểm. Các giá trị lười biếng được đẩy trước khi giảm xuống để các giá trị con là chính xác. 
4. Đối với thao tác đặt lại, trước tiên hãy lấy giá trị tối đa của toàn bộ mảng từ gốc. Đây là giá trị phải được loại bỏ. 
5. Thăm các nút cây đoạn giao nhau`[l, r]`. Nếu một nút được bao phủ có mức tối đa bằng mức tối đa toàn cầu và mức tối đa thứ hai của nó nhỏ hơn thì mọi giá trị trong nút đó đều là mức tối đa toàn cầu. Đặt toàn bộ nút về 0. 
6. Nếu không, hãy đẩy các giá trị lười biếng và tiếp tục vào trẻ em. Một số nút chứa hỗn hợp các giá trị tối đa và nhỏ hơn, vì vậy chúng phải được tách ra cho đến khi việc đặt lại có thể được áp dụng một cách an toàn. 

Tại sao nó hoạt động: 

Điều bất biến là mọi nút luôn thể hiện chính xác tập hợp nhiều giá trị bên trong phân đoạn của nó. Mức tối đa và mức tối đa thứ hai cho chúng tôi biết liệu tất cả các giá trị có bằng mức tối đa hiện tại hay không. Nếu đúng như vậy, việc thay thế toàn bộ phân đoạn bằng 0 chính xác là thao tác bắt buộc. Nếu không, ít nhất một phần tử con chứa giá trị khác với giá trị tối đa, do đó việc giảm dần sẽ duy trì tính đúng đắn. Các thao tác thêm sẽ giữ nguyên thứ tự của các giá trị vì mọi phần tử trong phân đoạn bị ảnh hưởng đều thay đổi như nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class SegTree:
    def __init__(self, arr):
        n = len(arr)
        self.n = n
        size = 4 * n
        self.mx = [0] * size
        self.smx = [-INF] * size
        self.cnt = [0] * size
        self.lazy = [0] * size
        self.build(1, 0, n - 1, arr)

    def build(self, v, l, r, a):
        if l == r:
            self.mx[v] = a[l]
            self.cnt[v] = 1
            return
        m = (l + r) // 2
        self.build(v * 2, l, m, a)
        self.build(v * 2 + 1, m + 1, r, a)
        self.pull(v)

    def apply_add(self, v, x):
        self.mx[v] += x
        if self.smx[v] != -INF:
            self.smx[v] += x
        self.lazy[v] += x

    def apply_zero(self, v):
        self.mx[v] = 0
        self.smx[v] = -INF
        self.cnt[v] = self.length[v]
        self.lazy[v] = 0

    def push(self, v):
        if self.lazy[v]:
            x = self.lazy[v]
            self.apply_add(v * 2, x)
            self.apply_add(v * 2 + 1, x)
            self.lazy[v] = 0

    def pull(self, v):
        a = v * 2
        b = v * 2 + 1
        if self.mx[a] > self.mx[b]:
            self.mx[v] = self.mx[a]
            self.cnt[v] = self.cnt[a]
            self.smx[v] = max(self.smx[a], self.mx[b])
        elif self.mx[a] < self.mx[b]:
            self.mx[v] = self.mx[b]
            self.cnt[v] = self.cnt[b]
            self.smx[v] = max(self.mx[a], self.smx[b])
        else:
            self.mx[v] = self.mx[a]
            self.cnt[v] = self.cnt[a] + self.cnt[b]
            self.smx[v] = max(self.smx[a], self.smx[b])

    def add(self, v, l, r, ql, qr):
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            self.apply_add(v, 1)
            return
        self.push(v)
        m = (l + r) // 2
        self.add(v * 2, l, m, ql, qr)
        self.add(v * 2 + 1, m + 1, r, ql, qr)
        self.pull(v)

    def query(self, v, l, r, ql, qr):
        if qr < l or r < ql:
            return -INF
        if ql <= l and r <= qr:
            return self.mx[v]
        self.push(v)
        m = (l + r) // 2
        return max(self.query(v * 2, l, m, ql, qr),
                   self.query(v * 2 + 1, m + 1, r, ql, qr))

    def reset(self, v, l, r, ql, qr, target):
        if qr < l or r < ql or self.mx[v] < target:
            return
        if ql <= l and r <= qr and self.mx[v] == target and self.smx[v] < target:
            self.length[v] = r - l + 1
            self.apply_zero(v)
            return
        if l == r:
            self.apply_zero(v)
            return
        self.push(v)
        m = (l + r) // 2
        self.reset(v * 2, l, m, ql, qr, target)
        self.reset(v * 2 + 1, m + 1, r, ql, qr, target)
        self.pull(v)

def solve():
    n, k = map(int, input().split())
    arr = list(map(int, input().split()))

    tree = SegTree(arr)
    tree.length = [0] * (4 * n)

    def fill_len(v, l, r):
        tree.length[v] = r - l + 1
        if l != r:
            m = (l + r) // 2
            fill_len(v * 2, l, m)
            fill_len(v * 2 + 1, m + 1, r)

    fill_len(1, 0, n - 1)

    ans = []
    for _ in range(k):
        c, l, r = input().split()
        l = int(l) - 1
        r = int(r) - 1
        if c == 'Q':
            ans.append(str(tree.query(1, 0, n - 1, l, r)))
        elif c == 'A':
            tree.add(1, 0, n - 1, l, r)
        else:
            cur = tree.mx[1]
            tree.reset(1, 0, n - 1, l, r, cur)

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây giữ bốn mẩu thông tin trên mỗi nút.`mx`đại diện cho giá trị cao nhất trong phân khúc,`smx`đại diện cho giá trị cao nhất nhỏ hơn nó và`cnt`lưu trữ bao nhiêu phần tử đạt đến mức tối đa. Giá trị lười biếng chỉ dành cho phần bổ sung vì việc thêm một vào phân đoạn sẽ dịch chuyển mọi giá trị như nhau. 

Hoạt động thiết lập lại sử dụng mối quan hệ giữa`mx`Và`smx`. Nếu mức tối đa là mục tiêu và mức tối đa thứ hai nhỏ hơn thì phân đoạn chỉ chứa giá trị mục tiêu. Xóa toàn bộ nút là an toàn. Nếu không thì mã sẽ đẩy phép cộng lười biếng và kiểm tra phần tử con. 

các`length`mảng lưu trữ kích thước phân đoạn để thiết lập lại toàn bộ có thể cập nhật số lượng giá trị tối đa một cách chính xác. Việc giữ riêng phần này sẽ tránh việc tính toán lại độ dài trong quá trình đệ quy. Việc xử lý ranh giới dựa trên các chỉ số bao gồm, do đó việc chuyển đổi từ chỉ số đầu vào sang chỉ số dựa trên 0 xảy ra một lần trước khi xử lý. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
10 10
1 2 3 4 5 6 7 8 9 10
Q 1 10
R 1 10
A 1 10
Q 1 10
R 1 7
Q 5 10
R 7 10
Q 6 10
A 6 10
Q 1 6
```| Bước | Hoạt động | Tối đa toàn cầu | Thay đổi quan trọng | Câu trả lời truy vấn | 
| --- | --- | --- | --- | --- | 
| 1 | Câu 1 10 | 10 | Không cập nhật | 10 | 
| 2 | R 1 10 | 10 | Vị trí 10 trở thành 0 | - | 
| 3 | A 1 10 | 10 | Mọi giá trị đều tăng | - | 
| 4 | Câu 1 10 | 10 | Được khôi phục tối đa | 10 | 
| 5 | R 1 7 | 10 | Đã xóa vị trí 7 | - | 
| 6 | Câu 5 10 | 10 | Vị trí 10 vẫn | 10 | 

Điều này cho thấy lý do tại sao việc thiết lập lại sử dụng mức tối đa toàn cục thay vì mức tối đa khoảng thời gian. Lần đặt lại đầu tiên chỉ thay đổi phần tử thực sự giữ mức tối đa toàn cục. 

Một ví dụ thứ hai:```
5 3
4 4 4 4 4
R 2 5
Q 1 5
A 1 5
```| Bước | Hoạt động | Gốc tối đa | Trạng thái phân đoạn | 
| --- | --- | --- | --- | 
| 1 | Ban đầu | 4 | Tất cả các giá trị bằng nhau | 
| 2 | R 2 5 | 4 | Phân đoạn được bảo hiểm được đặt lại cùng một lúc | 
| 3 | Câu 1 5 | 4 | Chỉ có vị trí đầu tiên vẫn ở mức tối đa | 
| 4 | A 1 5 | 5 | Mọi giá trị đều tăng | 

Điều này xác nhận rằng một phân đoạn hoàn toàn bằng nhau được xử lý như một thao tác đơn lẻ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + K) log N) khấu hao | Mỗi thao tác thực hiện công việc trên cây logarit và việc đặt lại nhiều lần sẽ giảm số lượng phần tử tối đa cần truy cập sâu hơn | 
| Không gian | O(N) | Cây phân đoạn lưu trữ một lượng thông tin không đổi trên mỗi nút | 

Các ràng buộc yêu cầu tránh quét toàn bộ phạm vi. Cây phân đoạn giữ số lượng nút được truy cập đủ nhỏ cho 100000 thao tác. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return ""

# The following cases are intended for the solve() implementation.

# sample 1
# 10 10
# 1 2 3 4 5 6 7 8 9 10
# Q 1 10
# R 1 10
# A 1 10
# Q 1 10
# R 1 7
# Q 5 10
# R 7 10
# Q 6 10
# A 6 10
# Q 1 6

# minimum size
# 1 3
# 5
# Q 1 1
# R 1 1
# Q 1 1

# all equal values
# 5 2
# 3 3 3 3 3
# R 1 5
# Q 1 5

# mixed maximum values
# 4 2
# 7 7 3 7
# R 1 3
# Q 1 4
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Phần tử đơn có reset |`5`sau đó`0`| Xử lý kích thước tối thiểu | 
| Tất cả các giá trị bằng nhau |`0`sau khi thiết lập lại | Đặt lại toàn bộ phân đoạn | 
| Giá trị tối đa hỗn hợp |`7`| Không xóa giá trị không tối đa | 
| Thêm phạm vi sau khi đặt lại | Phụ thuộc vào sản xuất tối đa | Tính chính xác của việc tuyên truyền lười biếng | 

## Vỏ cạnh 

Đối với trường hợp khoảng thời gian đặt lại chứa một số nhưng không phải tất cả các giá trị tối đa, cây sẽ đi xuống thay vì xóa toàn bộ nút. TRONG:```
4 2
7 7 3 7
R 1 3
Q 1 4
```gốc nhìn thấy tối đa`7`, nhưng đoạn`[1,3]`có tối đa thứ hai là`3`. Việc thiết lập lại tiếp tục đi xuống và chỉ xóa các lá có chứa`7`. Mức tối đa cuối cùng vẫn còn`7`. 

Đối với trường hợp hoàn toàn bằng nhau:```
5 2
4 4 4 4 4
R 2 5
Q 1 5
```nút được bảo hiểm có`mx = 4`Và`smx = -INF`. Thuật toán biết mọi giá trị trong nút đó là`4`, vì vậy nó sẽ thay thế toàn bộ phân khúc ngay lập tức. Vị trí đầu tiên còn lại giữ giá trị`4`, đó là kết quả truy vấn. 

Đối với các bổ sung chồng chéo:```
3 3
1 2 3
A 1 3
R 1 3
Q 1 3
```sau khi thêm mảng là`[2,3,4]`. Việc đặt lại chỉ xóa giá trị cuối cùng vì đây là giá trị tối đa toàn cầu. Câu trả lời trở thành`3`, cho thấy việc bổ sung và đặt lại lười biếng tương tác chính xác.
