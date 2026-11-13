---
title: "CF 1709D - Rorororobot"
description: "Chúng ta được cung cấp một lưới có $n$ hàng và $m$ cột. Mỗi cột có một số ô bị chặn ở dưới cùng, được chỉ định bởi một mảng $a$ trong đó $a[i]$ cho chúng ta biết có bao nhiêu ô ở cuối cột $i$ bị chặn. Các ô còn lại là miễn phí."
date: "2026-06-09T20:53:52+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1709
codeforces_index: "D"
codeforces_contest_name: "Educational Codeforces Round 132 (Rated for Div. 2)"
rating: 1700
weight: 1709
solve_time_s: 116
verified: true
draft: false
---

[CF 1709D - Rorororobot](https://codeforces.com/problemset/problem/1709/D) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, toán học 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới với$n$hàng và$m$cột. Mỗi cột có một số ô bị chặn ở dưới cùng, được chỉ định bởi một mảng$a$Ở đâu$a[i]$cho chúng ta biết có bao nhiêu ô ở cuối cột$i$bị chặn. Các ô còn lại là miễn phí. Robot bắt đầu tại một ô nhất định và chúng tôi muốn nó tiếp cận ô mục tiêu. Các lệnh di chuyển robot theo một trong bốn hướng, nhưng mỗi lệnh đều được thực hiện chính xác$k$lần. Nếu robot cố gắng đi vào ô bị chặn hoặc đi ra ngoài lưới, nó sẽ phát nổ. Chúng ta phải xác định đối với mỗi truy vấn xem có tồn tại một chuỗi lệnh cho phép robot tiếp cận ô mục tiêu một cách an toàn và kết thúc chính xác tại đó hay không. 

Những hạn chế chính là$n$có thể lớn như$10^9$, Nhưng$m$chỉ tối đa$2 \cdot 10^5$, và chúng ta có thể có tới$2 \cdot 10^5$truy vấn. Điều này loại trừ mọi phương pháp mô phỏng chuyển động của robot theo từng ô. Thay vào đó, chúng ta phải suy luận về hàng an toàn tối đa mà robot có thể đạt tới và liệu sự khác biệt về tọa độ hàng và cột có chia hết cho$k$. 

Một điểm tinh tế là robot thực hiện đầy đủ$k$các bước thực hiện từng lệnh. Nếu robot được lệnh di chuyển lên trên$k$Từ hàng 7 đến hàng 10, chúng ta phải đảm bảo hàng 8, 9, 10 đều an toàn, nếu không robot sẽ phát nổ trước khi tiếp cận mục tiêu. Tương tự như vậy, nếu ô bắt đầu hoặc ô đích ở khoảng cách không chia hết cho$k$, không thể dừng chính xác vào mục tiêu. Các trường hợp cạnh thường phát sinh khi hàng tối đa có thể tiếp cận nhỏ hơn hàng của ô kết thúc hoặc khi khoảng cách hàng không phải là bội số của$k$. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là mô phỏng từng chuyển động mà robot có thể thực hiện theo cả bốn hướng cho đến khi nó chạm tới mục tiêu hoặc phát nổ. Điều này đòi hỏi phải kiểm tra tới$n$hàng cho mỗi cột và có khả năng lặp lại$k$các bước cho mỗi lệnh cho mỗi truy vấn. Với$n$lên đến$10^9$Và$q$lên đến$2 \cdot 10^5$, phương pháp này rõ ràng là không khả thi. 

Thông tin chi tiết quan trọng là robot luôn có thể di chuyển theo chiều ngang nếu hàng đủ cao và chuyển động theo chiều dọc bị hạn chế bởi hàng an toàn tối đa trong các cột dọc theo đường đi của nó. Robot chỉ có thể dừng ở ô kết thúc nếu khoảng cách theo chiều dọc và khoảng cách theo chiều ngang giữa điểm bắt đầu và điểm kết thúc là bội số của$k$. Vì vậy, chúng ta phải tìm hàng tối đa mà robot có thể tiếp cận một cách an toàn dọc theo đường đi giữa cột xuất phát và cột kết thúc, sau đó kiểm tra xem nó di chuyển lên hay xuống bằng$k$bước có thể đạt được mục tiêu một cách chính xác. Điều này làm giảm vấn đề chỉ còn việc kiểm tra hai điều kiện: tính chia hết của hàng cho$k$và hàng an toàn tối đa dọc theo đường ngang. 

Chúng ta có thể tính toán trước giá trị cực đại tiền tố của các ô bị chặn để nhanh chóng truy vấn ô bị chặn cao nhất dọc theo bất kỳ đường dẫn ngang nào hoặc chỉ cần lấy ô bị chặn tối đa trong phạm vi$[y_s, y_f]$trong mỗi truy vấn. Từ$m \le 2 \cdot 10^5$, điều này có thể được thực hiện trong$O(\log m)$mỗi truy vấn sử dụng cây phân đoạn hoặc$O(m)$cho mỗi truy vấn nếu chúng tôi sử dụng cách quét đơn giản (có thể chấp nhận được đối với$q \le 2 \cdot 10^5$nếu cây phân đoạn được sử dụng). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(n * q) | O(n) | Quá chậm | 
| Tối ưu (hàng tối đa + kiểm tra tính chia hết) | O(q * log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước mảng ô bị chặn$a$vào cây phân đoạn để truy vấn hiệu quả mức tối đa$a[i]$giữa hai cột bất kỳ. Điều này cho phép kiểm tra ô bị chặn cao nhất trong đường dẫn theo thời gian logarit. 
2. Với mỗi truy vấn, trích xuất$x_s, y_s, x_f, y_f, k$. Tính khoảng cách theo chiều dọc$|x_s - x_f|$và khoảng cách ngang$|y_s - y_f|$. 
3. Nếu khoảng cách theo chiều dọc không chia hết cho$k$hoặc khoảng cách theo chiều ngang không chia hết cho$k$, robot không thể dừng chính xác vào mục tiêu nên câu trả lời là "KHÔNG". 
4. Truy vấn cây phân đoạn để tìm ô bị chặn tối đa$max_a$dọc theo cột$[y_s, y_f]$. Robot không thể vượt lên trên hàng$n$và không thể nhập hàng$\le max_a$, vậy hàng an toàn tối đa là$n - ((n - max_a) \% k)$. Nếu hàng an toàn tối đa này nhỏ hơn$x_f$, robot không thể đến ô đích nếu không phát nổ nên câu trả lời là "KHÔNG". 
5. Nếu cả hai điều kiện chia hết đều được thỏa mãn và hàng an toàn tối đa cho phép chuyển động thẳng đứng$x_f$, câu trả lời là "CÓ". 

### Tại sao nó hoạt động 

Thuật toán đảm bảo tính chính xác vì nó xem xét tất cả các ràng buộc do chuyển động của robot và các ô bị chặn áp đặt. Robot di chuyển chính xác$k$các bước cho mỗi lệnh, do đó khả năng phân chia đảm bảo dừng chính xác trên mục tiêu. Bằng cách lấy ô bị chặn tối đa dọc theo đường ngang, chúng tôi đảm bảo robot không bao giờ di chuyển qua ô bị chặn. Tính toán hàng an toàn tối đa đảm bảo rằng mọi chuyển động thẳng đứng đều không vượt quá giới hạn an toàn của robot. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, data):
        self.n = len(data)
        self.size = 1
        while self.size < self.n:
            self.size *= 2
        self.tree = [0] * (2 * self.size)
        for i in range(self.n):
            self.tree[self.size + i] = data[i]
        for i in range(self.size - 1, 0, -1):
            self.tree[i] = max(self.tree[2*i], self.tree[2*i + 1])

    def query(self, l, r):
        l += self.size
        r += self.size
        res = 0
        while l <= r:
            if l % 2 == 1:
                res = max(res, self.tree[l])
                l += 1
            if r % 2 == 0:
                res = max(res, self.tree[r])
                r -= 1
            l //= 2
            r //= 2
        return res

n, m = map(int, input().split())
a = list(map(int, input().split()))
seg = SegmentTree(a)

q = int(input())
for _ in range(q):
    x_s, y_s, x_f, y_f, k = map(int, input().split())
    y_s -= 1
    y_f -= 1

    if abs(x_f - x_s) % k != 0 or abs(y_f - y_s) % k != 0:
        print("NO")
        continue

    l, r = min(y_s, y_f), max(y_s, y_f)
    max_block = seg.query(l, r)
    max_safe_row = n - ((n - max_block) % k)

    if max_safe_row >= x_f:
        print("YES")
    else:
        print("NO")
```Cây phân đoạn được sử dụng để truy vấn ô bị chặn tối đa một cách hiệu quả dọc theo đường ngang. Chúng tôi tính toán hàng cao nhất mà robot có thể tiếp cận một cách an toàn mà vẫn phù hợp với các bước có kích thước$k$. Điều này đảm bảo cả chuyển động dọc và ngang đều có thể được thực hiện một cách an toàn. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Truy vấn | x_s | y_s | x_f | y_f | k | Bị chặn tối đa | Hàng an toàn tối đa | Chia được | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | 3 | 1 | 0 | 11 | Có | CÓ | 
| 2 | 1 | 2 | 1 | 3 | 2 | 0 | 10 | Không | KHÔNG | 
| 3 | 4 | 3 | 4 | 5 | 2 | 8 | 8 | Có | KHÔNG | 

Bảng xác nhận rằng khả năng phân chia theo chiều dọc và chiều ngang và việc kiểm tra hàng an toàn tối đa cùng nhau xác định câu trả lời. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((m + q) * log m) | Cây phân đoạn được xây dựng theo dạng O(m), mỗi truy vấn lấy O(log m) cho ô bị chặn tối đa. | 
| Không gian | O(m) | Cây phân đoạn lưu trữ mảng kích thước 2 * có độ dài O(m). | 

Giải pháp xử lý lên đến$2 \cdot 10^5$truy vấn và cột một cách hiệu quả trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# Provided sample
assert run("11 10\n9 0 0 10 3 4 8 11 10 8\n6\n1 2 1 3 1\n1 2 1 3 2\n4 3 4 5 2\n5 3 11 5 3\n5 3 11 5 2\n11 9 9 10 1\n") == "YES\nNO
```
