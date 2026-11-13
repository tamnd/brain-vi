---
title: "CF 1690G - Đếm Đoàn Tàu"
description: "Mỗi cỗ xe có tốc độ tối đa riêng. Khi tất cả các toa xe bắt đầu chuyển động, một toa xe không thể di chuyển nhanh hơn bất kỳ toa xe nào phía trước nó, do đó tốc độ thực tế của nó trở thành giá trị tối thiểu nhìn thấy từ xa về phía bên trái."
date: "2026-06-09T23:23:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1690
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 797 (Div. 3)"
rating: 2000
weight: 1690
solve_time_s: 285
verified: false
draft: false
---

[CF 1690G - Đếm các chuyến tàu](https://codeforces.com/problemset/problem/1690/G) 

**Xếp hạng:** 2000 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 4 phút 45 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cỗ xe có tốc độ tối đa riêng. Khi tất cả các toa xe bắt đầu chuyển động, một toa xe không thể di chuyển nhanh hơn bất kỳ toa xe nào phía trước nó, do đó tốc độ thực tế của nó trở thành giá trị tối thiểu nhìn thấy từ xa về phía bên trái. 

Nếu tốc độ tối đa là```
a = [10, 13, 5, 2, 6]
```thì tốc độ cuối cùng là```
[10, 10, 5, 2, 2]
```bởi vì mọi cỗ xe đều bị giới hạn bởi cỗ xe chậm nhất trước nó. 

Một đoàn tàu là một đoạn liên tiếp tối đa có tốc độ cuối cùng bằng nhau. Trong ví dụ trên các đoàn tàu là```
[10,10] [5] [2,2]
```vậy đáp án là 3 

Sau đó, chúng tôi nhận được thông tin cập nhật. Một bản cập nhật sẽ giảm một phần tử của mảng. Sau mỗi lần cập nhật, chúng tôi phải báo cáo số lượng tàu mới. 

Nhận xét quan trọng là tốc độ cuối cùng của việc vận chuyển`i`là```
min(a[1], a[2], ..., a[i])
```Một chuyến tàu mới bắt đầu chính xác khi tiền tố tối thiểu này giảm xuống. 

Điều đó có nghĩa là số lượng tàu bằng với số lượng vị trí tạo tiền tố mới tối thiểu. 

Các ràng buộc đủ lớn để không thể tính toán lại tất cả các cực tiểu tiền tố sau mỗi lần cập nhật. Trên tất cả các trường hợp thử nghiệm, cả hai`n`Và`m`tổng hợp thành`10^5`, vì vậy chúng ta cần thứ gì đó xung quanh`O((n + m) log n)`. Bất kỳ giải pháp nào quét toàn bộ mảng sau mỗi lần cập nhật sẽ thực hiện khoảng`10^10`hoạt động trong trường hợp xấu nhất. 

Một điểm tinh tế là các bản cập nhật chỉ làm giảm giá trị. Không có gì tăng thêm. Sự đơn điệu này là điều tạo nên một cấu trúc năng động hiệu quả. 

Coi như:```
a = [5, 8, 6]
```Điểm xuất phát của tàu là các vị trí:```
1 (value 5)
```Bây giờ giảm`a[2]`ĐẾN`4`.```
a = [5, 4, 6]
```Vị trí 2 trở thành điểm bắt đầu chuyến tàu mới vì nó nhỏ hơn mọi giá trị trước đó. 

Một giải pháp bất cẩn chỉ cập nhật thông tin ở chỉ số 2 sẽ bỏ lỡ thực tế là ranh giới đoàn tàu ở xa hơn về phía bên phải có thể biến mất. 

Một trường hợp khó khăn khác là sự bình đẳng:```
a = [5, 5, 5]
```Chỉ có một chuyến tàu. 

Một vị trí chỉ tạo một đoàn tàu mới khi giá trị của nó **hoàn toàn nhỏ hơn** so với tất cả các cực tiểu tiền tố trước đó. sử dụng`<=`thay vì`<`sẽ đếm sai ba chuyến tàu. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. 

Sau mỗi lần cập nhật, hãy xây dựng lại mảng tiền tố cực tiểu. Mỗi khi tiền tố tối thiểu giảm xuống, một chuyến tàu mới sẽ bắt đầu. Đếm những mức giảm này sẽ đưa ra câu trả lời. 

Điều này đúng vì tốc độ cuối cùng của mỗi cỗ xe chính xác là tiền tố tối thiểu của nó. Thật không may, việc xây dựng lại chi phí tối thiểu tiền tố`O(n)`mỗi truy vấn, dẫn đến`O(nm)`làm việc tổng thể. Với cả hai giá trị xung quanh`10^5`, tốc độ này quá chậm. 

Nhận xét quan trọng là chúng ta không thực sự quan tâm đến từng toa xe. Chúng tôi chỉ quan tâm đến các vị trí tạo tiền tố tối thiểu mới. 

Chúng ta hãy gọi những vị trí như vậy là **lãnh đạo**. 

Ví dụ:```
a = [10, 13, 5, 2, 6]
```Những người lãnh đạo là:```
1 (10)
3 (5)
4 (2)
```Số lượng người dẫn đầu chính xác là số lượng đoàn tàu. 

Bây giờ hãy xem điều gì xảy ra sau khi giảm. 

Giả sử vị trí`k`trở nên nhỏ hơn. Chỉ có hai điều có thể xảy ra. 

Đầu tiên,`k`có thể trở thành người lãnh đạo mới nếu nó giảm xuống dưới giá trị của người lãnh đạo trước đó. 

Thứ hai, một lần`k`trở thành người lãnh đạo (hoặc người lãnh đạo hiện tại thậm chí còn trở nên nhỏ hơn), một số người lãnh đạo ở bên phải của nó có thể ngừng làm người lãnh đạo vì tiền tố tối thiểu mới đạt đến họ sớm hơn. 

Các nhà lãnh đạo đương nhiên được sắp xếp theo vị trí và giá trị của họ đang giảm dần. Điều này cho phép chúng tôi loại bỏ các đường dẫn đầu bị ảnh hưởng bằng cách đi về bên phải cho đến khi chúng tôi gặp một đường dẫn đầu có giá trị nhỏ hơn. 

Chúng tôi duy trì đội ngũ lãnh đạo hiện tại một cách năng động. Một cấu trúc có trật tự cho phép chúng ta tìm ra những người lãnh đạo trước đó và tiếp theo trong`O(log n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O((n + m) log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đội ngũ lãnh đạo ban đầu. 

Quét từ trái sang phải trong khi vẫn duy trì giá trị nhỏ nhất được thấy cho đến nay. Chức vụ`i`trở thành người lãnh đạo nếu`a[i]`hoàn toàn nhỏ hơn mức tối thiểu đó. 
2. Lưu trữ các nhà lãnh đạo theo một cấu trúc có trật tự. 

Chúng ta cần nhanh chóng tìm ra người đứng đầu ngay trước hoặc sau bất kỳ vị trí nào. Cây Fenwick trên các vị trí dẫn đầu cung cấp chức năng này. 
3. Xử lý cập nhật`(k, d)`. 

Giảm bớt`a[k]`qua`d`. 
4. Tìm người lãnh đạo trước đó`k`. 

Nếu không có người lãnh đạo trước đó thì`k = 1`thực sự không có hạn chế từ bên trái. 
5. Kiểm tra xem`k`phải là người lãnh đạo. 

Chức vụ`k`là một nhà lãnh đạo chính xác khi```
a[k] < value_of_previous_leader
```hoặc khi nó là vị trí đầu tiên. 
6. Nếu`k`trở thành người lãnh đạo và chưa phải là người lãnh đạo, hãy đưa nó vào cơ cấu người lãnh đạo. 
7. Nếu`k`hiện đang là người lãnh đạo, hãy loại bỏ những người lãnh đạo không hợp lệ ở bên phải của nó. 

Liên tục nhìn vào người lãnh đạo tiếp theo sau`k`. 

Mặc dù giá trị của nó lớn hơn hoặc bằng`a[k]`, nó không thể tạo tiền tố tối thiểu mới được nữa, vì vậy hãy xóa nó đi. 

Dừng lại ngay khi đạt đến mức dẫn đầu có giá trị nhỏ hơn. 
8. Số lượng lãnh đạo hiện tại chính là câu trả lời sau lần cập nhật này. 

### Tại sao nó hoạt động 

Người dẫn đầu chính xác là một vị trí mà tiền tố tối thiểu giảm xuống. Vì ranh giới đoàn tàu xảy ra chính xác khi giảm tiền tố tối thiểu, nên các đoàn tàu dẫn đầu và các đoàn tàu là cùng một đối tượng. 

Tập hợp các nhà lãnh đạo luôn có giá trị giảm dần. Khi một giá trị ở vị trí`k`giảm, chỉ có tiền tố cực tiểu bắt đầu từ`k`có thể thay đổi. Vị trí trước đây`k`không bị ảnh hưởng. 

Nếu như`a[k]`trở nên nhỏ hơn giá trị người dẫn đầu trước đó,`k`phải trở thành người lãnh đạo mới. Bất kỳ nhà lãnh đạo nào sau này có giá trị không nhỏ hơn`a[k]`không còn có thể thể hiện mức giảm tiền tố tối thiểu nữa nên nó phải được loại bỏ. Đường dẫn đầu đầu tiên sau đó có giá trị nhỏ hơn vẫn hợp lệ và mọi đường dẫn đầu sau nó cũng vẫn hợp lệ vì giá trị của đường dẫn đầu đang giảm dần. 

Do đó, tập dẫn đầu được duy trì luôn chính xác là tập hợp các vị trí tiền tố tối thiểu, do đó kích thước của nó luôn là số lượng đoàn tàu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, delta):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] += delta
            idx += idx & -idx

    def sum(self, idx):
        bit = self.bit
        res = 0
        while idx > 0:
            res += bit[idx]
            idx -= idx & -idx
        return res

    def kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length() - 1)

        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1

        return idx + 1

def solve():
    t = int(input())

    for _ in range(t):
        line = input()
        while line.strip() == "":
            line = input()

        n, m = map(int, line.split())
        a = [0] + list(map(int, input().split()))

        fw = Fenwick(n)
        leader = [False] * (n + 1)

        cur_min = 10**18
        trains = 0

        for i in range(1, n + 1):
            if a[i] < cur_min:
                cur_min = a[i]
                leader[i] = True
                fw.add(i, 1)
                trains += 1

        def next_leader(pos):
            cnt = fw.sum(pos)
            if cnt >= trains:
                return 0
            return fw.kth(cnt + 1)

        ans = []

        for _ in range(m):
            k, d = map(int, input().split())
            a[k] -= d

            cnt_before = fw.sum(k - 1)

            if cnt_before == 0:
                should_be_leader = True
            else:
                prev_leader = fw.kth(cnt_before)
                should_be_leader = a[k] < a[prev_leader]

            if should_be_leader and not leader[k]:
                leader[k] = True
                fw.add(k, 1)
                trains += 1

            if leader[k]:
                nxt = next_leader(k)

                while nxt and a[nxt] >= a[k]:
                    leader[nxt] = False
                    fw.add(nxt, -1)
                    trains -= 1
                    nxt = next_leader(k)

            ans.append(str(trains))

        print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ một`1`ở mọi vị trí lãnh đạo và`0`ở nơi khác. Điều này cho phép chúng tôi tìm ra người tiền nhiệm và người kế nhiệm trong số các nhà lãnh đạo mà không cần lưu trữ BST cân bằng. 

các`kth()`phương pháp này là phép toán thống kê thứ tự Fenwick tiêu chuẩn. Đưa ra một cấp bậc, nó trả về vị trí của người lãnh đạo đó. 

Logic cập nhật phản ánh bằng chứng. Đầu tiên chúng tôi quyết định liệu`k`trở thành người lãnh đạo. Sau đó, nếu`k`là một nhà lãnh đạo, chúng tôi liên tục loại bỏ các nhà lãnh đạo bị thống trị ở bên phải của nó. 

Một sai lầm phổ biến là sử dụng`<=`khi quyết định liệu một vị trí có trở thành người lãnh đạo hay không. Các giá trị bằng nhau không tạo ra tiền tố tối thiểu mới, do đó điều kiện phải đúng`<`. 

Một sai lầm dễ mắc phải khác là loại bỏ những người lãnh đạo có`>`thay vì`>=`. Nếu hai người đứng đầu có giá trị bằng nhau thì người đứng sau không phải là vị trí tiền tố tối thiểu thực sự và phải biến mất. 

## Ví dụ đã hoạt động 

### Mẫu 1```
n = 4
a = [6, 2, 3, 7]
```Các nhà lãnh đạo ban đầu:```
1 (6), 2 (2)
```Số lượng tàu = 2. 

Cập nhật đầu tiên:```
(3, 2)
```Mảng trở thành:```
[6, 2, 1, 7]
```| Bước | Lãnh đạo | 
| --- | --- | 
| Trước khi cập nhật | {1, 2} | 
| Vị trí 3 giảm xuống 1 | {1, 2, 3} | 
| Vị trí 4 có giá trị 7 ≥ 1 | đã xóa | 
| Cuối cùng | {1, 2, 3} | 

Trả lời:`3`. 

Cập nhật lần thứ hai:```
(4, 7)
```Mảng trở thành:```
[6, 2, 1, 0]
```| Bước | Lãnh đạo | 
| --- | --- | 
| Trước khi cập nhật | {1, |
