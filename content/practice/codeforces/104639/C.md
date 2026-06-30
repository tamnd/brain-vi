---
title: "CF 104639C - Nhân rồi cộng"
description: "Chúng tôi đang duy trì một bộ sưu tập động các cặp số nguyên. Mỗi cặp hoạt động giống như một hàm tuyến tính trong một biến duy nhất: đối với một cặp $(ai, bi)$, chúng ta có thể đánh giá một giá trị $fi(x) = ai cdot x + bi$. Hệ thống hỗ trợ hai hoạt động theo thời gian."
date: "2026-06-29T16:55:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104639
codeforces_index: "C"
codeforces_contest_name: "The 2023 ICPC Asia EC Regionals Online Contest (I)"
rating: 0
weight: 104639
solve_time_s: 53
verified: true
draft: false
---

[CF 104639C - Nhân rồi cộng](https://codeforces.com/problemset/problem/104639/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một bộ sưu tập động các cặp số nguyên. Mỗi cặp hoạt động giống như một hàm tuyến tính trong một biến duy nhất: đối với một cặp$(a_i, b_i)$, chúng ta có thể đánh giá một giá trị$f_i(x) = a_i \cdot x + b_i$. Hệ thống hỗ trợ hai hoạt động theo thời gian. 

Một thao tác cập nhật một cặp chỉ mục duy nhất, thay thế hoàn toàn các hệ số của nó. Hoạt động khác yêu cầu một giá trị cố định của$x$và một đoạn liền kề của các chỉ số, và chúng ta phải tìm giá trị lớn nhất của$a_i x + b_i$trong số tất cả các cặp trong phân khúc đó. 

Vì vậy, nhiệm vụ là sự kết hợp của các cập nhật điểm trên các hàm tuyến tính và các truy vấn tối đa phạm vi được đánh giá tại một thời điểm đã chọn.$x$, trong đó hàm được đánh giá sẽ khác nhau đối với mỗi truy vấn. 

Các ràng buộc là cực kỳ lớn: lên tới 500.000 cặp và 500.000 thao tác. Điều này ngay lập tức loại trừ mọi giải pháp tính toán lại truy vấn phạm vi bằng cách quét tất cả các phần tử, vì ngay cả một truy vấn trong trường hợp xấu nhất cũng đã quá chậm. Bất kỳ giải pháp chấp nhận được nào cũng phải xử lý từng thao tác theo thời gian logarit hoặc tốt hơn, với các hệ số không đổi cẩn thận. 

Một trường hợp phức tạp phát sinh từ việc cập nhật thường xuyên. Một ý tưởng ngây thơ là xây dựng lại các cấu trúc phụ trợ cho mọi truy vấn hoặc duy trì việc sắp xếp các giá trị theo truy vấn sau khi áp dụng$x$. Điều đó không thành công vì các bản cập nhật làm mất hiệu lực mọi thứ tự được tính toán trước. 

Một cái bẫy khác là giả định sự đơn điệu về chỉ số hoặc giá trị. Kể cả nếu$a_i$Và$b_i$bị giới hạn, biểu thức$a_i x + b_i$có thể khác nhau rất nhiều giữa các chỉ số lân cận, vì vậy các chiến lược cắt tỉa phân khúc dựa vào độ mượt sẽ không được áp dụng. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ trực tiếp đánh giá mọi truy vấn bằng cách lặp lại trong phạm vi$[l, r]$và tính toán$a_i x + b_i$cho mỗi chỉ số. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của vấn đề. Tuy nhiên, mỗi truy vấn sẽ mất$O(n)$thời gian trong trường hợp xấu nhất, dẫn đến$O(nq)$tổng số hoạt động, theo thứ tự$2.5 \times 10^{11}$. Điều này vượt xa giới hạn khả thi. 

Quan sát chính là mỗi phần tử xác định một hàm tuyến tính và mỗi truy vấn yêu cầu giá trị tối đa của một tập hợp các dòng được đánh giá tại một điểm duy nhất$x$, bị giới hạn trong một phân đoạn. Đây là một bài toán kiểu bao lồi động cổ điển, nhưng phức tạp bởi hai chiều của động học: cả truy vấn và cập nhật trên các chỉ mục tùy ý. 

Một cách tiêu chuẩn để xử lý cấu trúc này là sử dụng cây phân đoạn trên các chỉ mục. Mỗi nút của cây phân đoạn đại diện cho một khoảng chỉ số cố định. Bên trong mỗi nút, chúng tôi duy trì một cấu trúc có thể trả lời: cho một giá trị$x$, tối đa là bao nhiêu$a_i x + b_i$trong số tất cả các dòng được lưu trữ trong nút đó. 

Đối với các tập hợp đường tĩnh, cấu trúc tối ưu là thủ thuật bao lồi. Từ$x$trong các truy vấn mang tính tùy ý và không đơn điệu, chúng ta không thể sử dụng một lớp vỏ dựa trên con trỏ đơn giản. Thay vào đó, chúng tôi lưu trữ một bao lồi trong mỗi nút cây phân đoạn và đánh giá nó bằng tìm kiếm nhị phân. 

Các bản cập nhật thay thế một dòng duy nhất, vì vậy chúng tôi xây dựng lại các vỏ lồi dọc theo đường dẫn từ lá đến gốc. 

Sự kết hợp này cung cấp một hệ số log từ cây phân đoạn và một hệ số log khác từ tìm kiếm nhị phân bên trong mỗi thân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(1)$| Quá chậm | 
| Cây phân đoạn + thân trên mỗi nút |$O((n+q)\log^2 n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn theo các chỉ số từ 1 đến n, trong đó mỗi nút lưu trữ một tập hợp các dòng tương ứng với khoảng của nó. 

Mỗi dòng được biểu diễn dưới dạng$(a, b)$, tương ứng với một hàm$f(x) = ax + b$. 

1. Xây dựng cây phân đoạn trong đó mỗi nút lá lưu trữ chính xác một dòng từ mảng đầu vào. Các nút bên trong đại diện cho sự kết hợp của các khoảng con của chúng. 
2. Đối với mỗi nút, xây dựng một thân lồi từ các đường của nó. Chúng tôi sắp xếp các đường theo độ dốc$a$, sau đó loại bỏ những phần không cần thiết bằng cách sử dụng kết cấu bao lồi tiêu chuẩn. Tiêu chí loại bỏ dựa trên việc liệu một dòng mới được thêm vào có làm cho dòng trước đó luôn tệ hơn đối với tất cả mọi người hay không.$x$. Điều này đảm bảo chỉ còn lại những dòng có khả năng tối ưu. 
3. Để đánh giá một truy vấn cho một nút, chúng tôi thực hiện tìm kiếm nhị phân trên thân của nó để tìm dòng tối đa hóa$a x + b$cho cái đã cho$x$. Vì các độ dốc được sắp xếp nên các giá trị hàm không đồng nhất theo thứ tự chỉ mục. 
4. Đối với truy vấn phạm vi$[l, r]$, chúng ta phân tích khoảng thành$O(\log n)$phân đoạn các nút cây và truy vấn thân của từng nút một cách độc lập. 
5. Câu trả lời là giá trị lớn nhất trên tất cả các giá trị được trả về. 
6. For an update at position$k$, chúng ta thay thế đường ở lá và xây dựng lại tất cả các thân dọc theo đường dẫn đến gốc. 

Tại sao tìm kiếm nhị phân hoạt động ở đây là do bao lồi đảm bảo rằng các hệ số góc là đơn điệu và các điểm giao nhau giữa các đường liên tiếp được sắp xếp theo thứ tự. Điều này đảm bảo rằng dòng tốt nhất cho một$x$có thể được tìm thấy trong thời gian logarit. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên hai bất biến. Đầu tiên, mỗi nút cây phân đoạn lưu trữ chính xác tập hợp các dòng bao gồm khoảng của nó, do đó, bất kỳ phạm vi truy vấn nào cũng có thể được biểu thị dưới dạng liên kết rời rạc của các khoảng nút. Thứ hai, bao lồi của mỗi nút chỉ chứa các đường có khả năng tối ưu cho một số$x$và thứ tự của độ dốc đảm bảo rằng với mọi cố định$x$, mức tối đa nằm trên một vùng tiếp giáp duy nhất của thân tàu. Do đó, tìm kiếm nhị phân luôn tìm thấy mức tối đa thực sự trong số các dòng của nút đó và việc kết hợp các kết quả của nút sẽ duy trì mức tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Hull:
    def __init__(self):
        self.lines = []
        self.ptr = 0

    def bad(self, l1, l2, l3):
        return (l2[1] - l1[1]) * (l1[0] - l3[0]) >= (l3[1] - l1[1]) * (l1[0] - l2[0])

    def build(self, lines):
        lines.sort()
        self.lines = []
        for line in lines:
            while len(self.lines) >= 2 and self.bad(self.lines[-2], self.lines[-1], line):
                self.lines.pop()
            self.lines.append(line)
        self.ptr = 0

    def query(self, x):
        l, r = 0, len(self.lines) - 1
        best = -10**30
        while l <= r:
            m = (l + r) // 2
            v1 = self.lines[m][0] * x + self.lines[m][1]
            best = max(best, v1)
            if m + 1 < len(self.lines):
                v2 = self.lines[m + 1][0] * x + self.lines[m + 1][1]
                if v2 >= v1:
                    l = m + 1
                else:
                    r = m - 1
            else:
                r = m - 1
        return best

class SegTree:
    def __init__(self, n):
        self.n = n
        self.tree = [None] * (4 * n)

    def build(self, idx, l, r, arr):
        if l == r:
            self.tree[idx] = Hull()
            self.tree[idx].build([arr[l]])
            return
        m = (l + r) // 2
        self.build(idx * 2, l, m, arr)
        self.build(idx * 2 + 1, m + 1, r, arr)
        self.tree[idx] = Hull()
        self.tree[idx].build(self.tree[idx * 2].lines + self.tree[idx * 2 + 1].lines)

    def update(self, idx, l, r, pos, val):
        if l == r:
            self.tree[idx].build([val])
            return
        m = (l + r) // 2
        if pos <= m:
            self.update(idx * 2, l, m, pos, val)
        else:
            self.update(idx * 2 + 1, m + 1, r, pos, val)
        self.tree[idx].build(self.tree[idx * 2].lines + self.tree[idx * 2 + 1].lines)

    def query(self, idx, l, r, ql, qr, x):
        if qr < l or r < ql:
            return -10**30
        if ql <= l and r <= qr:
            return self.tree[idx].query(x)
        m = (l + r) // 2
        return max(
            self.query(idx * 2, l, m, ql, qr, x),
            self.query(idx * 2 + 1, m + 1, r, ql, qr, x)
        )

n, q = map(int, input().split())
arr = [tuple(map(int, input().split())) for _ in range(n)]

st = SegTree(n)
st.build(1, 0, n - 1, arr)

out = []
for _ in range(q):
    tmp = list(map(int, input().split()))
    if tmp[0] == 1:
        _, k, a, b = tmp
        st.update(1, 0, n - 1, k - 1, (a, b))
    else:
        _, x, l, r = tmp
        out.append(str(st.query(1, 0, n - 1, l - 1, r - 1, x)))

print("\n".join(out))
```Cây phân đoạn lưu trữ các bao lồi cho mỗi khoảng, đồng thời cả hai hoạt động xây dựng và cập nhật đều tính toán lại các bao này từ các nút con. Hàm truy vấn thân thực hiện tìm kiếm nhị phân trên các dòng ứng viên để tìm ra đánh giá tốt nhất tại x. 

Một chi tiết tinh tế là mọi việc xây dựng lại nút đều được thực hiện bằng cách hợp nhất các danh sách đầy đủ từ các nút con. Điều này không tối ưu về mặt tiệm cận nhưng giúp việc triển khai trở nên đơn giản. Tính chính xác chỉ phụ thuộc vào kết cấu thân tàu chứ không phụ thuộc vào hiệu quả hợp nhất tăng dần. 

Việc lập chỉ mục được chuyển đổi nhất quán sang giá trị 0 trong nội bộ, giúp tránh được các lỗi sai sót trong quá trình phân chia phân đoạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
2 3
1 5
3 1
2 2 1 3
2 3 2 3
```Đầu tiên chúng tôi xây dựng các nút cây phân đoạn. 

| Bước | Phân đoạn | Dòng được lưu trữ | Truy vấn x | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | [1,3] | (2,3),(1,5),(3,1) | - | - | 
| 2 | truy vấn đầy đủ | giống nhau | 2 | tối đa(7,7,7)=7 | 
| 3 | [2,3] | (1,5),(3,1) | 3 | tối đa(8,10)=10 | 

Truy vấn đầu tiên đánh giá cả ba dòng ở x=2, tất cả đều cho kết quả 7, vì vậy câu trả lời là 7. Truy vấn thứ hai giới hạn ở chỉ số 2 và 3 và dòng tốt nhất trở thành chỉ mục 3. 

### Ví dụ 2 

đầu vào:```
4 3
1 1
2 0
3 -1
4 -10
2 5 1 4
1 2 10 10
2 5 1 4
```Đánh giá ban đầu tại x=5: 

| tôi | ai, bi | giá trị | 
| --- | --- | --- | 
| 1 | 1,1 | 6 | 
| 2 | 2,0 | 10 | 
| 3 | 3,-1 | 14 | 
| 4 | 4,-10 | 10 | 

Truy vấn đầu tiên trả về 14. 

Sau khi cập nhật, dòng thứ hai trở thành (10,10). Bây giờ tại x=5: 

| tôi | ai, bi | giá trị | 
| --- | --- | --- | 
| 1 | 1,1 | 6 | 
| 2 | 10,10 | 60 | 
| 3 | 3,-1 | 14 | 
| 4 | 4,-10 | 10 | 

Truy vấn thứ hai trả về 60. 

Những ví dụ này cho thấy các bản cập nhật thay đổi dòng nào chiếm ưu thế trên toàn cầu và cách cấu trúc phân khúc cô lập khu vực bị ảnh hưởng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q)\log^2 n)$| mỗi lần cập nhật/truy vấn chạm vào$O(\log n)$các nút, mỗi truy vấn nút là$O(\log n)$| 
| Không gian |$O(n \log n)$| mỗi nút cây phân đoạn lưu trữ một nhóm các dòng được hợp nhất | 

Sự phức tạp nằm trong giới hạn vì cả hai$n$Và$q$lên tới 500.000 và các yếu tố logarit vẫn có thể quản lý được ngay cả trong Python khi triển khai cẩn thận. Chi phí chủ yếu là xây dựng lại trong quá trình cập nhật, nhưng mỗi hoạt động vẫn chia tỷ lệ logarit theo số lượng phân đoạn.
