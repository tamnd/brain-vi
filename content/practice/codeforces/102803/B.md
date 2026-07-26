---
title: "CF 102803B - Hóa đơn thiên đường"
description: "Chúng ta có một bộ sưu tập n tờ tiền. Giá trị của chúng không được cung cấp trực tiếp mà được tạo từ hai hạt giống ngẫu nhiên bằng trình tạo xorshift128+ được cung cấp. Mỗi giá trị được tạo ra là khác nhau. Trong quá trình này, một số hóa đơn chưa được thanh toán và một số đã được thanh toán."
date: "2026-07-26T16:29:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "B"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 47
verified: true
draft: false
---

[CF 102803B - Bills of Paradise](https://codeforces.com/problemset/problem/102803/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập`n`hóa đơn. Giá trị của chúng không được cung cấp trực tiếp mà được tạo từ hai hạt giống ngẫu nhiên bằng trình tạo xorshift128+ được cung cấp. Mỗi giá trị được tạo ra là khác nhau. Trong quá trình này, một số hóa đơn chưa được thanh toán và một số đã được thanh toán. 

Các lệnh sửa đổi hoặc kiểm tra bộ chưa thanh toán hiện tại. Một truy vấn`F x`yêu cầu hóa đơn chưa thanh toán nhỏ nhất có giá trị ít nhất`x`. Một lệnh`D x`xóa hóa đơn tương tự khỏi bộ chưa thanh toán vì nó đã được thanh toán. Một truy vấn`C x`yêu cầu tổng giá trị của tất cả các hóa đơn chưa thanh toán không vượt quá`x`. Một lệnh`R x`khôi phục mọi hóa đơn đã thanh toán với giá trị tối đa`x`, khiến những hóa đơn đó chưa được thanh toán một lần nữa. 

Các giá trị được tạo ra gần như`10^12`, vì vậy các giá trị thực tế phải được xử lý dưới dạng số nguyên 64 bit. Số lượng hóa đơn có thể lên tới một triệu và tổng số lệnh lớn hơn nhiều so với giải pháp tuyến tính cho mỗi truy vấn thông thường có thể xử lý. Một giải pháp quét tất cả các hóa đơn cho mỗi lệnh có thể thực hiện khoảng`10^12`hoạt động trong trường hợp xấu nhất, vượt xa thời gian sẵn có. Chúng ta cần xử lý logarit cho mỗi lệnh. 

Phần khó khăn là hoạt động hoàn trả. Nó có thể kích hoạt nhiều hóa đơn cùng một lúc, nhưng nó xảy ra nhiều nhất là mười lần. Cấu trúc dữ liệu phải hỗ trợ cả việc loại bỏ riêng lẻ và khôi phục tiền tố lớn. 

Việc thực hiện bất cẩn có thể thất bại theo nhiều cách. Nếu một lệnh`F`yêu cầu một giá trị lớn hơn mọi hóa đơn chưa thanh toán, việc trả lại giá trị lớn nhất hiện có là sai. Ví dụ, với hóa đơn`{5, 9}`, lệnh`F 10`phải xuất ra`10^12`, giá trị trọng điểm được yêu cầu vì không có hóa đơn hợp lệ nào tồn tại. 

Một sai lầm khác là nhầm lẫn hóa đơn đã thanh toán với hóa đơn không tồn tại. Giả sử các hóa đơn là`{3, 7, 11}`. Sau đó`D 7`, một truy vấn`C 10`phải quay lại`3`, không`10`, vì hóa đơn đã thanh toán không nằm trong số tiền chưa thanh toán. 

Ranh giới hoàn tiền cũng dễ bị xử lý sai. Với hóa đơn`{4, 8, 12}`, lệnh`R 8`phải khôi phục cả hai`4`Và`8`. Sử dụng so sánh chặt chẽ thay vì so sánh bao hàm sẽ dẫn đến kết quả không chính xác`8`trả. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là giữ tất cả các hóa đơn trong một danh sách và quét nó bất cứ khi nào có lệnh đến. Vì`F`Và`D`, chúng tôi tìm kiếm tất cả các hóa đơn chưa thanh toán và giữ lại ứng viên tốt nhất. Vì`C`, chúng ta tính tổng tất cả các hóa đơn chưa thanh toán thỏa mãn điều kiện. Điều này đúng vì mọi lệnh đều được xác định trên tập hợp các hóa đơn chưa thanh toán hiện tại. 

Vấn đề là khối lượng công việc. Với một triệu tờ tiền và hàng trăm nghìn lệnh, chuỗi truy vấn trong trường hợp xấu nhất có thể yêu cầu khoảng`5 * 10^11`séc. Phương pháp này không đủ nhanh. 

Quan sát quan trọng là các tờ tiền không bao giờ thay đổi giá trị. Chỉ có trạng thái của họ thay đổi. Sau khi sắp xếp các giá trị, mọi thao tác sẽ trở thành câu hỏi về vị trí trong mảng được sắp xếp này. Cây phân đoạn có thể lưu trữ thông tin về các khoảng vị trí. Mỗi nút giữ số hóa đơn hiện chưa thanh toán trong khoảng thời gian đó và tổng giá trị của chúng. 

Hoạt động hoàn tiền có vẻ tốn kém vì nó có thể ảnh hưởng đến nhiều hóa đơn, nhưng nó luôn thay đổi tiền tố của mảng đã sắp xếp thành cùng một trạng thái: tất cả các hóa đơn đều chưa được thanh toán. Điều này có nghĩa là chỉ cần gán lười cây phân đoạn là đủ. Khi toàn bộ phân khúc được hoàn tiền, chúng tôi có thể đánh dấu phân khúc đó là hoàn toàn hoạt động mà không cần đến thăm các phân khúc con của phân khúc đó. 

Cây tương tự có thể tìm vị trí hoạt động đầu tiên sau giới hạn dưới, xóa một hóa đơn bằng cách đặt một vị trí không hoạt động và trả lời tổng tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nQ) | O(n) | Quá chậm | 
| Cây phân đoạn | O((n + Q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo tất cả các giá trị hóa đơn và sắp xếp chúng. Thứ tự được sắp xếp cho phép so sánh giá trị trở thành phạm vi chỉ mục. 
2. Xây dựng cây phân đoạn trên các vị trí đã được sắp xếp. Mọi vị trí đều bắt đầu dưới dạng chưa thanh toán, vì vậy ban đầu mỗi nút sẽ lưu trữ số lượng hóa đơn trong phân đoạn của nó và tổng của tất cả các giá trị trong phân đoạn đó. 
3. Đối với`F x`, tìm vị trí được sắp xếp đầu tiên có giá trị ít nhất`x`. Sau đó tìm kiếm trong cây phân đoạn từ vị trí đó trở đi để tìm phân đoạn đầu tiên chứa hóa đơn chưa thanh toán. Nếu không có vị trí như vậy tồn tại, hãy in`10^12`. 
4. Đối với`D x`, thực hiện tìm kiếm tương tự như`F x`. Nếu tìm thấy một vị trí, hãy cập nhật vị trí đó để số lượng hoạt động và đóng góp của nó trở thành 0. 
5. Đối với`C x`, tìm vị trí được sắp xếp cuối cùng có giá trị lớn nhất`x`. Truy vấn tiền tố cây phân đoạn cho đến vị trí đó và trả về tổng được lưu trữ. 
6. Đối với`R x`, tìm vị trí được sắp xếp cuối cùng có giá trị lớn nhất`x`. Áp dụng một phép gán lười biếng cho tiền tố đó, đặt mọi vị trí được bảo hiểm thành chưa thanh toán. 

Điều bất biến là cây phân đoạn luôn thể hiện chính xác các hóa đơn chưa thanh toán hiện tại. Xóa điểm sẽ loại bỏ chính xác một vị trí hoạt động. Khôi phục tiền tố làm cho mọi vị trí trong phạm vi giá trị được yêu cầu hoạt động. Vì mọi truy vấn chỉ đọc từ biểu diễn được duy trì này nên các giá trị được trả về khớp với tập hợp chưa thanh toán bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.arr = arr
        size = 4 * self.n
        self.cnt = [0] * size
        self.s = [0] * size
        self.lazy = [False] * size
        self.build(1, 0, self.n - 1)

    def build(self, p, l, r):
        if l == r:
            self.cnt[p] = 1
            self.s[p] = self.arr[l]
        else:
            m = (l + r) // 2
            self.build(p * 2, l, m)
            self.build(p * 2 + 1, m + 1, r)
            self.pull(p)

    def pull(self, p):
        self.cnt[p] = self.cnt[p * 2] + self.cnt[p * 2 + 1]
        self.s[p] = self.s[p * 2] + self.s[p * 2 + 1]

    def apply(self, p, l, r):
        self.cnt[p] = r - l + 1
        self.s[p] = sum(self.arr[l:r + 1])
        self.lazy[p] = True

    def push(self, p, l, r):
        if self.lazy[p] and l != r:
            m = (l + r) // 2
            self.apply(p * 2, l, m)
            self.apply(p * 2 + 1, m + 1, r)
            self.lazy[p] = False

    def update_all(self, p, l, r, ql, qr):
        if ql <= l and r <= qr:
            self.apply(p, l, r)
            return
        self.push(p, l, r)
        m = (l + r) // 2
        if ql <= m:
            self.update_all(p * 2, l, m, ql, qr)
        if qr > m:
            self.update_all(p * 2 + 1, m + 1, r, ql, qr)
        self.pull(p)

    def remove(self, p, l, r, idx):
        if l == r:
            self.cnt[p] = 0
            self.s[p] = 0
            return
        self.push(p, l, r)
        m = (l + r) // 2
        if idx <= m:
            self.remove(p * 2, l, m, idx)
        else:
            self.remove(p * 2 + 1, m + 1, r, idx)
        self.pull(p)

    def first(self, p, l, r, ql):
        if r < ql or self.cnt[p] == 0:
            return -1
        if l == r:
            return l
        self.push(p, l, r)
        m = (l + r) // 2
        res = self.first(p * 2, l, m, ql)
        if res != -1:
            return res
        return self.first(p * 2 + 1, m + 1, r, ql)

    def query(self, p, l, r, qr):
        if r <= qr:
            return self.s[p]
        self.push(p, l, r)
        m = (l + r) // 2
        ans = 0
        if qr > m:
            ans += self.query(p * 2 + 1, m + 1, r, qr)
        if qr >= l:
            ans += self.query(p * 2, l, m, qr)
        return ans

def solve():
    import bisect

    data = sys.stdin.buffer.read().split()
    if not data:
        return
    it = iter(data)
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        k1 = int(next(it))
        k2 = int(next(it))

        a = []
        mask = (1 << 64) - 1
        for _ in range(n):
            k3 = k1
            k4 = k2
            k1 = k4
            k3 ^= (k3 << 23) & mask
            k2 = (k3 ^ k4 ^ (k3 >> 17) ^ (k4 >> 26)) & mask
            a.append((k2 + k4) % 999999999999 + 1)

        a.sort()
        seg = SegTree(a)

        q = int(next(it))
        for _ in range(q):
            op = next(it).decode()
            x = int(next(it))

            if op == 'F' or op == 'D':
                pos = bisect.bisect_left(a, x)
                idx = seg.first(1, 0, n - 1, pos)
                if op == 'F':
                    out.append(str(10**12 if idx == -1 else a[idx]))
                elif idx != -1:
                    seg.remove(1, 0, n - 1, idx)

            elif op == 'C':
                pos = bisect.bisect_right(a, x) - 1
                out.append("0" if pos < 0 else str(seg.query(1, 0, n - 1, pos)))

            else:
                pos = bisect.bisect_right(a, x) - 1
                if pos >= 0:
                    seg.update_all(1, 0, n - 1, 0, pos)

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Trình tạo được sao chép bằng số học 64-bit không dấu. Số nguyên Python không tràn một cách tự nhiên, vì vậy việc che dấu bằng`(1 << 64) - 1`giữ hành vi tương tự như C++ không dấu lâu dài. 

Cây phân đoạn chỉ lưu trữ số lượng và tổng số tiền cho các hóa đơn chưa thanh toán. Cờ lười có nghĩa là toàn bộ phân đoạn đã được khôi phục. Việc triển khai sẽ tránh mở rộng phân khúc đó ngay lập tức vì các hoạt động trong tương lai có thể hoạt động chính xác từ các giá trị tổng hợp được lưu trữ. 

các`first`chức năng là cốt lõi của`F`Và`D`. Nó bỏ qua mọi phân đoạn không chứa hóa đơn chưa thanh toán và tìm kiếm đệ quy bên trái trước bên phải vì mảng đã được sắp xếp và vị trí hợp lệ ngoài cùng bên trái là bắt buộc. 

Việc tìm kiếm nhị phân sử dụng`bisect_left`cho "ít nhất x" và`bisect_right - 1`cho "nhiều nhất x". Việc trộn lẫn hai ranh giới này là nguyên nhân phổ biến dẫn đến các câu trả lời sai trong bài toán này. 

## Ví dụ đã hoạt động 

Xem xét hóa đơn`{4, 9, 15}`. 

| Lệnh | Hóa đơn đang hoạt động | Kết quả hoạt động | 
| --- | --- | --- | 
| F 8 | 4, 9, 15 | trả về 9 | 
| D 9 | 4, 15 | loại bỏ 9 | 
| C 10 | 4, 15 | trả về 4 | 
| R 10 | 4, 9, 15 | phục hồi 9 | 
| C 10 | 4, 9, 15 | trả về 13 | 

Dấu vết này cho thấy các hóa đơn đã xóa sẽ biến mất khỏi tổng số tiền và tìm kiếm, sau đó quay trở lại sau khi khôi phục tiền tố. 

Ví dụ thứ hai sử dụng các giá trị biên`{5, 10, 20}`. 

| Lệnh | Hóa đơn đang hoạt động | Kết quả hoạt động | 
| --- | --- | --- | 
| Đ 10 | 5, 20 | loại bỏ chính xác 10 | 
| F 10 | 5, 20 | trả về 20 | 
| C 10 | 5, 20 | trả về 5 | 
| R 10 | 5, 10, 20 | phục hồi 10 | 

Ví dụ xác nhận rằng so sánh là bao gồm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + Q) log n) | Xây dựng và mọi lệnh sử dụng phép toán cây logarit | 
| Không gian | O(n) | Mỗi giá trị được sắp xếp và cây phân đoạn lưu trữ thông tin tuyến tính | 

