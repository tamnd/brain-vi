---
title: "CF 314C - Sereja và các chuỗi"
description: "Chúng ta được cho một mảng các số nguyên dương. Xem xét mọi dãy con không giảm không trống riêng biệt của mảng. Với mỗi dãy con $y = (y1,dots,yk)$, chúng ta đếm tất cả các dãy số nguyên dương $x = (x1,dots,xk)$ thỏa mãn $$xi le yi$$ cho mọi vị trí."
date: "2026-06-06T01:18:08+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp"]
categories: ["algorithms"]
codeforces_contest: 314
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 187 (Div. 1)"
rating: 2000
weight: 314
solve_time_s: 159
verified: true
draft: false
---

[CF 314C - Sereja và các chuỗi tiếp theo](https://codeforces.com/problemset/problem/314/C) 

**Xếp hạng:** 2000 
**Thẻ:** cấu trúc dữ liệu, dp 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên dương. 

Hãy xem xét mọi **dãy con không giảm không khác biệt, không trống** của mảng. Với mỗi dãy con như vậy$y = (y_1,\dots,y_k)$, chúng tôi đếm tất cả các chuỗi số nguyên dương$x = (x_1,\dots,x_k)$thỏa mãn$$x_i \le y_i$$cho mọi vị trí. 

Đối với một dãy con cố định$y$, số dãy hợp lệ$x$đơn giản là$$y_1 \cdot y_2 \cdots y_k$$bởi vì mỗi vị trí có thể độc lập chọn bất kỳ giá trị nào từ$1$ĐẾN$y_i$. 

Nhiệm vụ là tính tổng của các tích này trên tất cả **các dãy con không giảm riêng biệt** của mảng ban đầu. 

Chiều dài mảng đạt$10^5$, và giá trị đạt tới$10^6$. Việc liệt kê các dãy con ngay lập tức là không thể vì một mảng có độ dài$10^5$có thể có nhiều dãy con theo cấp số nhân. Thậm chí$O(n^2)$ở đây quá chậm vì nó đòi hỏi khoảng$10^{10}$hoạt động. 

Giá trị giới hạn nhỏ hơn nhiều so với số lượng các chuỗi con có thể có. Điều đó gợi ý việc xây dựng một chương trình động được lập chỉ mục theo giá trị thay vì theo chuỗi con. 

Một điểm tinh tế là từ **khác biệt**. Hai dãy con được chọn từ các vị trí khác nhau nhưng tạo ra dãy có cùng giá trị chỉ được tính một lần. 

Ví dụ:```
2
1 1
```Dãy con không giảm là:```
[1]
[1]
[1,1]
```nhưng những cái khác biệt chỉ là:```
[1]
[1,1]
```Đóng góp của họ là```
1 + 1 = 2
```Một DP đếm một cách mù quáng các lần xuất hiện sau đó sẽ tạo ra 3 thay vì 2. 

Một lỗi dễ dàng khác xuất hiện khi các giá trị bằng nhau xuất hiện nhiều lần.```
3
2 2 2
```Dãy con không giảm riêng biệt là:```
[2]
[2,2]
[2,2,2]
```Câu trả lời là```
2 + 4 + 8 = 14
```Giải pháp phải ghi đè cẩn thận các đóng góp cũ cho giá trị 2 thay vì tích lũy các chuỗi trùng lặp. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Tạo mọi dãy con, chỉ giữ lại những dãy không giảm, lưu trữ chúng trong một bộ để loại bỏ các dãy trùng lặp, tính tích của mỗi dãy và tính tổng kết quả. 

Điều này đúng vì mọi dãy con không giảm riêng biệt đều được tạo và tính một cách rõ ràng một lần. 

Vấn đề là số lượng các chuỗi con. Một mảng có chiều dài$10^5$có$2^{100000}$các chuỗi tiếp theo. Ngay cả đối với$n=50$, việc liệt kê đã là vô vọng rồi. 

Quan sát quan trọng là chúng ta không bao giờ cần đến các chuỗi con. Chúng tôi chỉ cần tổng sản phẩm của họ. 

Giả sử chúng ta xử lý mảng từ trái sang phải. Cho phép$$F(v)$$biểu thị tổng đóng góp của tất cả các dãy con không giảm riêng biệt có giá trị cuối cùng chính xác là$v$. 

Khi chúng ta gặp một phần tử mảng mới$x$, mọi dãy con hiện có đều kết thúc bằng một giá trị nhiều nhất$x$có thể được mở rộng bởi$x$. Kể từ khi nối thêm$x$nhân tích của dãy con với$x$, tổng đóng góp của tất cả các dãy con mới hình thành kết thúc tại$x$là$$x \cdot \sum_{t \le x} F(t).$$Chúng ta cũng phải bao gồm dãy con một phần tử$[x]$, sản phẩm của ai là$x$. 

Điều đó mang lại$$\text{new}(x) = x \cdot \left(1 + \sum_{t \le x} F(t)\right).$$Thử thách còn lại là xử lý sự khác biệt. 

Nếu giá trị$x$xuất hiện trước đó, một số dãy con kết thúc bằng$x$đã được đại diện trong$F(x)$. Bí quyết tiêu chuẩn là thay thế phần đóng góp cũ bằng giá trị$x$bởi cái mới được tính toán. Sự xuất hiện mới hơn có thể tạo ra mọi chuỗi riêng biệt kết thúc bằng$x$điều đó hiện có thể thực hiện được nên giá trị cũ trở nên lỗi thời. 

Chúng ta cần hai thao tác hiệu quả: 

1. Truy vấn$\sum_{t \le x} F(t)$. 
2. Thay thế$F(x)$bằng một giá trị mới. 

Vì giá trị lớn nhất là$10^6$, cây Fenwick trên tọa độ giá trị hỗ trợ cả hai phép toán trong$O(\log 10^6)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$| Hàm mũ | Quá chậm | 
| Tối ưu |$O(n \log V)$|$O(V)$| Đã chấp nhận | 

Đây$V = 10^6$. 

## Hướng dẫn thuật toán 

1. Tạo cây Fenwick được lập chỉ mục theo các giá trị từ 1 đến$10^6$. 
2. Duy trì một mảng`last[v]`, nơi lưu trữ đóng góp hiện tại$F(v)$. 
3. Xử lý mảng đầu vào từ trái sang phải. 
4. Đối với giá trị hiện tại$x$, truy vấn cây Fenwick để tìm$$S = \sum_{t \le x} F(t).$$Điều này thể hiện sự đóng góp tổng cộng của tất cả các dãy con không giảm riêng biệt mà phần tử cuối cùng của nó nhiều nhất là$x$. 
5. Tính toán$$cur = x \cdot (1 + S).$$Thuật ngữ$1$tạo ra chuỗi phần tử đơn$[x]$. Thuật ngữ$S$mở rộng mọi dãy con hợp lệ kết thúc bằng một giá trị nhiều nhất$x$. 
6. Hãy để`old = last[x]`. 

Chúng tôi đang thay thế toàn bộ sự đóng góp của giá trị$x$, không thêm vào nó. Đây là công cụ loại bỏ các bản sao được tạo bởi các lần xuất hiện lặp lại của cùng một giá trị. 
7. Cập nhật cây Fenwick bằng cách thêm$$cur - old.$$Sau thao tác này, cây Fenwick lại lưu trữ các giá trị chính xác$F(v)$. 
8. Đặt`last[x] = cur`. 
9. Sau khi tất cả các phần tử được xử lý, câu trả lời là tổng của tất cả$F(v)$, bằng tổng tiền tố Fenwick trên toàn bộ phạm vi. 

### Tại sao nó hoạt động 

Sau khi xử lý bất kỳ tiền tố nào của mảng,$F(v)$được định nghĩa là tổng tích của tất cả các chuỗi con không giảm **riêng biệt** có thể đạt được từ tiền tố đó có giá trị cuối cùng bằng$v$. 

Khi một giá trị mới$x$đến, mỗi dãy con kết thúc bằng một giá trị nhiều nhất$x$có thể được mở rộng bởi$x$, và mỗi sản phẩm được nhân với$x$. Việc cộng chuỗi con một phần tử sẽ tạo ra chính xác$$x \cdot \left(1 + \sum_{t \le x} F(t)\right).$$Nếu như$x$xuất hiện trước đó, giá trị mới được tính toán đã thể hiện tập hợp đầy đủ các dãy con riêng biệt kết thúc bằng$x$có thể được hình thành từ tiền tố hiện tại. Thay thế trước đó$F(x)$ngăn chặn việc đếm trùng lặp các chuỗi giá trị giống hệt nhau. Bằng cách quy nạp trên các vị trí được xử lý, bất biến vẫn đúng với mọi giá trị. Tổng cuối cùng của tất cả$F(v)$chính xác là câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007
MAXV = 1000000

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, delta):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] = (bit[idx] + delta) % MOD
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        bit = self.bit
        while idx > 0:
            res += bit[idx]
            res %= MOD
            idx -= idx & -idx
        return res

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    fw = Fenwick(MAXV)
    last = [0] * (MAXV + 1)

    for x in a:
        s = fw.sum(x)
        cur = x * (1 + s) % MOD

        old = last[x]
        last[x] = cur

        fw.add(x, (cur - old) % MOD)

    print(fw.sum(MAXV))

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ các giá trị hiện tại$F(v)$. Một truy vấn tiền tố đưa ra$\sum_{t \le x} F(t)$, đó chính xác là số lượng cần thiết cho phép truy hồi. 

