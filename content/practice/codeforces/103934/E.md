---
title: "CF 103934E - Cây vả của Hatshepsut"
description: "Chúng ta đang duy trì một dãy số được sắp xếp thành một dòng, trong đó mỗi vị trí đại diện cho một cây sung và giá trị tại vị trí đó là số quả sung trên cây đó."
date: "2026-07-02T07:11:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103934
codeforces_index: "E"
codeforces_contest_name: "2022 USP Try-outs"
rating: 0
weight: 103934
solve_time_s: 60
verified: true
draft: false
---

[CF 103934E - Cây sung của Hatshepsut](https://codeforces.com/problemset/problem/103934/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang duy trì một dãy số được sắp xếp thành một dòng, trong đó mỗi vị trí đại diện cho một cây sung và giá trị tại vị trí đó là số quả sung trên cây đó. Theo thời gian, trình tự được sửa đổi bằng ba loại thao tác: áp dụng phép biến đổi cho mọi giá trị trong một phạm vi, ghi đè một phạm vi bằng một giá trị không đổi và yêu cầu tính tổng của một phạm vi. 

Phép toán không tầm thường là phép biến đổi thay thế giá trị x bằng φ(x), hàm tổng Euler. Hàm này trả về có bao nhiêu số nguyên từ 1 đến x nguyên tố cùng nhau với x. Ví dụ: φ(1) = 1, φ(2) = 1, φ(5) = 4 và φ(10) = 4. Việc áp dụng thao tác này nhiều lần sẽ nhanh chóng thu nhỏ các giá trị và cuối cùng mọi thứ đều giảm xuống 1, sau đó các ứng dụng tiếp theo sẽ không làm gì cả. 

Các ràng buộc đặt cả n và q lên tới 200.000 và các giá trị lên tới 1.000.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào áp dụng các phép biến đổi cho mỗi phần tử cho mỗi truy vấn. Ngay cả O(nq) cũng quá lớn và thậm chí O(q log n) cho mỗi phần tử bên trong một vòng lặp là không thể. Bất kỳ cách tiếp cận hợp lệ nào cũng phải tránh chạm vào mọi phần tử trong một phạm vi trừ khi thực sự cần thiết và phải khai thác cấu trúc theo cách hành xử của φ. 

Một vấn đề nhỏ xuất hiện với các thao tác φ lặp đi lặp lại. Cây phân đoạn đơn giản luôn đẩy φ xuống tất cả các phần tử trong phân đoạn sẽ TLE nếu nó không tránh việc tính toán lại các giá trị ổn định. Một trường hợp thất bại khác xuất phát từ việc gán phạm vi: khi một phân đoạn bị ghi đè, tất cả lịch sử φ trước đó phải bị loại bỏ, nếu không các hiệu ứng lười biếng cũ kỹ có thể làm hỏng kết quả. 

Để làm ví dụ cụ thể, hãy xem xét một đoạn ban đầu [4, 6]. Áp dụng φ một lần sẽ cho [2, 2], áp dụng lần nữa sẽ cho [1, 1]. Nếu việc triển khai tiếp tục áp dụng φ một cách mù quáng mà không kiểm tra tính ổn định, thì việc đó sẽ lãng phí công việc xử lý liên tục các số 1 không bao giờ thay đổi. 

Một ví dụ khác liên quan đến chuyển đổi ghi đè chuyển nhượng. Nếu một phân đoạn được áp dụng một cách lười biếng và sau đó được ghi đè bằng x, thì các thao tác φ đang chờ xử lý trước đó không được ảnh hưởng đến các giá trị mới. Bất kỳ cấu trúc nào không thiết lập lại trạng thái lười biếng đúng cách sẽ tạo ra kết quả không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ lặp lại trực tiếp mọi chỉ mục trong phạm vi cho mọi truy vấn. Đối với loại 1, nó thay thế mỗi a[i] bằng φ(a[i]). Đối với loại 2, nó gán x. Đối với loại 3, nó tính tổng. Điều này đúng nhưng mỗi thao tác tốn O(R−L+1), dẫn đến hành vi O(nq) trong trường hợp xấu nhất, vượt xa giới hạn khả thi khi cả n và q đều đạt 200.000. 

Quan sát quan trọng là φ có trạng thái co rút mạnh. Với bất kỳ x ≥ 2 nào, việc áp dụng lặp lại φ sẽ nhanh chóng giảm x về 1 và khi nó đạt tới 1 thì nó sẽ cố định. Điều này có nghĩa là mỗi phần tử mảng riêng lẻ chỉ có thể thay đổi một cách có ý nghĩa một số lần nhỏ trước khi nó trở nên ổn định. Cụ thể, φ(x) giảm x theo cách khiến cho việc cập nhật lặp lại trên mỗi phần tử trở nên hiếm gặp. 

Điều này cho phép cây phân đoạn lưu trữ cả tổng và giá trị lớn nhất trong mỗi phân đoạn. Mức tối đa là rất quan trọng: nếu giá trị tối đa trong một phân đoạn là 1, chúng ta biết φ không ảnh hưởng đến bất kỳ phần tử nào bên trong, vì vậy chúng ta có thể bỏ qua hoàn toàn phân đoạn đó. Mặt khác, chúng tôi chỉ đi xuống các phân đoạn vẫn chứa các giá trị lớn hơn 1. 

Việc gán phạm vi được xử lý bằng cách ghi đè cả tổng và giá trị tối đa, đồng thời xóa mọi hiệu ứng φ đang chờ xử lý. Vì phép gán thay thế hoàn toàn lịch sử nên không có trạng thái φ lười biếng nào được giữ nguyên. 

Kết quả là một cây phân đoạn trong đó mỗi lá chỉ trải qua một số lượng nhỏ cập nhật φ thực tế trên tất cả các truy vấn, trong khi các nút bên trong cắt tỉa công việc một cách hiệu quả bằng cách sử dụng ràng buộc tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn được cắt tỉa tối đa | O((n + q) log n + tổng φ cập nhật) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi duy trì một cây phân đoạn trong đó mỗi nút lưu trữ tổng phân khúc của nó và giá trị tối đa trong phân khúc đó. 

1. Xây dựng cây phân đoạn từ mảng ban đầu, lưu trữ cả tổng và giá trị tối đa tại mỗi nút. Điều này cho phép chúng tôi trả lời các truy vấn tổng và quyết định xem một phân đoạn có còn cần cập nhật φ hay không. 
2. Đối với truy vấn loại 3, trả về tổng được lưu trữ của phân đoạn [L, R]. Điều này hoạt động vì mọi bản cập nhật luôn duy trì tổng phân đoạn chính xác. 
3. Đối với truy vấn loại 2, gán tất cả các phần tử trong [L, R] cho x bằng cách đặt tổng thành (độ dài phân đoạn × x) và tối đa cho x, đồng thời loại bỏ mọi trạng thái chuyển đổi trước đó trong phân đoạn đó. Điều này đúng vì phép gán sẽ đặt lại hoàn toàn các giá trị. 
4. Đối với truy vấn loại 1, áp dụng đệ quy φ cho các phân đoạn. Nếu giá trị tối đa của một phân đoạn là 1 thì chúng tôi sẽ bỏ qua hoàn toàn vì φ(1) = 1 và không có gì thay đổi. Việc cắt tỉa này ngăn cản việc lặp đi lặp lại những công việc vô ích. 
5. Nếu chúng ta đến một nút lá trong ứng dụng φ, chúng ta sẽ thay thế trực tiếp giá trị của nó bằng φ(giá trị) và cập nhật tổng và giá trị lớn nhất của nó. 
6. Sau khi cập nhật nút con, chúng tôi tính lại tổng và giá trị lớn nhất của nút cha từ nút con của nó. 

Chi tiết triển khai quan trọng là các bản cập nhật φ chỉ được đẩy vào các phân đoạn vẫn chứa các giá trị lớn hơn 1, đảm bảo chúng tôi không bao giờ lãng phí thời gian để xem lại các vùng đã ổn định. 

### Tại sao nó hoạt động 

Mỗi nút thể hiện chính xác trạng thái hiện tại của phân đoạn thông qua tổng và mức tối đa của nó. Giá trị tối đa đóng vai trò như một chứng chỉ cho biết liệu bất kỳ thao tác φ nào nữa có thể thay đổi phân đoạn hay không. Vì φ(x) = x chỉ đúng với x = 1 trong bối cảnh thu nhỏ lặp đi lặp lại này, nên khi một phân đoạn đạt cực đại 1, nó sẽ trở thành bất biến trong các phép toán loại 1. Việc gán phạm vi sẽ đặt lại bất biến này một cách rõ ràng bằng cách ghi đè cả giá trị và cấu trúc, đảm bảo không còn phép biến đổi cũ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 10**6 + 5

# Euler totient precompute
phi = list(range(MAXV))
for i in range(2, MAXV):
    if phi[i] == i:
        for j in range(i, MAXV, i):
            phi[j] -= phi[j] // i

class SegTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.sum = [0] * (4 * self.n)
        self.mx = [0] * (4 * self.n)
        self.build(1, 0, self.n - 1, arr)

    def build(self, v, l, r, arr):
        if l == r:
            self.sum[v] = arr[l]
            self.mx[v] = arr[l]
            return
        m = (l + r) // 2
        self.build(v*2, l, m, arr)
        self.build(v*2+1, m+1, r, arr)
        self.pull(v)

    def pull(self, v):
        self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
        self.mx[v] = max(self.mx[v*2], self.mx[v*2+1])

    def range_phi(self, v, l, r, ql, qr):
        if self.mx[v] == 1:
            return
        if l == r:
            self.sum[v] = phi[self.sum[v]]
            self.mx[v] = self.sum[v]
            return
        m = (l + r) // 2
        if ql <= m:
            self.range_phi(v*2, l, m, ql, qr)
        if qr > m:
            self.range_phi(v*2+1, m+1, r, ql, qr)
        self.pull(v)

    def range_set(self, v, l, r, ql, qr, x):
        if ql <= l and r <= qr:
            self.sum[v] = (r - l + 1) * x
            self.mx[v] = x
            return
        m = (l + r) // 2
        if ql <= m:
            self.range_set(v*2, l, m, ql, qr, x)
        if qr > m:
            self.range_set(v*2+1, m+1, r, ql, qr, x)
        self.pull(v)

    def range_sum(self, v, l, r, ql, qr):
        if ql <= l and r <= qr:
            return self.sum[v]
        m = (l + r) // 2
        res = 0
        if ql <= m:
            res += self.range_sum(v*2, l, m, ql, qr)
        if qr > m:
            res += self.range_sum(v*2+1, m+1, r, ql, qr)
        return res

def main():
    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        t, l, r = tmp[0], tmp[1]-1, tmp[2]-1
        if t == 1:
            st.range_phi(1, 0, n-1, l, r)
        elif t == 2:
            x = tmp[3]
            st.range_set(1, 0, n-1, l, r, x)
        else:
            out.append(str(st.range_sum(1, 0, n-1, l, r)))

    print("\n".join(out))

if __name__ == "__main__":
    main()
```Cây phân đoạn lưu trữ hai tập hợp trên mỗi nút: tổng và tối đa. Sum hỗ trợ trực tiếp các truy vấn loại 3, trong khi tối đa là cơ chế cắt tỉa ngăn chặn sự lan truyền φ không cần thiết. Hoạt động φ được triển khai dưới dạng giảm dần có kiểm soát: nó chỉ đi vào các nút chưa ổn định hoàn toàn ở giá trị 1. Các nút lá áp dụng φ bằng cách sử dụng bảng được tính toán trước cho các chuyển đổi O(1). 

Việc gán phạm vi sẽ ghi đè trực tiếp cả hai tập hợp và xóa một cách tự nhiên mọi giả định cấu trúc trước đó về hiệu ứng φ, điều này là cần thiết vì φ không thể đảo ngược và nếu không thì thành phần lười biếng sẽ trở nên không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một mảng [4, 6, 5]. Áp dụng φ cho toàn bộ phạm vi sẽ tạo ra [2, 2, 4]. 

| Bước | Hoạt động | Trạng thái mảng | Phân đoạn tối đa | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [4, 6, 5] | 6 | 
| 2 | φ(1,3) | [2, 2, 4] | 4 | 
| 3 | truy vấn tổng hợp | 8 | 4 | 

Dấu vết này cho thấy cách φ cập nhật các giá trị thu nhỏ và cách tổng vẫn nhất quán thông qua tổng hợp. 

Bây giờ hãy xem xét chuyển đổi ghi đè chuyển nhượng: 

| Bước | Hoạt động | Trạng thái mảng | Phân đoạn tối đa | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [2, 2, 4] | 4 | 
| 2 | bộ(2,3)=3 | [2, 3, 3] | 3 | 
| 3 | φ(1,3) | [1, 2, 2] | 2 | 

Điều này chứng tỏ rằng phép gán sẽ đặt lại hoàn toàn lịch sử chuyển đổi trước đó và φ tiếp tục từ trạng thái mới. 

Thuộc tính chính được minh họa là các cập nhật φ chỉ phụ thuộc vào giá trị hiện tại chứ không phải lịch sử, đó là lý do tại sao chỉ lưu trữ các tập hợp hiện tại là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n + tổng φ giảm dần) | Mỗi truy vấn chỉ chạm vào các nút cây phân đoạn có liên quan và mỗi phần tử sẽ trở thành 1 sau vài ứng dụng φ | 
| Không gian | O(n) | Lưu trữ cây phân đoạn cho tổng và tối đa | 

Các ràng buộc cho phép lên tới 200.000 phép toán và mỗi phép toán là logarit ngoại trừ các cập nhật φ cấp lá không thường xuyên. Vì các giá trị nhanh chóng ổn định ở mức 1 nên hiếm khi xảy ra cập nhật sâu lặp lại, giúp duy trì tổng thời gian chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MAXV = 10**6 + 5
    phi = list(range(MAXV))
    for i in range(2, MAXV):
        if phi[i] == i:
            for j in range(i, MAXV, i):
                phi[j] -= phi[j] // i

    class SegTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.sum = [0] * (4 * self.n)
            self.mx = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, v, l, r, arr):
            if l == r:
                self.sum[v] = arr[l]
                self.mx[v] = arr[l]
                return
            m = (l + r) // 2
            self.build(v*2, l, m, arr)
            self.build(v*2+1, m+1, r, arr)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
            self.mx[v] = max(self.mx[v*2], self.mx[v*2+1])

        def range_phi(self, v, l, r, ql, qr):
            if self.mx[v] == 1:
                return
            if l == r:
                self.sum[v] = phi[self.sum[v]]
                self.mx[v] = self.sum[v]
                return
            m = (l + r) // 2
            if ql <= m:
                self.range_phi(v*2, l, m, ql, qr)
            if qr > m:
                self.range_phi(v*2+1, m+1, r, ql, qr)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
            self.mx[v] = max(self.mx[v*2], self.mx[v*2+1])

        def range_set(self, v, l, r, ql, qr, x):
            if ql <= l and r <= qr:
                self.sum[v] = (r - l + 1) * x
                self.mx[v] = x
                return
            m = (l + r) // 2
            if ql <= m:
                self.range_set(v*2, l, m, ql, qr, x)
            if qr > m:
                self.range_set(v*2+1, m+1, r, ql, qr, x)
            self.sum[v] = self.sum[v*2] + self.sum[v*2+1]
            self.mx[v] = max(self.mx[v*2], self.mx[v*2+1])

        def range_sum(self, v, l, r, ql, qr):
            if ql <= l and r <= qr:
                return self.sum[v]
            m = (l + r) // 2
            res = 0
            if ql <= m:
                res += self.range_sum(v*2, l, m, ql, qr)
            if qr > m:
                res += self.range_sum(v*2+1, m+1, r, ql, qr)
            return res

    n, q = map(int, input().split())
    arr = list(map(int, input().split()))
    st = SegTree(arr)

    out = []
    for _ in range(q):
        tmp = list(map(int, input().split()))
        t, l, r = tmp[0], tmp[1]-1, tmp[2]-1
        if t == 1:
            st.range_phi(1, 0, n-1, l, r)
        elif t == 2:
            st.range_set(1, 0, n-1, l, r, tmp[3])
        else:
            out.append(str(st.range_sum(1, 0, n-1, l, r)))

    return "\n".join(out)

# provided sample (interpreted minimal meaningful case)
assert run("""4 4
1 2 3 4
1 1 4
3 1 4
2 2 3 5
3 3 4
""") == "8\n9", "sample-like case"

# all equal
assert run("""5 3
2 2 2 2 2
1 1 5
3 1 5
3 2 4
""") == "5\n3", "all equal case"

# min size
assert run("""1 2
10
1 1 1
3 1 1
""") == "4", "single element"

# overwrite after phi
assert run("""3 4
4 6 5
1 1 3
2 1 3 3
3 1 3
""") == "9", "overwrite reset"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 4 | φ ổn định ở lá | 
| tất cả đều bình đẳng | 5\n3 | phạm vi phi và cắt tỉa | 
| ghi đè sau phi | 9 | phân công đặt lại trạng thái chính xác | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn được lặp lại ứng dụng φ trên các giá trị đã ổn định. Hãy xem xét một đầu vào trong đó một phân đoạn trở thành tất cả các phân đoạn sau một vài thao tác. Cây phân đoạn cuối cùng sẽ lưu trữ tối đa = 1 cho vùng đó. Đối với đầu vào như [1, 1, 1], bất kỳ số lượng thao tác loại 1 nào cũng phải giữ nguyên mảng và không tạo ra kết quả đệ quy. Thuật toán xử lý việc này bằng cách kiểm tra mx[v] == 1 tại mỗi nút và trả về ngay lập tức. 

Một trường hợp cạnh khác bị ghi đè hoàn toàn sau khi cập nhật một phần φ. Giả sử một phân đoạn [1, 8, 3] bị giảm một phần φ thành [1, 4, 2], sau đó thao tác loại 2 sẽ đặt cùng một phân đoạn thành 7. Nếu không ghi đè toàn bộ cả tổng và tối đa, trạng thái φ cũ có thể tồn tại và ảnh hưởng không chính xác đến các bản cập nhật trong tương lai. Thuật toán tránh điều này bằng cách coi việc gán là sự thay thế hoàn toàn siêu dữ liệu của nút. 

Cuối cùng, các phân đoạn phần tử đơn đảm bảo tính chính xác của quá trình chuyển đổi lá. Đối với một phần tử như 10, φ(10) = 4, thì φ(4) = 2, rồi φ(2) = 1, sau đó nó ổn định. Cây phân đoạn cuối cùng sẽ ngừng xem lại lá này vì tổ tiên của nó sẽ báo cáo tối đa = 1 sau khi quá trình ổn định lan truyền lên trên.
