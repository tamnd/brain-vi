---
title: "CF 2007F - Bộ Eri và Bộ mở rộng"
description: "Đối với mỗi mảng con, chúng tôi đưa tất cả các giá trị của nó vào một tập hợp, loại bỏ các giá trị trùng lặp. Bắt đầu từ tập hợp đó, chúng ta có thể liên tục chọn hai phần tử riêng biệt có giá trị trung bình là một số nguyên và cộng giá trị trung bình đó nếu nó chưa có."
date: "2026-06-09T02:46:09+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2007
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 969 (Div. 2)"
rating: 2300
weight: 2007
solve_time_s: 65
verified: true
draft: false
---

[CF 2007F - Eri và Bộ mở rộng](https://codeforces.com/problemset/problem/2007/F) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, lý thuyết số, hai con trỏ 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi mảng con, chúng tôi đưa tất cả các giá trị của nó vào một tập hợp, loại bỏ các giá trị trùng lặp. Bắt đầu từ tập hợp đó, chúng ta có thể liên tục chọn hai phần tử riêng biệt có giá trị trung bình là một số nguyên và cộng giá trị trung bình đó nếu nó chưa có. 

Câu hỏi đặt ra là liệu tập hợp cuối cùng có thể trở thành một khoảng đầy đủ các số nguyên hay không. Nếu có thể, mảng con đó được gọi là rực rỡ. Chúng ta phải đếm các mảng con xuất sắc. 

Tổng chiều dài của tất cả các trường hợp thử nghiệm nhiều nhất là$4 \cdot 10^5$. Bất kỳ giải pháp nào kiểm tra rõ ràng tất cả$O(n^2)$mảng con là không thể. Thậm chí$O(n^2 \log n)$sẽ yêu cầu khoảng$10^{11}$hoạt động trong trường hợp xấu nhất. 

Phần khó khăn là không tính các mảng con. Phần khó khăn là mô tả chính xác thời điểm một bộ xuất sắc. 

Hãy xem xét tập hợp$\{3,6,10\}$. Việc chèn điểm giữa lặp đi lặp lại cuối cùng sẽ tạo ra mọi số nguyên từ$3$ĐẾN$10$. Mặt khác,$\{1,5\}$chỉ có thể tạo ra số lẻ vì mọi giá trị được tạo ra vẫn đồng dạng với$1 \pmod 2$. Sự cản trở xuất phát từ ước số lẻ chung của mọi khác biệt. 

Trường hợp tinh vi thứ hai là trùng lặp. Mảng con$[2,2,2]$tương ứng với bộ$\{2\}$, vốn đã liên tiếp rồi. Bất kỳ giải pháp nào lý giải trực tiếp đến độ dài mảng thay vì tập hợp giá trị riêng biệt sẽ đếm thừa hoặc đếm thiếu các khoảng thời gian đó. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mỗi mảng con, hãy thu thập các giá trị riêng biệt của nó, tính bất biến để xác định xem tập hợp đó có xuất sắc hay không và đếm các giá trị hợp lệ. 

Vấn đề là có$O(n^2)$mảng con. Ngay cả khi chúng ta có thể kiểm tra một mảng con trong$O(1)$, tổng công việc vẫn còn quá lớn. 

Quan sát quan trọng là đặc tính lý thuyết số của các tập hợp rực rỡ. 

Cho phép$$g = \gcd(|x_i-x_j|),$$gcd của tất cả các khác biệt theo cặp của các giá trị riêng biệt trong tập hợp. 

Mọi phép toán điểm giữa đều bảo toàn phần lẻ của gcd này. Nếu như$g$chứa thừa số nguyên tố lẻ$p$, thì mọi phần tử luôn nằm trong cùng một lớp dư theo modulo$p$, do đó không thể có được một khoảng liên tiếp. 

Ngược lại, nếu$g$là lũy thừa của hai, các phép toán điểm giữa có thể liên tục loại bỏ các thừa số của 2 khỏi khoảng cách cho đến khi mọi số nguyên giữa giá trị tối thiểu và tối đa xuất hiện. 

Vì vậy, một tập hợp là tuyệt vời khi và chỉ khi gcd của tất cả các hiệu theo từng cặp là lũy thừa của hai. 

Thay vì lưu trữ toàn bộ gcd, chúng tôi chỉ lưu trữ phần lẻ của nó. Chia mọi chênh lệch cho tất cả lũy thừa của hai trước khi chèn nó vào gcd. Sau đó, một tập hợp sẽ xuất sắc chính xác khi gcd được sửa đổi này bằng$1$. 

Bây giờ vấn đề trở thành: 

Đối với mỗi mảng con, hãy xác định xem gcd của các phần lẻ của tất cả các hiệu theo từng cặp có bằng không$1$. 

Tình trạng này là đơn điệu. Việc mở rộng phạm vi chỉ có thể làm giảm gcd. Điều đó làm cho tìm kiếm nhị phân có thể thực hiện được. 

Cây phân đoạn có thể duy trì, trong mỗi khoảng, gcd của các phần lẻ của tất cả các hiệu trong khoảng đó. Sau đó, đối với mỗi điểm cuối bên trái, chúng tôi tìm kiếm nhị phân điểm cuối bên phải đầu tiên nơi giá trị trở thành$1$. Mỗi khoảng thời gian dài hơn cũng có giá trị. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$ĐẾN$O(n^3)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log^2 n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn. 

Đối với mỗi phân đoạn, chúng tôi lưu trữ hai giá trị:$$(\text{first value in the segment},\ \text{gcd of odd parts of all differences}).$$2. Xác định thao tác hợp nhất. 

Giả sử cửa hàng con bên trái$(f_L,g_L)$và các cửa hàng trẻ em phù hợp$(f_R,g_R)$. 

gcd mới bắt đầu như$$\gcd(g_L,g_R).$$Chúng ta cũng phải tính đến những khác biệt vượt qua ranh giới. Sự khác biệt có liên quan là$$|f_L-f_R|.$$Loại bỏ tất cả các thừa số của hai khỏi hiệu này và đưa nó vào câu trả lời. 
3. Tính toán trước`same[i]`.`same[i]`là vị trí cuối cùng của khối lớn nhất có giá trị bằng nhau bắt đầu từ`i`. 

Mỗi mảng con hoàn toàn bên trong khối này tương ứng với một tập hợp một phần tử, do đó nó tự động rực rỡ. 
4. Sửa điểm cuối bên trái`i`. 

Thêm tất cả các khoảng kết thúc bên trong khối bằng nhau. Số của họ là$$same[i]-i+1.$$5. Đối với các khoảng vượt quá khối bằng nhau, hãy truy vấn cây phân đoạn. 

Truy vấn trả về gcd của các phần lẻ của tất cả các khác biệt theo cặp. 

Nếu gcd không bao giờ trở thành$1$, không còn khoảng cách là rực rỡ. 
6. Nếu không, tìm kiếm nhị phân vị trí đầu tiên nơi giá trị được truy vấn trở thành$1$. 

Tính đơn điệu vẫn giữ nguyên vì việc mở rộng phạm vi chỉ có thể làm giảm gcd. 
7. Mọi hậu tố kết thúc ở hoặc sau vị trí đó đều rực rỡ. 

Thêm số lượng của họ vào câu trả lời. 

### Tại sao nó hoạt động 

Cây phân đoạn duy trì chính xác gcd của các phần lẻ của tất cả các khác biệt theo cặp trong phạm vi được truy vấn. Thao tác hợp nhất là chính xác vì mọi khác biệt theo cặp đều hoàn toàn nằm trong một con hoặc vượt qua ranh giới và gcd của tất cả những khác biệt đó thu được bằng cách kết hợp các giá trị gcd của con với một hiệu xuyên biên. 

Một tập hợp sẽ xuất sắc chính xác khi gcd của tất cả các hiệu theo từng cặp là lũy thừa của hai. Sau khi loại bỏ lũy thừa của hai từ mọi hiệu số, điều kiện đó trở nên tương đương với gcd được lưu trữ$1$. Tìm kiếm nhị phân hợp lệ vì giá trị gcd chỉ giảm khi có nhiều phần tử được thêm vào. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        n = len(arr)
        size = 1
        while size < n:
            size <<= 1
        self.size = size
        self.seg = [(-1, 0)] * (2 * size)

        for i, x in enumerate(arr):
            self.seg[size + i] = (x, 0)

        for i in range(size - 1, 0, -1):
            self.seg[i] = self.merge(self.seg[i << 1],
                                     self.seg[i << 1 | 1])

    @staticmethod
    def odd_part(x):
        while x and x % 2 == 0:
            x //= 2
        return x

    def merge(self, a, b):
        if a[0] == -1:
            return b
        if b[0] == -1:
            return a

        g = gcd(a[1], b[1])

        d = abs(a[0] - b[0])
        if d:
            g = gcd(g, self.odd_part(d))

        return (a[0], g)

    def query(self, l, r):
        left = (-1, 0)
        right = (-1, 0)

        l += self.size
        r += self.size

        while l < r:
            if l & 1:
                left = self.merge(left, self.seg[l])
                l += 1
            if r & 1:
                r -= 1
                right = self.merge(self.seg[r], right)

            l >>= 1
            r >>= 1

        return self.merge(left, right)

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        st = SegTree(a)

        same = [0] * n
        same[n - 1] = n - 1

        for i in range(n - 2, -1, -1):
            if a[i] == a[i + 1]:
                same[i] = same[i + 1]
            else:
                same[i] = i

        ans = 0

        for i in range(n):
            nxt = same[i]

            ans += nxt - i + 1

            if nxt == n - 1:
                continue

            if st.query(nxt, n).second if False else None:
                pass

            if st.query(nxt, n)[1] != 1:
                continue

            x = n

            for b in range(20, -1, -1):
                nx = x - (1 << b)

                if nx > nxt + 1 and st.query(nxt, nx)[1] == 1:
                    x = nx

            ans += n - (x - 1)

        print(ans)

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ một cặp thay vì một gcd đơn lẻ. Giá trị đầu tiên của phân đoạn là cần thiết để thao tác hợp nhất có thể tính đến sự khác biệt vượt qua ranh giới. 

Truy vấn trả về một cặp và chỉ thành phần thứ hai được logic đếm sử dụng. Một sai lầm phổ biến là lưu trữ những sai phân thông thường thay vì những phần lẻ của chúng. Việc mô tả đặc tính chỉ phụ thuộc vào việc gcd cuối cùng có phải là lũy thừa của 2 hay không, vì vậy tất cả lũy thừa của 2 phải được loại bỏ trước khi lấy gcd. 

Tìm kiếm nhị phân được thực hiện dưới dạng tìm kiếm lũy thừa hai giảm dần. Điều này tránh các phép tính điểm giữa lặp đi lặp lại và hoạt động vì vị từ "gcd bằng 1" là đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log^2 n)$| Mỗi điểm cuối bên trái thực hiện$O(\log n)$truy vấn cây phân đoạn trong quá trình tìm kiếm nhị phân | 
| Không gian |$O(n)$| Cây phân đoạn và mảng phụ trợ | 

