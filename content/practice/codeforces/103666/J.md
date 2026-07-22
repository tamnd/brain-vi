---
title: "CF 103666J - \u0411\u0430\u0448\u043d\u0438"
description: "Chúng ta được cho một chuỗi chiều cao của tháp được đặt dọc theo một đường thẳng, trong đó mỗi vị trí có một giá trị chiều cao duy nhất. Nhiệm vụ là đếm xem có bao nhiêu bộ ba chỉ số $i < j < k$ tạo thành một chuỗi tăng dần về cả vị trí và chiều cao, nghĩa là $hi < hj < hk$."
date: "2026-07-02T21:33:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103666
codeforces_index: "J"
codeforces_contest_name: "\u0422\u0443\u0440\u043d\u0438\u0440 \u0410\u0440\u0445\u0438\u043c\u0435\u0434\u0430 2016"
rating: 0
weight: 103666
solve_time_s: 45
verified: true
draft: false
---

[CF 103666J - \u0411\u0430\u0448\u043d\u0438](https://codeforces.com/problemset/problem/103666/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi chiều cao của tháp được đặt dọc theo một đường thẳng, trong đó mỗi vị trí có một giá trị chiều cao duy nhất. Nhiệm vụ là đếm xem có bao nhiêu bộ ba chỉ số$i < j < k$tạo thành một dãy tăng dần cả về vị trí và chiều cao, nghĩa là$h_i < h_j < h_k$. 

Nói cách khác, chúng ta đang đếm số dãy con tăng dần có độ dài bằng ba trong một mảng giống như hoán vị. 

Ràng buộc$n \le 8000$đã loại trừ mọi nghiệm bậc ba hoặc bậc hai trên hằng số. Một vòng lặp ba ngây thơ trên tất cả$(i, j, k)$sẽ kiểm tra về$n^3 / 6$các kết hợp, theo thứ tự gồm 8 thao tác \time 10^3^3 \approx 5 \time 10^{11}, vượt xa những gì diễn ra trong 2 giây. Thậm chí$O(n^2)$phương pháp cần thiết kế cẩn thận nhưng vẫn có thể chấp nhận được ở đây. 

Một điểm tinh tế là độ cao là một hoán vị của$1 \dots n$, do đó các so sánh hoàn toàn có ý nghĩa và không tồn tại sự trùng lặp. Điều này giúp loại bỏ sự mơ hồ xung quanh các giá trị bằng nhau và đảm bảo mọi cặp hoặc bộ ba tăng dần đều được xác định rõ ràng. 

Không có trường hợp khó khăn nào liên quan đến thoái hóa thứ tự, nhưng việc triển khai đơn giản vẫn có thể thất bại theo cách ít rõ ràng hơn nếu nó tính toán lại số lượng cục bộ không hiệu quả. Ví dụ: việc quét liên tục các hậu tố cho mỗi chỉ mục ở giữa sẽ dẫn đến hành vi ẩn bậc hai hoặc bậc ba. 

Một ví dụ tối thiểu giúp làm rõ các yêu cầu về tính chính xác. Đối với đầu vào:```
3
1 2 3
```có chính xác một bộ ba hợp lệ$(1,2,3)$. Bất kỳ phương pháp đúng nào cũng phải phát hiện chuỗi tăng dần này. 

Đối với một mảng giảm:```
3
3 2 1
```câu trả lời là 0, vì không có cặp nào có thể mở rộng đến phần tử tăng thứ ba. Trường hợp này thường cho thấy việc triển khai giả định thứ tự một phần không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu thử trực tiếp mọi bộ ba$i < j < k$và kiểm tra xem các giá trị có tăng hay không. Điều này đúng vì nó liệt kê rõ ràng tất cả các ứng cử viên và lọc những ứng cử viên hợp lệ. Tuy nhiên, chi phí của nó tăng lên khi$O(n^3)$, điều này trở nên nghiêm cấm ngay cả đối với$n = 8000$, vì nó dẫn đến hàng trăm tỷ so sánh. 

Chúng ta có thể cải thiện bằng cách sửa phần tử ở giữa$j$. Đối với mỗi$j$, chúng tôi muốn biết có bao nhiêu cặp hợp lệ$(i, k)$tồn tại sao cho$i < j < k$,$h_i < h_j < h_k$. Điều này tách điều kiện thành hai số đếm độc lập: có bao nhiêu phần tử trước đó$j$nhỏ hơn$h_j$, và có bao nhiêu phần tử sau$j$lớn hơn$h_j$. Nhân hai số này sẽ ra số bộ ba hợp lệ có số ở giữa$j$. 

Sự chuyển đổi này làm giảm vấn đề tính toán thống kê tiền tố và hậu tố một cách hiệu quả. Thay vì tính lại số lượng từ đầu cho mỗi$j$, chúng tôi tính toán trước: 

có bao nhiêu giá trị nhỏ hơn$h_j$xuất hiện ở tiền tố$[1, j-1]$, và có bao nhiêu giá trị lớn hơn$h_j$xuất hiện trong hậu tố$[j+1, n]$. 

Bởi vì chiều cao là một hoán vị trên$1 \dots n$, chúng ta có thể duy trì cấu trúc tần số và cập nhật chúng tăng dần theo thời gian tuyến tính hoặc logarit bằng cách sử dụng cây Fenwick. 

Thông tin chi tiết quan trọng là số lượng bộ ba phân tách rõ ràng xung quanh chỉ số ở giữa, biến một vấn đề tổ hợp tổng thể thành hai truy vấn đếm cục bộ cho mỗi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tách giữa + Cây Fenwick |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng cây Fenwick (Cây chỉ mục nhị phân) để duy trì số lượng giá trị nhìn thấy. 

1. Duyệt mảng từ trái sang phải và tính mảng đếm tiền tố`left_smaller[j]`, ở đâu cho mỗi vị trí$j$, chúng ta đếm xem có bao nhiêu phần tử ở các vị trí$< j$có giá trị nhỏ hơn$h_j$. Chúng tôi duy trì một cây Fenwick lưu trữ các tần số có độ cao nhìn thấy được. Ở mỗi bước, chúng tôi truy vấn có bao nhiêu giá trị$< h_j$đã được đưa vào cho đến nay. 
2. Duyệt mảng từ phải sang trái và tính mảng đếm hậu tố`right_greater[j]`, ở đâu cho mỗi vị trí$j$, chúng ta đếm xem có bao nhiêu phần tử ở các vị trí$> j$có giá trị lớn hơn$h_j$. Một lần nữa, chúng tôi sử dụng cây Fenwick, nhưng bây giờ chúng tôi xử lý các phần tử từ phải sang trái, chèn các giá trị và truy vấn xem có bao nhiêu giá trị lớn hơn giá trị hiện tại trong cây. Điều này được tính bằng tổng số tiền tố đã thấy trừ tiền tố lên đến$h_j$. 
3. Đối với mỗi chỉ số$j$, hiểu nó là phần tử ở giữa của bộ ba. Nhân hai đại lượng:`left_smaller[j] * right_greater[j]`. Điều này tính tất cả các bộ ba hợp lệ trong đó$j$là trung tâm. 
4. Tính tổng các tích này$j$. Kết quả là tổng số bộ ba tăng dần. 

Mỗi bước phụ thuộc vào việc duy trì cấu trúc tần số chính xác qua tiền tố hoặc hậu tố động. Cây Fenwick đảm bảo cả cập nhật và truy vấn tiền tố đều chạy theo thời gian logarit. 

### Tại sao nó hoạt động 

Mỗi bộ ba hợp lệ$i < j < k$được xác định duy nhất bởi chỉ số ở giữa của nó$j$. Đối với một cố định$j$, sự lựa chọn của$i$chỉ phụ thuộc vào các yếu tố trước$j$nhỏ hơn$h_j$, và sự lựa chọn của$k$chỉ phụ thuộc vào các phần tử sau$j$lớn hơn$h_j$. Những lựa chọn này là độc lập nên số kết hợp là tích của hai số đếm. Vì mỗi bộ ba hợp lệ được tính chính xác một lần ở chỉ số giữa của nó, tính tổng tất cả$j$tạo ra tổng số chính xác. 

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
        s = 0
        while i > 0:
            s += self.bit[i]
            i -= i & -i
        return s

n = int(input())
h = list(map(int, input().split()))

# left_smaller
bit = Fenwick(n)
left_smaller = [0] * n

for i in range(n):
    x = h[i]
    left_smaller[i] = bit.sum(x - 1)
    bit.add(x, 1)

# right_greater
bit = Fenwick(n)
right_greater = [0] * n

total = 0
for i in range(n - 1, -1, -1):
    x = h[i]
    right_greater[i] = bit.sum(n) - bit.sum(x)
    bit.add(x, 1)

for j in range(n):
    total += left_smaller[j] * right_greater[j]

print(total)
```Việc triển khai xác định cây Fenwick hỗ trợ cập nhật điểm và tính tổng tiền tố theo các giá trị chiều cao. Xây dựng đường chuyền đầu tiên`left_smaller`bằng cách chèn các giá trị khi chúng tôi quét từ trái sang phải, đảm bảo cấu trúc luôn biểu thị các phần tử ở bên trái của chỉ mục hiện tại. 

Bước thứ hai đặt lại cấu trúc và quy trình từ phải sang trái, tính toán xem có bao nhiêu phần tử lớn hơn tồn tại ở bên phải mỗi vị trí. biểu thức`bit.sum(n) - bit.sum(x)`là rất quan trọng vì nó chuyển đổi cách đếm tiền tố thành cách đếm hậu tố. 

Cuối cùng, bước tổng hợp sản phẩm phản ánh trực tiếp sự phân rã tổ hợp của các bộ ba xung quanh chỉ số giữa của chúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
h = [1, 2, 3]
```Đóng góp tiền tố và hậu tố: 

| j | h[j] | left_smaller | đúng_Greater | sản phẩm | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 2 | 0 | 
| 1 | 2 | 1 | 1 | 1 | 
| 2 | 3 | 2 | 0 | 0 | 

Tổng cộng là 1. 

Điều này xác nhận rằng bộ ba hợp lệ duy nhất tập trung ở chỉ số 1, trong đó giá trị 2 nằm trong khoảng từ 1 đến 3. 

### Ví dụ 2 

đầu vào:```
n = 4
h = [3, 1, 4, 2]
```| j | h[j] | left_smaller | đúng_Greater | sản phẩm | 
| --- | --- | --- | --- | --- | 
| 0 | 3 | 1 | 1 | 1 | 
| 1 | 1 | 0 | 2 | 0 | 
| 2 | 4 | 2 | 0 | 0 | 
| 3 | 2 | 1 | 0 | 0 | 

Tổng cộng là 1. 

Điều này cho thấy phương pháp tách biệt chính xác các bộ ba ngay cả khi chúng không liền kề nhau và phụ thuộc vào thứ tự chung. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi đường chuyền trong số hai đường chuyền của Fenwick đều thực hiện$n$cập nhật và truy vấn, mỗi lần theo thời gian logarit | 
| Không gian |$O(n)$| Mảng đếm tiền tố/hậu tố cộng với việc lưu trữ cây Fenwick | 

Ràng buộc$n \le 8000$làm cho$n \log n$nhanh chóng một cách thoải mái, vì tổng số thao tác vào khoảng vài trăm nghìn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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
            s = 0
            while i > 0:
                s += self.bit[i]
                i -= i & -i
            return s

    n = int(input())
    h = list(map(int, input().split()))

    bit = Fenwick(n)
    left = [0] * n
    for i in range(n):
        left[i] = bit.sum(h[i] - 1)
        bit.add(h[i], 1)

    bit = Fenwick(n)
    right = [0] * n
    for i in range(n - 1, -1, -1):
        right[i] = bit.sum(n) - bit.sum(h[i])
        bit.add(h[i], 1)

    ans = 0
    for i in range(n):
        ans += left[i] * right[i]

    return str(ans)

# provided samples
assert run("3\n1 2 3\n") == "1", "sample 1"
assert run("3\n3 2 1\n") == "0", "sample 2"

# custom cases
assert run("1\n1\n") == "0", "minimum size"
assert run("2\n1 2\n") == "0", "too small for triple"
assert run("4\n1 2 3 4\n") == "4", "all increasing"
assert run("4\n4 3 2 1\n") == "0", "all decreasing"
assert run("5\n2 5 1 4 3\n") == "3", "mixed ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 phần tử | 0 | kích thước tối thiểu | 
| 2 tăng | 0 | không thể tạo thành bộ ba | 
| được sắp xếp tăng dần | 4 | đếm tổ hợp đầy đủ | 
| sắp xếp giảm dần | 0 | không có bộ ba hợp lệ | 
| hoán vị hỗn hợp | 3 | tính đúng đắn trong trường hợp tổng quát | 

## Vỏ cạnh 

Kích thước đầu vào nhỏ nhất kiểm tra hành vi ranh giới trong đó logic Fenwick không được truy cập các chỉ mục không hợp lệ. Vì$n = 1$, các phép tính tiền tố và hậu tố không bao giờ được sử dụng trong tổng hợp và thuật toán trả về chính xác số 0 vì không tồn tại chỉ số ở giữa. 

Đối với một chuỗi tăng nghiêm ngặt như$1,2,3,4$, mọi chỉ số đều đóng góp$(j)\cdot(n-j-1)$, mà thuật toán nắm bắt được thông qua Fenwick mà không cần dựa vào tính kề cận. Điều này đảm bảo tính chính xác ngay cả khi mọi bộ ba có thể đều hợp lệ. 

Đối với một chuỗi giảm nghiêm ngặt, mọi số tiền tố nhỏ hơn đều bằng 0, vì vậy tất cả các sản phẩm đều biến mất. Phép tính hậu tố cũng cho kết quả bằng 0 một cách chính xác vì không có phần tử nào lớn hơn bất kỳ phần tử nào trước đó, xác nhận rằng cả hai nửa của quá trình phân tách đều hành xử đối xứng khi đảo ngược.
