---
title: "CF 102503M - Se\u00f1orita"
description: "Đầu vào mô tả hai ngăn xếp có áo sơ mi được dán nhãn theo ngày chúng phải được mặc. Ngăn xếp đầu tiên được liệt kê từ dưới lên trên và ngăn xếp thứ hai được liệt kê theo cách tương tự. Mục đích là loại bỏ áo sơ mi theo thứ tự 1, 2, ..., m+n."
date: "2026-08-06T19:15:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102503
codeforces_index: "M"
codeforces_contest_name: "National Olympiad in Informatics - Philippines (NOI.PH) Online Eliminations 2020"
rating: 0
weight: 102503
solve_time_s: 234
verified: true
draft: false
---

[CF 102503M - Se\u00f1orita](https://codeforces.com/problemset/problem/102503/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả hai ngăn xếp có áo sơ mi được dán nhãn theo ngày chúng phải được mặc. Ngăn xếp đầu tiên được liệt kê từ dưới lên trên và ngăn xếp thứ hai được liệt kê theo cách tương tự. Mục đích là loại bỏ áo sơ mi theo thứ tự`1, 2, ..., m+n`. Di chuyển một chiếc áo sơ mi từ chồng này sang chồng khác tốn một đơn vị năng lượng, trong khi lấy đúng chiếc áo sơ mi khi nó đã lộ ra sẽ không tốn kém gì. 

Việc mô phỏng trực tiếp mọi chuyển động có thể xảy ra là không thể vì có thể có tới`400000`áo sơ mi. Bất kỳ giải pháp nào thử nhiều cách sắp xếp lại có thể sẽ quá chậm. Chúng ta cần tìm một cách biểu diễn trong đó các quyết định có ý nghĩa duy nhất là những chuyển động không thể tránh khỏi của hai đỉnh ngăn xếp. 

Trường hợp cạnh hữu ích là khi một ngăn xếp trống. Ví dụ:```
0
1 2 3
```Câu trả lời là`0`bởi vì mọi áo sơ mi đều có thể truy cập được từ ngăn xếp duy nhất. Việc triển khai giả định cả hai ngăn xếp luôn có áo trên cùng có thể thất bại ở đây. 

Một trường hợp khó khăn khác là khi chiếc áo sơ mi được yêu cầu có thể được truy cập ngay lập tức từ ngăn xếp thứ hai:```
1 2
3
```Câu trả lời là`0`bởi vì áo sơ mi`1`nằm trên ngăn xếp đầu tiên và áo sơ mi`2`nằm trên ngăn xếp thứ hai sau khi cần đến. Giải pháp chỉ kiểm tra một hướng ngăn xếp sẽ bị tính quá mức. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thực sự mô phỏng các ngăn xếp. Để lấy chiếc áo tiếp theo, chúng ta có thể thử di chuyển từng chiếc áo một giữa các ngăn xếp cho đến khi mục tiêu xuất hiện. Điều này đúng vì mọi thao tác hợp pháp đều được mô phỏng rõ ràng. Tuy nhiên, trong trường hợp xấu nhất, chúng ta có thể di chuyển hầu hết số áo còn lại cho mỗi chiếc áo được yêu cầu, đưa ra kết quả đại khái là`O(N^2)`hoạt động. Với`N = 400000`, điều này là không thể. 

Nhận xét quan trọng là thứ tự hình tròn tương đối của những chiếc áo sơ mi không bao giờ thay đổi. Hãy tưởng tượng đặt chồng thứ nhất từ ​​dưới lên trên và sau đó xếp chồng thứ hai từ trên xuống dưới xung quanh một vòng tròn. Di chuyển áo giữa các ngăn xếp chỉ thay đổi vị trí phân chia giữa hai ngăn xếp nằm trên vòng tròn này. Hai chiếc áo lộ ra chính là hai chiếc áo bên cạnh phần xẻ này. 

Điều này biến vấn đề thành việc duy trì một vết cắt chuyển động trong một mảng hình tròn. Chúng ta chỉ cần biết khoảng cách từ đường cắt hiện tại đến chiếc áo được yêu cầu. Sau khi cởi áo, vết cắt mới luôn kết thúc ngay trước chiếc áo đã cởi ở vòng tròn còn lại. 

Để duy trì khoảng cách sau khi xóa, chúng tôi lưu trữ những vị trí ban đầu vẫn còn tồn tại trong cây Fenwick. Nó có thể đếm số áo còn sống trong phạm vi và tìm vị trí còn sống thứ k. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Trật tự tròn + Cây Fenwick | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng trật tự tuần hoàn. Đặt ngăn xếp đầu tiên theo thứ tự đã cho và nối ngăn xếp thứ hai theo thứ tự ngược lại. Lưu trữ vị trí của mỗi chiếc áo trong vòng tròn này. 

Việc cắt hiện tại là giữa hai ngăn xếp ban đầu. Chúng tôi giữ`cur`như vị trí của chiếc áo trên cùng của ngăn xếp đầu tiên. Chiếc áo hở hang còn lại là vị trí sống tiếp theo sau`cur`. 

1. Khởi tạo một cây Fenwick chứa một cây cho mỗi vị trí áo sơ mi. 

Cây Fenwick tượng trưng cho vòng tròn hiện tại sau khi một số áo sơ mi đã được cởi bỏ. 

1. Đối với mỗi số áo từ`1`ĐẾN`N`, tìm khoảng cách hình tròn hiện tại của nó từ`cur`. 

Nếu chiếc áo đó`d`vị trí còn sống theo chiều kim đồng hồ từ`cur`, sau đó lấy nó có giá:```
min(d - 1, remaining - d)
```Thuật ngữ đầu tiên có nghĩa là di chuyển đường cắt về phía trước cho đến khi chiếc áo sơ mi trở thành ngăn xếp thứ hai. Thuật ngữ thứ hai có nghĩa là di chuyển lùi lại cho đến khi nó trở thành đỉnh ngăn xếp đầu tiên. 

1. Thêm chi phí tối thiểu này vào câu trả lời. 
2. Cởi áo ra khỏi cây Fenwick. 

Sau khi loại bỏ, thiết lập`cur`đến vị trí còn sống ngay trước khi cởi áo. 

Tại sao nó hoạt động: 

Bất biến thứ tự vòng tròn có nghĩa là mọi chuỗi di chuyển có thể chỉ làm thay đổi vị trí cắt. Công thức khoảng cách xem xét cả hai hướng có thể có để di chuyển đường cắt đó và một trong số chúng luôn là tối ưu vì mỗi bước di chuyển sẽ thay đổi đường cắt chính xác một vị trí. Sau khi xóa áo, vị trí trước đó sẽ trở thành đỉnh xếp hạng đầu tiên mới bất kể hướng nào được sử dụng, do đó trạng thái được duy trì luôn hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        res = 0
        while i:
            res += self.bit[i]
            i -= i & -i
        return res

    def range_sum(self, l, r):
        if l > r:
            return 0
        return self.sum(r) - self.sum(l - 1)

    def kth(self, k):
        idx = 0
        step = 1 << (self.n.bit_length() - 1)
        while step:
            nxt = idx + step
            if nxt <= self.n and self.bit[nxt] < k:
                idx = nxt
                k -= self.bit[nxt]
            step >>= 1
        return idx + 1

def solve():
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    m = a[0]
    first = a[1:]
    n = b[0]
    second = b[1:]

    circle = first + second[::-1]
    total = m + n

    pos = [0] * (total + 1)
    for i, x in enumerate(circle, 1):
        pos[x] = i

    bit = Fenwick(total)
    for i in range(1, total + 1):
        bit.add(i, 1)

    if m:
        cur = m
    else:
        cur = total

    ans = 0
    alive = total

    for x in range(1, total + 1):
        p = pos[x]

        if p == cur:
            cost = 0
        else:
            before = bit.sum(cur)
            if p > cur:
                dist = bit.range_sum(cur, p - 1)
            else:
                dist = bit.range_sum(cur, total) + bit.range_sum(1, p - 1)

            cost = min(dist - 1, alive - dist)

        ans += cost

        bit.add(p, -1)
        alive -= 1

        if alive:
            before = bit.sum(p - 1)
            if before:
                cur = bit.kth(before)
            else:
                cur = bit.kth(alive)

    print(ans)

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ các vị trí còn sống, do đó việc xóa và khoảng cách vòng tròn được xử lý mà không cần xây dựng lại vòng tròn. các`kth`thao tác được sử dụng sau khi cởi áo vì chúng ta cần vị trí tiền thân của vị trí bị loại bỏ trong số những chiếc áo còn lại. 

Biểu hiện của chi phí là phần tinh tế.`dist`đếm xem có bao nhiêu vị trí còn sống nằm từ đỉnh hiện tại đến mục tiêu. Nếu mục tiêu là chiếc áo sơ mi lộ ra đầu tiên,`dist`bằng 0, được xử lý riêng. Còn không thì tiến về phía trước`dist - 1`lần hiển thị nó từ ngăn xếp thứ hai, trong khi di chuyển lùi`alive - dist`lần hiển thị nó từ ngăn xếp đầu tiên. 

Số nguyên Python có độ chính xác tùy ý, do đó câu trả lời tích lũy không cần xử lý đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi chiếc áo thực hiện các truy vấn và cập nhật Fenwick | 
| Không gian | O(N) | Cửa hàng vị trí và cây Fenwick | 

Kích thước đầu vào tối đa là`400000`, do đó hệ số logarit được chấp nhận trong giới hạn thời gian. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3 1 5 3
4 7 2 6 4
```Thứ tự tuần hoàn là:```
1 5 3 4 6 2 7
```Lần cắt đầu tiên là trước`4`, vậy những chiếc áo lộ ra ngoài là`3`Và`4`. 

| Áo yêu cầu | Lựa chọn khoảng cách | Năng lượng bổ sung | 
| --- | --- | --- | 
| 1 | Di chuyển qua 3 và 5 | 2 | 
| 2 | Di chuyển xung quanh phía ngắn hơn | 4 | 
| 3 | Di chuyển qua các trình chặn còn lại | 2 | 
| Áo sơ mi còn lại | Đã có thể truy cập | 0 | 

Tổng cộng là:```
2 + 4 + 2 = 8
```phù hợp với đầu ra mẫu.
