---
title: "CF 2006C - Bộ Eri và bộ mở rộng"
description: "Đối với mỗi mảng con của mảng đã cho, chúng tôi đưa tất cả các giá trị của nó vào một tập hợp, loại bỏ các giá trị trùng lặp. Bắt đầu từ tập hợp đó, chúng ta có thể liên tục chọn hai số hiện có có trung bình cộng là số nguyên và chèn giá trị trung bình đó vào."
date: "2026-06-08T13:32:58+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "divide-and-conquer", "math", "number-theory", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2006
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 969 (Div. 1)"
rating: 2300
weight: 2006
solve_time_s: 173
verified: false
draft: false
---

[CF 2006C - Eri và Bộ mở rộng](https://codeforces.com/problemset/problem/2006/C) 

**Đánh giá:** 2300 
**Tags:** cấu trúc dữ liệu, chia để trị, toán, lý thuyết số, hai con trỏ 
**Thời gian giải:** 2m 53s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi mảng con của mảng đã cho, chúng tôi đưa tất cả các giá trị của nó vào một tập hợp, loại bỏ các giá trị trùng lặp. Bắt đầu từ tập hợp đó, chúng ta có thể liên tục chọn hai số hiện có có trung bình cộng là số nguyên và chèn giá trị trung bình đó vào. 

Một mảng con được gọi là rực rỡ nếu sau hữu hạn nhiều lần chèn như vậy, tập hợp có thể trở thành một khối gồm các số nguyên liên tiếp. 

Nhiệm vụ là đếm xem có bao nhiêu mảng con xuất sắc. 

Thử thách đầu tiên là hiểu được hoạt động tính trung bình thực sự thay đổi điều gì. Hoạt động không bao giờ tạo ra các giá trị nằm ngoài khoảng giữa mức tối thiểu và tối đa hiện tại. Nó chỉ điền vào những điểm còn thiếu trong khoảng đó. 

Thách thức thứ hai là kích thước đầu vào. Trên tất cả các trường hợp thử nghiệm, tổng chiều dài tối đa là$4 \cdot 10^5$. Việc quét bậc hai trên tất cả các mảng con sẽ yêu cầu khoảng$8 \cdot 10^{10}$trong trường hợp xấu nhất vượt xa giới hạn. Bất kỳ lời giải nào được chấp nhận đều phải gần tuyến tính hoặc$O(n \log n)$. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Hãy xem xét một mảng con bao gồm toàn bộ các giá trị bằng nhau:```
[5, 5, 5]
```Bộ này chỉ đơn giản là$\{5\}$, vốn đã liên tiếp rồi. Bất kỳ giải pháp nào tính toán một cách mù quáng một gcd sai phân đều phải xử lý thực tế là không có sai phân nào khác 0 cả. 

Coi như:```
[1, 5]
```Sự khác biệt duy nhất là$4$. Chúng ta có thể chèn$3$, sau đó$2$, sau đó$4$, cuối cùng thu được mọi số nguyên từ$1$ĐẾN$5$. Câu trả lời thật tuyệt vời mặc dù tập hợp ban đầu không liên tục. 

Bây giờ hãy xem xét:```
[1, 7]
```Sự khác biệt là$6$. Mọi giá trị chúng tôi tạo ra sẽ luôn phù hợp với$1 \pmod 2$. Chúng ta không bao giờ có thể có được cả số lẻ và số chẵn, vì vậy khoảng thời gian liên tiếp là không thể. 

Những ví dụ này gợi ý rằng đại lượng quyết định không phải là phạm vi mà là cấu trúc gcd của mọi khác biệt. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi mảng con, xây dựng tập hợp tương ứng, mô phỏng việc đóng dưới các phần chèn điểm giữa và kiểm tra xem có thể đạt được khoảng liên tiếp hay không. 

Bản thân việc mô phỏng đã đắt tiền rồi. có$O(n^2)$các mảng con và thậm chí việc mô tả phần đóng có thể yêu cầu nhiều phần chèn vào. Cách tiếp cận này là hoàn toàn không khả thi. 

Quan sát quan trọng đến từ việc nghiên cứu những gì bảo toàn theo phương pháp trung bình. 

Lấy các giá trị riêng biệt được sắp xếp của một tập hợp và trừ đi giá trị tối thiểu từ mọi phần tử. Bản dịch không thay đổi gì về quá trình này. Sau đó, mọi phần tử đều là bội số của$$g = \gcd(x_2-x_1,\;x_3-x_2,\ldots).$$Khi chúng ta chèn giá trị trung bình, chúng ta đang thay thế hai bội số của$g$bằng bội số khác của$g/2$nếu có thể. Việc lặp lại quá trình này chỉ có thể chia gcd cho lũy thừa hai. Cuối cùng, bao đóng chứa chính xác các số cách nhau bởi$$\frac{g}{2^k},$$trong đó tất cả các yếu tố của hai đã được loại bỏ khỏi$g$. Tập hợp có thể trở thành liên tiếp nếu khoảng cách cuối cùng là$1$, có nghĩa là$g$không chứa thừa số nguyên tố lẻ. Tương đương,$g$phải là lũy thừa của hai. Đây chính xác là quan sát của biên tập. 

Đối với một mảng con$[l,r]$, gcd liên quan là gcd của tất cả các hiệu từng cặp của các giá trị của nó. Một danh tính tiêu chuẩn mang lại$$\gcd(a_{l+1}-a_l,\ldots,a_r-a_{r-1})$$sử dụng các giá trị tuyệt đối. 

Định nghĩa$$d_i = |a_{i+1}-a_i|.$$Khi đó, một mảng con có độ dài ít nhất là hai sẽ xuất sắc nếu gcd của phân đoạn tương ứng trong mảng sai phân là lũy thừa của hai. Các mảng con một phần tử luôn rực rỡ. 

Bây giờ vấn đề trở thành: 

Đếm các phân đoạn của mảng hiệu có gcd là lũy thừa của hai. 

Giải pháp hai con trỏ được chấp nhận duy trì một cửa sổ trượt gcd trên mảng sai phân. Bởi vì gcd chỉ giảm khi cửa sổ mở rộng nên vị trí đầu tiên mà gcd không còn là lũy thừa của hai chuyển động đơn điệu. Điều này cho phép quét tuyến tính tổng thể sau khi sử dụng cấu trúc gcd giống như hàng đợi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot \text{simulation})$| Lớn | Quá chậm | 
| Hai con trỏ tối ưu + Hàng đợi GCD |$O(n)$khấu hao |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$$d_i = |a_{i+1}-a_i|$$vì$0 \le i < n-1$. 