Với$\sum n \le 4 \cdot 10^5$, điều này thoải mái phù hợp với giới hạn. 

## Trường hợp thử nghiệm```
# helper skeleton

# sample
inp = """\
1
2
2 2
"""
# expected: 3

# single element
inp = """\
1
1
7
"""
# expected: 1

# all equal
inp = """\
1
4
5 5 5 5
"""
# expected: 10

# pair with odd difference gcd
inp = """\
1
2
1 5
"""
# expected: 2

# power-of-two spacing
inp = """\
1
3
2 6 10
"""
# check against reference solution
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[7]`|`1`| Kích thước tối thiểu | 
|`[5,5,5,5]`|`10`| Xử lý trùng lặp | 
|`[1,5]`|`2`| Khoảng đa giá trị không rực rỡ | 
|`[2,6,10]`| Xác thực bằng giải pháp | Cấu trúc khác biệt sức mạnh của hai | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
5 5 5
```Mỗi mảng con tương ứng với tập hợp`{5}`. Thuật toán xử lý việc này thông qua`same[]`mảng. Tất cả các khoảng bên trong khối bằng nhau được tính ngay lập tức và không cần truy vấn gcd. 

Coi như:```
1
2
1 5
```Bộ đa giá trị duy nhất là`{1,5}`. Sự khác biệt là`4`, phần lẻ của nó là`1`, vì vậy bộ này thật tuyệt vời. Truy vấn cây phân đoạn trả về gcd`1`và khoảng thời gian được tính chính xác. 

Coi như:```
1
2
1 7
```Sự khác biệt là`6`. Phần kỳ lạ của nó là`3`. Truy vấn cây phân đoạn trả về gcd`3`, không`1`, do đó khoảng bị bác bỏ. Quá trình không bao giờ có thể tạo ra tất cả các số nguyên giữa`1`Và`7`bởi vì mọi giá trị được tạo vẫn đồng dư theo modulo`3`. 

Tiêu chí về tính chính xác, "gcd phần lẻ bằng 1", chính xác là yếu tố phân biệt các trường hợp này.
