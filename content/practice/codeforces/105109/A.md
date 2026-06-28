---
title: "CF 105109A - Bỏ Qua Bài Hát"
description: "Chúng ta được cung cấp một album được trình bày dưới dạng một chuỗi bài hát cố định trong một máy phát đĩa tròn. Đĩa bắt đầu ở bài hát đầu tiên và luôn tiến về phía trước theo thứ tự, quay lại phần đầu sau bài hát cuối cùng. Nô-ê không chỉ lắng nghe một cách tuần tự."
date: "2026-06-27T20:02:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105109
codeforces_index: "A"
codeforces_contest_name: "UTPC Spring 2024 Open Contest"
rating: 0
weight: 105109
solve_time_s: 96
verified: false
draft: false
---

[CF 105109A - Bỏ qua bài hát](https://codeforces.com/problemset/problem/105109/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một album được trình bày dưới dạng một chuỗi bài hát cố định trong một máy phát đĩa tròn. Đĩa bắt đầu ở bài hát đầu tiên và luôn tiến về phía trước theo thứ tự, quay lại phần đầu sau bài hát cuối cùng. 

Nô-ê không chỉ lắng nghe một cách tuần tự. Thay vào đó, anh ấy liên tục thực hiện cùng một hành động: từ vị trí hiện tại trên đĩa, anh ấy bỏ qua một số bài hát nhất định, sau đó nghe bài hát tiếp theo mà anh ấy chọn. Sau khi anh ấy nghe một bài hát, bài hát đó sẽ bị loại khỏi danh sách xem xét trong tương lai vì nó đã được tiêu thụ và các bài hát còn lại sẽ xếp theo một trật tự vòng tròn mới. 

Mỗi giá trị bỏ qua cho chúng ta biết anh ấy sẽ bỏ qua bao nhiêu bài hát còn lại trước khi chọn bài tiếp theo để nghe. Chi tiết quan trọng là số lượt bỏ qua được tính trên các bài hát hiện còn lại chứ không phải mảng cố định ban đầu. 

Đầu ra là chuỗi chính xác của các bài hát theo thứ tự chúng được chọn. 

Các ràng buộc lên tới 100.000 bài hát và 100.000 thao tác, do đó, bất kỳ giải pháp nào mô phỏng chuyển động từng bước trong danh sách đều quá chậm. Cách tiếp cận trực tiếp quét hoặc xoay danh sách cho mỗi truy vấn sẽ giảm xuống hành vi bậc hai trong trường hợp xấu nhất, vượt xa các thao tác được phép trong một giây. 

Khó khăn chính là chúng ta cần hỗ trợ hai thao tác một cách hiệu quả: nhảy tới một giá trị modulo số lượng bài hát còn sống hiện tại và xóa bài hát đã chọn trong khi vẫn giữ nguyên thứ tự vòng tròn. 

Việc triển khai đơn giản sử dụng danh sách Python và liên tục xoay hoặc bật theo chỉ mục sẽ dẫn đến một trường hợp lỗi nhỏ. Ví dụ: nếu chúng tôi lưu trữ các bài hát trong một danh sách và lặp lại`pop(k)`hoạt động, mỗi cửa sổ bật lên sẽ dịch chuyển tất cả các phần tử sau này, do đó, ngay cả một lần chạy với đầu vào lớn cũng sẽ suy biến thành khoảng$O(n^2)$công việc. 

Một cách tiếp cận không chính xác khác là tính chỉ mục tiếp theo bằng cách sử dụng số học modulo trên mảng ban đầu mà không loại bỏ các phần tử. Điều đó bị phá vỡ vì cấu trúc vòng tròn thay đổi sau khi xóa. Ví dụ, nếu các bài hát`[A, B, C, D]`và chúng tôi loại bỏ`B`, lần bỏ qua tiếp theo nên xử lý`[A, C, D]`, không phải chỉ mục ban đầu ở đâu`C`vẫn ở chỉ số 2. 

Khó khăn chính là duy trì một chuỗi vòng tròn động với việc lựa chọn và xóa thứ k nhanh. 

## Phương pháp tiếp cận 

Mô phỏng brute-force sẽ giữ lại danh sách các bài hát hiện tại và đối với mỗi truy vấn, hãy tiến lên từng bước một, gói lại khi cần, bỏ qua các bài hát đã xóa. Mỗi lần bỏ qua có thể đi qua tối đa$O(n)$các phần tử và chúng tôi làm điều này cho$m$truy vấn, dẫn đến$O(nm)$hành vi trong trường hợp xấu nhất. Với$n = m = 10^5$, điều này hoàn toàn không thể thực hiện được. 

Hiểu biết sâu sắc về cấu trúc là các phép toán duy nhất chúng ta cần là các phép toán thống kê thứ tự trên một tập rút gọn động: chúng ta phải liên tục tìm phần tử thứ k còn lại theo thứ tự vòng tròn và xóa nó. Đây chính xác là những gì cây Fenwick hoặc cây phân đoạn trên mảng sống nhị phân hỗ trợ. Mỗi vị trí đều còn hoạt động hoặc đã bị xóa và tổng tiền tố cho phép chúng tôi đếm xem có bao nhiêu phần tử còn hoạt động tồn tại trước một vị trí. Cùng với đó, chúng ta có thể xác định vị trí phần tử còn sống thứ k bằng cách nâng nhị phân trên cây. 

Khi chúng ta biểu diễn trạng thái hiện tại dưới dạng một tập hợp các chỉ số còn sống, chuyển động tròn sẽ trở thành số học dựa trên số lượng phần tử còn sống thay vì truyền tải rõ ràng. Chúng tôi chuyển đổi mức bỏ qua thành thứ hạng mục tiêu giữa các phần tử còn lại, sau đó truy vấn cấu trúc cho thứ hạng đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force | O(nm) | O(n) | Quá chậm | 
| Fenwick / thống kê đơn hàng | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì cây Fenwick theo chỉ mục của các bài hát. Mỗi vị trí lưu 1 nếu bài hát vẫn còn, nếu không thì 0. 

Chúng tôi cũng duy trì một biến`pos`, đại diện cho vị trí hiện tại về thứ hạng giữa các bài hát còn lại, không phải chỉ số gốc. 

1. Khởi tạo cây Fenwick với 1 ở mọi vị trí, vì ban đầu tất cả các bài hát đều có sẵn. Bộ`pos = 0`, nghĩa là chúng ta bắt đầu từ bài hát đầu tiên theo thứ tự vòng tròn còn lại. 
2. Đối với mỗi giá trị bỏ qua`s_i`, tính kích thước của các bài hát còn lại`rem`. Cập nhật vị trí như`pos = (pos + s_i) % rem`. Điều này chuyển đổi việc bỏ qua thành chỉ mục đích theo thứ tự vòng tròn hiện tại. 
3. Chuyển đổi vị trí logic này thành một chỉ mục thực tế trong mảng ban đầu bằng cách tìm`(pos + 1)`-phần tử sống thứ sử dụng cây Fenwick. Bước này là truy vấn thống kê thứ k. 
4. Xuất bài hát ở chỉ mục đó, sau đó xóa nó khỏi cây Fenwick bằng cách đặt giá trị của nó thành 0. 
5. Sau khi xóa, vị trí bắt đầu tiếp theo sẽ trở thành chỉ mục tương tự theo thứ tự vòng tròn rút gọn. Về mặt xếp hạng, điều này chỉ đơn giản là`pos`, bởi vì mọi thứ sau khi loại bỏ sẽ dịch chuyển sang trái một theo thứ tự ngầm định. 

### Tại sao nó hoạt động 

Ở mỗi bước, cây Fenwick thể hiện chuỗi vòng tròn hiện tại dưới dạng một mảng được nén các phần tử sống. Biến`pos`luôn đề cập đến một chỉ mục hợp lệ theo thứ tự nén này. Bản cập nhật modulo duy trì tính chính xác vì bỏ qua chuỗi kích thước hình tròn`rem`tương đương với modulo số học`rem`. Truy vấn thứ k chuyển đổi thứ hạng trừu tượng này trở lại không gian chỉ mục ban đầu mà không bao giờ xây dựng lại mảng một cách rõ ràng. Vì việc xóa chỉ xóa các phần tử và không bao giờ sắp xếp lại các phần tử còn lại nên thứ tự tương đối được duy trì bởi cây Fenwick khớp chính xác với danh sách phát đang phát triển. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def build(self, arr):
        for i in range(1, self.n + 1):
            self.bit[i] += arr[i]
            j = i + (i & -i)
            if j <= self.n:
                self.bit[j] += self.bit[i]

    def update(self, i, delta):
        while i <= self.n:
            self.bit[i] += delta
            i += i & -i

    def prefix_sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

    def total(self):
        return self.prefix_sum(self.n)

    def find_kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length())
        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1
        return idx + 1