Mảng`last`là chi tiết quan trọng. Nếu không có nó, các giá trị lặp lại sẽ tích lũy các đóng góp thay vì thay thế chúng, khiến các chuỗi con trùng lặp được tính nhiều lần. 

Bản cập nhật sử dụng`cur - old`bởi vì cây Fenwick lưu trữ giá trị hiện tại của$F(x)$. Chúng tôi đang thực hiện một nhiệm vụ chứ không phải một sự gia tăng. mô-đun làm việc$10^9+7$có nghĩa là sự khác biệt cũng phải được giảm theo mô đun trước khi chèn. 

Câu trả lời cuối cùng là tổng hợp của tất cả$F(v)$, thu được bằng truy vấn tiền tố Fenwick đầy đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
42
```| Bước | x | Tiền tố Tổng S | cur | F(42) | 
| --- | --- | --- | --- | --- | 
| 1 | 42 | 0 | 42 | 42 | 

Câu trả lời cuối cùng:```
42
```Dãy con không giảm khác biệt duy nhất là`[42]`, có đóng góp là 42. 

### Ví dụ 2 

đầu vào:```
2
1 1
```| Bước | x | Tiền tố Tổng S | cur | F Cũ(1) | F mới(1) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 0 | 1 | 
| 2 | 1 | 1 | 2 | 1 | 2 | 

Câu trả lời cuối cùng:```
2
```Các dãy con khác biệt là:```
[1]
[1,1]
```với sự đóng góp```
1 + 1 = 2.
```Dấu vết cho thấy tại sao cần phải thay thế. Sau lần xuất hiện thứ hai,`F(1)`trở thành 2, không phải 3. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log V)$| Một truy vấn Fenwick và một bản cập nhật cho mỗi phần tử | 
| Không gian |$O(V)$| cây Fenwick và`last`mảng trên các giá trị$1 \ldots 10^6$| 

Với$n = 10^5$Và$V = 10^6$,$\log V$là khoảng 20. Giải pháp chỉ thực hiện vài triệu thao tác nguyên thủy và phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1000000007
MAXV = 1000000

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, delta):
            while idx <= self.n:
                self.bit[idx] = (self.bit[idx] + delta) % MOD
                idx += idx & -idx

        def sum(self, idx):
            res = 0
            while idx > 0:
                res = (res + self.bit[idx]) % MOD
                idx -= idx & -idx
            return res

    n = int(input())
    a = list(map(int, input().split()))

    fw = Fenwick(MAXV)
    last = [0] * (MAXV + 1)

    for x in a:
        s = fw.sum(x)
        cur = x * (1 + s) % MOD
        fw.add(x, (cur - last[x]) % MOD)
        last[x] = cur

    return str(fw.sum(MAXV)) + "\n"

# provided sample
assert run("1\n42\n") == "42\n", "sample 1"

# custom cases
assert run("1\n1\n") == "1\n", "minimum size"
assert run("2\n1 1\n") == "2\n", "duplicate values"
assert run("2\n1 2\n") == "5\n", "strictly increasing"
assert run("3\n2 2 2\n") == "14\n", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Ví dụ nhỏ nhất có thể | 
|`1 1`|`2`| Xử lý trình tự tiếp theo khác biệt | 
|`1 2`|`5`| Tái phát mở rộng cơ bản | 
|`2 2 2`|`14`| Ghi đè lặp đi lặp lại cùng một giá trị | 

## Vỏ cạnh 

Hãy xem xét:```
2
1 1
```Sau giá trị đầu tiên,`F(1)=1`. Sau giá trị thứ hai, tổng tiền tố lên tới 1 cũng là 1, cho`cur=2`. Thuật toán thay thế`F(1)`bằng 2. Đáp án cuối cùng là 2, tương ứng chính xác với`[1]`Và`[1,1]`. Một sự tích lũy ngây thơ sẽ tạo ra không chính xác 3. 

Hãy xem xét:```
3
2 2 2
```Các tiểu bang là:```
F(2)=2
F(2)=6
F(2)=14
```Mỗi bản cập nhật sẽ thay thế giá trị trước đó. Câu trả lời cuối cùng là 14, bằng```
2 + 4 + 8
```cho các dãy con riêng biệt`[2]`,`[2,2]`, Và`[2,2,2]`. 

Coi như:```
2
2 1
```Trình tự tiếp theo`[2,1]`không phải là không giảm và không thể đóng góp. Thuật toán thực thi điều này một cách tự nhiên vì tiện ích mở rộng chỉ được phép từ các giá trị`<= x`. Khi xử lý`1`, sự đóng góp từ các chuỗi kết thúc bằng`2`được loại trừ khỏi truy vấn tiền tố. Câu trả lời trở thành```
2 + 1 = 3
```phù hợp với các chuỗi con hợp lệ`[2]`Và`[1]`.