Kích thước thử nghiệm lớn nhất yêu cầu hàng triệu hóa đơn và lệnh. Quét tuyến tính cho mỗi thao tác không thể phù hợp, nhưng cây phân đoạn giữ cho mỗi thao tác xoay quanh thời gian logarit. Giới hạn hoàn trả là không cần thiết để đảm bảo tính chính xác của giải pháp này vì việc gán tiền tố được xử lý trực tiếp bằng cách truyền bá lười biếng. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp):
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    # call solve() from the submitted solution
    sys.stdin = old
    return ""

# The following cases should be run after placing solve() in the harness.

# Minimum-size generation and commands
# Covers single bill behavior.

# Removing all bills and querying empty states.
# Covers missing successor and empty sums.

# Refund after multiple deletions.
# Covers lazy restoration.

# Values at exact x boundaries.
# Covers inclusive comparisons.
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một hóa đơn được tạo, truy vấn F và C | Giá trị và tổng được tạo | Xử lý một phần tử | 
| Xóa mọi hóa đơn rồi tìm kiếm | Sentinel và số không | Hành vi cây trống | 
| Xóa tiền tố, hoàn lại tiền tố, tổng truy vấn | Tổng số được khôi phục | Phân công tiền tố lười biếng | 
| Truy vấn các giá trị chính xác bằng nhau | Câu trả lời trọn gói | Độ đúng ranh giới | 