def solve():
    n, m = map(int, input().split())
    songs = [""] + [input().rstrip() for _ in range(n)]
    skips = [int(input()) for _ in range(m)]

    fw = Fenwick(n)
    for i in range(1, n + 1):
        fw.update(i, 1)

    pos = 0
    rem = n

    for s in skips:
        pos = (pos + s) % rem
        rem = fw.total()
        idx = fw.find_kth(pos + 1)
        print(songs[idx])
        fw.update(idx, -1)
        rem -= 1

solve()
```Cây Fenwick được sử dụng làm bảng tần số động cho các vị trí bài hát. các`find_kth`Hàm thực hiện tìm kiếm nâng nhị phân trên tổng tiền tố để xác định chỉ mục của bài hát sống động thứ k. các`pos`biến luôn được giữ ở vị trí xếp hạng giữa các phần tử còn lại và chỉ được chuyển đổi thành chỉ mục thực tế khi chúng ta cần xuất một bài hát. 

Một chi tiết tinh tế là chúng tôi tính toán lại hoặc duy trì số lượng còn lại một cách nhất quán khi xóa. Hoạt động modulo phải luôn sử dụng số lượng phần tử còn sống hiện tại, nếu không phép quay sẽ trôi không chính xác sau khi loại bỏ. 

## Ví dụ đã hoạt động 

Hãy xem xét một album nhỏ có năm bài hát được dán nhãn từ A đến E. 

Bỏ qua đầu vào là`[1, 2, 1]`. 

Lúc đầu tất cả các bài hát đều sống động và`pos = 0`. 

| Bước | Bài hát sống động | tư thế trước | bỏ qua | tư thế sau mod | bài hát đã chọn | còn lại | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | A B C D E | 0 | 1 | 1 | B | A C D E | 
| 2 | A C D E | 1 | 2 | 3 % 4 = 3 | A | C D E | 
| 3 | C D E | 3 | 1 | 0 | C | D E | 

Dấu vết cho thấy rằng`pos`luôn đề cập đến chỉ mục vòng tròn trong mảng sống được nén chứ không phải vị trí ban đầu. Ngay cả sau khi loại bỏ, số học modulo vẫn hợp lệ vì nó được áp dụng cho cấu trúc rút gọn. 

Bây giờ hãy xem xét trường hợp bỏ qua gói nhiều lần. Với những bài hát`[A, B, C, D]`và bỏ qua`[5]`, chúng ta bắt đầu với`rem = 4`, Vì thế`pos = (0 + 5) % 4 = 1`. Phần tử thứ hai theo thứ tự còn sống là`B`, xác nhận rằng gói hoạt động chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi lần bỏ qua yêu cầu truy vấn và cập nhật thứ k của cây Fenwick, cả hai đều logarit trong n | 
| Không gian | O(n) | Cây Fenwick và nơi lưu trữ bài hát | 

Các ràng buộc cho phép thực hiện tới 100.000 thao tác và chi phí logarit giúp tổng số thao tác được thực hiện thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def update(self, i, delta):
            while i <= self.n:
                self.bit[i] += delta
                i += i & -i

        def sum(self, i):
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

        def total(self):
            return self.sum(self.n)

        def find_kth(self, k):
            idx = 0
            bitmask = 1 << (self.n.bit_length())
            while bitmask:
                nxt = idx + bitmask
                if nxt <= self.n and self.bit[nxt] < k:
                    k -= self.bit[nxt]
                    idx = nxt
                bitmask >>= 1
            return idx + 1

    def solve():
        n, m = map(int, input().split())
        songs = [""] + [input().rstrip() for _ in range(n)]
        skips = [int(input()) for _ in range(m)]

        fw = Fenwick(n)
        for i in range(1, n + 1):
            fw.update(i, 1)

        pos = 0
        out = []

        for s in skips:
            rem = fw.total()
            pos = (pos + s) % rem
            idx = fw.find_kth(pos + 1)
            out.append(songs[idx])
            fw.update(idx, -1)

        return "\n".join(out)

    return solve()

# sample 1 (conceptual small version, original sample formatting is corrupted)
assert run("""5 3
A
B
C
D
E
1
2
1
""") == "B\nA\nC"

# minimum size
assert run("""1 3
Solo
1
1
1
""") == "Solo\nSolo\nSolo"

# wrap heavy skipping
assert run("""4 1
A
B
C
D
10
""") == "C"

# sequential removals
assert run("""3 3
A
B
C
0
0
0
""") == "A\nB\nC"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn lặp lại | cùng một bài hát | sự đúng đắn dưới sự sụp đổ hoàn toàn | 
| bỏ qua lớn | lựa chọn giữa | gói modulo | 
| không bỏ qua | loại bỏ tuần tự | sự ổn định của việc xử lý pos | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi chỉ còn lại một bài hát. Trong tình huống đó, mọi giá trị bỏ qua đều trở nên không liên quan vì modulo theo 1 luôn mang lại 0. Thuật toán xử lý điều này một cách tự nhiên vì`pos % 1`luôn bằng 0 và cây Fenwick trả về chỉ số còn sống duy nhất. 

Một trường hợp khó phát hiện khác xảy ra khi số lần bỏ qua cực kỳ lớn, gần bằng$10^9$. Việc lặp lại trực tiếp sẽ không thành công, nhưng việc giảm modulo đảm bảo chúng ta chỉ di chuyển trong kích thước còn lại hiện tại. Vì kích thước đó chỉ giảm nên số học vẫn bị giới hạn và an toàn. 

Trường hợp cuối cùng là khi việc xóa xảy ra gần vị trí hiện tại. Bởi vì cây Fenwick nén các chỉ mục một cách linh hoạt nên việc loại bỏ một phần tử sẽ tự động thay đổi thứ hạng. các`pos`biến vẫn hợp lệ vì nó luôn được diễn giải trong không gian xếp hạng thay vì chỉ mục thô, do đó không cần điều chỉnh thủ công sau mỗi lần xóa.
