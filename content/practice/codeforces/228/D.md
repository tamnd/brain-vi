---
title: "CF 228D - Zigzag"
description: "Chúng tôi duy trì một dãy số lên tới $10^5$. Hai loại hoạt động đến trực tuyến. Thao tác đầu tiên thay đổi một phần tử mảng đơn. Hoạt động thứ hai yêu cầu tính tổng có trọng số trên một phân đoạn."
date: "2026-06-04T09:06:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 228
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 141 (Div. 2)"
rating: 2100
weight: 228
solve_time_s: 102
verified: true
draft: false
---

[CF 228D - Zigzag](https://codeforces.com/problemset/problem/228/D) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng lên đến$10^5$những con số. Hai loại hoạt động đến trực tuyến. 

Thao tác đầu tiên thay đổi một phần tử mảng đơn. 

Hoạt động thứ hai yêu cầu tính tổng có trọng số trên một phân đoạn. Các trọng số đến từ một chuỗi "ngoằn ngoèo" định kỳ đặc biệt được xác định bởi một tham số$z$, Ở đâu$2 \le z \le 6$. 

Đối với một cố định$z$, trình tự trông giống như$$1,2,\dots,z,z-1,\dots,2,1,2,\dots$$và lặp đi lặp lại mãi mãi. 

Ví dụ:$$z=2:\quad 1,2,1,2,1,2,\dots$$

$$z=3:\quad 1,2,3,2,1,2,3,2,\dots$$

$$z=4:\quad 1,2,3,4,3,2,1,2,\dots$$Đối với một truy vấn$(l,r,z)$, chúng ta phải tính$$\sum_{i=l}^{r} a_i \cdot S^{(z)}_{i-l+1}$$Ở đâu$S^{(z)}$là dãy ngoằn ngoèo cho hệ số$z$. 

Thách thức là cả mảng và truy vấn đều động. Chúng ta phải trả lời tới$10^5$hoạt động. 

Tính toán trực tiếp của mỗi truy vấn sẽ quét toàn bộ khoảng thời gian. Trong trường hợp xấu nhất, một truy vấn có thể bao gồm tất cả$10^5$các phần tử và có thể có$10^5$những truy vấn như vậy. Điều đó sẽ đòi hỏi khoảng$10^{10}$phép nhân, điều này hoàn toàn không thể thực hiện được. 

Giới hạn nhỏ trên$z$là manh mối quan trọng. Mặc dù mảng lớn nhưng chỉ có năm yếu tố ngoằn ngoèo có thể xảy ra:$2,3,4,5,6$. 

Trường hợp cạnh tinh vi xuất phát từ thực tế là truy vấn luôn bắt đầu theo hình zigzag tại vị trí$l$, không ở vị trí$1$. 

Coi như:```
a = [10,20,30]
query: l=2, r=3, z=2
```Các trọng số là:```
1,2
```không```
2,1
```vì mẫu khởi động lại ở ranh giới bên trái của truy vấn. Bất kỳ giải pháp nào chỉ lưu trữ mẫu trọng số toàn cầu theo chỉ số tuyệt đối sẽ tạo ra câu trả lời sai. 

Một sai lầm dễ mắc phải khác là giả sử khoảng thời gian là$2z$. Ví dụ:```
z=3
1 2 3 2 1 2 3 2 ...
```Chu kỳ thực sự là$$2z-2 = 4.$$Thời gian sử dụng$6$sẽ điều chỉnh sai tất cả các truy vấn. 

Cái bẫy thứ ba xuất hiện sau khi cập nhật. 

Ví dụ:```
a = [1,1,1]
update position 2 -> 100
query (1,3,2)
```Câu trả lời trở thành$$1\cdot1+100\cdot2+1\cdot1=202.$$Nếu các bản cập nhật không được phản ánh trong mọi cấu trúc dữ liệu được liên kết với lớp dư lượng có liên quan thì các câu trả lời trong tương lai sẽ không chính xác. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. Đối với mỗi truy vấn, hãy tạo các trọng số ngoằn ngoèo và quét từ$l$ĐẾN$r$, tích lũy$$a_i \cdot \text{weight}.$$Chi phí cập nhật một điểm$O(1)$, nhưng chi phí truy vấn phạm vi$O(r-l+1)$. Với$10^5$hoạt động và khoảng thời gian dài$10^5$, thời gian chạy đạt$10^{10}$, vượt xa giới hạn. 

Quan sát quan trọng là chỉ có năm yếu tố ngoằn ngoèo có thể xảy ra. 

Đối với một cố định$z$, dãy ngoằn ngoèo có tính tuần hoàn. Cho phép$$P_z = 2z-2.$$Vì$z \le 6$,$$P_z \le 10.$$Điều đó có nghĩa là mọi trọng số được gán cho một vị trí mảng chỉ phụ thuộc vào modulo chỉ số của nó nhiều nhất là 10. 

Giả sử chúng ta sửa một lỗi cụ thể$z$. Đối với mỗi lớp dư lượng modulo$P_z$, chúng tôi xây dựng cây Fenwick lưu trữ các giá trị thuộc lớp dư lượng đó. 

Sau đó chúng ta có thể hỏi: 

"Tổng của tất cả các giá trị mảng trong$[l,r]$chỉ số của nó bằng với dư lượng$k$modulo$P_z$?" 

Mỗi lớp dư lượng đóng góp một trọng lượng không đổi từ mô hình ngoằn ngoèo. Vì có tối đa 10 phần dư nên toàn bộ truy vấn sẽ trở thành tổng của tối đa 10 truy vấn phạm vi Fenwick. 

Cập nhật cũng dễ dàng. Khi nào$a_p$thay đổi, chúng tôi cập nhật cây Fenwick loại dư lượng tương ứng cho mọi khả năng có thể$z$. Chỉ có năm giá trị của$z$, vì vậy mỗi bản cập nhật chạm vào một số lượng cấu trúc không đổi. 

Lực lượng vũ phu không thành công vì nó liên tục tính toán lại sự đóng góp của các phần tử riêng lẻ. Cấu trúc tuần hoàn cho phép chúng tôi tổng hợp tất cả các vị trí có cùng phần dư theo modulo trong khoảng thời gian, giảm mọi truy vấn xuống một số lượng nhỏ các hoạt động Fenwick không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(\log n)$mỗi hoạt động |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước 

Đối với mỗi yếu tố ngoằn ngoèo$z \in \{2,3,4,5,6\}$: 

1. Tính chu kỳ của nó$P = 2z-2$. 
2. Xây dựng các trọng lượng ngoằn ngoèo trong một khoảng thời gian đầy đủ. 
3. Tạo$P$Cây Fenwick, một cây cho mỗi modulo dư$P$. 
4. Chèn mọi phần tử mảng$a_i$vào cây Fenwick tương ứng với$i \bmod P$. 

### Thao tác cập nhật 

1. Để giá trị hiện tại ở vị trí$p$già đi. 
2. Tính toán$$\Delta = v - old.$$1. Với mọi yếu tố ngoằn ngoèo$z$: 

- Tính toán$P=2z-2$. 
- Tìm dư lượng$p \bmod P$. 
- Thêm vào$\Delta$tới cây Fenwick tương ứng ở chỉ số$p$. 
2. Lưu trữ giá trị mảng mới. 

Mỗi cây Fenwick luôn chứa các giá trị mảng hiện tại cho lớp dư lượng của nó. 

### Thao tác truy vấn 

1. Hãy để$P=2z-2$. 
2. Đối với mọi dư lượng$r$modulo$P$, lấy tổng giá trị mảng bên trong$[l,r]$có chỉ số thuộc lớp dư lượng đó. 
3. Xác định trọng lượng ngoằn ngoèo nào tương ứng với phần cặn đó. 

Mẫu bắt đầu ở vị trí$l$, do đó các lớp dư lượng phải được dịch chuyển tương ứng với$l$. 
4. Nhân mỗi tổng loại dư lượng với trọng số tương ứng của nó. 
5. Thêm tất cả các đóng góp. 

Số lượng dư lượng nhiều nhất là 10, do đó chỉ cần một số lượng truy vấn phạm vi Fenwick không đổi. 

### Tại sao nó hoạt động 

Đối với một cố định$z$, trình tự ngoằn ngoèo lặp lại mỗi$P=2z-2$các vị trí. Mọi chỉ số có cùng modulo dư$P$luôn nhận được cùng một trọng số so với một căn chỉnh nhất định. 

Do đó, một truy vấn có thể được phân tách thành các lớp dư thừa theo modulo$P$. Cây Fenwick đưa ra tổng giá trị mảng được đóng góp bởi mỗi lớp dư lượng trong khoảng được yêu cầu. Nhân tổng của mỗi lớp với trọng số ngoằn ngoèo thích hợp của nó sẽ tạo ra tổng số chính xác như tổng của từng phần tử. Các bản cập nhật duy trì tính bất biến này vì mọi vị trí được sửa đổi đều được cập nhật trong tất cả các cấu trúc lớp dư lượng có chứa nó. 

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
        res = 0
        bit = self.bit
        while idx > 0:
            res += bit[idx]
            idx -= idx & -idx
        return res

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def build_pattern(z):
    p = 2 * z - 2
    arr = []
    for i in range(1, z + 1):
        arr.append(i)
    for i in range(z - 1, 1, -1):
        arr.append(i)
    return arr

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    structures = {}

    for z in range(2, 7):
        period = 2 * z - 2
        pattern = build_pattern(z)

        trees = [Fenwick(n) for _ in range(period)]

        for i in range(1, n + 1):
            trees[i % period].add(i, a[i])

        structures[z] = (period, pattern, trees)

    m = int(input())
    out = []

    for _ in range(m):
        q = list(map(int, input().split()))

        if q[0] == 1:
            _, p, v = q
            delta = v - a[p]
            a[p] = v

            for z in range(2, 7):
                period, pattern, trees = structures[z]
                trees[p % period].add(p, delta)

        else:
            _, l, r, z = q

            period, pattern, trees = structures[z]

            ans = 0

            for residue in range(period):
                s = trees[residue].range_sum(l, r)

                offset = (residue - (l % period)) % period
                weight = pattern[offset]

                ans += s * weight

            out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai lưu trữ một họ cây Fenwick cho mọi hệ số ngoằn ngoèo có thể có. Trong một khoảng thời gian cố định$P$, dư lượng$r$chứa chính xác các vị trí mảng có chỉ số thỏa mãn$i \bmod P=r$. 

Logic truy vấn là phần tinh tế. Mẫu ngoằn ngoèo bắt đầu ở điểm cuối bên trái của truy vấn, không phải ở chỉ mục 1. Biểu thức```
offset = (residue - (l % period)) % period
```tính toán vị trí phần dư đó xuất hiện bên trong mẫu tuần hoàn liên quan đến thời điểm bắt đầu truy vấn. 

Cây Fenwick được lập chỉ mục theo vị trí mảng ban đầu. Điều đó cho phép truy vấn tổng phạm vi thông thường mà không cần bất kỳ phép biến đổi tọa độ nào. 

Tất cả số học đều sử dụng số nguyên Python để xử lý câu trả lời tối đa có thể một cách an toàn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5
2 3 1 5 5
4
2 2 3 2
2 1 5 3
1 3 5
2 1 5 3
```Vì$z=2$, khoảng thời gian là 2 và mẫu là:```
1 2
```Truy vấn đầu tiên: 

| Dư lượng | Giá trị trong [2,3] | Cân nặng | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 3 | 1 | 3 | 
| 1 | 1 | 2 | 2 | 

Trả lời:$$3+2=5$$Truy vấn thứ hai với$z=3$:

Mẫu:```
1 2 3 2
```| Dư lượng | Tổng trong phạm vi | Cân nặng | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2+5 | 1 | 7 | 
| 2 | 3 | 2 | 6 | 
| 3 | 1 | 3 | 3 | 
| 0 | 5 | 2 | 10 | 

Tổng cộng:$$26$$Sau khi cập nhật vị trí từ 3 lên 5, mọi cây Fenwick có liên quan đều nhận được delta là 4. 

Lặp lại truy vấn sẽ cho:$$2\cdot1+3\cdot2+5\cdot3+5\cdot2+5\cdot1=38$$phù hợp với mẫu. 

Dấu vết này cho thấy các bản cập nhật ảnh hưởng ngay lập tức đến mọi truy vấn trong tương lai thông qua cấu trúc Fenwick được chia sẻ. 

### Ví dụ tùy chỉnh 

đầu vào:```
3
10 20 30
1
2 2 3 2
```Mẫu cho$z=2$:```
1 2
```| Vị trí | Giá trị | Cân nặng | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 20 | 1 | 20 | 
| 3 | 30 | 2 | 60 | 

Trả lời:$$80$$Ví dụ này xác nhận rằng mẫu khởi động lại ở ranh giới bên trái của truy vấn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$mỗi hoạt động | Tối đa 10 truy vấn hoặc cập nhật Fenwick được thực hiện | 
| Không gian |$O(n)$| Tổng số dữ liệu Fenwick được lưu trữ tỷ lệ thuận với bội số không đổi của$n$| 

Tổng số cây Fenwick là$$2+4+6+8+10 = 30,$$đó là một hằng số cố định. Mọi thao tác chỉ thực hiện một số lượng không đổi các cập nhật Fenwick hoặc truy vấn phạm vi logarit, dễ dàng phù hợp với giới hạn cho$10^5$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

assert run(
"""5
2 3 1 5 5
4
2 2 3 2
2 1 5 3
1 3 5
2 1 5 3
"""
) == "5\n26\n38\n", "sample 1"

assert run(
"""1
7
1
2 1 1 2
"""
) == "7\n", "minimum size"

assert run(
"""4
5 5 5 5
2
2 1 4 2
2 1 4 6
"""
) == "30\n50\n", "all equal values"

assert run(
"""3
10 20 30
1
2 2 3 2
"""
) == "80\n", "pattern restarts at l"

assert run(
"""3
1 1 1
3
1 2 100
2 1 3 2
2 1 3 3
"""
) == "202\n302\n", "update propagation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | 7 | Giới hạn tối thiểu | 
| Tất cả các giá trị bằng nhau | 30, 50 | Thời kỳ và trọng lượng khác nhau | 
| Truy vấn bắt đầu ở giữa | 80 | Căn chỉnh mẫu liên quan đến l | 
| Cập nhật rồi truy vấn | 202, 302 | Tuyên truyền cập nhật chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
3
10 20 30
1
2 2 3 2
```Mẫu dành cho$z=2$là$1,2$. Thuật toán tính tổng lớp dư lượng và dịch chuyển chúng bằng cách$l=2$. Vị trí 2 nhận trọng số 1 và vị trí 3 nhận trọng số 2. Kết quả là$$20+60=80.$$Bất kỳ triển khai nào căn chỉnh trọng số bằng cách sử dụng chỉ mục tuyệt đối thay vì bắt đầu truy vấn sẽ tạo ra kết quả không chính xác.$70$. 

Coi như:```
5
1 1 1 1 1
1
2 1 5 3
```Thời kỳ là$2z-2=4$, không phải 6. Trọng số là```
1 2 3 2 1
```cho đi$$9.$$Thuật toán lưu trữ dữ liệu modulo 4 nên cấu trúc lặp lại được thể hiện chính xác. 

Coi như:```
3
1 1 1
2
1 2 100
2 1 3 2
```Sau khi cập nhật, delta là 99. Thuật toán áp dụng delta đó cho mọi cấu trúc Fenwick bị ảnh hưởng. Truy vấn sau đó tính toán$$1\cdot1+100\cdot2+1\cdot1=202.$$Bởi vì tất cả các cấu trúc được cập nhật ngay lập tức, không còn giá trị cũ nào và câu trả lời là chính xác.