Một mảng con$a[l..r]$với$l<r$tương ứng với đoạn chênh lệch$d[l..r-1]$. 

### 1. Xây dựng mảng khác biệt 

Đối với mỗi cặp liền kề, lưu trữ sự khác biệt tuyệt đối của nó. 

Một mảng con xuất sắc chính xác khi gcd của phân đoạn sai phân tương ứng của nó là lũy thừa của hai. 

### 2. Xử lý sự khác biệt bằng 0 một cách cẩn thận 

Nếu một số giá trị liên tiếp bằng nhau thì mảng hiệu chứa các số 0. 

Một gcd chỉ chứa các số 0 không tự nhiên phù hợp với thử nghiệm lũy thừa hai, nhưng mảng con ban đầu rất tuyệt vời vì tập hợp của nó có kích thước bằng một. 

Nhóm triển khai các số 0 này chạy riêng biệt, chính xác như trong giải pháp hai con trỏ chính thức. 

### 3. Duy trì gcd cửa sổ trượt 

Sử dụng hàng đợi gcd hai ngăn tiêu chuẩn. 

Cấu trúc hỗ trợ: 

- nối vào bên phải, 
- loại bỏ từ bên trái, 
- truy vấn gcd của cửa sổ hiện tại. 

Tất cả các hoạt động đều được khấu hao$O(1)$. 

### 4. Di chuyển ranh giới bên phải 

Đối với mỗi ranh giới bên trái, hãy mở rộng ranh giới bên phải trong khi cửa sổ gcd **không** là lũy thừa của hai. 

Vị trí đầu tiên nơi gcd trở thành lũy thừa của hai cũng xác định tất cả các điểm cuối bên phải lớn hơn, bởi vì việc thu nhỏ cửa sổ chỉ có thể làm tăng gcd và việc mở rộng chỉ có thể làm giảm nó. 

### 5. Đếm các mảng con hợp lệ 

Khi điểm cuối bên phải hợp lệ nhỏ nhất được biết thì mọi điểm cuối bên phải lớn hơn cũng hợp lệ. 

Thêm số lượng điểm cuối như vậy vào câu trả lời. 

Các mảng con một phần tử được tính riêng. 

### Tại sao nó hoạt động 

Đối với bất kỳ tập hợp số nguyên nào, phép tính trung bình chỉ có thể loại bỏ thừa số 2 khỏi gcd của tất cả hiệu. Các thừa số nguyên tố lẻ không bao giờ biến mất. Việc đóng trở thành một khoảng liên tiếp chính xác khi gcd cuối cùng bằng$1$, điều này xảy ra chính xác khi gcd ban đầu là lũy thừa của hai. 

Đối với một mảng con, gcd đó bằng gcd của các hiệu tuyệt đối liền kề bên trong mảng con. Do đó, vấn đề giảm xuống còn việc kiểm tra xem phân đoạn gcd của mảng sai phân có phải là lũy thừa của hai hay không. 

Cửa sổ trượt duy trì chính xác gcd đó. Tính đơn điệu của gcd khi mở rộng cửa sổ đảm bảo rằng ranh giới giữa điểm cuối bên phải không hợp lệ và hợp lệ chỉ di chuyển về phía trước, tạo ra số lượng hai con trỏ tuyến tính. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def is_pow2(x):
    return x > 0 and (x & (x - 1)) == 0

class GCDQueue:
    def __init__(self):
        self.left = []
        self.right = []

    def push(self, x):
        g = x if not self.right else gcd(self.right[-1][1], x)
        self.right.append((x, g))

    def _move(self):
        while self.right:
            x = self.right.pop()[0]
            g = x if not self.left else gcd(self.left[-1][1], x)
            self.left.append((x, g))

    def pop(self):
        if not self.left:
            self._move()
        self.left.pop()

    def query(self):
        if not self.left:
            return self.right[-1][1] if self.right else 0
        if not self.right:
            return self.left[-1][1]
        return gcd(self.left[-1][1], self.right[-1][1])

    def size(self):
        return len(self.left) + len(self.right)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        if n == 1:
            out.append("1")
            continue

        d = [abs(a[i + 1] - a[i]) for i in range(n - 1)]
        m = n - 1

        ans = n
        q = GCDQueue()
```