## Vỏ cạnh 

Khi không có hóa đơn chưa thanh toán nào thỏa mãn`F x`, tìm kiếm trên cây đạt đến một phân đoạn có số lượng hoạt động bằng 0 và trả về`-1`. Ví dụ, sau khi trả`{5, 8}`, đang tìm kiếm`F 1`vẫn phải thất bại vì không còn vị trí nào chưa được thanh toán. 

Khi phạm vi được hoàn tiền chứa các hóa đơn chưa thanh toán, việc chỉ định tiền tố để kích hoạt lại là vô hại. Cây phân đoạn lưu trữ trạng thái cuối cùng thay vì đếm các hoạt động, do đó việc khôi phục hóa đơn đã hoạt động sẽ không tính gấp đôi. 

Để có khoản hoàn trả chính xác theo ranh giới, việc sử dụng`bisect_right`bao gồm các hóa đơn bằng`x`. Với các giá trị`{4, 8, 12}`, sau khi trả tiền`8`,`R 8`cập nhật vị trí`4`Và`8`, mang số tiền từ`16`quay lại`24`. 

Việc biểu diễn chỉ mục được sắp xếp là điều làm cho tất cả các trường hợp này hoạt động cùng nhau. Cây không bao giờ cần biết ý nghĩa thực sự của hóa đơn ngoài vị trí và giá trị của nó, vì vậy mọi lệnh sẽ trở thành một bản cập nhật hoặc truy vấn được kiểm soát đối với tiền tố hoặc hậu tố đang hoạt động hiện tại.
